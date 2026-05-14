---
name: test-driven-development
description: TDD red-green-refactor cycle for features, bug fixes, refactors, and regression fixes — tests before implementation
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [testing, tdd, implementation, bugfix, refactor]
    requires_toolsets: [terminal]
---

# Test-Driven Development (TDD)

## Overview

Write the test first. Watch it fail. Write minimal code to pass.

**Core principle:** If you didn't watch the test fail, you don't know if it tests the right thing.

## When to Use

**Always:** New features, bug fixes, refactoring, behavior changes.

**Exceptions (ask your human partner):** Throwaway prototypes, generated code, configuration files.

## The Iron Law

Write code before the test? Delete it. Start over. No keeping it as "reference."

## Red-Green-Refactor

### RED — Write Failing Test
Write one minimal test showing what should happen. One behavior, clear name, real code (no mocks unless unavoidable).

### Verify RED — Watch It Fail
**MANDATORY.** Run the test. Confirm it fails because the feature is missing (not typos or errors).

### GREEN — Minimal Code
Write simplest code to pass the test. Don't add features beyond what the test requires.

### Verify GREEN — Watch It Pass
**MANDATORY.** Run test suite. Confirm new test passes AND other tests still pass.

### REFACTOR — Clean Up
After green only: remove duplication, improve names, extract helpers. Keep tests green. Don't add behavior.

### Repeat
Next failing test for next feature.

## Verification Checklist

Before marking work complete:
- Every new function/method has a test
- Watched each test fail before implementing
- Each test failed for expected reason
- Wrote minimal code to pass each test
- All tests pass with pristine output
- Tests use real code (mocks only if unavoidable)
- Edge cases and errors covered

## Red Flags — Delete Code and Start Over

- Code written before test
- Test passes immediately (never saw it fail)
- "I'll test after" / "just this once"
- "Keep as reference" / "adapt existing code"

## When Stuck

| Problem | Solution |
|---------|----------|
| Don't know how to test | Write wished-for API first. Ask your partner. |
| Test too complicated | Design too complicated. Simplify interface. |
| Must mock everything | Code too coupled. Use dependency injection. |

## Anti-Patterns

See `references/testing-anti-patterns.md` for common pitfalls: testing mock behavior instead of real behavior, adding test-only methods, mocking without understanding.

## Final Rule

Production code → test exists and failed first. Otherwise → not TDD. No exceptions without your human partner's permission.
