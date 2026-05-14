---
name: systematic-debugging
description: Root-cause investigation for bugs, failing tests, flaky behavior, and build failures before attempting fixes
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [debugging, root-cause, testing, failures, investigation]
    requires_toolsets: [terminal]
---

# Systematic Debugging

## Overview

Random fixes waste time and create new bugs. Quick patches mask underlying issues.

**Core principle:** ALWAYS find root cause before attempting fixes.

## The Four Phases

Complete each phase before proceeding to the next.

### Phase 1: Root Cause Investigation

**BEFORE attempting ANY fix:**
1. **Read error messages carefully** — stack traces, line numbers, error codes
2. **Reproduce consistently** — exact steps, every time
3. **Check recent changes** — git diff, new dependencies, config changes
4. **Gather evidence in multi-component systems** — log data at each boundary to find WHERE it breaks
5. **Trace data flow** — where does the bad value originate? See `references/root-cause-tracing.md`

### Phase 2: Pattern Analysis
1. Find working examples of similar code
2. Compare against references — read completely, don't skim
3. Identify differences between working and broken
4. Understand dependencies and assumptions

### Phase 3: Hypothesis and Testing
1. Form single hypothesis: "X is root cause because Y"
2. Test with SMALLEST possible change — one variable at a time
3. Verify: worked → Phase 4. Didn't work → new hypothesis

### Phase 4: Implementation
1. Create failing test case (use `test-driven-development` skill)
2. Implement single fix addressing root cause
3. Verify: test passes, no regressions
4. **If 3+ fixes failed:** STOP — question the architecture, discuss with your partner

## Quick Reference

| Phase | Key Activity | Success Criteria |
|-------|-------------|------------------|
| 1. Root Cause | Read errors, reproduce, trace | Understand WHAT and WHY |
| 2. Pattern | Find working examples, compare | Identify differences |
| 3. Hypothesis | Form theory, test minimally | Confirmed or rejected |
| 4. Implementation | Test, fix, verify | Bug resolved, tests pass |

## Red Flags — STOP and Return to Phase 1

- "Quick fix for now, investigate later"
- "Just try changing X and see"
- Proposing solutions before tracing data flow
- 3+ fix attempts without success → question architecture

## Supporting Techniques

See `references/` for detailed techniques:
- `references/root-cause-tracing.md` — trace bugs backward through call stack
- `references/defense-in-depth.md` — add validation at multiple layers
- `references/condition-based-waiting.md` — replace timeouts with condition polling

## Related Skills
- **test-driven-development** — for creating failing test case (Phase 4)
- **verification-before-completion** — verify fix before claiming success
