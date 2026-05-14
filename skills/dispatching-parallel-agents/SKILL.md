---
name: dispatching-parallel-agents
description: Dispatch 2+ independent tasks to parallel workers when tasks have no shared state or sequential dependencies
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [parallelism, delegation, independent-tasks, orchestration]
---

# Dispatching Parallel Agents

## Overview

When you have multiple unrelated failures or independent tasks, investigating them sequentially wastes time. Dispatch one worker per independent problem domain and let them work concurrently.

**Core principle:** One worker per independent problem. Fresh context, no interference.

## When to Use

- 3+ tasks/failures with different root causes across different subsystems
- Each problem can be understood without context from others
- No shared state between investigations
- Workers won't edit the same files

**Don't use when:** failures are related, need full system context, or workers would interfere.

## The Pattern

### 1. Identify Independent Domains
Group failures/tasks by what's broken. Each domain must be independent — fixing one doesn't affect others.

### 2. Create Focused Worker Tasks
Each worker gets:
- **Specific scope** — one test file or subsystem
- **Clear goal** — what to fix/implement
- **Constraints** — don't change other code
- **Expected output** — summary of findings and changes

### 3. Dispatch in Parallel
Delegate each task to its own worker simultaneously.

### 4. Review and Integrate
- Read each summary
- Verify fixes don't conflict
- Run full test suite
- Integrate all changes

## Worker Prompt Structure

Good prompts are focused, self-contained, specific about output:

```markdown
Fix the 3 failing tests in src/agents/agent-tool-abort.test.ts:
1. "should abort tool with partial output capture"
2. "should handle mixed completed and aborted tools"
3. "should properly track pendingToolCount"

Your task: Read tests, identify root cause, fix. Do NOT just increase timeouts.
Return: Summary of root cause and changes made.
```

## Common Mistakes

- **Too broad** ("fix all tests") → worker gets lost. Be specific.
- **No context** → paste error messages and test names
- **No constraints** → worker might refactor everything
- **Vague output** → "return summary of root cause and changes"

## Verification

After workers return:
1. Review each summary
2. Check for conflicts (same files edited?)
3. Run full test suite
4. Spot check — workers can make systematic errors

## Real-World Impact

From debugging session (2025-10-03):
- 6 failures across 3 files
- 3 agents dispatched in parallel
- All investigations completed concurrently
- All fixes integrated successfully
- Zero conflicts between agent changes
