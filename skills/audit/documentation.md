# Audit: Documentation

Check that the codebase has agent-friendly documentation.

This is a context check, not a universal requirement list.
If a repo solves navigation well without `AGENTS.md`, say that.

## What to Check

### AGENTS.md or Equivalent
```bash
# Look for agent instructions
ls -la AGENTS.md CLAUDE.md CODEX.md .cursorrules 2>/dev/null

# Check content quality if exists
wc -l AGENTS.md 2>/dev/null
```

**Score:**
- Clear agent-facing entry point exists: +5
- > 50 lines with substance: +3
- Points to deeper docs: +2

### Architecture Documentation
```bash
# Look for architecture docs
ls -la docs/ARCHITECTURE.md ARCHITECTURE.md docs/architecture/ 2>/dev/null

# Check for design docs
find docs -name "*.md" 2>/dev/null | head -10
```

**Score:**
- Architecture doc exists: +4
- Design docs exist: +3
- Docs are current (< 6 months old): +3

### README Quality
```bash
# Check README exists and has substance
wc -l README.md 2>/dev/null

# Look for key sections
grep -E "^#{1,2} (Install|Setup|Usage|Getting Started|Development)" README.md 2>/dev/null
```

**Score:**
- README exists: +2
- Has setup instructions: +2
- Has development section: +2
- > 100 lines: +1

## Output

```yaml
category: documentation
score: X/20
grade: A|B|C|D
findings:
  - agents_md_exists: true|false
  - agents_md_lines: N
  - architecture_docs: true|false
  - readme_quality: high|medium|low
  - docs_freshness: current|stale|unknown
issues:
  - "No AGENTS.md found"
  - "Architecture documentation missing"
recommendations:
  - "Consider adding AGENTS.md or equivalent — see fix/typescript/add-agents-md.md"
  - "Consider documenting architecture in docs/ARCHITECTURE.md"
```
