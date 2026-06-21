# Advanced Playwright Automation Framework

![Playwright](https://img.shields.io/badge/Playwright-1.61.0-2EAD33?style=flat-square&logo=playwright&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-6.0-3178C6?style=flat-square&logo=typescript&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/CI-GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![License](https://img.shields.io/badge/License-ISC-blue?style=flat-square)

A production-ready end-to-end test automation framework built with **Playwright** and **TypeScript**.
Tests run against the [TTA Practice Application](https://app.thetestingacademy.com/playwright/).

---

## What's Inside

| Layer | Tools |
|---|---|
| Test Runner | Playwright Test |
| Language | TypeScript + ts-node |
| Design Pattern | Page Object Model + Custom Fixtures |
| Test Data | Faker.js · csv-parse · ExcelJS · dotenv |
| Reporting | Allure · Playwright HTML Report |
| Code Quality | ESLint · Prettier · eslint-plugin-playwright |
| CI/CD | GitHub Actions (sharded, 4-way parallel) |

---

## Project Structure

```
src/
├── pages/        # Page Object classes (one per page)
├── fixtures/     # Custom Playwright fixtures
├── tests/        # Spec files
├── testdata/     # JSON · CSV · XLSX · TypeScript interfaces
└── utils/        # Logger · FileReader · DataFactory · Locator helpers
```

---

## Getting Started

```bash
# Install dependencies
npm install

# Install browsers
npx playwright install --with-deps

# Copy env file and set your environment
cp .env.example .env
```

---

## Running Tests

```bash
npm test                                      # Full suite
npx playwright test --headed                  # Watch the browser
npx playwright test --ui                      # Interactive UI mode
npx playwright test --grep @smoke             # Smoke tests only
npx playwright test --grep @regression        # Regression suite
npx playwright test --project=chromium        # Single browser
npx playwright test --debug                   # Step-through debug
```

---

## Test Tags

| Tag | Purpose |
|---|---|
| `@smoke` | Core happy-path flows |
| `@regression` | Edge cases and negative scenarios |
| `@e2e` | Full end-to-end user journeys |
| `@visual` | Screenshot / visual regression |
| `@P0 / @P1 / @P2` | Priority levels |

---

## Environments

Set `TTA_ENV` in your `.env` file to switch environments:

| `TTA_ENV` | URL |
|---|---|
| `qa` | https://app.thetestingacademy.com |
| `stg` | https://staging.thetestingacademy.com |
| `dev` | http://localhost:8082 |

---

## Reports

```bash
npx playwright show-report          # Playwright HTML report
allure open allure-report           # Allure report
```

Failure artifacts (traces, screenshots, videos) are saved to `test-results/`.

---

## CI/CD

Tests run automatically on every push and pull request via GitHub Actions.
The suite splits into **4 parallel shards** to cut execution time by ~75%.

---

## Author

**Saumitra Tripathi** · [@st7394](https://github.com/st7394)
