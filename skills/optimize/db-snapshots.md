# Optimize: Database Snapshots

Replace slow migrations with pre-seeded database snapshots.

## The Problem

Migrations that take 10+ minutes destroy agent throughput. Every workspace needs a fresh database, and waiting for migrations blocks work.

**Symptoms:**
- `npm run db:migrate` takes > 2 minutes
- Fixtures/seeds take > 1 minute
- Agents timeout waiting for database

## The Solution

Pre-seed a database once, snapshot it, restore the snapshot per workspace.

## Approach by Database

### SQLite

Easiest. Just copy the file.

```bash
# Create seeded database once
npm run db:migrate
npm run db:seed
cp data/app.db data/app.seed.db

# In workspace setup (hooks.after_create)
cp data/app.seed.db data/app.db
```

**WORKFLOW.md hook:**
```yaml
hooks:
  after_create: |
    cp /path/to/seeds/app.seed.db ./data/app.db
```

### PostgreSQL (Docker)

Use Docker volumes or pg_dump.

**Option A: Volume snapshot**
```bash
# Create seeded volume
docker compose up -d db
npm run db:migrate && npm run db:seed
docker compose stop db
docker run --rm -v projectname_pgdata:/data -v $(pwd):/backup alpine \
  tar czf /backup/pgdata-seed.tar.gz -C /data .

# Restore in workspace
docker run --rm -v projectname_pgdata:/data -v $(pwd):/backup alpine \
  tar xzf /backup/pgdata-seed.tar.gz -C /data
```

**Option B: pg_dump/pg_restore**
```bash
# Create dump
pg_dump -Fc mydb > seed.dump

# Restore (faster than migrations)
pg_restore -d mydb --clean seed.dump
```

### MySQL

Similar to PostgreSQL:

```bash
# Dump
mysqldump mydb > seed.sql

# Restore
mysql mydb < seed.sql
```

## Workspace Integration

### Symphony/Orchestra Hook

```yaml
workspace:
  root: ./workspaces

hooks:
  after_create: |
    # Restore database snapshot instead of running migrations
    if [ -f ./seeds/db-snapshot.tar.gz ]; then
      tar xzf ./seeds/db-snapshot.tar.gz -C ./data/
    else
      npm run db:migrate
      npm run db:seed
    fi
```

### Keep Snapshots Fresh

Create a scheduled job to regenerate snapshots when schema changes:

```bash
#!/bin/bash
# scripts/refresh-db-snapshot.sh

npm run db:migrate
npm run db:seed
# Create snapshot
tar czf seeds/db-snapshot.tar.gz -C data .
git add seeds/db-snapshot.tar.gz
git commit -m "chore: refresh db snapshot"
```

## Measuring Impact

```bash
# Before (migrations)
time npm run db:migrate
# → 8m 41s

# After (snapshot restore)
time tar xzf seeds/db-snapshot.tar.gz -C data/
# → 3s
```

## Tradeoffs

**Pros:**
- 100x+ faster workspace setup
- Deterministic state
- Works offline

**Cons:**
- Snapshot must be regenerated on schema changes
- Binary file in repo (use LFS for large snapshots)
- May mask migration bugs

## Success Criteria

- [ ] Snapshot file exists and is < 100MB
- [ ] Workspace setup uses snapshot, not migrations
- [ ] Startup time reduced by > 50%
- [ ] CI job regenerates snapshot on schema changes
