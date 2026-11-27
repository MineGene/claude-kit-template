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
6. After the PR is opened, read its body back (`gh pr view`, or the GitHub tool if gh is blocked) and
   delete any footer or attribution line a tool added, even with the attribution setting on.
