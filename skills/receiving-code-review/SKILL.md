---
name: receiving-code-review
description: Evaluate code review feedback with technical rigor — verify before implementing, push back when warranted
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [code-review, feedback, triage, requested-changes]
    requires_toolsets: [git]
---

# Code Review Reception

## Overview

Evaluate code review feedback with technical rigor. Verify before implementing. Push back when warranted.

**Core principle:** Technical correctness over social comfort. Actions speak louder than performative agreement.

## The Response Pattern

1. **READ** — complete feedback without reacting
2. **UNDERSTAND** — restate requirement in own words (or ask)
3. **VERIFY** — check against codebase reality
4. **EVALUATE** — technically sound for THIS codebase?
5. **RESPOND** — technical acknowledgment or reasoned pushback
6. **IMPLEMENT** — one item at a time, test each

## If Anything Is Unclear

Stop. Do not implement partial understanding. Ask about all unclear items before starting — items may be related, and partial understanding leads to wrong implementation.

## External Reviewer Feedback

Before implementing suggestions from external reviewers:
- Is it technically correct for THIS codebase?
- Does it break existing functionality?
- What's the reason for the current implementation?
- Does the reviewer understand the full context?

If suggestion seems wrong → push back with technical reasoning.
If it conflicts with prior architectural decisions → discuss with your team first.

## When to Push Back

- Suggestion breaks existing functionality
- Reviewer lacks full context
- Violates YAGNI (unused feature — check actual usage)
- Technically incorrect for this stack
- Legacy/compatibility reasons exist

**How:** Technical reasoning, specific questions, reference working tests. Never defensive.

## Implementation Order

For multi-item feedback:
1. Clarify anything unclear FIRST
2. Implement in priority order: blocking issues → simple fixes → complex fixes
3. Test each fix individually
4. Verify no regressions

## Acknowledging Correct Feedback

When feedback is correct: just fix it. The code shows you heard the feedback.
- Good: "Fixed. [brief description]" or "Good catch — [issue]. Fixed in [location]."
- Avoid: performative gratitude, excessive agreement

## Correcting Pushback

If you pushed back and were wrong: state correction factually, move on.
- "You were right — I checked [X] and it does [Y]. Implementing now."

## Common Mistakes

- **Blind implementation** — verify against codebase first
- **Batch without testing** — one at a time, test each
- **Assuming reviewer is right** — check if it breaks things
- **Avoiding pushback** — technical correctness > comfort
- **Partial implementation** — clarify all items first
| Can't verify, proceed anyway | State limitation, ask for direction |

## Real Examples

**Performative Agreement (Bad):**
```
Reviewer: "Remove legacy code"
❌ "You're absolutely right! Let me remove that..."
```

**Technical Verification (Good):**
```
Reviewer: "Remove legacy code"
✅ "Checking... build target is 10.15+, this API needs 13+. Need legacy for backward compat. Current impl has wrong bundle ID - fix it or drop pre-13 support?"
```

**YAGNI (Good):**
```
Reviewer: "Implement proper metrics tracking with database, date filters, CSV export"
✅ "Grepped codebase - nothing calls this endpoint. Remove it (YAGNI)? Or is there usage I'm missing?"
```

**Unclear Item (Good):**
```
your human partner: "Fix items 1-6"
You understand 1,2,3,6. Unclear on 4,5.
✅ "Understand 1,2,3,6. Need clarification on 4 and 5 before implementing."
```

## GitHub Thread Replies

When replying to inline review comments on GitHub, reply in the comment thread (`gh api repos/{owner}/{repo}/pulls/{pr}/comments/{id}/replies`), not as a top-level PR comment.

## The Bottom Line

**External feedback = suggestions to evaluate, not orders to follow.**

Verify. Question. Then implement.

No performative agreement. Technical rigor always.
