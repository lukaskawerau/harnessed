# Harness Audit — Full Assessment

Run a complete harness engineering audit on a codebase.

## Usage

```
Read this skill and audit the repository at [path].
Generate a report with scores and recommendations.
```

## Process

1. Run each audit skill in order
2. Collect scores (A/B/C/D) per category
3. Calculate overall harness readiness score (0-100)
4. Generate actionable recommendations

## Audit Checklist

### 1. Feedback Loops (see `feedback-loops.md`)
- [ ] CI exists and runs on every PR
- [ ] Tests exist and pass
- [ ] Pre-commit hooks for lint/format
- [ ] Build time < 5 minutes

### 2. Documentation (see `documentation.md`)
- [ ] AGENTS.md or equivalent exists
- [ ] Architecture documented
- [ ] README explains setup clearly

### 3. Type Safety (see `type-safety.md`)
- [ ] Strict type checking enabled
- [ ] External input validated at boundaries
- [ ] No `any` types (or equivalent)

### 4. Code Navigation (see `code-navigation.md`)
- [ ] ast-index or similar available
- [ ] Clear module/package structure
- [ ] Reasonable file sizes

### 5. Startup Time (see `startup-time.md`)
- [ ] Dev environment boots in < 60 seconds
- [ ] Tests run in < 5 minutes
- [ ] Containers are reasonably sized

### 6. Observability (see `observability.md`)
- [ ] Structured logging (JSON)
- [ ] Logs are queryable
- [ ] Error messages are actionable

## Scoring

| Grade | Score | Meaning |
|-------|-------|---------|
| A | 80-100 | Agent-ready, minimal friction |
| B | 60-79 | Functional, some gaps |
| C | 40-59 | Significant work needed |
| D | 0-39 | Major overhaul required |

## Output Format

```markdown
# Harness Audit Report: [project-name]

**Overall Score:** 62/100 (Grade: B)
**Date:** YYYY-MM-DD
**Audited by:** [agent-name]

## Summary

[2-3 sentence overview]

## Category Scores

| Category | Grade | Score | Key Issues |
|----------|-------|-------|------------|
| Feedback Loops | B | 15/20 | CI slow (8min) |
| Documentation | D | 5/20 | No AGENTS.md |
| ... | ... | ... | ... |

## Priority Recommendations

1. **[High]** Add AGENTS.md — see `fix/typescript/add-agents-md.md`
2. **[High]** Speed up CI — see `optimize/ci-speedup.md`
3. **[Medium]** Add structured logging — see `fix/typescript/structured-logging.md`

## Detailed Findings

[Per-category details]
```
