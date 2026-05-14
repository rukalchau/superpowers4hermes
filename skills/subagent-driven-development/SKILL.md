---
name: subagent-driven-development
description: Execute implementation plans by delegating per-task to workers with staged spec-compliance and code-quality review
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [execution, delegation, review, orchestration, subagent]
    requires_toolsets: [terminal, git]
---

# Subagent-Driven Development

Execute plan by dispatching fresh worker per task, with two-stage review after each: spec compliance first, then code quality.

**Why:** Workers get isolated context with precisely crafted instructions. They never inherit your session history. This keeps them focused and preserves your context for coordination.

**Core principle:** Fresh worker per task + two-stage review = high quality, fast iteration.

**Continuous execution:** Don't pause between tasks. Only stop for: unresolvable blockers, genuine ambiguity, or all tasks complete.

## When to Use

- Have an implementation plan with mostly independent tasks
- Want to stay in the current session (vs. `executing-plans` for parallel sessions)
- Tasks are well-specified enough for isolated workers

## The Process

1. **Read plan** — extract all tasks with full text, note context
2. **Per task:**
   - Dispatch implementer worker (see `references/implementer-prompt.md`)
   - Answer any questions the worker has
   - Worker implements, tests, commits, self-reviews
   - Dispatch spec reviewer (see `references/spec-reviewer-prompt.md`)
   - If issues found → worker fixes → re-review until passing
   - Dispatch code quality reviewer (see `references/code-quality-reviewer-prompt.md`)
   - If issues found → worker fixes → re-review until passing
   - Mark task complete
3. **After all tasks** — dispatch final reviewer for entire implementation
4. **Complete** — use `finishing-a-development-branch` skill

## Model Selection

- **Mechanical tasks** (1-2 files, clear spec): use fast/cheap model
- **Integration tasks** (multi-file, pattern matching): use standard model
- **Architecture/review tasks**: use most capable model

## Handling Worker Status

- **DONE:** Proceed to spec review
- **DONE_WITH_CONCERNS:** Read concerns, address if correctness-related, then review
- **NEEDS_CONTEXT:** Provide missing info and re-dispatch
- **BLOCKED:** Assess → provide context, upgrade model, split task, or escalate to human

## Key Rules

- Never start on main/master without user consent
- Never skip reviews (both stages required)
- Spec compliance BEFORE code quality (in that order)
- If reviewer finds issues → worker fixes → re-review (don't skip loop)
- Don't dispatch parallel workers (conflicts)
- Provide full task text to workers (don't make them read files)

## Related Skills
- **using-git-worktrees** — ensures isolated workspace
- **writing-plans** — creates the plan this skill executes
- **requesting-code-review** — review template for reviewer workers
- **finishing-a-development-branch** — completes development after all tasks
- **test-driven-development** — workers follow TDD for each task
