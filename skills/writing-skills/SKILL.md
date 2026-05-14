---
name: writing-skills
description: Author, port, or validate Hermes skills — frontmatter, discovery descriptions, lazy references, and testing
version: 1.0.0
author: Adapted from obra/superpowers
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [meta, skill-authoring, documentation, frontmatter]
    requires_toolsets: [terminal]
---

# Writing Skills

## Overview

Author skills that AI agents can discover, load, and follow. Skills teach techniques, patterns, or workflows via structured markdown with YAML frontmatter.

**Core principle:** Skills must be discoverable (good description), loadable (concise body <500 words), and effective (tested with pressure scenarios).

## Skill Structure

```
skills/
  skill-name/
    SKILL.md              # Main document (required)
    references/           # Heavy reference files (loaded on demand)
```

### YAML Frontmatter

```yaml
---
name: skill-name-with-hyphens
description: Use when [triggering conditions and symptoms]
version: 1.0.0
author: Your Name
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [relevant, keywords]
    requires_toolsets: [terminal, git]
---
```

**Description rules:**
- Start with "Use when..." — describe triggering conditions only
- Never summarize the skill's workflow (agents may follow the description instead of reading the body)
- Keep under 500 characters
- Third person, technology-agnostic unless skill is tech-specific

### Body Guidelines

- **Target:** <500 words for the SKILL.md body
- **Overview:** Core principle in 1-2 sentences
- **When to Use:** Symptoms and situations (bullet list)
- **Core Process:** Steps or pattern (concise)
- **Common Mistakes:** What goes wrong + fixes
- Move heavy reference (100+ lines), examples, and templates to `references/`

## Discovery Optimization

Make your skill findable:
- **Keywords:** error messages, symptoms, synonyms agents might search for
- **Naming:** verb-first, active voice (`using-git-worktrees` not `git-worktree-usage`)
- **Tags:** concrete terms agents associate with the problem domain

## Testing Skills

Test before deploying. A skill without a failing test first may teach the wrong thing.

**Process:**
1. Run baseline scenario WITHOUT the skill — document failures/rationalizations
2. Write skill addressing those specific violations
3. Re-run scenario WITH skill — verify compliance
4. Find new loopholes → add counters → re-verify

**Test approaches by type:**
- **Discipline skills** (rules): pressure scenarios — does agent comply under stress?
- **Technique skills** (how-to): application scenarios — can agent apply correctly?
- **Reference skills** (docs): retrieval scenarios — can agent find and use info?

## Common Mistakes

- **Description summarizes workflow** → agents shortcut by following description, skip body
- **Too long** (>500 words) → wastes context on every load; move details to references/
- **No testing** → untested skills always have gaps
- **Vague triggers** → agents can't tell when to activate the skill
- **Force-loading references** → burns context; let agents load lazily from references/

## References

- `references/anthropic-best-practices.md` — official skill authoring guidance
- `references/persuasion-principles.md` — making discipline skills stick
- `references/testing-skills-with-subagents.md` — detailed testing methodology
- `references/graphviz-conventions.dot` — flowchart style rules
- `references/render-graphs.js` — render skill flowcharts to SVG
