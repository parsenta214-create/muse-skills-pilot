# Colab CLI Operation

Public pilot adaptation: personal and environment-specific details removed; substance unchanged.

Version 1.0, created 2026-11-08. Operate Google Colab runtimes via the official `google-colab-cli`. Keep an operation ledger (for example `LEDGER.md` in the skill folder) — read it before every operation and update it after every operation. The ledger holds live state only (account, sessions, compute-unit balance, fixes, current job); it is not part of this public skill.

When to use: provision a Colab GPU/TPU session, run code or a script on it, sync files, check compute-unit balance, or stop a session. When not to use: do not use it for publishing, contacting anyone, or spending beyond the Colab compute units the user already holds. A new session is billable — creating one is a spend decision; create only when the user asked for that work.

## Invocation

- Binary: `colab`, installed in a Python virtual environment (for example `<venv>/bin/colab`, Python 3.12, package `google-colab-cli` 0.7.5).
- There may be no global `colab` on PATH. Always use the virtual-environment path.
- Where system pip refuses installs (PEP 668), the virtual environment is the install route. Never use `--break-system-packages`.
- **Do NOT reinstall, upgrade, or run `colab update` without re-applying the 411 patch** (see below) — reinstalling reverts it.

## Authentication

- Account: the user's Google account. Authenticated via OAuth2 remote copy-paste flow (user approved in person, code pasted in chat, never stored).
- Provider: `--auth=oauth2` (default in installed 0.7.5). Token cached in the OS keyring (`service_name="google-colab-cli"`, `username="oauth_token"`); a legacy `~/.config/colab-cli/token.json` migrates into the keyring.
- First use on a fresh install needs the user's in-person browser consent. Never ask for or store the auth code; it is single-use and expires in minutes.
- **Do NOT confuse `colab auth` with CLI authentication.** `colab auth` injects VM-side GCP credentials into the running kernel (for BigQuery/GCS from notebook code). It is orthogonal to CLI auth and never fixes a CLI 401/403.
- Verify auth in one shot: `colab sessions` (read-only) or `colab whoami` (hidden debug command: email, scopes, audience, expiry). A 401/403 is almost always missing credentials or scopes.

## Mental model

- A session == a live Jupyter kernel on a rented VM. `colab new` allocates a billable VM; `colab stop` releases it. Sessions stay alive while the kernel is active.
- Kernel state PERSISTS across `colab exec` / `colab repl` calls in the same session (same kernel; only the websocket closes on exit). Imports, variables, and functions survive between calls. `colab stop` and `colab restart-kernel` reset it.
- Default working directory is `/content`. Prefer absolute paths (`/content/...`).
- `colab` is fire-and-forget: each command authenticates, does one thing, exits.

## Commands

| Command | Use |
| --- | --- |
| `colab new -s NAME [--gpu GPU] [--tpu TPU] [--high-mem]` | Allocate a VM. Always pass `-s NAME` (omitted name = random 6-hex, ambiguous later). GPUs: T4, L4, G4, H100, A100. TPUs: v5e1, v6e1. |
| `colab sessions` | List server-side assignments; orphans with no local record show as `[?]`. |
| `colab status [-s NAME]` | Hardware, IDLE/BUSY, last execution. |
| `colab restart-kernel [-s NAME]` | Reset the kernel, keep the VM. |
| `colab stop [-s NAME]` | Terminate the VM, release resources. Always stop when done — idle VMs burn compute units. |
| `colab url [-s NAME] [--open]` | Browser URL attaching the Colab web UI to the CLI session. |
| `colab exec [-s NAME] [-f FILE] [--output-image PATH]` | Run a local `.py` or `.ipynb` on the VM (read locally, sent to the kernel). Piped stdin also works. |
| `colab run [--gpu GPU] [--tpu TPU] [--keep] SCRIPT [ARGS...]` | `new` + `exec` + `stop` in one command; exit code propagates; its own `[colab]` chatter goes to stderr, script output to stdout. |
| `colab repl [-s NAME]` | Interactive REPL (piped stdin exits on EOF; interactive TTY hangs an agent — never run unpiped). |
| `colab console [-s NAME]` | Raw TTY shell via tmux (piped stdin works; output contains terminal-control bytes — filter with `grep -a`). |
| `colab ssh [-s NAME] [--proxy-mode] [-i KEY]` | SSH shell over WebSocket, or ProxyCommand bridge. |
| `colab ls / upload / download / rm / edit [-s NAME]` | File ops on the VM (`/content/...` paths). |
| `colab install [-s NAME] [-r FILE \| PKG...]` | Install packages on the VM via `uv` (falls back to `pip`). |
| `colab log [-s NAME] [-n N] [-o FILE]` | Session history; `-o summary.ipynb` (also `.md`, `.txt`, `.jsonl`) exports it. |
| `colab usage` | Compute-unit usage rate and balance. |
| `colab version` | Installed CLI version. |
| `colab drivemount [-s NAME] [PATH]` | Mount Drive at `/content/drive` (interactive — needs a human at a terminal; never run from an agent). |

Gotchas:
- An unrecognized `--gpu` value silently falls back to A100 (then usually fails). Before calling `colab new`, verify the `--gpu` value is one of T4 / L4 / G4 / H100 / A100. A `400` on `colab new` with an accelerator means no quota/entitlement for it on your account — fall back to `--gpu T4` or CPU.
- Accelerator availability is tier-gated; do not assume a GPU/TPU will allocate.
- `colab exec` on a `.ipynb` runs each code cell and writes `<basename>_output.ipynb` next to the input.
- Execution timeouts: `exec`'s `--timeout` is in **seconds**, default 30 — always set it for long jobs.
- Isolate parallel/agent runs with `--config <path>` (scratch sessions file).

## The 411 empty-POST patch (local, venv-only)

- Symptom (2026-11-08): `colab new --gpu G4` failed with server error 411 — the CLI sent a bodyless POST to the assign endpoint, and Colab still 411s with `Content-Length: 0`.
- Fix applied in the virtual environment only: `<venv>/lib/python3.12/site-packages/colab_cli/client.py`, in `_issue_request` (~line 229):
  ```python
  # Local patch 2026-11-08: Colab's assign endpoint answers 411 Length
  # Required to a bodyless POST, and still 411s with Content-Length: 0,
  # so send a minimal non-empty body. Only applies when the caller
  # passed no body at all; reinstalling the package reverts this.
  if method.upper() == "POST" and not any(
      k in kwargs for k in ("data", "json", "files", "content")
  ):
      kwargs["data"] = b"{}"
  ```
- **Reinstalling or upgrading the package reverts this patch.** After any reinstall, re-apply it before doing real work. Check the patch is present with `grep "Local patch 2026-11-08" <venv>/lib/python3.12/site-packages/colab_cli/client.py` (free and certain) — a `colab new` re-verification costs compute units, so use it only when the patch's presence is in doubt.
- The patch applies to **all** bodyless POSTs, not just assign (side effects on other endpoints unconfirmed). No package version pinning is in place (0.7.5).
- Upstream is not patched (a locally cloned repo is the pristine reference). If a future upstream release fixes the 411, the local patch becomes unnecessary — check the upstream CHANGELOG first.

## Upstream reference

- Upstream repo: https://github.com/googlecolab/google-colab-cli — clone it locally as a read-only mirror (`--depth 1`) if needed.
- Read first for full docs: `README.md`, `docs/01_session_management.md` … `docs/06_ssh_access.md`, `skills/colab-operator/SKILL.md` (the repo's own operator skill), `AGENTS.md` (repo development rules — do NOT follow its release/tagging workflow unless the user explicitly asks for a release).
- Update the mirror with `git pull` when the CLI misbehaves or a new version is needed. Never edit the clone for operation; the live patch lives in the virtual environment.
- Known upstream caveat (external report, unverified here): a `uv` upgrade once pulled `jupyter_kernel_client` 1.0.2, which renamed `KernelClient` and broke `colab` before allocating a VM. If `colab` fails at startup after an upgrade, check that dependency first.

## Safety

- Every `colab new` allocates a real, billable VM. Record every endpoint created and clean up with `colab stop` before declaring work done. Verify with `colab sessions` that nothing was orphaned.
- For a bounded job, prefer `colab run` (without `--keep` it stops the VM when the script ends) instead of leaving a session up. Never leave a session idle: stop it when the job is done, then verify with `colab sessions` and `colab usage`.
- An orphan `[?]` that `colab stop` cannot stop is unassigned with the internal API `state.client.unassign(endpoint)` (from the upstream AGENTS.md). Verify the endpoint against the `colab sessions` output before calling it. It is an internal API and may break on upgrade.
- `colab sessions` orphans (`[?]`) are server-side assignments with no local record. Unassign them when they are genuinely leaked — **but never stop a session the user is actively using in the browser**. If in doubt, ask.
- Cost: G4 ≈ 10.05 compute units/hr per session (measured 2026-11-08: 1 session → 10.05/hr, 2 sessions → 20.10/hr; check current pricing before a long job). Check `colab usage` before starting a long job and report the burn rate.
- Never run `colab repl`, `colab console`, `colab auth`, or `colab drivemount` interactively from an agent — they expect a TTY and will hang. `repl`/`console` accept piped stdin and exit on EOF; `auth`/`drivemount` genuinely require a human.

## Recovery

- "Session not found" / 404 / 401 on exec: the backend pruned the VM. `colab exec`/`repl` clean up local state automatically — run `colab sessions` and re-create with `colab new`.
- Execution timeout or wedged kernel: `colab restart-kernel -s NAME` (keeps the VM, resets the kernel), or `colab stop` then `colab new`.
- 411 on `new`: the local patch was lost (reinstall/upgrade) — re-apply it, then retry.

## Ledger discipline

- Keep an operation ledger (for example `LEDGER.md` in the skill folder). Update it after every operation: sessions created/stopped, compute-unit balance, fixes applied, issues hit, and the current job's state.
- Keep the ledger short and factual: what, when (local time), and the numbers. No speculation.
