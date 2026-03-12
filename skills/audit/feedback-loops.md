# Audit: Feedback Loops

Check that the codebase has fast, reliable feedback mechanisms.

## What to Check

### CI/CD Pipeline
```bash
# Look for CI configuration
ls -la .github/workflows/ .gitlab-ci.yml .circleci/ Jenkinsfile 2>/dev/null

# Check if CI runs on PRs
grep -r "pull_request" .github/workflows/ 2>/dev/null
```

**Score:**
- CI exists and runs on PRs: +5
- CI runs tests: +5
- CI runs linting: +3
- CI runs type checking: +2

### Test Suite
```bash
# Find test files
find . -name "*.test.*" -o -name "*.spec.*" -o -name "test_*.py" | head -20

# Check for test command in package.json / pyproject.toml
grep -E "\"test\":|pytest|jest|vitest|mocha" package.json pyproject.toml 2>/dev/null
```

**Score:**
- Tests exist: +5
- Test command documented: +2
- Coverage reporting: +3

### Pre-commit Hooks
```bash
# Check for husky, pre-commit, or similar
ls -la .husky/ .pre-commit-config.yaml 2>/dev/null
cat package.json | grep -A5 "husky\|lint-staged" 2>/dev/null
```

**Score:**
- Pre-commit hooks exist: +3
- Runs formatting: +1
- Runs linting: +1

### Build/Test Time
```bash
# Time the test suite
time npm test 2>&1 | tail -5
# or
time pytest 2>&1 | tail -5
```

**Score:**
- Tests < 1 min: +5
- Tests < 5 min: +3
- Tests < 10 min: +1
- Tests > 10 min: 0

## Output

```yaml
category: feedback-loops
score: X/20
grade: A|B|C|D
findings:
  - ci_exists: true|false
  - ci_runs_tests: true|false
  - ci_runs_lint: true|false
  - tests_exist: true|false
  - test_time_seconds: N
  - precommit_hooks: true|false
issues:
  - "CI exists but doesn't run tests"
  - "No pre-commit hooks configured"
recommendations:
  - "Add test step to CI workflow"
  - "See fix/typescript/add-precommit.md"
```
