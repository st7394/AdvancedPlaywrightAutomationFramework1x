# CLAUDE.md — Project Instructions for Claude Code

## Project Overview

This is an advanced Playwright + TypeScript end-to-end test automation framework targeting the TTA practice application.

## Quality Gates — Run After Every Change

After making any code change, always run these checks in order:

```bash
npm run typecheck       # tsc --noEmit — zero errors required
npm run lint            # ESLint — zero errors required
npm run format:check    # Prettier — all files must be formatted
npx playwright test --grep @smoke  # smoke suite must stay green
```

Never report a task as done if any of these fail. Fix the failure first.

## Project Structure

```
src/
├── pages/       # Page Object classes — one file per page
├── fixtures/    # Custom Playwright fixtures (test-base.ts)
├── tests/       # Spec files — organized by feature
├── testdata/    # JSON · CSV · XLSX · TypeScript interfaces
├── utils/       # Logger · FileReader · DataFactory · Reporters
└── config/      # Environment config (envs.ts)
```

## Coding Rules

- **Locators** must use arrow functions: `loginBtn = () => this.page.locator('#login')`
- **Never use** `as any` or `// @ts-ignore` without a written reason in a comment
- **Never disable** ESLint rules inline without a comment explaining why
- **Every test** must carry at least one tag: `@smoke`, `@regression`, `@e2e`, `@P0`, `@P1`, or `@P2`
- **New files** go in the correct `src/` subfolder — never in the root
- **Imports** use path aliases: `@pages/`, `@utils/`, `@fixtures/`, `@testdata/`, `@config/`
- **Logger** — use `import logger from '@utils/logger'` or `createLogger('ClassName')` for scoped logs
- **No comments** explaining what code does — only comments explaining WHY when non-obvious

## Key Files

| File | Purpose |
|---|---|
| `playwright.config.ts` | Browser projects, reporters, timeouts, env resolution |
| `tsconfig.json` | TypeScript config with strict mode and path aliases |
| `src/utils/logger.ts` | Winston logger — shared root + scoped child loggers |
| `src/utils/CustomTTAReporter.ts` | Branded HTML test reporter |
| `.github/workflows/playwright.yml` | CI pipeline — runs on push/PR to main |
| `rules/quality-gates.md` | Full quality gate definitions |

## Environment Setup

```bash
npm install
npx playwright install --with-deps
cp .env.example .env        # set TTA_ENV=qa
```

## Scripts

| Script | Command |
|---|---|
| `npm test` | Run full suite |
| `npm run typecheck` | TypeScript check |
| `npm run lint` | ESLint |
| `npm run format` | Prettier auto-fix |
| `npm run format:check` | Prettier check only |
| `npm run build` | Compile to dist/ |

## Commit Rules

- Run all quality gates before committing
- Use conventional commit format: `feat:`, `fix:`, `refactor:`, `chore:`, `docs:`, `ci:`
- Never use `git push --force` on main
- Never skip pre-commit hooks with `--no-verify`
