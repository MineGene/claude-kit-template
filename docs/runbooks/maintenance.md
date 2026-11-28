# Maintenance runbook

Stop and ask a person if: a step would change billing, secrets or branch rules.

Opened by the monthly maintenance issue. Run on haiku at low effort; keep haiku sessions under 100k tokens (higher price above).

1. Trim CLAUDE.md and AGENTS.md: delete anything Claude now does correctly without being told.
2. Remove agents and commands that have not been used in a month, or whose instructions are now in CLAUDE.md or enforced by CI.
3. Check the model choices in `.claude/agents/` and your plan usage (skip the usage check on the None billing route).
4. If the project came from a template repo, compare with it, or with the kit if the session is given it. Skip this when neither is available.
5. Do not touch dependencies: Dependabot files those as PRs.
6. Open a PR with any changes, then close the monthly issue with the PR link. If nothing needed changing, close it with a "No changes" comment.
