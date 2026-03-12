# Audit: Startup Time

Measure how long it takes to boot a development environment.

## Why This Matters

Symphony/Orchestra create isolated workspaces per task. If startup takes 10 minutes, agent throughput collapses.

**Targets:**
- Ideal: < 30 seconds
- Acceptable: < 60 seconds  
- Problematic: < 5 minutes
- Blocking: > 5 minutes

## What to Measure

### Dependency Installation
```bash
# Node.js
rm -rf node_modules && time npm install 2>&1 | tail -3

# Python
rm -rf .venv && time python -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt 2>&1 | tail -3
```

### Database Setup
```bash
# If using migrations
time npm run db:migrate 2>&1 | tail -3
# or
time python manage.py migrate 2>&1 | tail -3
```

### Container Boot (if applicable)
```bash
# Docker Compose
time docker compose up -d 2>&1 | tail -5

# Check total service count
docker compose ps | wc -l
```

### Full Dev Environment
```bash
# Time from clone to running
time (npm install && npm run dev &) 2>&1
# Kill after first successful response
```

## Identifying Bottlenecks

```bash
# Large dependencies
du -sh node_modules/* | sort -hr | head -10

# Slow migrations
# Add timing to each migration step

# Container image sizes
docker images | grep -E "project-name|postgres|redis"
```

## Output

```yaml
category: startup-time
score: X/20
grade: A|B|C|D
findings:
  - dependency_install_seconds: N
  - db_setup_seconds: N
  - container_boot_seconds: N
  - total_startup_seconds: N
  - service_count: N
  - largest_deps: ["dep1", "dep2"]
bottlenecks:
  - component: "database migration"
    time_seconds: 480
    recommendation: "Use pre-seeded snapshots"
  - component: "container pull"
    time_seconds: 120
    recommendation: "Use local registry cache"
issues:
  - "Total startup time: 10m 23s (target: <60s)"
recommendations:
  - "See optimize/db-snapshots.md for pre-seeded databases"
  - "See optimize/container-diet.md for image optimization"
```
