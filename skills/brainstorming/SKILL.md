---
name: brainstorming
description: Feature ideation, design exploration, requirements clarification, and pre-implementation spec creation
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [design, ideation, requirements, specification]
    requires_toolsets: [terminal]
---

# Brainstorming Ideas Into Designs

Help turn ideas into fully formed designs and specs through natural collaborative dialogue.

Start by understanding the current project context, then ask questions one at a time to refine the idea. Once you understand what you're building, present the design and get user approval.

**GATE:** Do NOT write any code or take implementation actions until you have presented a design and the user has approved it.

## Checklist

Complete in order:

1. **Explore project context** — check files, docs, recent commits
2. **Offer visual companion** (if topic involves visual questions) — see `references/visual-companion.md`
3. **Ask clarifying questions** — one at a time, understand purpose/constraints/success criteria
4. **Propose 2-3 approaches** — with trade-offs and your recommendation
5. **Present design** — in sections scaled to complexity, get approval after each section
6. **Write design doc** — save to `docs/specs/YYYY-MM-DD-<topic>-design.md` and commit
7. **Spec self-review** — check for placeholders, contradictions, ambiguity, scope
8. **User reviews written spec** — ask user to review before proceeding
9. **Transition** — invoke `writing-plans` skill to create implementation plan

## The Process

**Understanding the idea:**
- Check current project state (files, docs, commits)
- If request describes multiple independent subsystems, flag it — decompose before refining
- Ask questions one at a time. Prefer multiple choice. One question per message.
- Focus on: purpose, constraints, success criteria

**Exploring approaches:**
- Propose 2-3 approaches with trade-offs
- Lead with your recommendation and reasoning

**Presenting the design:**
- Scale each section to its complexity
- Ask after each section whether it looks right
- Cover: architecture, components, data flow, error handling, testing

**Design for isolation:**
- Break into smaller units with clear purpose and well-defined interfaces
- Each unit should be understandable and testable independently

**In existing codebases:**
- Follow existing patterns. Only propose changes that serve the current goal.

## After the Design

1. Write spec to `docs/specs/YYYY-MM-DD-<topic>-design.md`, commit
2. Self-review: placeholder scan, consistency, scope, ambiguity — fix inline
3. Ask user to review the spec
4. Once approved, invoke the `writing-plans` skill

## Key Principles

- One question at a time
- YAGNI ruthlessly
- Always explore 2-3 alternatives
- Present design incrementally, validate each section

## References

- `references/visual-companion.md` — browser-based visual companion guide
- `references/spec-document-reviewer-prompt.md` — spec review template
