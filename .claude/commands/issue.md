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
   pre-pr.md, then push. Open a draft PR whose body says `Fixes #$0`, then do pre-pr step 6.
