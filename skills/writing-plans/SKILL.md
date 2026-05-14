---
name: writing-plans
description: Turn approved specs or requirements into step-by-step implementation plans with files, tests, and verification
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [planning, specification, task-breakdown, implementation-plan]
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context and questionable taste. Document everything: which files to touch, code, testing, verification. Bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`
(User preferences for plan location override this default)

## Scope Check

If the spec covers multiple independent subsystems, suggest breaking into separate plans — one per subsystem. Each plan should produce working, testable software on its own.

## File Structure

Before defining tasks, map out which files will be created or modified. Design units with clear boundaries. Prefer smaller, focused files. Files that change together should live together.

## Task Granularity

Each step is one action (2-5 minutes):
- Write the failing test → Run to verify failure → Implement minimal code → Run to verify pass → Commit

## Task Structure

Each task needs:
- **Files:** exact paths (create/modify/test)
- **Steps:** checkbox syntax (`- [ ]`) with complete code in every step
- **Verification:** exact commands with expected output
- **Commit:** message and files to include

## No Placeholders

Every step must contain actual content. Never write "TBD", "TODO", "add appropriate error handling", "similar to Task N", or steps that describe without showing code.

## Self-Review

After writing the plan:
1. **Spec coverage** — can you point to a task for each requirement?
2. **Placeholder scan** — search for red flags from the No Placeholders rule
3. **Type consistency** — do names/signatures match across tasks?

Fix issues inline. If a spec requirement has no task, add one.

## Execution Handoff

After saving, offer execution choice:
1. **Subagent-Driven** — use `subagent-driven-development` skill (fresh worker per task + review)
2. **Inline Execution** — use `executing-plans` skill (batch execution with checkpoints)

## References

See `references/plan-document-reviewer-prompt.md` for the plan review template.
