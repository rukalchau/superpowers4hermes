---
name: finishing-a-development-branch
description: Complete development work — merge, PR, branch cleanup, or worktree cleanup after tests pass
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [git, merge, pr, branch-cleanup, integration]
    requires_toolsets: [git, terminal]
---

# Finishing a Development Branch

## Overview

Guide completion of development work by presenting clear options and handling chosen workflow.

**Core principle:** Verify tests → Detect environment → Present options → Execute choice → Clean up.

## The Process

### Step 1: Verify Tests

Run the project's test suite. If tests fail, stop — cannot proceed until they pass.

### Step 2: Detect Environment

Determine if you're in a normal repo or a git worktree:
- **Normal repo:** Standard 4 options, no worktree cleanup
- **Named-branch worktree:** Standard 4 options, cleanup depends on provenance
- **Detached HEAD:** Reduced 3 options (no merge), no cleanup

### Step 3: Determine Base Branch

Identify what branch this work split from (main, master, etc.).

### Step 4: Present Options

**Normal/named-branch:**
1. Merge back to base branch locally
2. Push and create a Pull Request
3. Keep the branch as-is
4. Discard this work

**Detached HEAD:**
1. Push as new branch and create PR
2. Keep as-is
3. Discard this work

### Step 5: Execute Choice

- **Merge:** checkout base → pull → merge → verify tests → cleanup worktree → delete branch
- **PR:** push branch → create PR with summary and test plan → keep worktree alive
- **Keep:** report branch name and path, don't cleanup
- **Discard:** confirm with user first → cleanup worktree → force-delete branch

### Step 6: Cleanup Workspace

Only for merge and discard. Navigate to main repo root before removing worktree:
```bash
cd "$MAIN_ROOT"
git worktree remove "$WORKTREE_PATH"
git worktree prune
```

Only remove worktrees that were created by your tooling (under `.worktrees/` or `worktrees/`). If the host environment owns the workspace, leave it in place.

## Quick Reference

| Option | Merge | Push | Keep Worktree | Cleanup Branch |
|--------|-------|------|---------------|----------------|
| Merge locally | yes | — | — | yes |
| Create PR | — | yes | yes | — |
| Keep as-is | — | — | yes | — |
| Discard | — | — | — | yes (force) |

## Common Mistakes

- **Skipping test verification** — always verify before offering options
- **Cleaning up worktree for PR** — user needs it for iteration
- **Deleting branch before removing worktree** — worktree reference blocks deletion
- **Running worktree remove from inside it** — always cd to main repo root first

**Cleaning up harness-owned worktrees**
- **Problem:** Removing a worktree the harness created causes phantom state
- **Fix:** Only clean up worktrees under `.worktrees/`, `worktrees/`, or `~/.config/superpowers/worktrees/`

**No confirmation for discard**
- **Problem:** Accidentally delete work
- **Fix:** Require typed "discard" confirmation

## Red Flags

**Never:**
- Proceed with failing tests
- Merge without verifying tests on result
- Delete work without confirmation
- Force-push without explicit request
- Remove a worktree before confirming merge success
- Clean up worktrees you didn't create (provenance check)
- Run `git worktree remove` from inside the worktree

**Always:**
- Verify tests before offering options
- Detect environment before presenting menu
- Present exactly 4 options (or 3 for detached HEAD)
- Get typed confirmation for Option 4
- Clean up worktree for Options 1 & 4 only
- `cd` to main repo root before worktree removal
- Run `git worktree prune` after removal
