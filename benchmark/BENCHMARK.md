# harnessed Benchmark

Measure the effectiveness of harnessed skills by comparing agent performance with and without them.

## Methodology

### Setup

1. Select diverse OSS projects (different stacks, sizes, harness readiness)
2. Define a standard "harness engineering" task for each
3. Run agents with and without skills
4. Measure outcomes

### Metrics

| Metric | Description |
|--------|-------------|
| **Time to completion** | Minutes from start to PR opened |
| **Quality score** | Manual review: 1-5 scale |
| **Bugs introduced** | Count of issues found in review |
| **CI pass rate** | Did the PR pass CI on first try? |
| **Iterations needed** | How many attempts before success? |

### Test Protocol

**Without skills (baseline):**
```
Prompt: "Add harness engineering to this repository. 
Make it ready for AI coding agents. 
Add AGENTS.md, CI, improve documentation."
```

**With skills:**
```
Prompt: "Read skills/audit/SKILL.md from harnessed repo.
Run a full harness audit on this repository.
Then apply the recommended fixes using the fix skills."
```

### Controls

- Same model (e.g., Codex, Claude Sonnet)
- Same temperature/settings
- Fresh clone each run
- Same reviewer for quality scoring

## Target Projects

See `projects/projects.yaml` for the full list.

### Selection Criteria

- **Diverse stacks:** TypeScript, Python, Java, Go, Rust
- **Diverse sizes:** Small (<10k LOC), Medium (10-100k), Large (>100k)
- **Diverse readiness:** Already good, needs work, complete mess
- **Active maintenance:** Someone will notice/care about PRs

### Candidate Projects

| Project | Stack | Size | Current Readiness |
|---------|-------|------|-------------------|
| [TBD] | TypeScript | Small | Low |
| [TBD] | Python | Medium | Medium |
| [TBD] | Java Spring | Large | Low |
| [TBD] | Go | Medium | High |

## Running Benchmarks

```bash
# 1. Clone project
git clone <project-url> /tmp/benchmark-run-N

# 2. Record baseline metrics
./scripts/measure-baseline.sh /tmp/benchmark-run-N

# 3. Run agent WITHOUT skills, record time
time codex "Add harness engineering to this repo..." 

# 4. Fresh clone
rm -rf /tmp/benchmark-run-N
git clone <project-url> /tmp/benchmark-run-N

# 5. Run agent WITH skills, record time
time codex "Read harnessed/skills/audit/SKILL.md and apply to this repo..."

# 6. Compare PRs, score quality
```

## Results Format

```yaml
project: example-project
stack: typescript
loc: 15000
date: 2026-03-15

baseline:
  time_minutes: 45
  quality_score: 2
  bugs: 3
  ci_passed: false
  iterations: 4

with_skills:
  time_minutes: 18
  quality_score: 4
  bugs: 0
  ci_passed: true
  iterations: 1

improvement:
  time_reduction: 60%
  quality_increase: +2
  bugs_prevented: 3
```

## Publishing Results

Results will be published in:
- `benchmark/results/` — Raw YAML files
- Blog post with analysis
- Potentially: academic paper on harness engineering effectiveness
