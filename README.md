# Complexity Divider

A Claude Code skill for using complexity metrics as a feedback loop mechanism to achieve 300-700% development velocity gains.

> Based on the [Compound Engineering](https://www.vincirufus.com/posts/compound-engineering/) methodology.

## Why?

When AI generates code nearly instantaneously, **feedback mechanisms become the true bottleneck**. High complexity is a leading indicator of bugs, slow tests, and maintenance burden.

```
High Complexity → Hard to Test → Slow Feedback → Reduced Velocity
        ↓ (divide)
Low Complexity → Easy to Test → Fast Feedback → Compound Gains
```

## Installation

### 1. Copy skill files to your project

```bash
# Clone this repo (or download files directly)
git clone https://github.com/vnakhate/complexity-divider.git

# Copy skills to your project
cp -r complexity-divider/.claude/skills/* /path/to/your/project/.claude/skills/
```

Or manually:

```bash
mkdir -p your-project/.claude/skills
curl -o your-project/.claude/skills/complexity-divider.md \
  https://raw.githubusercontent.com/vnakhate/complexity-divider/main/.claude/skills/complexity-divider.md
```

### 2. Add ESLint rules

```javascript
// .eslintrc.cjs or eslint.config.js
rules: {
  'complexity': ['error', { max: 15 }],
  'max-depth': ['error', 4],
  'max-nested-callbacks': ['error', 3],
  'max-lines-per-function': ['warn', { max: 100, skipComments: true }],
  'max-params': ['warn', 5],
}
```

### 3. Add verify scripts

```json
{
  "scripts": {
    "verify": "npm run lint && npm run test:run && tsc --noEmit",
    "verify:quick": "vitest run --changed",
    "verify:full": "npm run verify && npx playwright test"
  }
}
```

### 4. Remove CI bypasses

Remove any `|| true` from lint/test commands in your CI workflows.

## Included Skills

| File | Purpose |
|------|---------|
| `complexity-divider.md` | Main skill - comprehensive complexity management |
| `work-with-complexity.md` | Work protocol - applied during implementation |

## Thresholds (2026)

| Metric | Green | Yellow | Red |
|--------|-------|--------|-----|
| Cyclomatic (per function) | 1-8 | 9-15 | >15 |
| Cognitive (per function) | 1-10 | 11-20 | >20 |
| Lines per function | 1-50 | 51-100 | >100 |
| File total complexity | 1-40 | 41-75 | >75 |
| Complexity delta (PR) | -any | +0-15% | +>15% |

## Quick Reference

### Commands

```bash
npm run lint              # Check complexity violations
npm run verify            # Full verification (lint + test + typecheck)
npm run verify:quick      # Only changed files
npm run verify:full       # Includes E2E tests
```

### When to Divide

- Any function with complexity > 15
- Any file with total complexity > 75
- Any PR that increases complexity > 15%

### Dividing Strategies

1. **Extract Function** - Break large functions into smaller, focused ones
2. **Early Returns** - Replace nested ifs with guard clauses
3. **Replace Switch with Map** - Use lookup tables instead of switch statements
4. **Named Boolean Conditions** - Extract complex conditions into named variables

## Multi-Language Support

The skill includes setup for:

- **JavaScript/TypeScript** - ESLint
- **Python** - Ruff/Flake8
- **Ruby** - Rubocop
- **Go** - gocyclo

See `complexity-divider.md` for detailed configuration per language.

## License

MIT License - See [LICENSE](LICENSE) file.

## Attribution

Based on the compound engineering methodology from [vincirufus.com/posts/compound-engineering](https://www.vincirufus.com/posts/compound-engineering/).
