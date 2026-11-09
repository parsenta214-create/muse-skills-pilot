# Koyuki-Equivalent Environment — Public Asset Pack (Pilot)

A set of agent skills and guides for building an environment where another person can work with capabilities equivalent to a personal commander agent. Pilot version: skills + guides, not the agent itself.

## What this is, and what it is not

- This pack contains procedures, working styles, and setup guides. It does not contain the agent model itself, any platform account, or any person's data.
- The agent in the original environment is a hosted model. What is reproduced here is how it works: how it researches, archives, consults other AIs, reviews specifications, and supervises implementation.
- Services are framed as generally US–Japan composable.

## Contents

### Skills (10)

- `research/` — Checked, multi-language research. Split a topic into checkable claims (C001, ...), give each a state, check against independent sources, run a countercheck, write a report that keeps conflicts visible.
- `archive/` — Verified archive. Keep original, extraction, AI summary, and verification record in separate layers; verify size and SHA-256 after saving.
- `ai-browser-consultation/` — Consult an external AI (ChatGPT, Claude, Gemini, Grok; DeepSeek optional) through the browser. Declare identity first, send text only, treat answers as claims to check, never sign in or spend on the user's behalf.
- `x_post_fetch/` — Read an X post and its thread over plain HTTPS.
- `sns-monitor/` — Monitor social sources.
- `nim-rag/` — Local document search with evidence and similarity.
- `colab-cli/` — Operate Google Colab runtimes from the command line.
- `grok-search-layer/` — Search recent X posts through the xAI search tool.
- `browser_artifact_capture/` — Capture and verify a file produced in the browser before reporting it as delivered.
- `external_review_verification/` — Verify an external review before acting on it.

Each skill is a `SKILL.md` (the procedure) plus a `skill.yaml` (name, title, description).

### Guides (in `docs/`)

- `docs/modes.md` — Nine operating modes: hallucination-suppression, professional counterargument, 100-point improvement, deep-research, sellable-project design, work-time-reduction, reverse-roadmap, failure-prediction, and AI-team formation. Plus the red-line rule.
- `docs/ai-corporation.md` — Commander agent plus specialist consultants: routing, declaration before consulting another AI, masking personal information, recording results, human user as final decider.
- `docs/vibe-coding-setup.md` — Vibe-coding department setup: commander writes the specification, a coding agent (AntiGravity, command-line interface) implements it, an outside reviewer checks it. SPEC → review (P0/P1/P2) → freeze → implement → verify.

## What was changed for publication

Adapted from a personal workflow. Substance unchanged; only publication clean-up was done:

- Removed personal names, private example cases, Drive file IDs, and internal paths.
- Replaced environment-specific tool names and paths with generic descriptions, so the skills work in other agent environments.
- Removed region-specific service references. Consultants are centred on ChatGPT, Claude, Gemini, and Grok, with DeepSeek as an optional additional service.
- No personal names, no secrets, no API keys, no passwords, no tokens in these files. The internal asset-map draft (`ASSET-MAP-DRAFT.md`) is not part of the public pack.

## Prerequisites for the reader

The reader supplies their own accounts, sign-ins, subscriptions, and payments, all done by hand:

- An agent environment and a GitHub account.
- A Google account; Google AI Pro is a paid subscription, contracted manually, if wider AntiGravity use is wanted. Check current pricing at contract time.
- Accounts for the consultant AIs they want to use (ChatGPT, Claude, Gemini, Grok, and optionally DeepSeek).
- No password or API key is included in this pack, and none should be added to it.

## Limitations — read before using

- `x_post_fetch/` is not self-contained. It depends on an `x_thread` helper that is not in this pack; the skill uses a `<path-to-your-x_thread-helper>` placeholder. Supply or build an equivalent helper first.
- `ai-browser-consultation/` refers to the reader's own foundation documents and builder runbook as placeholders. Tier routing works only if the reader supplies their own.
- The light-model reviewer slot in `ai-browser-consultation/` is unfilled. The reader must name their own light model.
- `sns-monitor/` assumes a Facebook connector exists in the reader's environment; it does not provide one.
- `colab-cli/` keeps a dated cost example (G4, measured 2026-11-08) with a check-current-pricing note; do not treat it as current pricing. Its 411 patch is preserved verbatim, but whether it is still needed depends on the reader's installed version — check the upstream CHANGELOG first.
- `grok-search-layer/` keeps its search cap only as an example cap; the reader must set their own.
- `nim-rag/` generalises model names to lightweight / strong model; the reader must choose their own models.

## Red lines

Publishing, contacting anyone, changing a profile or setting, spending money, deleting, public sharing, and adopting a manuscript all need the user's explicit approval of the exact content or action. Preparation and drafting are not approval.

## License

MIT License. See `LICENSE`.

## Status

Pilot. If these work well in public, more material may follow in the same form. Issues and improvements are welcome, but please do not include personal data or secrets in them.
