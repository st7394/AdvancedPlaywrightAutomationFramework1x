# GEMINI.md — Project Instructions for Gemini CLI

## Project
Advanced Playwright + TypeScript end-to-end test automation framework.
Target application: https://app.thetestingacademy.com

## Quality Gates — Run After Every Change

```bash
npm run typecheck                        # TypeScript — zero errors required
npm run lint                             # ESLint — zero errors required
npm run format:check                     # Prettier — all files must be formatted
npx playwright test --grep @smoke        # Smoke suite — must stay green
```

Never complete a task if any gate fails. Fix the error first.

## Project Structure

```
src/
├── pages/       # Page Object classes — one per page, extend BasePage
├── fixtures/    # Custom fixtures — always import test from here
├── tests/       # Spec files — tagged with @smoke / @regression / @P0 etc.
├── testdata/    # JSON · CSV · XLSX · TypeScript type definitions
├── utils/       # logger · CustomTTAReporter · FileReader · DataFactory
└── config/      # Environment → base URL resolution
```

## Key Coding Patterns

### Page Object (always arrow-function locators)
```typescript
export class LoginPage extends BasePage {
  usernameInput = () => this.page.locator('#username');
  passwordInput = () => this.page.locator('#password');

  async login(user: string, pass: string) {
    await this.usernameInput().fill(user);
    await this.passwordInput().fill(pass);
  }
}
```

### Test (always tagged, always from fixture)
```typescript
import { test, expect } from '@fixtures/test-base';

test('@smoke @P0 Login succeeds with valid credentials', async ({ loginPage }) => {
  await loginPage.login('standard_user', 'secret_sauce');
  await loginPage.verifyDashboardVisible();
});
```

### Logger
```typescript
import { createLogger } from '@utils/logger';
const log = createLogger('CartPage');
log.info('Adding item to cart');
log.error('Cart badge not found', error);
```

## Path Aliases
`@pages/*` · `@fixtures/*` · `@utils/*` · `@testdata/*` · `@config/*`

## Rules
- No `as any`, no `// @ts-ignore`, no inline `eslint-disable` without reason
- Every test must have at least one tag
- All new files go in the correct `src/` subfolder
- All quality gates must pass before committing
