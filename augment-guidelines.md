# Augment Code Guidelines — Advanced Playwright Framework

## Project Summary
Advanced Playwright + TypeScript end-to-end test automation framework.
Target: https://app.thetestingacademy.com

## Mandatory Quality Gates
Run after every change — all must pass before completing any task:

| Step | Command | Must Pass |
|---|---|---|
| 1. Type Check | `npm run typecheck` | Zero TS errors |
| 2. Lint | `npm run lint` | Zero ESLint errors |
| 3. Format | `npm run format:check` | All files formatted |
| 4. Smoke | `npx playwright test --grep @smoke` | All smoke tests green |

Run all at once:
```bash
npm run typecheck && npm run lint && npm run format:check && npx playwright test --grep @smoke
```

## Architecture

| Layer | Location | Rule |
|---|---|---|
| Page Objects | `src/pages/` | Extend `BasePage`, arrow-function locators |
| Fixtures | `src/fixtures/test-base.ts` | Always import `test` from here |
| Spec files | `src/tests/` | Tagged with `@smoke` / `@regression` / `@P0` etc. |
| Test data | `src/testdata/` | JSON · CSV · XLSX · types.ts |
| Utilities | `src/utils/` | logger · reporter · FileReader · DataFactory |
| Config | `src/config/` | Environment URL resolution |

## TypeScript Rules
- Strict mode is enabled — explicit types everywhere
- `noUnusedLocals` and `noUnusedParameters` are on — remove dead code
- Path aliases active: `@pages/` · `@fixtures/` · `@utils/` · `@testdata/` · `@config/`
- No `as any`, no `// @ts-ignore`

## Playwright Rules
- Locators must be arrow functions on the class (prevents stale elements)
- Every test must have at least one tag
- Import `test` and `expect` from `@fixtures/test-base`, not `@playwright/test`
- Use `createLogger('ClassName')` from `@utils/logger` for all log output

## Commit Rules
- Format: `feat:` · `fix:` · `refactor:` · `chore:` · `docs:` · `ci:` · `test:`
- All quality gates must pass before committing
- Never force-push to main
