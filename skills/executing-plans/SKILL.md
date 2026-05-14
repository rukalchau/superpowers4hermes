---
name: executing-plans
description: Execute a written implementation plan in one session with checkpoints and blocker handling
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [execution, implementation, checkpoints, plans]
    requires_toolsets: [terminal, git]
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Note:** If subagents are available, prefer the `subagent-driven-development` skill for higher-quality results with per-task review cycles.

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically — identify any questions or concerns
3. If concerns: raise them with your human partner before starting
4. If no concerns: create a task checklist and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in-progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Mark as completed

### Step 3: Complete Development

After all tasks complete and verified:
- Use the `finishing-a-development-branch` skill to verify tests, present options, execute choice

## When to Stop

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## Key Rules
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent

## Related Skills
- **using-git-worktrees** — ensures isolated workspace
- **writing-plans** — creates the plan this skill executes
- **finishing-a-development-branch** — complete development after all tasks
