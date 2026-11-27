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
