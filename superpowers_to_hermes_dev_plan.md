# Adapting obra/superpowers for Hermes Agent: Development Plan

## Overview

`obra/superpowers` is an agentic skills framework originally built for Claude Code (Anthropic's CLI agent). It provides structured workflows — TDD, brainstorming, planning, debugging, and code execution — via markdown-based **SKILL.md** files loaded on demand. Hermes Agent (by Nous Research) also uses a markdown-based skills system with YAML frontmatter, making a port conceptually straightforward. However, the two systems differ in tool names, directory conventions, activation mechanics, and plugin architecture. This plan outlines the gap analysis and the work needed to bridge them.

---

## System Comparison

| Dimension | obra/superpowers (Claude Code) | Hermes Agent |
|-----------|-------------------------------|--------------|
| **Skill file format** | `SKILL.md` with `name`/`description` frontmatter | `SKILL.md` with richer YAML frontmatter (tags, toolset requirements, platform, env vars) |
| **Skill activation** | `Skill` tool call (Claude Code), `activate_skill` (Gemini CLI) | 3-level lazy loading; agent auto-activates by description match |
| **Tool names** | `Read`, `Write`, `Bash`, `Computer`, `Skill`, `TodoWrite` | `terminal`, `web_extract`, `file_read`, `file_write`, `skill_manage` |
| **Directory** | `skills/<skill-name>/SKILL.md` inside plugin repo | `~/.hermes/skills/<category>/<skill-name>/SKILL.md` |
| **Plugin system** | `plugin.json` + `marketplace.json`, installed via `claude-code plugin install` | Toolset registration via Python `registry.register()`, or pure markdown skills |
| **Sub-skills / refs** | `references/` folder, `@file` syntax to force-load | `references/` folder, level-2 lazy loading |
| **MCP support** | Via `mcp-cli` lab skill | Native MCP toolset integration |
| **OS support** | macOS/Linux, some Windows bugs | macOS/Linux (`platforms:` field filters per OS) |
| **Subagent support** | First-class (dispatching via `Skill` tool) | Supported via Hermes's own task delegation |

---

## Gap Analysis

### 1. Frontmatter Schema Mismatch

Superpowers skills use a minimal 2-field frontmatter (`name`, `description`). Hermes expects a richer schema with optional fields like `tags`, `requires_toolsets`, `requires_tools`, `fallback_for_toolsets`, `platforms`, and `required_environment_variables`. Every ported skill needs its frontmatter upgraded.

**Work required:** Write a migration script that reads `name`/`description` from each `SKILL.md` and emits valid Hermes frontmatter with sensible defaults, inferring tags from skill category directories.

### 2. Tool Name Translation

Superpowers skill content references Claude Code tool names. Hermes uses different tool identifiers:

| Claude Code | Hermes Equivalent |
|-------------|-------------------|
| `Bash` / `shell` | `terminal` |
| `Read` | `file_read` |
| `Write` | `file_write` |
| `WebFetch` | `web_extract` |
| `Skill` | `skill_manage` (load/read) |
| `TodoWrite` | No direct equivalent — use `terminal` + text file or adapt |
| `Computer` | No equivalent — skip or stub |

**Work required:** A sed/awk pass over each `SKILL.md` to replace tool name references in prose and code blocks. Some skills that rely heavily on `TodoWrite` or `Computer` may need deeper rewrites.

### 3. Activation Trigger Differences

Superpowers uses an explicit "invoke skill before any action" model — Claude Code must call the `Skill` tool to load content. Hermes auto-matches skills at Level 0 using description text; it loads content at Level 1 when it decides to use it. This means:

- Skills that contain "REQUIRED: Invoke this skill before responding" type instructions need those lines removed or rephrased.
- The `using-superpowers` gatekeeper skill (which enforces skill-first behavior) is Claude Code–specific and has **no equivalent needed in Hermes** — Hermes handles discovery natively.

**Work required:** Strip Claude Code–specific meta-instructions from `SKILL.md` files. The content/technique portions remain valid.

### 4. `@file` Force-Load Syntax

Superpowers uses `@path/to/file.md` syntax to force-load reference files into context. Hermes uses its level-2 lazy loading — references in `references/` are fetched by the agent on demand. No special `@` syntax is needed or supported.

**Work required:** Replace `@reference/file.md` citations with prose instructions telling the agent to load the file from `references/` when needed.

### 5. Plugin Registration

Superpowers ships as a Claude Code plugin (`plugin.json` + `marketplace.json`). Hermes has no equivalent plugin manifest. Skills are installed by placing directories under `~/.hermes/skills/`.

**Work required:** Write an install script (`install.sh`) that copies the ported skill directories into `~/.hermes/skills/superpowers/`.

---

## Development Plan

### Phase 0 — Inventory & Prioritization (Week 1)

**Goal:** Understand the full scope before writing a single line.

- Clone `obra/superpowers` and list all skills in `skills/`
- Categorize each as: (a) portable as-is, (b) needs light editing, (c) needs deep rewrite, (d) Claude Code–specific / skip
- Classify (d) candidates: `using-superpowers`, `subagent-driven-development`, any skill that directly calls `Computer` tool
- Produce a priority list: start with high-value, broadly applicable skills (TDD, brainstorming, systematic-debugging, writing-plans)

**Deliverables:** `inventory.csv` with skill name, category, porting effort (S/M/L), and skip flag.

---

### Phase 1 — Tooling (Week 1–2)

**Goal:** Build automation to eliminate manual repetition.

#### 1.1 Frontmatter Upgrade Script (`upgrade_frontmatter.py`)

```python
# Reads SKILL.md, extracts name + description, emits Hermes-compatible frontmatter
# Infers tags from parent directory name
# Adds: version: 1.0.0, platforms: [macos, linux], metadata.hermes.tags
```

#### 1.2 Tool Name Translator (`translate_tools.py`)

```python
# Regex replacements for all tool name mentions in SKILL.md prose + code blocks
# Handles: Bash→terminal, Read→file_read, Write→file_write, WebFetch→web_extract
# Outputs a diff for manual review before applying
```

#### 1.3 Install Script (`install.sh`)

```bash
#!/usr/bin/env bash
# Copies ported skills to ~/.hermes/skills/superpowers/<skill-name>/
# Respects existing skills — does not overwrite without --force flag
```

**Deliverables:** Three utility scripts, tested on 2–3 sample skills.

---

### Phase 2 — High-Priority Skill Ports (Week 2–4)

Port the 10–12 most valuable superpowers skills. Run each through the tooling from Phase 1, then do a manual review pass.

**Target skills (priority order):**

| Skill | Porting Effort | Notes |
|-------|---------------|-------|
| `test-driven-development` | Medium | Remove rationalization table headers referencing CC tools |
| `brainstorming` | Light | Mostly prose; clean port |
| `systematic-debugging` | Light | Mostly prose |
| `writing-plans` | Medium | References `TodoWrite` → adapt to `terminal` + markdown file |
| `executing-plans` | Medium | Subagent dispatch language needs reframing for Hermes |
| `defensive-programming` | Light | Pure technique, no CC tools |
| `condition-based-waiting` | Light | Testing technique; agnostic |
| `root-cause-tracing` | Light | Pure debugging pattern |
| `writing-skills` | Medium | Meta-skill for creating more skills; adapt TDD/test sections |
| `git-workflow` | Medium | Shell commands translate well via `terminal` |
| `finding-duplicate-functions` | Heavy | Two-phase LLM call — needs Hermes subagent adapter |
| `using-tmux-for-interactive-commands` | Heavy | Requires `terminal` with tmux; test on Hermes sandbox first |

**Per-skill process:**

1. Run frontmatter upgrade + tool name translator
2. Manual review: remove CC-specific meta-instructions, verify examples
3. Write a test scenario (per superpowers' own TDD philosophy for skills)
4. Load in Hermes, run test scenario, verify agent compliance
5. Iterate (REFACTOR phase) until passing

---

### Phase 3 — Hermes-Native Enhancements (Week 3–5)

Take advantage of Hermes features that superpowers doesn't have.

#### 3.1 Add Toolset Gating

Use `requires_toolsets` and `fallback_for_toolsets` to make skills context-aware:

```yaml
metadata:
  hermes:
    requires_toolsets: [git]         # git-workflow skill only shows when git toolset is active
    fallback_for_toolsets: [browser] # show web-search skill only when browser toolset is absent
```

#### 3.2 Add Environment Variable Declarations

Skills that need API keys (e.g., any LLM-assisted skill) can declare them explicitly:

```yaml
required_environment_variables:
  - name: ANTHROPIC_API_KEY
    prompt: "Enter your Anthropic API key for subagent dispatch"
    help: "Get one at https://console.anthropic.com"
```

#### 3.3 Rewrite `using-superpowers` as a Hermes Getting-Started Skill

The original `using-superpowers` skill is a Claude Code gatekeeper. Replace it with a lightweight Hermes orientation skill:

```yaml
---
name: superpowers-orientation
description: Use at the start of complex development tasks to select the right superpowers skill
version: 1.0.0
metadata:
  hermes:
    tags: [Getting Started, Workflow]
---
```

Content should list available skills, their triggers, and priority order (process skills before implementation skills).

---

### Phase 4 — MCP Integration Bridge (Week 4–5)

Superpowers-lab includes an `mcp-cli` skill for on-demand MCP tool invocation. Hermes supports MCP natively as toolsets. Bridge this by:

- Writing a Hermes toolset adapter that registers MCP servers as Hermes toolsets
- Porting the `mcp-cli` SKILL.md to describe when to use each MCP toolset vs. invoking raw
- Testing with at least 3 MCP servers: GitHub, Postgres, and Notion

---

### Phase 5 — Validation & Community (Week 5–6)

**5.1 Regression Testing**

Run the full superpowers test scenario suite (pressure scenarios from `testing-skills-with-subagents.md`) against Hermes. Document pass/fail per skill and iterate.

**5.2 Token Budget Audit**

Hermes has a 3-level loading system; skills at Level 0 must stay lightweight. Audit all ported skills against the superpowers word count targets:
- Getting-started workflows: < 150 words
- Frequently-loaded skills: < 200 words
- Other skills: < 500 words

**5.3 Publish & Contribute**

- Publish ported skills to a fork or separate repo (e.g., `superpowers-hermes`)
- Submit a PR to `obra/superpowers-skills` with a `platforms: hermes` flag on compatible skills
- Open issues on `hermesagent` repo for any gaps discovered (missing tool equivalents, frontmatter features needed)

---

## Risk Register

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Hermes skill activation is less aggressive than CC's explicit invocation | High | Medium | Add strong triggering keywords to descriptions; test activation rate |
| `TodoWrite` has no Hermes equivalent | High | Low | Replace with `terminal` writing a `.md` checklist file; Hermes can read it back |
| `Computer` tool skills (browser GUI) are incompatible | Medium | Low | Skip or replace with `web_extract` + `terminal` for CLI browser tools |
| Subagent dispatch semantics differ | Medium | High | Test executing-plans and subagent-driven-development skills heavily in Phase 2 |
| Windows skill failures (existing issue #51 in superpowers) | Low | Low | Use `platforms: [macos, linux]` filter; Windows support is a separate effort |

---

## Quick-Start Checklist (Minimal Viable Port)

For a fast proof-of-concept before committing to the full plan, port just these 4 skills manually:

- [ ] `brainstorming` — lowest effort, highest day-to-day value
- [ ] `test-driven-development` — enforces quality discipline
- [ ] `systematic-debugging` — high ROI for any developer
- [ ] `writing-plans` — enables structured task delegation

Test each in a real Hermes session. If they work well, proceed with Phase 1 tooling to scale the port to the full library.

---

## Appendix: Hermes SKILL.md Template for Ported Skills

```yaml
---
name: skill-name-here
description: Use when [specific triggering conditions from original superpowers description]
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux]
metadata:
  hermes:
    tags: [Category, Workflow]
    requires_toolsets: []          # fill in if applicable
    fallback_for_toolsets: []      # fill in if applicable
---

## When to Use
[Preserve original "When to Use" section verbatim or lightly edited]

## Procedure
[Original "Core Pattern" or "Implementation" section, with tool names translated to Hermes equivalents]

## Pitfalls
[Original "Common Mistakes" section]

## Verification
[Original "Real-World Impact" or add a new verification checklist]
```
