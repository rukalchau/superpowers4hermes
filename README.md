# Superpowers for Hermes Agent

A lightweight skill set adapted from [obra/superpowers](https://github.com/obra/superpowers) for use with Hermes Agent (Nous Research).

## Installation

Copy the `skills/` directory into your Hermes skills path:

```bash
cp -r skills/* ~/.hermes/skills/
```

## Skills

| Skill | Description |
|-------|-------------|
| **brainstorming** | Socratic design refinement — refines rough ideas through questions, explores alternatives |
| **test-driven-development** | RED-GREEN-REFACTOR cycle with testing anti-patterns reference |
| **systematic-debugging** | 4-phase root cause process (root-cause-tracing, defense-in-depth, condition-based-waiting) |
| **writing-plans** | Break work into bite-sized tasks with exact file paths and verification steps |
| **executing-plans** | Batch execution with human checkpoints |
| **subagent-driven-development** | Fast iteration with two-stage review (spec compliance, then code quality) |
| **dispatching-parallel-agents** | Concurrent subagent workflows |
| **requesting-code-review** | Pre-review checklist |
| **receiving-code-review** | Responding to feedback |
| **using-git-worktrees** | Parallel development branches |
| **finishing-a-development-branch** | Merge/PR decision workflow |
| **verification-before-completion** | Ensure it's actually fixed before declaring done |
| **writing-skills** | Create new skills following best practices |

## Workflow

1. **brainstorming** — Refine the idea before coding
2. **writing-plans** — Break into small, verifiable tasks
3. **subagent-driven-development** / **executing-plans** — Execute tasks with review
4. **test-driven-development** — RED-GREEN-REFACTOR for each piece
5. **requesting-code-review** — Review between tasks
6. **finishing-a-development-branch** — Wrap up cleanly

## Philosophy

- **Test-Driven Development** — Write tests first, always
- **Systematic over ad-hoc** — Process over guessing
- **Complexity reduction** — Simplicity as primary goal
- **Evidence over claims** — Verify before declaring success

## Adaptation Notes

See [superpowers_to_hermes_dev_plan.md](superpowers_to_hermes_dev_plan.md) for the full gap analysis and porting plan. Key differences from the original:

- Hermes handles skill discovery natively (no gatekeeper skill needed)
- Tool names differ: `Bash`→`terminal`, `Read`→`file_read`, `Write`→`file_write`, `WebFetch`→`web_extract`
- Skills use Hermes YAML frontmatter schema with tags, toolset requirements, and platform fields

## License

MIT License — see LICENSE file for details.

## Credits

Adapted from [obra/superpowers](https://github.com/obra/superpowers) by Jesse Vincent.
