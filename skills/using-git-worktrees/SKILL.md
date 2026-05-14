---
name: using-git-worktrees
description: Create isolated workspace for feature or bugfix work via git worktree or platform-native tools
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [git, worktree, isolation, branch, workspace]
    requires_toolsets: [git, terminal]
---

# Using Git Worktrees

## Overview

Ensure work happens in an isolated workspace. Detect existing isolation first, then create if needed.

**Core principle:** Detect existing isolation → Use platform tools if available → Fall back to git worktree → Setup → Verify baseline.

## Step 0: Detect Existing Isolation

Before creating anything, check if you're already isolated:

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
```

- **If `GIT_DIR != GIT_COMMON`** (and not a submodule): already in a worktree. Skip to Step 3.
- **If in a normal repo:** ask user consent before creating a worktree, or honor any declared preference.

Guard against submodules: check `git rev-parse --show-superproject-working-tree` — if it returns a path, you're in a submodule, not a worktree.

## Step 1: Create Isolated Workspace

**1a. Native tools (preferred):** If your platform provides a worktree creation tool (e.g., `EnterWorktree`, `/worktree` command), use it. Skip to Step 3.

**1b. Git fallback:** Only if no native tool exists.

Directory priority:
1. User-declared preference in instructions
2. Existing `.worktrees/` directory (or `worktrees/`)
3. Default: `.worktrees/` at project root

**Safety:** Verify the directory is git-ignored before creating. If not ignored, add to `.gitignore` and commit first.

```bash
git worktree add "$path" -b "$BRANCH_NAME"
cd "$path"
```

If permission denied (sandbox): work in place instead.

## Step 3: Project Setup

Auto-detect and run: `npm install`, `cargo build`, `pip install`, `go mod download`, etc.

## Step 4: Verify Clean Baseline

Run tests. If they fail, report and ask. If they pass, report ready:
```
Worktree ready at <path>
Tests passing (<N> tests)
Ready to implement <feature>
```

## Common Mistakes

- **Fighting the harness** — don't use `git worktree add` when platform already provides isolation
- **Skipping detection** — always check Step 0 first to avoid nested worktrees
- **Skipping ignore verification** — worktree contents will pollute git status
- **Proceeding with failing baseline** — can't distinguish new bugs from pre-existing ones
