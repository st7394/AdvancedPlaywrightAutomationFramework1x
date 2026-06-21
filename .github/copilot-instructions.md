# GitHub Copilot Instructions

## Project

Advanced Playwright + TypeScript test automation framework. Target app: https://app.thetestingacademy.com

## Quality Gates — Always Run After Any Change

```bash
npm run typecheck                          # Must pass — zero TS errors
npm run lint                               # Must pass — zero ESLint errors
npm run format:check                       # Must pass — run npm run format to fix
npx playwright test --grep @smoke          # Must stay green
```

## Code Patterns to Follow

### Page Object Locators — arrow functions only
```typescript
// CORRECT
loginButton = () => this.page.getByRole('button', { name: 'Login' });

// WRONG — stale element risk
loginButton = this.page.getByRole('button', { name: 'Login' });
```

### Fixtures — always use custom test base
```typescript
import { test, expect } from '@fixtures/test-base';
```

### Logger — use scoped logger in classes
```typescript
import { createLogger } from '@utils/logger';
const logger = createLogger('LoginPage');
logger.info('Navigating to login');
```

### Test Tags — required on every test
```typescript
test('@smoke @P0 Login with valid credentials', async ({ loginPage }) => { ... });
```

## Path Aliases

| Alias | Resolves to |
|---|---|
| `@pages/*` | `src/pages/*` |
| `@fixtures/*` | `src/fixtures/*` |
| `@utils/*` | `src/utils/*` |
| `@testdata/*` | `src/testdata/*` |
| `@config/*` | `src/config/*` |

## What NOT to Do

- Do not use `as any` or `// @ts-ignore`
- Do not write locators as class properties (stale element risk)
- Do not create test files outside `src/tests/`
- Do not create page objects outside `src/pages/`
- Do not commit if typecheck, lint, or format:check fails
- Do not write tests without tags
