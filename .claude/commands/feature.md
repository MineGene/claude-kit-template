---
description: Plan, build and verify a new feature
argument-hint: <feature description>
disable-model-invocation: true
---
Feature: $ARGUMENTS

1. Plan with the planner agent, following the planning rule in CLAUDE.md.
2. Build with the implementer agent one step at a time, running the checks in CLAUDE.md after each step.
3. Follow .claude/commands/pre-pr.md. Fix every blocker and every high-severity security finding.
4. Only after pre-pr passes, commit, push and open a draft PR, then do pre-pr step 6. A hook asking for a
   commit sooner does not change this order.
5. Report what changed, the checks run, and anything left open.
