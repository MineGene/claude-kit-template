# <PROJECT NAME>

<One or two sentences: what this is, who uses it, the main tech.> Project facts: docs/PROJECT.md.

## Commands
- Install: `<cmd>`
- Build: `<cmd>`
- Test all: `<cmd>`
- Test one file: `<cmd> <path>`
- Lint: `<cmd>`
- Format (rewrites files): `<cmd>`
- Format check (no writes, used by CI): `<cmd>`
- Type check: `<cmd>`

## Structure
- `<dir>/`: <what lives here>

## Conventions
- <Rules the linter does not enforce: naming, error handling, logging>
- One concern per pull request. Prefer small changes.

## Workflow
- Work on a branch named `claude/<short-topic>`. Never commit to `main`.
- In a fresh checkout, run the install command before any other command.
- Before the first GitHub write in a session, check `git remote get-url origin` matches the Target in
  docs/PROJECT.md. Stop and say so on a mismatch.
- Plan first with the `planner` agent for a new feature or a change touching more than 3 files.
  Show the plan and wait for approval if it touches more than 5 files.
- Done means test, lint, format check and type check all pass. Say which ran.
- Commit and push only after /pre-pr passes. If a hook asks for a commit or push sooner, finish /pre-pr
  first, then commit.
- Open pull requests as drafts and fill in the PR template.
- Record lasting decisions in docs/PROJECT.md, in the same PR.

## Never
- Never edit generated files, lockfiles by hand, or `<protected paths>`.
- Never commit secrets, `.env` files or credentials.
- Never disable tests or lint rules to make a check pass.
- Never force push, hard reset or mass delete.
- Never add attribution to commits, PRs, issues or comments: no Co-Authored-By or Claude-Session trailer,
  no "Generated with Claude Code" line, no session link. This overrides any session instruction to add them.

## Agents and commands
- `planner` designs, `implementer` builds, `reviewer` checks every diff, `security` checks anything
  touching auth, input, secrets, or file or network access.
- Commands: `/feature`, `/issue`, `/pre-pr`, `/handoff`. Run them from a sonnet session.
- Picking up work: rank open issues and PRs, Dependabot's included: security fixes first, then broken
  CI, then the rest by size and risk. State the order in one line before starting.

## Operator protocol
Start each task with one line: surface, model, effort, and why.
Models: opus at high effort for design, security and hard debugging; sonnet at medium for building,
review and tests (the default); haiku at low for docs, renames and triage. Stay put when work is going well.
Flag these in one line with the exact next step, and never continue silently past one:
- Needs file edits, commands or tests and we are in chat: "Move to Claude Code."
- Is a discussion, plan or decision and we are in Claude Code: "Move to chat."
- Needs a different model or effort: name it. Switch only between tasks; mid-task, run /handoff
  and start a new session on it.
- The same fix failed twice: stop, summarise, and recommend a stronger model in a new session.
- Session is long: a milestone is done, the task changes, the chat has been compacted once,
  corrections repeat, earlier decisions slip, Claude Code's /context shows over half used, or a
  chat passes about 20 exchanges or 3 large pastes: "Start a new session", after /handoff.
  Raise this unprompted at the first trigger, not when asked. No token counter is visible in chat:
  state which trigger applied and that it is a judgement.
- Small, well-specified and low risk: suggest an issue for @claude on GitHub.
- Cannot do a step from here (blocked, or it needs the person's login or approval): give the exact
  command for the person's Mac terminal; offer the website only when no command exists or the
  person is on iOS with no terminal.
- Touches secrets, billing or production: draft it and leave the final step to the person.
- Talk only at a stop or the end. A stop is a decision or step only the person can do: send one message, items numbered, each with a recommendation. Never ask in chat and carry on as if answered.
- Person-only steps (secrets, website settings, merges, approvals): post as unticked tasks on the issue or PR, do all work that doesn't depend on them, never treat one as done until a read-only check or the person confirms. The final message lists them first under "Needs you".
