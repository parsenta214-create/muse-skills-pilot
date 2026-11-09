# Vibe-Coding Setup

Draft for public use - personal and environment-specific details removed.

This guide describes how to set up a vibe-coding department: the commander agent supervises and decides, a coding agent does the implementation, and an outside reviewer checks the work. It is written for a low-memory virtual machine, where the command-line interface is the only practical route.

## What this setup is

- **Commander ([Agent name]).** Writes the specification, calls the coding agent, checks the result, and reports to the user. Supervision and decisions stay with the commander; the final decision stays with the human user.
- **Coding agent (AntiGravity, command-line interface).** Implements only what the written specification says. It does not decide scope, and it does not work without an approved specification.
- **Reviewer (for example ChatGPT).** Reviews the specification and the code in the browser, with the same-chat rule: a re-review goes to the same chat, with only the diff and the list of resolved findings, so resolved items are not reopened without new evidence.

## Prerequisites checklist

- [ ] An agent environment for the commander (your own account and workspace).
- [ ] A GitHub account, and a repository for the project (private is fine to start).
- [ ] A Google account, for AntiGravity sign-in.
- [ ] Google AI Pro, or an equivalent paid plan, if you want wider AntiGravity use. This is a paid subscription the user contracts manually. Check current pricing at contract time; do not rely on any price quoted in an old guide.
- [ ] Accounts for the review consultants you plan to use (for example ChatGPT, Claude, Gemini, Grok; DeepSeek is an optional extra). Sign in to each one yourself, in person.
- [ ] A working folder for the coding agent, kept separate from the repository's main working copy when the implementation touches a live project. The commander syncs and commits; the coding agent does not push on its own.

## What to install

- [ ] Install the AntiGravity command-line tool (`agy`). Check the official installation instructions at install time and confirm the version you installed.
- [ ] Confirm the tool runs (`agy --version` or the equivalent) and that it lists the available models.
- [ ] Keep the coding agent's working folder under version control. Check its status with short read-only commands before and after work.
- [ ] Do not store tokens, passwords, or API keys in chat or in files. Use your platform's secure credential storage or environment variables, and never output a credential value.

## What is manual (the user does it, in person)

- [ ] Google sign-in for AntiGravity, including any verification step.
- [ ] Subscribing to Google AI Pro (or an equivalent plan), and paying for it. Check current pricing and terms at contract time.
- [ ] Signing in to each review consultant.
- [ ] Approving each specification before the coding agent runs it.
- [ ] Approving each commit and push, and each delivery to a device or a store.

The commander never signs in, pays, or approves on the user's behalf.

## What is out of scope in a low-memory virtual machine

- [ ] AntiGravity Desktop and IDE: no display and no desktop session in this kind of VM, so there is no way to launch them. The command-line interface is the only route in scope.
- [ ] Resident processes, Docker, Kubernetes, and a local always-on language model: do not add them when free memory is small (for example, well under 1 GiB). Check free memory before starting heavy work.
- [ ] The AntiGravity SDK: this is a separate route that uses an API key and its own billing, distinct from the command-line tool's Google sign-in. It is deferred in this setup; the commander is the interface to AntiGravity (write the specification, call the CLI, verify the result).
- [ ] Direct browser automation that needs a desktop session: do not try to work around a platform policy that blocks it.

## The work loop (generic terms)

1. **Write a specification.** One specification per change. State the scope, the current behaviour, the wanted behaviour, the tests, and the acceptance criteria. Give it an ID (for example `SPEC-001`) and a version.
2. **Review the specification.** Send it to the reviewer. The reviewer returns a verdict with findings graded by severity: P0 (must fix, blocks adoption), P1 (should fix before adoption), P2 (minor, may be deferred with a reason). Fix the findings, then send only the diff and the resolved-findings list for re-review.
3. **Freeze the specification.** Adoption and freezing are the user's decision. Once frozen, the specification is the contract: the coding agent implements it as written, and unadopted drafts remain proposals only.
4. **Implement.** The commander calls the coding agent with the frozen specification, the working folder, and the allowed commands stated explicitly in the request. The coding agent works inside its folder only. Commands outside the allowed list are refused automatically; state the allowed list in every request.
5. **Verify.** The commander runs the tests and the build independently and checks the result against the specification, item by item. A report is split into completed work, evidence, unfinished work, and next steps. Nothing is called complete without evidence.
6. **Escalate on deadlock.** If the usual reviewer loop does not converge after several rounds, escalate to a stronger model with the organised background and the unresolved findings, rather than looping again.
7. **Deliver only with approval.** Delivery, installation on a device, and publication each need the user's explicit approval of the exact file and action.

## Practical notes from use

- State the allowed commands in every request to the coding agent. A request without them is likely to be refused or to fail part-way.
- Ask for evidence with every claim in a report. Anything not checked is written as unconfirmed, not as done.
- Quotas are shown as percentages only, and the exact limits are not published. Do not start the interface just to check a quota; checking consumes quota. When a quota error appears, stop that route, do not retry immediately, and report the reset time if one is given.
- Keep one working session at a time for the coding agent where the tool allows only one account at a time. Switching accounts needs a fresh sign-in.
- An allow-list is not a sandbox. Verification after the work matters as much as the allow-list: check what actually changed, and check the repository state, before reporting completion.
