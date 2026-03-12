# Fix: Add AGENTS.md (TypeScript)

Create an AGENTS.md file that helps AI agents navigate the codebase.

## When to Use

- Audit found: "No AGENTS.md"
- Starting harness engineering on a TypeScript/Node project

## Steps

### 1. Analyze Project Structure

```bash
# Get directory layout
find . -type d -not -path "*/node_modules/*" -not -path "*/.git/*" | head -30

# Find key entry points
ls -la src/index.ts src/main.ts src/app.ts index.ts 2>/dev/null

# Check package.json scripts
cat package.json | jq '.scripts'
```

### 2. Identify Key Files

Look for:
- Entry points (`src/index.ts`, `src/main.ts`)
- Config files (`tsconfig.json`, `vite.config.ts`, etc.)
- Database schema (`src/db/schema.ts`, `prisma/schema.prisma`)
- API routes (`src/routes/`, `src/api/`)

### 3. Generate AGENTS.md

Create file with this structure:

```markdown
# AGENTS.md — [Project Name]

This is your map. Follow it.

## What is [Project Name]?

[1-2 sentence description from README or package.json]

## Repository Structure

\`\`\`
src/           → Application source code
  lib/         → Shared utilities and core logic
  routes/      → API routes / pages
  db/          → Database schema and queries
tests/         → Test files
docs/          → Documentation
\`\`\`

## Before You Code

1. Read `docs/ARCHITECTURE.md` if it exists
2. Run `pnpm install` to set up dependencies
3. Run `pnpm test` to verify everything works

## Commands

\`\`\`bash
pnpm dev          # Start dev server
pnpm test         # Run tests
pnpm build        # Build for production
pnpm lint         # Run linter
\`\`\`

## Key Files

| File | Purpose |
|------|---------|
| `src/index.ts` | Application entry point |
| `src/db/schema.ts` | Database schema |
| `tsconfig.json` | TypeScript configuration |

## When Stuck

1. Run `pnpm test` — does it pass?
2. Check for type errors: `pnpm typecheck`
3. Read related test files for examples
```

### 4. Verify

```bash
# Check file exists and has content
wc -l AGENTS.md
cat AGENTS.md
```

## Success Criteria

- [ ] AGENTS.md exists at repo root
- [ ] Contains project description
- [ ] Lists key commands
- [ ] Maps repository structure
- [ ] Points to key files
