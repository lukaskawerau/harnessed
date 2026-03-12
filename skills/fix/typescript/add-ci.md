# Fix: Add CI Pipeline (TypeScript)

Add a GitHub Actions CI workflow for TypeScript/Node projects.

## When to Use

- Audit found: "No CI pipeline"
- Project uses TypeScript/Node and GitHub

## Steps

### 1. Detect Project Setup

```bash
# Check package manager
ls -la pnpm-lock.yaml package-lock.json yarn.lock bun.lockb 2>/dev/null

# Check for existing scripts
cat package.json | jq '.scripts | keys'

# Check TypeScript config
ls -la tsconfig.json
```

### 2. Create Workflow Directory

```bash
mkdir -p .github/workflows
```

### 3. Generate CI Workflow

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: pnpm/action-setup@v4  # Remove if not using pnpm
        with:
          version: 9

      - uses: actions/setup-node@v4
        with:
          node-version: "22"
          cache: "pnpm"  # Change to npm/yarn if needed

      - name: Install dependencies
        run: pnpm install --frozen-lockfile

      - name: Lint
        run: pnpm lint
        continue-on-error: true  # Remove once linting passes

      - name: Type check
        run: pnpm typecheck || pnpm tsc --noEmit

      - name: Test
        run: pnpm test
```

### 4. Adapt to Project

**If using npm:**
- Remove pnpm/action-setup step
- Change cache to "npm"
- Change commands to `npm ci`, `npm run lint`, etc.

**If using yarn:**
- Remove pnpm/action-setup step
- Change cache to "yarn"
- Change commands to `yarn install --frozen-lockfile`, etc.

**If scripts don't exist:**
Add to package.json:
```json
{
  "scripts": {
    "lint": "eslint . || oxlint .",
    "typecheck": "tsc --noEmit",
    "test": "vitest run || jest"
  }
}
```

### 5. Verify

```bash
# Check workflow syntax
cat .github/workflows/ci.yml

# Commit and push to trigger
git add .github/
git commit -m "ci: add GitHub Actions workflow"
```

## Success Criteria

- [ ] `.github/workflows/ci.yml` exists
- [ ] Workflow runs on push and PR
- [ ] Runs lint, typecheck, and test steps
- [ ] First CI run completes (even if some steps fail)
