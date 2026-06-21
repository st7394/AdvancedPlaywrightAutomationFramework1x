# Advanced Playwright Automation Framework

![Playwright](https://img.shields.io/badge/Playwright-1.61.0-2EAD33?style=for-the-badge&logo=playwright&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-6.0-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-LTS-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-CI%2FCD-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![License](https://img.shields.io/badge/License-ISC-blue?style=for-the-badge)

A production-grade, end-to-end test automation framework built with **Playwright** and **TypeScript**, following the **Page Object Model** architecture with custom fixtures, multi-format test data support, branded reporting, and fully integrated CI/CD via GitHub Actions.

> Target Application: [TTA Practice Pages](https://app.thetestingacademy.com/playwright/)

---

## Table of Contents

- [Architecture Overview](#architecture-overview)
- [Tech Stack](#tech-stack)
- [Folder Structure](#folder-structure)
- [Design Patterns](#design-patterns)
- [Test Data Management](#test-data-management)
- [Configuration & Environments](#configuration--environments)
- [Test Execution](#test-execution)
- [Reporting & Observability](#reporting--observability)
- [CI/CD Pipeline](#cicd-pipeline)
- [Getting Started](#getting-started)
- [Scripts Reference](#scripts-reference)
- [Code Quality Gates](#code-quality-gates)

---

## Architecture Overview

The framework is organized into **10 core pillars** split across two concerns — **Framework Code** and **Infrastructure & Config**.

```
┌─────────────────────────────────────────────────────────────────────┐
│                  ADVANCED PLAYWRIGHT FRAMEWORK                       │
├──────────────────────────────┬──────────────────────────────────────┤
│      FRAMEWORK CODE          │     INFRASTRUCTURE & CONFIG          │
├──────────────────────────────┼──────────────────────────────────────┤
│                              │                                      │
│  ┌────────────────────────┐  │  ┌──────────────────────────────┐   │
│  │  1. Page Objects       │  │  │  6. playwright.config.ts     │   │
│  │  src/pages/            │  │  │     + .env / envs.ts         │   │
│  └────────────────────────┘  │  └──────────────────────────────┘   │
│                              │                                      │
│  ┌────────────────────────┐  │  ┌──────────────────────────────┐   │
│  │  2. Fixtures + Specs   │  │  │  7. Test Execution + Tags    │   │
│  │  src/fixtures/         │  │  │     @smoke @e2e @regression  │   │
│  │  src/tests/            │  │  └──────────────────────────────┘   │
│  └────────────────────────┘  │                                      │
│                              │  ┌──────────────────────────────┐   │
│  ┌────────────────────────┐  │  │  8. Reports & Artifacts      │   │
│  │  3. Utils / Helpers    │  │  │     HTML / Allure / Custom   │   │
│  │  src/utils/            │  │  └──────────────────────────────┘   │
│  └────────────────────────┘  │                                      │
│                              │  ┌──────────────────────────────┐   │
│  ┌────────────────────────┐  │  │  9. CI/CD + Docker           │   │
│  │  4. Test Data          │  │  │     GitHub Actions / Shards  │   │
│  │  src/testdata/         │  │  └──────────────────────────────┘   │
│  └────────────────────────┘  │                                      │
│                              │  ┌──────────────────────────────┐   │
│  ┌────────────────────────┐  │  │  10. Quality Gates           │   │
│  │  5. Observability      │  │  │      ESLint + Prettier       │   │
│  │  logs/ + tta-report/   │  │  │      Husky + VCS             │   │
│  └────────────────────────┘  │  └──────────────────────────────┘   │
│                              │                                      │
└──────────────────────────────┴──────────────────────────────────────┘
```

### Data Flow

```
┌──────────┐     ┌──────────┐     ┌────────────┐     ┌──────────────┐
│ Test Data│────▶│ Fixtures │────▶│ Page Objs  │────▶│  Browser     │
│ JSON/CSV │     │ test-base│     │ POM Classes│     │  Chromium    │
│ XLSX/Fake│     │ .ts      │     │ (Actions + │     │  Firefox     │
└──────────┘     └──────────┘     │  Asserts)  │     │  WebKit      │
                                  └─────┬──────┘     └──────┬───────┘
                                        │                   │
                                        ▼                   ▼
                                  ┌──────────┐     ┌──────────────┐
                                  │  Utils   │     │  Artifacts   │
                                  │  Logger  │     │  Trace/Video │
                                  │  Locator │     │  Screenshot  │
                                  └──────────┘     └──────┬───────┘
                                                          │
                                                          ▼
                                                   ┌──────────────┐
                                                   │   Reports    │
                                                   │ HTML / Allure│
                                                   │ TTA Reporter │
                                                   └──────────────┘
```

---

## Tech Stack

| Layer | Tool | Version | Purpose |
|---|---|---|---|
| Test Runner | `@playwright/test` | ^1.61.0 | Core test execution engine |
| Language | `TypeScript` | ^6.0 | Strict typing, path aliases |
| Runtime | `Node.js` | LTS | JavaScript runtime |
| Script Runner | `ts-node` | ^10.9 | Run `.ts` files without pre-compile |
| Test Data | `@faker-js/faker` | ^10.5 | Dynamic fake data generation |
| Test Data | `csv-parse` | ^7.0 | Parse CSV product/data fixtures |
| Test Data | `exceljs` | ^4.4 | Read/write Excel test matrices |
| Environment | `dotenv` | ^17.4 | Load `.env` files per environment |
| Reporting | `allure-playwright` | ^3.10 | Allure HTML reporting |
| Linting | `eslint` | ^10.5 | Static code analysis |
| Linting | `eslint-plugin-playwright` | ^2.10 | Playwright-specific lint rules |
| Linting | `@typescript-eslint/*` | ^8.61 | TypeScript ESLint integration |
| Formatting | `prettier` | ^3.8 | Consistent code formatting |
| Cross-platform | `cross-env` | ^10.1 | Set env vars on Windows/Mac/Linux |
| CI/CD | GitHub Actions | — | Automated test execution + sharding |

---

## Folder Structure

```
advance-playwright-framework/
│
├── .env.example                        # Environment variable template
├── .github/
│   └── workflows/
│       └── playwright.yml              # CI/CD pipeline (sharded matrix)
│
├── package.json                        # Scripts + all devDependencies
├── playwright.config.ts                # Browser projects, reporters, retries
├── tsconfig.json                       # TypeScript with path aliases
│
├── src/
│   │
│   ├── config/
│   │   └── envs.ts                     # Base URL map (qa/stg/dev/prod)
│   │
│   ├── fixtures/
│   │   └── test-base.ts                # Custom fixtures extending Playwright
│   │
│   ├── pages/                          # One POM class per page
│   │   ├── BasePage.ts                 # Shared: navigate, waitForLoad
│   │   ├── LoginPage.ts
│   │   ├── InventoryPage.ts
│   │   ├── ItemDetailPage.ts
│   │   ├── CartPage.ts
│   │   ├── CheckoutStepOnePage.ts
│   │   ├── CheckoutStepTwoPage.ts
│   │   └── CheckoutCompletePage.ts
│   │
│   ├── testdata/
│   │   ├── users.json                  # Valid/invalid/locked user fixtures
│   │   ├── products.csv                # Product data for data-driven tests
│   │   ├── checkout.xlsx               # Multi-sheet checkout scenarios
│   │   └── types.ts                    # TypeScript interfaces for all data
│   │
│   ├── tests/                          # Spec files (tagged with @smoke etc.)
│   │   ├── login.spec.ts
│   │   ├── inventory.spec.ts
│   │   ├── cart.spec.ts
│   │   ├── checkout.spec.ts
│   │   ├── negative.spec.ts
│   │   └── data-driven.spec.ts
│   │
│   └── utils/
│       ├── UtilElementLocator.ts       # Flex locator wrapper (string | Locator)
│       ├── Logger.ts                   # Winston-based console + file logger
│       ├── DataFactory.ts              # Faker.js integration for dynamic data
│       ├── FileReader.ts               # JSON / CSV / XLSX parsing utilities
│       ├── DateUtil.ts                 # Timestamp & date format helpers
│       └── CustomTTAReporter.ts        # Branded HTML reporter implementation
│
├── logs/                               # Winston log output + HAR files
├── test-results/                       # Traces, videos, screenshots (on failure)
├── playwright-report/                  # Built-in Playwright HTML report
└── tta-report/                         # Custom branded TTA HTML report
```

---

## Design Patterns

### 1. Page Object Model (POM)

Each page is a class with three tiers of members:

```
Page Class
├── Locators   → Arrow functions returning fresh Locator on each call
├── Actions    → async methods: navigate(), login(), addToCart()
└── Assertions → async expect methods: verifyTitle(), verifyCartCount()
```

Arrow-function locators prevent stale-element issues:

```typescript
// src/pages/LoginPage.ts
export class LoginPage extends BasePage {
  // Locators — fresh DOM query on every call
  usernameInput = () => this.page.locator('#username');
  passwordInput = () => this.page.locator('#password');
  loginButton   = () => this.page.getByRole('button', { name: 'Login' });
  errorMessage  = () => this.page.locator('.error-message');

  // Actions
  async login(username: string, password: string) {
    await this.usernameInput().fill(username);
    await this.passwordInput().fill(password);
    await this.loginButton().click();
  }

  // Assertions
  async verifyLoginError(expectedMsg: string) {
    await expect(this.errorMessage()).toHaveText(expectedMsg);
  }
}
```

---

### 2. Fixtures Architecture

Custom fixtures inject page objects into tests — no manual `new` instantiation inside specs:

```
base.extend<TestFixtures>({
  loginPage    → new LoginPage(page)
  inventoryPage → new InventoryPage(page)
  cartPage     → new CartPage(page)
  ...
})
```

```typescript
// src/fixtures/test-base.ts
import { test as base } from '@playwright/test';
import { LoginPage } from '../pages/LoginPage';
import { CartPage }  from '../pages/CartPage';

type TestFixtures = {
  loginPage: LoginPage;
  cartPage:  CartPage;
};

export const test = base.extend<TestFixtures>({
  loginPage: async ({ page }, use) => { await use(new LoginPage(page)); },
  cartPage:  async ({ page }, use) => { await use(new CartPage(page)); },
});

export { expect } from '@playwright/test';
```

Usage in a spec:

```typescript
// src/tests/cart.spec.ts
import { test, expect } from '../fixtures/test-base';

test('@smoke Add item to cart', async ({ loginPage, cartPage }) => {
  await loginPage.login('standard_user', 'secret_sauce');
  await cartPage.addItem('Backpack');
  await cartPage.verifyCartCount(1);
});
```

---

### 3. UtilElementLocator — Flex Locator Wrapper

Accepts `string | Locator`, centralizing timeouts and common actions:

```
UtilElementLocator(page, selector)
├── click()
├── fill(value)
├── type(value)
├── clear()
├── selectByValue(value)
├── getText() → string
├── getValue() → string
├── getAttr(name) → string
├── waitForVisible()
└── waitForPageLoad()
```

---

### 4. Module Pattern for Multi-Page Flows

Business-level flows combine multiple page objects into a single reusable module:

```
LoginModule
└── uses LoginPage + BasePage
└── encapsulates: navigate → fill credentials → submit → verify dashboard
```

---

## Test Data Management

### Data Sources

```
┌─────────────────────────────────────────────────────────┐
│                   TEST DATA SOURCES                      │
├──────────────┬──────────────────────────────────────────┤
│  users.json  │  validUsers[], invalidUsers[],            │
│              │  lockedUser, newUserTemplate              │
├──────────────┼──────────────────────────────────────────┤
│ products.csv │  Large product datasets, bulk scenarios   │
├──────────────┼──────────────────────────────────────────┤
│checkout.xlsx │  Multi-sheet checkout test matrices       │
├──────────────┼──────────────────────────────────────────┤
│  Faker.js    │  Runtime-generated users, emails, cards   │
└──────────────┴──────────────────────────────────────────┘
```

### Test Users

| User | Scenario |
|---|---|
| `standard_user` | Normal full checkout flow |
| `locked_out_user` | Account locked error validation |
| `problem_user` | Broken UI / element quirks |
| `performance_glitch_user` | Slow interactions & timeouts |
| `error_user` | Intermittent failures |
| `visual_user` | Visual regression baseline |

### TypeScript Interfaces (`src/testdata/types.ts`)

```typescript
export interface User {
  username: string;
  password: string;
  role:     string;
}

export interface Product {
  id:    string;
  name:  string;
  price: number;
}

export interface CheckoutData {
  firstName:  string;
  lastName:   string;
  postalCode: string;
}
```

---

## Configuration & Environments

Environment switching via `TTA_ENV` or `NODE_ENV`:

```
┌───────────┬──────────────────────────────────────────────┐
│ TTA_ENV   │ BASE_URL                                      │
├───────────┼──────────────────────────────────────────────┤
│ qa        │ https://app.thetestingacademy.com             │
│ stg       │ https://staging.thetestingacademy.com         │
│ dev       │ http://localhost:8082                         │
│ prod      │ https://app.thetestingacademy.com             │
└───────────┴──────────────────────────────────────────────┘
```

Create a `.env` file from the template:

```bash
cp .env.example .env
```

`.env.example`:

```
TTA_ENV=qa
BASE_URL=https://app.thetestingacademy.com
```

### playwright.config.ts — Key Settings

```
fullyParallel   → true  (local)
retries         → 2     (CI only)
workers         → 1     (CI) / auto (local)
reporter        → html + allure + custom TTA
trace           → on-first-retry
browsers        → Chromium, Firefox, WebKit
```

---

## Test Execution

### Run Commands

```bash
# Full test suite
npx playwright test

# Headed mode (watch the browser)
npx playwright test --headed

# Interactive UI mode
npx playwright test --ui

# Debug mode (step through tests)
npx playwright test --debug

# Filter by tag
npx playwright test --grep @smoke
npx playwright test --grep @regression
npx playwright test --grep @e2e

# Filter by browser project
npx playwright test --project=chromium
npx playwright test --project=firefox
npx playwright test --project=webkit

# Sharded execution (for CI parallelism)
npx playwright test --shard=1/4
npx playwright test --shard=2/4
npx playwright test --shard=3/4
npx playwright test --shard=4/4

# Single spec file
npx playwright test src/tests/login.spec.ts
```

### Test Tags

```
@smoke       → Happy-path core flows (fastest feedback)
@regression  → Comprehensive edge cases
@e2e         → Full user journeys end-to-end
@visual      → Screenshot / visual regression
@flaky       → Quarantined unstable tests
@P0          → Critical priority
@P1          → High priority
@P2          → Medium priority
```

### End-to-End Checkout Flow

```
Login (standard_user)
    │
    ▼
Browse Inventory
    │
    ▼
Add to Cart (Backpack + Bike Light)
    │
    ▼
Verify Cart Badge Count
    │
    ▼
Proceed to Checkout
    │
    ▼
Fill Details (firstName, lastName, postalCode)
    │
    ▼
Review Order (subtotal + tax + total)
    │
    ▼
Finish Purchase
    │
    ▼
Assert Thank You Page ✓
```

---

## Reporting & Observability

### Report Stack

```
┌─────────────────────────────────────────────────────────┐
│                   REPORT STACK                           │
├────────────────────┬────────────────────────────────────┤
│ playwright-report/ │ Built-in Playwright HTML report     │
├────────────────────┼────────────────────────────────────┤
│ allure-report/     │ Allure rich HTML (timeline + steps) │
├────────────────────┼────────────────────────────────────┤
│ tta-report/        │ Custom branded TTA HTML reporter    │
└────────────────────┴────────────────────────────────────┘
```

### Artifacts Collected on Failure

```
test-results/
├── trace.zip            ← Step-by-step timeline + DOM snapshots
├── screenshot-*.png     ← Full-page failure screenshot
├── video.webm           ← Full test video recording
├── console.txt          ← Browser console log output
└── network.har          ← Complete network request/response archive
```

### View Reports

```bash
# Playwright HTML report
npx playwright show-report

# Allure report
allure generate allure-results --clean -o allure-report
allure open allure-report

# Custom TTA report
open tta-report/index.html
```

---

## CI/CD Pipeline

### GitHub Actions Workflow

```
Trigger: push to main | PR to main
         └── manual dispatch (optional tag filter)
                │
                ▼
┌───────────────────────────────────────────┐
│           Matrix Strategy (4 shards)      │
├──────────┬──────────┬──────────┬──────────┤
│ Shard 1/4│ Shard 2/4│ Shard 3/4│ Shard 4/4│
│ ~25% tests│~25% tests│~25% tests│~25% tests│
└────┬─────┴────┬─────┴────┬─────┴────┬─────┘
     │          │          │          │
     └──────────┴────┬─────┴──────────┘
                     ▼
             merge-reports job
                     │
                     ▼
          Publish to GitHub Pages
```

### Pipeline Steps

```yaml
1. actions/checkout@v4
2. actions/setup-node@v4  (Node LTS)
3. npm ci
4. npx playwright install --with-deps
5. npx playwright test --shard=${{ matrix.shard }}
6. actions/upload-artifact@v4  (playwright-report, retention: 30 days)
```

### Environment Variables (CI)

```
CI=true
NODE_VERSION=20
TTA_ENV=qa
```

---

## Getting Started

### Prerequisites

- Node.js LTS (v20+)
- Git

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/st7394/AdvancedPlaywrightAutomationFramework1x.git
cd AdvancedPlaywrightAutomationFramework1x

# 2. Install all dependencies
npm install

# 3. Install Playwright browsers
npx playwright install --with-deps

# 4. Set up environment
cp .env.example .env
```

### Run Your First Test

```bash
# Run all tests
npx playwright test

# Run smoke suite only
npx playwright test --grep @smoke

# Open interactive UI mode
npx playwright test --ui
```

---

## Scripts Reference

| Script | Command | Description |
|---|---|---|
| `test` | `playwright test` | Run full test suite |
| `test:headed` | `playwright test --headed` | Run with visible browser |
| `test:ui` | `playwright test --ui` | Interactive UI mode |
| `test:debug` | `playwright test --debug` | Step-through debug mode |
| `test:smoke` | `playwright test --grep @smoke` | Smoke suite only |
| `test:regression` | `playwright test --grep @regression` | Regression suite |
| `test:report` | `playwright show-report` | Open HTML report |
| `lint` | `eslint src/**/*.ts` | Run ESLint |
| `lint:fix` | `eslint src/**/*.ts --fix` | Auto-fix lint errors |
| `format` | `prettier --write "src/**/*.ts"` | Format all TS files |
| `build` | `tsc` | Compile TypeScript |

---

## Code Quality Gates

```
Pre-commit (Husky)
├── ESLint          → ban absolute XPath, ban nth-child selectors
├── Prettier        → enforce consistent formatting
├── tsc --noEmit    → TypeScript compile check (no emit)
└── commitlint      → enforce conventional commit messages

CI Gate
├── forbidOnly      → fail build if test.only left in code
├── retries: 2      → retry on CI to catch flakiness
└── workers: 1      → sequential on CI for stability
```

### Commit Convention

```
feat:     new test or feature
fix:      bug fix in tests or framework
refactor: code restructure without behavior change
chore:    dependency updates, config changes
docs:     README or documentation updates
ci:       GitHub Actions workflow changes
```

---

## Browser Coverage

```
┌──────────────────────────────────────────────┐
│              BROWSER MATRIX                   │
├─────────────┬──────────────┬─────────────────┤
│  Chromium   │   Firefox    │    WebKit        │
│  (Chrome)   │   (Mozilla)  │    (Safari)      │
│  Desktop    │   Desktop    │    Desktop       │
├─────────────┴──────────────┴─────────────────┤
│  Coming soon: Mobile Chrome (Pixel 5)         │
│              Mobile Safari (iPhone 12)        │
│              Microsoft Edge                   │
└──────────────────────────────────────────────┘
```

---

## Author

**Saumitra Tripathi**
GitHub: [@st7394](https://github.com/st7394)

---

## License

ISC — open source, free for commercial use with attribution.
