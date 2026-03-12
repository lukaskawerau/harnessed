# Harness Skills 🔧

A collection of skills for AI coding agents to audit, fix, and optimize codebases for harness engineering.

> ⚠️ **STATUS: Early Development** — Skills are being written and tested.

## What is this?

When you point [Orchestra](https://github.com/lukaskawerau/orchestra) or [Symphony](https://github.com/openai/symphony) at an existing codebase, the agent needs to understand:

1. **What's missing?** (audit)
2. **How to fix it?** (fix — stack-specific)
3. **How to make it faster?** (optimize)

These skills teach agents how to do harness engineering on any codebase.

## Quick Start

Point your agent at a skill:

```
Read skills/audit/feedback-loops.md and audit this repository.
```

Or run the full audit:

```
Read skills/audit/SKILL.md and run a complete harness audit on this repository.
```

## Skills

### Audit Skills
Assess a repo's agent-readiness.

| Skill | What it checks |
|-------|----------------|
| `feedback-loops` | CI, tests, pre-commit hooks, build time |
| `documentation` | AGENTS.md, architecture docs, README |
| `type-safety` | Strict types, validation at boundaries |
| `code-navigation` | ast-index, clear structure |
| `startup-time` | Time to boot dev environment |
| `observability` | Structured logging, queryable metrics |

### Fix Skills (Stack-Specific)
Remediate issues found by audit.

| Stack | Skills |
|-------|--------|
| TypeScript/Node | `add-agents-md`, `add-ci`, `add-ast-index`, `structured-logging` |
| Python | `add-agents-md`, `add-ci`, `fixtures-optimization` |
| Java Spring | *(community welcome)* |
| PHP Symfony | `add-agents-md`, `optimize-fixtures` |

### Optimize Skills
Make the feedback loop faster.

| Skill | What it does |
|-------|--------------|
| `ci-speedup` | Parallelize, cache, reduce test scope |
| `container-diet` | Shrink images, layer caching |
| `fixture-factory` | Lightweight, composable test data |
| `db-snapshots` | Pre-seeded databases vs migrations |

## Benchmarking

We test these skills against real OSS projects to measure effectiveness.

**Methodology:**
1. Agent attempts harness engineering WITHOUT skills
2. Agent attempts same task WITH skills (fresh checkout)
3. Compare: time, quality, bugs introduced

See `benchmark/` for projects and results.

## Contributing

Add skills for your stack! See `CONTRIBUTING.md`.

## Integration

- **Orchestra** — Runs these skills when onboarding new projects
- **Symphony** — Can reference skills in WORKFLOW.md prompts
- **Any agent** — Skills are just markdown; point any agent at them

## License

MIT
