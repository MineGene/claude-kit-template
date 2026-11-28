# Claude Agentic Project Starter Kit

Nov 27, 2026 · @J

Start every Claude project from this kit and change only what the customisation table allows. Every project then gets the same files, names, defaults and stage order, whoever runs it.

Setup is two short rounds of questions, an approved brief, then fixed stages. Layers 1 to 5 are repo files; Layer 6 is the operator protocol that tells the person when to switch model, effort, surface or session. Last checked against the Claude Code, GitHub and Claude pricing docs on 2026-11-28.

## How to use this kit

**For Claude:** run the stages in Project establishment, then After establishment, in order. Create exactly the files in the repo layout and fill every `<PLACEHOLDER>` from `docs/PROJECT.md`. Ask about anything you cannot fill, and add nothing the customisation table does not list. Until stage 5 puts it in CLAUDE.md and the Project instructions, follow the Operator protocol block in the Layer 1 template directly.

**For a person:** answer two rounds of questions and approve the brief. Your only other jobs are the steps marked *Person* in the stage table: create the repo, merge the bootstrap PR, set branch rules, paste the Project instructions.

**Where the kit lives.** One public GitHub template repo, `KIT-OWNER/claude-kit-template`, holds the kit itself as `KIT.md` plus every file in the repo layout with its placeholders, except the two Claude workflows. It holds nothing personal, so any account or organisation can use it. Change the kit there, in one PR, never inside a project. For stage 0 chats, keep a copy of the kit in the knowledge of one Claude Project, "Starter kit".

**Entry paths.** Every project comes in one of three ways, depending on the state of its repo.

| Repo state | How the kit gets in | Start at |
| --- | --- | --- |
| Not started (no repo yet) | `gh repo create OWNER/name --template KIT-OWNER/claude-kit-template --private` (or `--public`) | Stage 2 |
| Very new (repo exists, little or no code) | The Claude Code session clones the template and copies its files and `KIT.md` into the `claude/bootstrap` branch; if it cannot reach the template, paste the kit text under the prompt | Stage 2, scaffolding only if there is no code |
| Old (real code, maybe its own CI or CLAUDE.md) | The same copy, then the "Existing repo" customisation row: intake answers come from the repo, and files are merged, not overwritten | Stage 3 |

`KIT.md` stays in the project only during setup: the stage 6 PR adds the Claude workflows from it, then deletes it.

**Once per account or organisation**

- Install the Claude GitHub app on it. This is website only, and an organisation admin may need to approve it.
- Sign `gh` in to it with `gh auth login`, and run `gh auth switch` before its admin commands.
- Store the subscription token: once per repo on a personal account, or once for an organisation with `gh secret set CLAUDE_CODE_OAUTH_TOKEN --org ORG --visibility all`.
- Note its GitHub plan: rulesets on private repos need a paid plan, for accounts and organisations alike.
- If Claude's GitHub connection is signed in as a different login, add that login as a collaborator, and test with a throwaway repo first.

**Target check (before any GitHub write)**

The Target section of `docs/PROJECT.md`, filled from intake question 6, is the one place the GitHub login, owner and repo are written. Every `OWNER/REPO` in the kit's commands comes from it. Before the first GitHub write of any session, check the target, and stop on a mismatch.

- **Claude Code:** `git remote get-url origin` must point at the target repo. If it does not, the session was opened on the wrong repo: Claude says so and stops.
- **Your terminal, before admin commands:**

```bash
gh auth switch --user LOGIN
gh api user --jq .login
gh repo view OWNER/REPO --json nameWithOwner,viewerPermission --jq '.nameWithOwner + " " + .viewerPermission'
```

The second line must print the target login, and the third the target repo followed by `ADMIN`. For a repo that does not exist yet the third line fails: run the `gh repo create … --template` command only once the first two lines show the right login.

- **Claude Project:** its connected repo must be the target repo.

**Build the template repo (once, in Claude Code)**

```text
Create a public GitHub repo KIT-OWNER/claude-kit-template from the kit text below.
Put the kit text in KIT.md at the root, and add every file in the kit's repo layout with its placeholders
left in, except claude.yml and claude-review.yml. Nothing personal and no secrets anywhere.
Mark it as a template (gh repo edit KIT-OWNER/claude-kit-template --template). If gh is blocked here,
give me the exact commands to run. Finish with the repo link and the file list.

<kit text>
```

| Layer | What it is | Files | Why it saves money |
| --- | --- | --- | --- |
| 1. Context | Project facts and rules | `CLAUDE.md` | Loads every session, so short means cheap |
| 2. Roles | Subagents with their own model, effort and tools | `.claude/agents/*.md` | Strong model only where judgement matters |
| 3. Permissions | What Claude may run unprompted | `.claude/settings.json` | Fewer approval stops and wasted turns |
| 4. Workflows | Saved prompts run as slash commands | `.claude/commands/*.md` | No retyping long prompts |
| 5. Automation | Plain CI, Claude on GitHub, a monthly reminder | `.github/**` | Cheap checks run before any model |
| 6. Operator protocol | When to switch model, effort, surface or session; handoffs; runbooks | Block in `CLAUDE.md`, handoff comments on issues and PRs, `docs/runbooks/` | Stops long, drifting sessions |

## Core principles

- **Same every time.** Same files, names, defaults and stage order in every project. Deviate only through the customisation table, and record each deviation in `docs/PROJECT.md`.
- **Smallest setup that works.** Four agents, four commands, four workflows. Add more only for a named, repeated need.
- **One home per fact.** Each rule lives in one file and others point to it. The one deliberate copy is the operator protocol block, which also goes in the Claude Project instructions because chat cannot read the repo.
- **Deterministic first, model second.** Tests, lint, types and dependency checks run in plain CI. Claude reviews only what already passes.
- **Right model and effort for the job.** Strong model at high effort for design and security, mid model at medium effort for building and review, small model at low effort for docs and triage.
- **Verify against current docs.** Model names, action versions and settings keys change. Recheck anything listed under "Setup checks (stage 6)".

## Project establishment (stages 0 to 5)

Establishment turns an idea into an approved brief, then a working repo with every kit file in place. No repo file is created until the brief is approved.

**Intake rules for Claude**

- Ask two rounds of five questions, then stop asking. Wait for answers between rounds. Skip any question a handoff or the request already answers, and confirm those answers in one line.
- Never guess an essential answer. If one is still missing after round 2, list it under Open questions in the brief. If the person waives the questions ("use defaults"), fill each essential from what they gave, mark it ASSUMPTION and list those first; approving the brief confirms them.
- Fill everything else from the defaults table. Mark each filled value `DEFAULT` or `ASSUMPTION` in the brief, so the person can override any of them in one reply.
- Only for a public product, regulated data or a team of three or more, also ask about scale and accessibility, environments, review rules, and release and rollback.
- List the biggest risks you see under Open questions; do not ask the person for them.

**Starting prompt (stage 0, new chat in the Starter kit Project, on sonnet at medium effort)**

```text
Start stage 0 of the Claude Agentic Project Starter Kit in this Project's knowledge for: <one-line idea>.
Follow the kit's Operator protocol block from now on. <Optional: "Use defaults" to skip the questions.>
<Optional: paste a handoff; confirm its answers instead of asking them again.>
```

**Round 1: what and why**

1. What is it, and who is it for?
2. What must the first version do (up to three behaviours), and what is out of scope?
3. What check tells us the first version is done?
4. Language and framework, or should Claude propose two options with trade-offs?
5. Where will it run, and is there a deadline?

**Round 2: repo and risk**

6. Target: which GitHub login, which owner (that account or an organisation), which repo (or none yet), public or private, and the owner's GitHub plan (Free, Pro, Team)? Confirm all of it back in one line.
7. Does it handle personal data, payments, credentials or anything regulated?
8. Who works on it: one person, a team, or several Claude sessions at once?
9. How much should GitHub automation do (review only, fix small issues, or open feature PRs), and should it bill your Claude subscription or nothing (no Claude workflows on GitHub)?
10. Is there anything Claude must never touch?

**Defaults**

| Setting | Default |
| --- | --- |
| Default branch | `main` |
| Licence | MIT if public; none if private |
| Tests | One unit test per first-version behaviour; no coverage target |
| Lint, format, types | The stack's standard tools, one command each |
| Secrets | `.env` locally (gitignored); GitHub Actions secrets in CI |
| Environments | Local only until the first deploy; `release.md` decides the rest |
| Branch rules on `main` | Require a PR and the CI check; block force push and deletion; approvals 0 for one person, 1 for a team |
| Merging | The person merges; auto-merge off until stage 10 |
| GitHub automation | Automatic PR review, plus `@claude` on request |
| Model spend | Your Claude subscription only, through a token; never paid API tokens; no fast mode (2x price), no Fable or Mythos models |
| Surfaces | Chat for planning, Claude Code for building, GitHub for small issues |
| Check-ins | Short answers; Claude speaks up only at operator-protocol moments. For a fixed format, fill the Response style line (customisation table) |
| Attribution in commits and PRs | Off: the attribution block in .claude/settings.json, the Never rule in CLAUDE.md, and the commit check in /pre-pr; delete the block and the rule to keep Claude Code's default |

**Stages**

| # | Stage | Who | Setup (surface, model, effort) | Session | Done when |
| --- | --- | --- | --- | --- | --- |
| 0 | Intake and brief | Claude; person approves | Chat, sonnet, medium (opus, high if Claude must choose the stack) | New chat in the "Starter kit" Claude Project, which has the kit in its knowledge. Stays open as your guide through stage 4, and checks session A's paste-back against the brief | The brief is approved (kept in chat or a doc until the repo exists) |
| 1 | Create the repo | *Person*, or Claude with `gh` | Claude Code, sonnet, medium | Claude Code session A: stages 1 to 3, one paste | Repo exists with a first commit on main, from the template repo or with README, `.gitignore` and LICENSE |
| 2 | Scaffold | Claude | Same as stage 1 | Session A | A minimal app, one passing test, and lint, format and type config exist; each real command runs |
| 3 | Bootstrap the kit | Claude, with the Claude Code prompt | Same as stage 1 | Session A ends with its finish list and paste-back block; no handoff needed | Every file in the repo layout exists except the two Claude workflows, filled from the brief |
| 4 | Merge the bootstrap PR, then branch rules | *Person* | No model; your terminal with gh signed in, or the GitHub website (see How to set them in Layer 5) | None; ask the stage 0 chat if stuck | The single bootstrap PR is merged once CI is green and the paste-back matches the brief, with no line-by-line review, so nothing ever goes straight to `main`; then branch rules are on |
| 5 | Project instructions and first issues | *Person* pastes; Claude creates issues | Claude Code, sonnet, medium | Claude Code session B, new: `docs/PROJECT.md` carries the context. The stage 0 chat's handoff goes on the first issue as a comment, and the chat closes; later sessions start from the comments on their issue or PR | The Claude Project is set up from docs/claude-project.md, with the repo in its knowledge; three to five issues have acceptance criteria |

After stage 5, one Claude Code session covers one task: one issue, one `/feature` run or one PR.

Branch rules need a public repo or a paid plan: rulesets work in public repos on GitHub Free, and in private repos on Pro, Team and Enterprise Cloud ([GitHub docs](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets)). For a private repo on Free, skip the rules and record that in `docs/PROJECT.md`. GitHub never lets you approve your own PR, which is why one-person projects use 0 approvals.

**docs/PROJECT.md template**

```markdown
# <Project name>

## Summary
<Two sentences: what it is, who it is for.>

## Done for the first version
- <Measurable check>

## Scope
In: <up to three behaviours>
Out: <explicit exclusions>

## Technology
<Language, framework, runtime, hosting, key dependencies. Install, test, lint, format and type-check commands.>

## Target
GitHub login: <login>. Owner: <account or organisation>. Repo: <owner/name, or none yet>. Visibility: <public or private>.

## Repo and delivery
<GitHub plan, branch rules, approvals, automation level, who merges.>

## Data and protected areas
<Sensitive data handled; paths, services or data Claude must never touch.>

## Environments and secrets
<Environment list; where each secret lives. Never the secret itself.>

## Budget and working style
<Billing route (subscription token, or None), who works on it, surfaces.>

## Decisions
- <Date>: <decision> because <reason>

## Deviations from the kit
- <What changed from the kit and why>

## Open questions
- <Question>, <who must answer>

## Defaults and assumptions
- DEFAULT: <setting taken from the kit's defaults table>
- ASSUMPTION: <what was assumed and why>
```

**Claude Code prompt (stages 1 to 3, one paste)**

Claude Code cannot open a claude.ai doc link. Give Claude Code the kit one of three ways: the template repo (preferred: KIT.md and the kit's files are already there, or are copied in from it; see Entry paths); for a local session, the kit exported as Markdown and saved next to the repo folder, not inside it, so it is never committed; or, for a cloud session, which sees only the checked-out repo, the kit's text pasted under the prompt. Before pasting, set the session to sonnet at medium effort with /model and /effort. The paste-back block at the end is how the stage 0 chat checks the work against the brief, because chat cannot reliably read the repo. If chat can read it through a GitHub connection, it may look there too, but the paste-back is the record.

```text
Run stages 1 to 3 of the Claude Agentic Project Starter Kit: <"KIT.md, already here" | path to the kit file | "the kit text pasted below">.
Follow the kit's Operator protocol block from now on.
Before any GitHub write, run the kit's target check against the Target in the brief, and stop on a mismatch.
1. Repo: <create owner/name, public or private, with gh and a README so main has a first commit | it already exists here>.
2. Scaffold: a minimal app, one passing test, lint, format and type config. Confirm each real command runs.
3. Write the approved brief below to docs/PROJECT.md, then create or fill every file in the kit's repo layout
   except claude.yml and claude-review.yml. Keep KIT.md until stage 6.
Rules: keep every file short; fill every placeholder from docs/PROJECT.md; ask me about anything you cannot fill;
add nothing beyond the kit; record any deviation in docs/PROJECT.md. Once the repo exists, work on one branch, claude/bootstrap: commit there, open one draft PR into main when
stage 3 is done, and open no other PR. I merge it at stage 4.
Finish with: files created, assumptions made, the steps I must do by hand, and one code block for me to paste
into the stage 0 chat: the PR link, `git log --oneline main..HEAD`, `git diff --stat main...HEAD`, the last
lines of each check command's output, and the full project manifest (package.json, pyproject.toml or similar).

<approved brief>

<kit text, if pasted>
```

**`docs/claude-project.md`: Claude Project instructions (stage 5, up to 25 non-blank lines, not counting the copied protocol block)**

```text
Project: <name>. <One-sentence summary.>
Repo: <owner/name>, added to this Project's knowledge; refresh it after each merge. Project facts: docs/PROJECT.md.
You cannot see unmerged branches or Claude Code sessions: ask me for a paste-back block when you need them.
Your role here: planning, scope, trade-offs and docs. Building happens in Claude Code.
Ask before changing scope. Keep answers short. Show code only when it is the deliverable.
At the end of a working session, write a handoff for me to add as a comment on its issue or PR: goal, state, decisions, next steps, open questions, dead ends, any running list, recommended setup. Its first line: "Read this handoff. Reply 'ready' and the first next step in one line, then take no action until I send a command."
Never: <protected areas>.
Response style (optional, up to 6 lines): <headings, list style, code blocks for paste content, use of bold>

<the Operator protocol block from CLAUDE.md, copied unchanged>
```

**Establishment is complete when**

- [ ] Every essential question is answered or listed under Open questions
- [ ] The brief is approved and committed as `docs/PROJECT.md`
- [ ] The bootstrap PR is merged with CI green
- [ ] Every command in CLAUDE.md runs
- [ ] `/feature`, `/issue`, `/pre-pr` and `/handoff` show in `/help` and run this project's files (a personal skill with the same name wins over a project command, so rename any clash)
- [ ] Branch rules are on, or skipped and recorded for a private repo on GitHub Free
- [ ] The Claude Project instructions are pasted in and `docs/PROJECT.md` is in its knowledge, through the repo connection
- [ ] Three to five first issues exist, each with acceptance criteria

## Repo layout

Every project ends up with exactly these files: all of them after stage 3, except the two Claude workflows, which arrive at stage 6. KIT.md is there only during setup and goes in the stage 6 PR. Anything else is added later for a named need and recorded as a deviation.

```text
<repo>/
├── README.md
├── LICENSE                        # Public repos only
├── .gitignore                     # Includes the lines below
├── CLAUDE.md                      # Layer 1 plus the Layer 6 block, under 100 lines
├── AGENTS.md                      # Layer 1: points any other agent to the same context
├── docs/
│   ├── PROJECT.md                 # Brief, decisions, deviations
│   ├── claude-project.md          # Layer 6: the Claude Project instructions
│   └── runbooks/
│       └── maintenance.md         # Layer 6; release.md and incident.md when needed
├── .claude/
│   ├── settings.json              # Layer 3
│   ├── agents/
│   │   ├── planner.md             # Layer 2: opus, high
│   │   ├── implementer.md         # Layer 2: sonnet, medium
│   │   ├── reviewer.md            # Layer 2: sonnet, medium, read-only
│   │   └── security.md            # Layer 2: opus, high, read-only
│   └── commands/
│       ├── feature.md             # Layer 4
│       ├── issue.md               # Layer 4
│       ├── pre-pr.md              # Layer 4
│       └── handoff.md             # Layer 6
└── .github/
    ├── dependabot.yml             # Layer 5: GitHub files dependency updates as PRs
    ├── workflows/
    │   ├── ci.yml                 # Layer 5: no model, the only required check
    │   ├── claude.yml             # Layer 5: stage 6
    │   ├── claude-review.yml      # Layer 5: stage 6
    │   └── maintenance.yml        # Layer 5: opens a monthly issue, no model
    ├── ISSUE_TEMPLATE/
    │   └── task.md
    └── pull_request_template.md
```

`.gitignore` always includes these lines:

```text
.env
.env.*
CLAUDE.local.md
.claude/settings.local.json
.claude/permission-log.jsonl
```

`CLAUDE.local.md` and `.claude/settings.local.json` hold personal, machine-specific overrides. Everything else an agent needs lives in the repo or on GitHub: handoffs are comments on the issue or PR they belong to, so every agent and device sees them, and lasting decisions go in `docs/PROJECT.md` through a PR. Nothing personal goes in any of them.

## Layer 1: CLAUDE.md

CLAUDE.md is read at the start of every Claude Code session, so every line costs tokens on every run. Keep it under 100 lines. Put in facts Claude cannot discover from the code and commands it would otherwise guess wrong. This template is the one home for the workflow rules, the definition of done and the operator protocol; other files point here.

```markdown
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
- Session is long: a milestone is done, corrections repeat, earlier decisions slip, Claude Code's
  /context shows over half used, or a chat passes about 20 exchanges or 3 large pastes:
  "Start a new session", after /handoff.
- Small, well-specified and low risk: suggest an issue for @claude on GitHub.
- Cannot do a step from here (blocked, or it needs the person's login or approval): give the exact
  command for the person's Mac terminal; offer the website only when no command exists or the
  person is on iOS with no terminal.
- Touches secrets, billing or production: draft it and leave the final step to the person.
- Talk only at a stop or the end. A stop is a decision or step only the person can do: send one message, items numbered, each with a recommendation. Never ask in chat and carry on as if answered.
- Person-only steps (secrets, website settings, merges, approvals): post as unticked tasks on the issue or PR, do all work that doesn't depend on them, never treat one as done until a read-only check or the person confirms. The final message lists them first under "Needs you".
```

**Rules for keeping it lean**

- Move long reference material to `docs/` and link it. Claude reads it only when needed.
- Do not repeat what an agent file or `settings.json` already says.
- Each month, delete anything Claude now does correctly without being told.
- If a rule keeps being ignored, make it a hook or a CI check instead of a longer sentence.
- The Operator protocol block is also copied into docs/claude-project.md, the Claude Project instructions. Change both together.

**AGENTS.md** (for coding agents other than Claude)

```markdown
# Agent guide

Project facts: docs/PROJECT.md. Working rules, commands and the definition of done: CLAUDE.md.
Read both before changing anything; they apply to every agent, not only Claude.
Task context and handoffs live in the comments on the issue or PR you are working on: read them first.
Never commit secrets or personal details.
```

## Layer 2: core subagents

Four agents cover most projects. Each has a distinct job, tool list, model and effort, and lives at `.claude/agents/<name>.md` as YAML frontmatter plus a system prompt. Claude reads the `description` to decide when to hand work over, so it is written as a trigger.

| Agent | Job | Model | Effort | Tools | Edits files |
| --- | --- | --- | --- | --- | --- |
| planner | Design, breakdown, risks | `opus` | high | Read, Grep, Glob | No |
| implementer | Write and fix code, run checks | `sonnet` | medium | Read, Edit, Write, Grep, Glob, Bash | Yes |
| reviewer | Review a diff for correctness and style | `sonnet` | medium | Read, Grep, Glob, Bash in plan mode | No |
| security | Review for vulnerabilities and secrets | `opus` | high | Read, Grep, Glob | No |

Use the aliases `opus`, `sonnet` and `haiku` so the files never go stale; pin a full model ID only for a reproducible workflow. `tools` takes plain tool names, so the reviewer gets Bash with `permissionMode: plan` (read-only commands only) rather than a command pattern. A subagent cannot talk to the person, so it returns questions to the main session.

**planner.md**

```markdown
---
name: planner
description: Use before any new feature, architecture decision, or change touching more than 3 files. Produces a plan; never writes code.
model: opus
effort: high
tools: Read, Grep, Glob
---
You are the planning agent. Read the relevant code and docs/PROJECT.md, then return:
1. Goal in one sentence.
2. Files to change, each with the reason.
3. Ordered steps small enough to test one at a time.
4. How each step will be verified.
5. Risks, and open questions for the person. Do not guess answers to them.
Keep the plan under 40 lines. Do not write or edit code.
```

**implementer.md**

```markdown
---
name: implementer
description: Use to write or fix code once a plan exists, or for a small, well-defined change.
model: sonnet
effort: medium
tools: Read, Edit, Write, Grep, Glob, Bash
---
You are the implementation agent. Follow the plan and CLAUDE.md.
- Make the smallest change that satisfies the step, then run the Done checks in CLAUDE.md's Workflow section.
- If a check fails twice for the same reason, stop and report what you tried.
- Change a test only if the test itself is wrong, and say so.
- Finish with: files changed, checks run and their results, anything left undone.
```

**reviewer.md**

```markdown
---
name: reviewer
description: Use after code changes and before a pull request. Reviews the diff against the plan and CLAUDE.md. Read-only.
model: sonnet
effort: medium
tools: Read, Grep, Glob, Bash
permissionMode: plan
---
You are a code reviewer with fresh eyes. Read `git status --short` and `git diff <base branch>`, which includes uncommitted changes; read new untracked files directly. Use Bash only for read-only git commands.
Check correctness, missing tests, edge cases, naming, dead code and scope creep.
Report each finding as: severity (blocker, should-fix, nit), file and line, problem, fix.
If there are no blockers, say "No blockers". Do not edit files.
```

**security.md**

```markdown
---
name: security
description: Use for any change touching authentication, authorisation, user input, file or network access, or secrets. Read-only.
model: opus
effort: high
tools: Read, Grep, Glob
---
You are a security reviewer. Assume the author made a mistake and look for it.
Check injection, broken access control, unsafe deserialisation, path traversal, SSRF, hard-coded secrets,
weak crypto, logging of sensitive data and missing input validation. Dependency vulnerabilities are CI's job.
Report each finding with severity, file and line, how it could be exploited, and the fix.
Say which areas you did not check. Do not edit files.
```

**When to add a fifth agent:** only when a task recurs and needs different instructions, tools or a separate review from all four. Typical additions are in the customisation table. Name the need in `docs/PROJECT.md` before creating the file.

## Layer 3: permissions

`.claude/settings.json` is committed and shared. It lets Claude run safe commands without asking, asks before risky ones and blocks destructive ones. Replace each `<… cmd>` with the exact command from CLAUDE.md, for example `Bash(npm run test *)`.

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "attribution": { "commit": "", "pr": "", "sessionUrl": false },
  "permissions": {
    "allow": [
      "Edit(/<source dir>/**)",
      "Edit(/<test dir>/**)",
      "Edit(/docs/**)",
      "Bash(<test cmd> *)",
      "Bash(<lint cmd> *)",
      "Bash(<format cmd> *)",
      "Bash(<format check cmd> *)",
      "Bash(<type check cmd> *)",
      "Bash(git status *)",
      "Bash(git diff *)",
      "Bash(git log *)",
      "Bash(git add *)",
      "Bash(git commit *)",
      "Bash(git switch *)",
      "Bash(git checkout -b *)",
      "Bash(gh issue view *)",
      "Bash(gh issue list *)",
      "Bash(gh issue create *)",
      "Bash(gh pr view *)",
      "Bash(gh pr diff *)"
    ],
    "ask": [
      "Bash(git push *)",
      "Bash(gh pr create *)",
      "Bash(<install cmd> *)"
    ],
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Read(./secrets/**)",
      "Bash(rm -rf *)",
      "Bash(git push --force *)",
      "Bash(git push -f *)",
      "Bash(git reset --hard *)",
      "Bash(sudo *)",
      "Bash(curl *)",
      "Bash(wget *)"
    ]
  },
  "hooks": {
    "PermissionRequest": [
      { "matcher": "*", "hooks": [ { "type": "command",
        "command": "{ cat; echo; } >> \"$CLAUDE_PROJECT_DIR/.claude/permission-log.jsonl\"" } ] }
    ]
  }
}
```

**Rules**

- Allow narrow command patterns, never a bare `Bash`. Put the `*` after the subcommand. A trailing space and star also matches the bare command, so Bash(npm test \*) covers npm test. The space is literal, though, so Bash(npm run format \*) does not cover npm run format:check: give each script its own rule.
- Rules are checked deny first, then ask, then allow, so deny always wins. Keep secrets and destructive commands in `deny`.
- Rules match the command text Claude writes; they are not a sandbox. `/usr/bin/curl`, or a script that opens `.env` itself, gets past them. Turn on Claude Code's sandbox when you need a hard boundary.
- These allow rules do not apply on GitHub. A run with no trust prompt ignores the repo's allow rules (deny and ask still apply), so each Claude workflow lists its tools in `--allowedTools` (Layer 5). See the [permissions docs](https://code.claude.com/docs/en/permissions).
- Add a pattern only after Claude has been blocked on it twice. "Yes, and don't ask again" saves to `settings.local.json`, so move rules the team needs into this file by hand. In cloud sessions gh can be blocked (its GraphQL calls fail), so a command that says gh falls back to the session's GitHub tool.

**File edits.** The three `Edit` rules let Claude change source, tests and docs without asking, while edits to CLAUDE.md, `.claude/` and `.github/` still prompt. Replace `<source dir>` and `<test dir>` with the real folders; a leading `/` anchors a rule to the repo root. One `Edit` rule also covers the Write tool, because Claude Code checks file changes against `Edit` and `Read` rules only, and `Write(path)` rules never match. Do not set `defaultMode` to `acceptEdits` here: it accepts edits across the whole working directory, config included ([permissions docs](https://code.claude.com/docs/en/permissions)).

**Attribution.** The `attribution` block removes Claude Code's commit trailer and PR footer (an empty string hides each) and the session link (`sessionUrl: false`). Delete the block to keep Claude Code's defaults. The block only covers Claude Code's own defaults: cloud sessions, and some versions for the `Claude-Session:` trailer, add the lines anyway. The control is the Never rule in CLAUDE.md, which overrides a session's instruction to add them. The backstop is the commit check in `/pre-pr`, run after committing and before the first push. Cloud sessions always add a footer to the PR body, because the session's GitHub proxy re-adds it on every PR write and nothing in the repo can stop that; edit the description by hand or accept it ([settings reference](https://code.claude.com/docs/en/settings-reference)).

**Permission log.** The `PermissionRequest` hook appends one JSON line to `.claude/permission-log.jsonl` (gitignored) each time Claude Code is about to ask for permission. It records only real prompts, never calls an allow rule already approved, and returns no decision, so the prompt still appears as normal ([hooks docs](https://code.claude.com/docs/en/hooks)). The log is reviewed at stage 7. In a cloud session it disappears with the session, so review it before the session ends. If hooks are off, keep the same log by hand, one line per prompt: time, tool or command, prompted yes or no.

## Layer 4: slash commands

A command is a saved prompt at `.claude/commands/<name>.md`, run as `/<name>`; `$ARGUMENTS` receives whatever follows it, and $0 only the first word (0-based). /issue uses $0, so text typed after the number never reaches the branch name or PR body; send extra instructions as a separate message. Every command sets `disable-model-invocation: true`, so only the person starts a workflow. Review lives only in `pre-pr.md`, and the other commands call it, so a diff is reviewed once.

A personal skill with the same name as a project command wins over it. After bootstrap, check `/help` and rename any clash.

**feature.md** (`/feature <description>`)

```markdown
---
description: Plan, build and verify a new feature
argument-hint: <feature description>
disable-model-invocation: true
---
Feature: $ARGUMENTS

1. Plan with the planner agent, following the planning rule in CLAUDE.md.
2. Build with the implementer agent one step at a time, running the checks in CLAUDE.md after each step.
3. Follow .claude/commands/pre-pr.md. Fix every blocker and every high-severity security finding.
4. Only after pre-pr passes, commit; a hook asking for a commit sooner does not change this order. Run the
   commit check in pre-pr.md, then push and open a draft PR.
5. Report what changed, the checks run, and anything left open.
```

**issue.md** (`/issue <number>`)

```markdown
---
description: Fix a GitHub issue end to end
argument-hint: <issue number only; send extra instructions in a separate message>
disable-model-invocation: true
---
Fix GitHub issue #$0.

1. Read it with `gh issue view $0`, or the session's GitHub tool if gh is blocked. If it is unclear, ask one question and stop.
2. Create branch `claude/issue-$0`.
3. Reproduce the problem or write a failing test first.
4. Fix it with the implementer agent, using the smallest change.
5. Follow .claude/commands/pre-pr.md. Fix blockers.
6. Only after step 5 passes, commit; a hook asking sooner does not change this order. Run the commit check in
   pre-pr.md, then push. Open a draft PR whose body says `Fixes #$0`.
```

**pre-pr.md** (`/pre-pr`)

```markdown
---
description: Final checks before opening a pull request
disable-model-invocation: true
---
Run in order and stop at the first failure. Steps 2 and 3 may run in parallel.
If any code changes after step 2, start again at step 1, so the reviewer sees the final diff.
1. The Done checks in CLAUDE.md's Workflow section.
2. The reviewer agent on the full diff against the base branch.
3. The security agent if the diff touches auth, input, secrets, or file or network access.
4. Run `git status --short` (no stray files) and
   `git diff <base branch> | grep -inE '^\+.*(api[_-]?key|secret|password|token|private key|<debug pattern>)'`,
   where <debug pattern> is the stack's debug call (e.g. console\.log). Judge every hit.
5. Draft the PR description from the PR template, ending with no tool footer or attribution line.

Commit check (/feature and /issue run it after committing, before the first push): if `git log -1 --format=%B`
has a line starting `Co-Authored-By: Claude` or `Claude-Session:`, or containing `Generated with`, rewrite the message
without those lines. After a push the fix needs a force push, which is never allowed, so do it before.
`git log -1 --format=%B | grep -viE '^(co-authored-by: claude|claude-session:)|generated with' | git commit --amend -F -`
```

Add a command only after you have typed the same long prompt three times. `/handoff` is part of Layer 6.

## Layer 5: GitHub automation

Four workflows, two templates and branch rules. Plain CI is the only required check. The Claude workflows are never required, so a model outage never blocks a merge. `ci.yml`, `maintenance.yml` and the templates come at stage 3; the two Claude workflows at stage 6.

**Setup for the Claude workflows (stage 6)**

Use the billing route recorded in `docs/PROJECT.md`. Both Claude workflows use the subscription token; the kit never uses paid API tokens. See the [setup docs](https://code.claude.com/docs/en/github-actions).

| Route | Get the credential | Secret name | Auth line in both Claude workflows | Cost |
| --- | --- | --- | --- | --- |
| Subscription (default) | `claude setup-token`, run once on a Mac with Claude Code installed (for example with Homebrew); after that, app-only use needs nothing else. Without it, use the None route | `CLAUDE_CODE_OAUTH_TOKEN` | `claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}` | Uses your plan's usage limits; no extra bill. Tied to the person who made it, so not for shared team repos |
| None (no spend, or no local CLI) | Nothing | None | No Claude workflows: skip claude.yml and claude-review.yml | No model cost on GitHub. Stage 6 skips the two Claude workflow checks; record the route as a deviation |

1. Get the credential for your route, from the table.
2. Install the [Claude GitHub app](https://github.com/apps/claude) on the repo. This is website only.
3. From your terminal (`gh` signed in as a repo admin), store it under the route's secret name, for example `gh secret set CLAUDE_CODE_OAUTH_TOKEN --repo OWNER/REPO`. `gh` asks for the value, so it never appears in your shell history or passes through Claude. No terminal: the repo's Settings, then Secrets and variables, then Actions.
4. Commit `claude.yml` and `claude-review.yml` through a PR.

`/install-github-app` in Claude Code can do steps 2 and 3 for you. If it proposes workflow files, use the kit's instead so every project matches.

**ci.yml: no model, runs on every PR and on main**

```yaml
name: CI
on:
  pull_request:
  push:
    branches: [main]
concurrency:
  group: ci-${{ github.ref }}
  cancel-in-progress: true
permissions:
  contents: read
jobs:
  check:
    if: ${{ !github.event.repository.is_template }}
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - uses: actions/checkout@v6
      # <runtime setup with dependency cache, e.g. actions/setup-node>
      # Use each action's current major version from its releases page; if you cannot check, say so.
      - run: <install command>
      - run: <lint command>
      - run: <format check command>
      - run: <type check command>
      - run: <test command>
      - run: <dependency audit command, e.g. npm audit --audit-level=high>
```

The `if` line skips the job in the template repo itself, where the placeholders cannot run; repos generated from the template run it. `maintenance.yml` has the same line.

Also turn on Dependabot alerts at stage 4 (see How to set them, under Branch rules).

**dependabot.yml: GitHub files dependency updates, no model**

```yaml
version: 2
updates:
  - package-ecosystem: "<ecosystem, e.g. npm>"
    directory: "/"
    schedule:
      interval: "weekly"
    groups:
      minor-and-patch:
        patterns: ["*"]
        update-types: ["minor", "patch"]
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

Dependabot files each update as a PR, which is GitHub's own way of filing them: minor and patch updates grouped into one, each major on its own. It also keeps the workflows' action versions current. CI runs on these PRs, and the Claude review skips them, because Dependabot PRs get no Actions secrets. Claude picks them up by the priority rule in CLAUDE.md, keeping companion packages such as type definitions on the same major as what they describe.

**claude.yml: Claude answers @claude in issues and PRs**

```yaml
name: Claude
on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened]
concurrency:
  group: claude-${{ github.event.issue.number || github.event.pull_request.number }}
  cancel-in-progress: false
jobs:
  claude:
    if: |
      (github.event_name == 'issues' && contains(github.event.issue.body, '@claude')) ||
      (github.event_name != 'issues' && contains(github.event.comment.body, '@claude'))
    runs-on: ubuntu-latest
    timeout-minutes: 30
    permissions:
      contents: write
      pull-requests: write
      issues: write
      id-token: write
      actions: read
    steps:
      - uses: actions/checkout@v6
        with:
          fetch-depth: 1
      # <runtime setup, same as ci.yml>
      - run: <install command>
      - uses: anthropics/claude-code-action@v1
        with:
          claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
          claude_args: |
            --model sonnet
            --max-turns 20
            --allowedTools "Bash(<test cmd> *),Bash(<lint cmd> *),Bash(<format check cmd> *),Bash(<type check cmd> *)"
```

**claude-review.yml: automatic review that comments on the PR**

```yaml
name: Claude Review
on:
  pull_request:
    types: [opened, synchronize, ready_for_review, reopened]
concurrency:
  group: review-${{ github.event.pull_request.number }}
  cancel-in-progress: true
jobs:
  review:
    if: github.event.pull_request.draft == false && github.actor != 'dependabot[bot]'
    runs-on: ubuntu-latest
    timeout-minutes: 15
    permissions:
      contents: read
      pull-requests: read
      issues: read
      id-token: write
    steps:
      - uses: actions/checkout@v6
        with:
          fetch-depth: 1
      - uses: anthropics/claude-code-action@v1
        with:
          claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
          plugin_marketplaces: "https://github.com/anthropics/claude-code.git"
          plugins: "code-review@claude-code-plugins"
          prompt: "/code-review:code-review --comment ${{ github.repository }}/pull/${{ github.event.pull_request.number }}"
          claude_args: |
            --model sonnet
            --max-turns 10
            --allowedTools "mcp__github_inline_comment__create_inline_comment"
```

This follows the official review example ([docs](https://code.claude.com/docs/en/github-actions)). The `--comment` flag and the inline-comment tool are what make the review appear on the PR; without them it goes only to the run log. `id-token: write` is required for the app's default sign-in. The local `reviewer` agent is for Claude Code; GitHub uses the code-review plugin. PRs from forks of a public repo get no secrets, so they are not reviewed. Use `--model opus` only on security-sensitive repos. Anthropic's managed Code Review is an alternative that needs no workflow file.

**maintenance.yml: monthly reminder, no model**

```yaml
name: Monthly maintenance
on:
  schedule:
    - cron: "0 22 1 * *"
  workflow_dispatch:
permissions:
  issues: write
jobs:
  open-issue:
    if: ${{ !github.event.repository.is_template }}
    runs-on: ubuntu-latest
    timeout-minutes: 5
    steps:
      - run: gh issue create --repo "$GITHUB_REPOSITORY" --title "Monthly maintenance $(date +%Y-%m)" --body "Follow docs/runbooks/maintenance.md."
        env:
          GH_TOKEN: ${{ github.token }}
```

**Issue template** `.github/ISSUE_TEMPLATE/task.md`

```markdown
---
name: Task
about: A change for Claude or a person to pick up
---
## Goal
<One sentence: what should be true when this is done.>

## Context
<Links, files, screenshots, error messages.>

## Acceptance criteria
- [ ] <Observable check>
- [ ] <Observable check>

## Out of scope
<What not to touch.>
```

**Pull request template** `.github/pull_request_template.md`

```markdown
## What and why
<Two or three sentences.>

## How it was tested
<Commands run, results.>

## Risk
<Areas affected, rollback plan.>

## Decisions
<Anything added to docs/PROJECT.md, or "None".>

Fixes #
```

**Branch rules on `main` (stage 4)**

- Require a pull request. Approvals: 0 for one person, 1 for a team.
- Require the `check` job from CI to pass. Never require the Claude workflows.
- Block force pushes and deletion of `main`.
- Auto-merge stays off until stage 10.

**How to set them (person, stage 4)**

Run these from your own terminal, signed in with `gh auth login` as a repo admin. Claude Code tries them first, but a cloud session usually cannot: its GitHub proxy blocks admin API writes with a 403. On iOS, with no terminal, use the website steps below. Replace `OWNER/REPO`, and set `required_approving_review_count` to 1 for a team ([ruleset schema](https://docs.github.com/en/rest/repos/rules)).

```bash
gh api -X POST repos/OWNER/REPO/rulesets --input - <<'EOF'
{
  "name": "main",
  "target": "branch",
  "enforcement": "active",
  "conditions": { "ref_name": { "include": ["~DEFAULT_BRANCH"], "exclude": [] } },
  "rules": [
    { "type": "deletion" },
    { "type": "non_fast_forward" },
    { "type": "pull_request", "parameters": {
        "required_approving_review_count": 0,
        "dismiss_stale_reviews_on_push": false,
        "require_code_owner_review": false,
        "require_last_push_approval": false,
        "required_review_thread_resolution": false } },
    { "type": "required_status_checks", "parameters": {
        "strict_required_status_checks_policy": false,
        "required_status_checks": [ { "context": "check" } ] } }
  ]
}
EOF
gh api -X PUT repos/OWNER/REPO/vulnerability-alerts
```

No signed-in terminal? Use the website instead: in the repo's Settings, open Rules, then Rulesets, and add a branch ruleset for the default branch with the rules above. Then turn on Dependabot alerts in Settings' security section.

## Layer 6: operator protocol

Layer 6 makes Claude coach the person on model, effort, surface and sessions, and keeps work moving between sessions. Its rules are the Operator protocol block in the CLAUDE.md template (Layer 1), copied unchanged into the Claude Project instructions; this section adds only what the block cannot hold.

Claude cannot see its own token use, so the session-length trigger uses checks anyone can count: over half used on /context in Claude Code, and about 20 exchanges or 3 large pastes in a chat. Planned boundaries are in the stage table's Session column. After stage 5, one Claude Code session covers one task, and every new session starts from the handoff comments on its issue or PR. In Claude Code the person can check with `/context`; `/compact` shrinks a session worth keeping and `/clear` starts clean. Set the main session's model and effort with `/model`; use /effort to change effort alone; subagents set their own in frontmatter.

**Which surface for which job**

| Job | Surface | Why |
| --- | --- | --- |
| Intake, scope, trade-offs, planning, docs | Chat | No file side effects; cheap to explore |
| Writing and running code, tests, refactors | Claude Code | Direct file and command access; permissions apply |
| Small, well-specified issues and PR reviews | GitHub (`@claude`) | Runs unattended; the PR is the review step |
| Anything touching secrets, billing or production | A person, with Claude drafting | A person does the irreversible step |
| Repo admin steps and secrets: branch rulesets, Dependabot alerts, Actions secrets, labels | Claude Code first; then the person's Mac terminal, with gh signed in as a repo admin; the GitHub website on iOS | Cloud Claude Code's GitHub proxy blocks admin API writes, and secrets should never pass through a model |

**App-first.** Chat and Claude Code both run in the Claude app on iOS and in Claude on the web, with Claude Code as cloud sessions, so every Claude step works from either. Use the template repo, because pasting the kit on a phone is impractical. Only two things need more: admin steps run from a Mac terminal (or the website on iOS), and the subscription token needs Claude Code installed on a Mac once.

**Where each surface reads from**

No surface sees everything, so every hand-over between surfaces is a file, a paste or a refresh.

| Surface | Reads | Does not see | So |
| --- | --- | --- | --- |
| Chat in the Starter kit Project (stage 0) | Attached files, pasted text, connected apps | The repo, Claude Code sessions | The kit is in its knowledge; paste paste-back blocks |
| Chat in the Project (stage 5 on) | Project instructions and knowledge; the repo only once it is added to knowledge | Unmerged branches; anything since the last refresh | Add the repo to the Project's knowledge after the bootstrap merge and refresh it after each merge; use paste-backs for unmerged work |
| Claude Code | The repo folder with its CLAUDE.md and `.claude/`, the prompt, files you give it a path to | claude.ai docs, chats, Project knowledge | Paste prompts; it reads handoffs from issue and PR comments; give it the kit through the template repo, a file next to the repo (local) or pasted text (cloud) |
| GitHub (`@claude`, review) | The checked-out repo, CLAUDE.md, the issue or PR text | Chats and local files | Put everything Claude needs in the issue, using the template |

Recommended: connect the repo to the Claude Project as knowledge at stage 5, so chat can check what Claude Code built. Project knowledge is a copy, not a live view: refresh it after merges.

**/handoff command** `.claude/commands/handoff.md`

```markdown
---
description: Post a handoff on the current issue or PR so any session can continue
disable-model-invocation: true
---
Write a handoff for this session and post it as a comment on the issue or PR being worked on
(gh issue comment or gh pr comment, or the GitHub tool if gh is blocked). If there is neither, say so
and print it instead. Start it with this line, then use exactly these headings:
"Read this handoff. Reply 'ready' and the first next step in one line, then take no action until I send a command."

# Handoff
Updated: <date and time> by <chat, Claude Code or GitHub>
## Target: <GitHub login>, <owner/repo>
## Goal: <current objective in one sentence>
## State: done (with PR or commit links), in progress (where), branch, tests passing or failing
## Decisions made: <decision> because <reason>
## Next steps: numbered, the first one exact
## Open questions and blockers: <question>, <who must answer>
## Do not: dead ends already tried
## Running list (optional): any list the session keeps, such as kit issues in a dry run
## Recommended setup: surface, model, effort, first prompt

Be specific: branch names, file paths, commands run, failing tests. Move any lasting decision into
docs/PROJECT.md on the current branch. Never include personal details, secrets or tokens.
Finish by printing the comment's link.
```

In chat, Claude writes the same handoff as a reply, with the same first line, and you add it as a comment on the issue it belongs to. If the Project's GitHub connection can post comments, chat posts it itself. Every new session starts by reading the comments on its issue or PR, so a handoff reaches every surface and device without pasting.

**Runbooks** live in `docs/runbooks/`, one file each, under 40 lines, as numbered steps Claude can follow. Each starts with "Stop and ask a person if:" followed by the conditions where Claude must not go on alone.

| Runbook | Create when | Core steps |
| --- | --- | --- |
| `maintenance.md` | Stage 3 | Opened by the monthly issue: trim CLAUDE.md and AGENTS.md, remove unused agents and commands, check model choices and plan usage (skipped on None). If the project came from a template repo, compare with it, or with the kit if the session is given it; skip this when neither is available. Dependencies are not part of it: Dependabot files those as PRs. Stop and ask a person if a step would change billing, secrets or branch rules. Finish by closing the monthly issue, with the PR link or, if nothing needed changing, a "No changes" comment |
| `release.md` | Before the first release | Confirm CI green, review changes, tag, deploy, smoke test, note the release in its PR |
| `incident.md` | When there are real users | Stabilise or roll back first, capture evidence, fix on a branch, write a short postmortem in `docs/` |

Add another runbook only after the task has happened twice.

## After establishment (stages 6 to 11)

Run these in order. Each stage makes the next one safer and cheaper. Unless a stage says otherwise, use Claude Code on sonnet at medium effort; the agents pick their own models inside it.

6. **Prove the setup on a trivial change.** Follow the Layer 5 setup steps; on the None billing route, skip the Claude workflows and their checks. The stage 6 PR adds the two Claude workflows from KIT.md, then deletes KIT.md (on None, it only deletes KIT.md). Then run `/feature` on a throwaway change, such as a version flag, and open its PR. Also test with a trivial `@claude` issue and a deliberately flawed PR, and work through every item under Setup checks below. The kit's PermissionRequest hook logs every approval prompt to .claude/permission-log.jsonl. Do not switch to auto-accept during stages 6 and 7: auto-accepted calls are never asked about, so they never reach the log. No real project work starts until they all pass, so a failure here is the setup's, not the project's.
7. **First feature, hand-guided.** In a sonnet session, run `/feature` on the smallest first issue. Then review the permission log: each repeated prompt becomes an allow rule or stays a deliberate ask. Record every ignored instruction in the handoff's running list, and fix `settings.json` and CLAUDE.md from them.
8. **Tighten over two or three features.** Repeated corrections go into CLAUDE.md, repeated prompts into commands, repeated mistakes into CI checks.
9. **Hand over small issues.** Mention `@claude` on small, well-specified issues. Review every PR. Track cost per merged PR in your plan's usage.
10. **Automate the merge path.** Once reviews are trusted, turn on auto-merge for PRs with green CI (plus an approval on team repos). Keep a person on anything touching security, data or money. A PR whose CI is already green merges the moment auto-merge is on, so test it with the recipe under Setup checks.
11. **Maintain monthly.** The maintenance workflow opens an issue on the 1st; follow `maintenance.md`. Run it on haiku at low effort, and keep haiku sessions under 100k tokens (higher price above).

**Setup checks (stage 6).** These are documented but not yet tested together. When one fails, fix the kit, not just the project:

- The reviewer agent with `permissionMode: plan` can read diffs but cannot edit files. Test it: ask the reviewer to change one line in a file, then confirm with git status that nothing changed.
- `claude.yml` can run the test command through its `--allowedTools` list.
- `claude-review.yml` posts inline comments on the flawed PR and skips drafts.
- `/feature`, `/issue`, `/pre-pr` and `/handoff` run this project's files, with no clash in `/help`.
- The ruleset blocks a direct push to `main` and requires the `check` job.
- Each check command runs without a prompt both bare and with arguments, for example the test command alone and on one file. A prompt means a missing allow rule; it shows in the permission log, so add the rule from there.
- Running /issue with a number followed by extra words creates the branch claude/issue-\<number> and nothing longer, which proves $0 works in command files.
- /handoff posts its comment on the current issue or PR, and a new session opened in the Claude app on iOS picks the work up from that comment alone.
- A Claude Code session opened on a different repo stops at the target check before writing anything.

**Auto-merge test (stage 10).** Run it from your terminal with `gh` signed in as a repo admin; a cloud session's proxy may block these calls. Pick an open PR with green CI, then re-run its CI and turn on auto-merge on the same line, so the PR waits for the new run before merging:

```bash
gh repo edit OWNER/REPO --enable-auto-merge
RUN=$(gh run list --repo OWNER/REPO --branch <pr branch> --workflow CI --limit 1 --json databaseId --jq '.[0].databaseId')
gh run rerun "$RUN" --repo OWNER/REPO && gh pr merge <pr number> --repo OWNER/REPO --auto --squash
```

The test passes when the PR shows auto-merge as enabled while CI runs, then merges by itself once CI is green.

## Cost control playbook

The biggest savings come from sending less to the model, and from not using it where a script will do.

| Lever | Do this | Typical effect |
| --- | --- | --- |
| Lean context | CLAUDE.md under 100 lines; long material in `docs/`, linked | Cheaper every session |
| Stable prefix | Cache hits cost 0.05x base on Opus and Sonnet 5.5 (0.1x others); 5-minute writes cost 1.25x. Do not switch model or toggle tools or MCP mid-session. To resume a large session after a long break, /handoff and start new | Cached input is cheap; changes break the cache |
| Lean tools | Turn off unused connectors, MCP servers, browser and computer-use tools before a session; their definitions are re-sent on every request | Smaller input on every request |
| Model and effort tiers | opus/high for design and security, sonnet/medium for building and review, haiku/low for docs and triage. As of Nov 2026: Opus 5.5 costs 2x Sonnet 5.5, Haiku 5.5 costs 1/20 of Sonnet, and output costs 5x input, so effort and short reports matter most | Largest per-token saving |
| Sonnet main session | Run commands from a sonnet session; agents pick their own models | No Opus paying to coordinate |
| Deterministic gates | Lint, types, tests and dependency audit in `ci.yml` before any review | No paying to find what a linter finds |
| One review pass | Review lives in `pre-pr.md` only | No double reviews of one diff |
| Human-started commands | `disable-model-invocation: true` on every command | No workflow started by accident |
| Scoped tools | Read-only planner, reviewer and security | Fewer runaway edits and loops |
| Turn and time caps | `--max-turns` and `timeout-minutes` on every Claude workflow | Hard ceiling per run |
| Skip and cancel | Skip drafts; cancel superseded review runs | No duplicate reviews per push |
| Spend limit | Subscription token only, capped by your plan's limits | Hard monthly ceiling |
| Fresh sessions | One task (issue, feature or PR) per Claude Code session; a new chat at the protocol's checks; `/handoff` instead of long threads | Smaller context per turn |
| Review the spend | Cost per merged PR, monthly | Finds the expensive habit |

**Signals to add an agent or command**

- You paste the same instructions at the start of three or more sessions.
- One role keeps needing a different tool set from the rest.
- You want a review done without the author's context.

**Signals to remove one**

- It has not been used in a month.
- Two agents give overlapping answers.
- Its instructions are now in CLAUDE.md or enforced by CI.

## Customising per project

Keep the core as it is and change only the rows that apply. Record each change in `docs/PROJECT.md` under Deviations from the kit.

| Project type | Change |
| --- | --- |
| Existing repo | Read the existing CLAUDE.md, CI and `.claude/` first. Merge rather than overwrite, keep the existing CI as the gate and add only missing steps |
| Web app | Add a `ui` agent (sonnet, medium) for components and accessibility; allow the dev-server and end-to-end test commands |
| API or backend | Run the security agent on every PR; add a migrations rule under Never in CLAUDE.md |
| Library | Add a `docs` agent (haiku, low); require changelog entries in the PR template |
| Data or ML | Add a `tester` agent; deny reads of raw data paths; lower `--max-turns` on review |
| Infrastructure | Deny apply and deploy commands in `settings.json`; a person runs them. Team repos require an approval on every PR |
| Docs or content | Drop `security`; use haiku for implementer and reviewer |
| Tight budget | Skip `claude-review.yml`; keep CI, the turn caps and the deny list |
| Fixed response style | Fill the optional Response style line in the Project instructions, up to 6 lines, for example fixed headings, dot points, code blocks for anything to paste, bold only for warnings. It counts toward the 25-line cap, which counts non-blank lines only; the copied protocol block does not count. Copy it into CLAUDE.md only if Claude Code should follow it too, since it then loads every session. |
| Claude Code only (no chat, Project or website) | Run stage 0 in Claude Code, in the empty project folder, with the starting prompt; the approved brief becomes docs/PROJECT.md. Skip the paste-back blocks and the Project parts of stage 5, and check work with git and gh in the terminal instead. Every person step uses its gh command. The only browser steps left are installing the Claude GitHub app and any sign-in that claude setup-token opens. |

## Final checklist (after stage 7)

- [ ] The establishment checklist is complete
- [ ] `/feature` has shipped one feature end to end through a PR
- [ ] The Claude GitHub app is installed, the secret is set, and the billing route from docs/PROJECT.md is in place
- [ ] `claude.yml` answers an `@claude` mention on a test issue
- [ ] `claude-review.yml` comments on a deliberately flawed PR and skips drafts
- [ ] Every Claude workflow has a timeout, a turn cap and `concurrency`
- [ ] `maintenance.yml` has opened its first issue (start it once with Run workflow)
- [ ] Every item under "Setup checks (stage 6)" has passed

## Sources

Checked on 2026-11-28.

- [Claude API pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- [Claude Code: configure permissions](https://code.claude.com/docs/en/permissions)
- [Claude Code: subagents](https://code.claude.com/docs/en/sub-agents)
- [Claude Code: skills and commands](https://code.claude.com/docs/en/skills)
- [Claude Code: GitHub Actions](https://code.claude.com/docs/en/github-actions)
- [GitHub: available rules for rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets)
