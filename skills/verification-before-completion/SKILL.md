---
name: verification-before-completion
description: Verify work is actually complete — run commands, inspect output, confirm evidence before claiming done
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [verification, testing, completion, quality-gate]
    requires_toolsets: [terminal]
---

# Verification Before Completion

## Overview

Never claim work is complete without fresh verification evidence. Evidence before claims, always.

## The Gate

Before claiming any status:
1. **IDENTIFY** — what command proves this claim?
2. **RUN** — execute the full command (fresh, complete)
3. **READ** — full output, check exit code, count failures
4. **VERIFY** — does output confirm the claim?
5. **ONLY THEN** — make the claim with evidence

Skip any step = unverified claim.

## What Counts as Verification

| Claim | Requires | NOT Sufficient |
|-------|----------|----------------|
| Tests pass | Test command output: 0 failures | Previous run, "should pass" |
| Build succeeds | Build command: exit 0 | Linter passing |
| Bug fixed | Reproduce original symptom: passes | "Code changed" |
| Regression test | Red-green cycle verified | Test passes once |
| Requirements met | Line-by-line checklist check | Tests passing |
| Worker completed | VCS diff shows correct changes | Worker reports "success" |

## Red Flags — Stop and Verify

- Using "should", "probably", "seems to"
- Expressing satisfaction before running commands
- About to commit/push/PR without fresh test run
- Trusting worker/agent success reports without checking
- Relying on partial verification for full claims
- Thinking "just this once"

## Key Patterns

**Tests:** Run → read output → "All 34 tests pass" (with evidence)

**Regression (TDD):** Write test → run (pass) → revert fix → run (MUST FAIL) → restore → run (pass)

**Requirements:** Re-read plan → create checklist → verify each item → report gaps or completion

**Delegated work:** Worker reports success → check diff → verify changes independently

## When to Apply

Always before: completion claims, committing, PR creation, task completion, moving to next task.

**No shortcuts. Run the command. Read the output. Then claim the result.**
