<div align="center">

# 🎭 Advanced Playwright Automation Framework

### Production-grade end-to-end test automation with TypeScript, POM, and CI/CD

<br/>

[![Playwright](https://img.shields.io/badge/Playwright-1.61.0-2EAD33?style=for-the-badge&logo=playwright&logoColor=white)](https://playwright.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-6.0-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Node.js](https://img.shields.io/badge/Node.js-LTS-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org)
[![GitHub Actions](https://img.shields.io/badge/CI%2FCD-GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)](https://github.com/features/actions)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com)
[![License](https://img.shields.io/badge/License-ISC-6366F1?style=for-the-badge)](LICENSE)

<br/>

> Built by **[Saumitra Tripathi](https://github.com/st7394)** · Target: [TTA Practice App](https://app.thetestingacademy.com/playwright/)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Running Tests](#-running-tests)
- [Test Tags & Priority](#-test-tags--priority)
- [Environments](#-environments)
- [Reporting](#-reporting)
- [Quality Gates](#-quality-gates)
- [CI/CD Pipeline](#-cicd-pipeline)
- [Docker](#-docker)
- [AI Assistant Support](#-ai-assistant-support)

---

## 🧭 Overview

This is a **production-grade** test automation framework designed for scale. It combines the best practices from enterprise automation — Page Object Model, custom fixtures, multi-environment config, branded reporting, sharded CI execution, and strict quality gates enforced across every AI coding assistant used in the project.

```
UI Tests (Playwright)  +  API Tests  +  Data-Driven Tests
           │
    TypeScript + Strict Mode
           │
    Page Object Model + Fixtures
           │
    Custom TTA Reporter + Allure + HTML
           │
    GitHub Actions (4 shards, ~75% faster CI)
```

---

## 🛠 Tech Stack

| Category | Tool | Version | Purpose |
|---|---|---|---|
| **Test Runner** | Playwright Test | ^1.61.0 | Core execution engine |
| **Language** | TypeScript | ^6.0 | Strict typing, path aliases |
| **Runtime** | ts-node | ^10.9 | Run `.ts` without pre-compile |
| **Test Data** | Faker.js | ^8.4 | Dynamic fake data generation |
| **Test Data** | csv-parse | ^6.2 | CSV fixture parsing |
| **Test Data** | ExcelJS | ^4.4 | Excel test matrix read/write |
| **Environment** | dotenv | ^17.4 | `.env` file loading |
| **Logging** | Winston | ^3.19 | Structured console + file logs |
| **Reporting** | Allure Playwright | ^3.9 | Rich HTML test reports |
| **Linting** | ESLint + plugins | ^9.39 | Static analysis |
| **Formatting** | Prettier | ^3.8 | Consistent code style |
| **CI/CD** | GitHub Actions | — | Sharded parallel execution |
| **Container** | Docker | — | Consistent cross-env runs |

---

## 📁 Project Structure

```
📦 advance-playwright-framework
│
├── 📂 src/
│   ├── 📂 api/              # API test helpers & request clients
│   ├── 📂 config/           # Environment URL resolution (qa/stg/dev/prod)
│   ├── 📂 fixtures/         # Custom Playwright fixtures (test-base.ts)
│   ├── 📂 pages/            # Page Object classes — one file per page
│   ├── 📂 testdata/         # JSON · CSV · XLSX · TypeScript interfaces
│   ├── 📂 tests/            # Spec files organized by feature
│   └── 📂 utils/
│       ├── 📄 logger.ts              # Winston logger (shared + scoped)
│       └── 📄 CustomTTAReporter.ts  # Branded HTML reporter
│
├── 📂 test-results/         # Traces · Screenshots · Videos (on failure)
├── 📂 tta-report/           # Custom branded HTML report output
├── 📂 rules/                # Quality gate definitions (source of truth)
├── 📂 docs/                 # Project documentation
│
├── 📄 playwright.config.ts  # Browser projects, reporters, timeouts
├── 📄 tsconfig.json         # TypeScript config + path aliases
├── 📄 Dockerfile            # Container config for consistent runs
├── 📄 package.json          # Scripts + all devDependencies
├── 📄 CLAUDE.md             # Claude Code instructions
├── 📄 GEMINI.md             # Gemini CLI instructions
└── 📂 .github/
    ├── 📄 copilot-instructions.md   # GitHub Copilot instructions
    └── 📂 workflows/
        └── 📄 playwright.yml        # CI pipeline (sharded)
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js LTS (v20+)
- Git

### Setup

```bash
# 1. Clone the repo
git clone https://github.com/st7394/AdvancedPlaywrightAutomationFramework1x.git
cd AdvancedPlaywrightAutomationFramework1x

# 2. Install all dependencies
npm install

# 3. Install Playwright browsers
npx playwright install --with-deps

# 4. Set up environment
cp .env.example .env
# Edit .env → set TTA_ENV=qa
```

---

## ▶️ Running Tests

```bash
# Run everything
npm test

# Headed mode — watch the browser
npm run test:headed

# Interactive Playwright UI
npm run test:ui

# Debug mode — step through tests
npm run test:debug

# Filter by tag
npx playwright test --grep @smoke
npx playwright test --grep @regression
npx playwright test --grep @e2e

# Filter by browser
npm run test:chromium
npm run test:firefox
npm run test:webkit

# Sharded (CI-style, 4 workers)
npx playwright test --shard=1/4
npx playwright test --shard=2/4
npx playwright test --shard=3/4
npx playwright test --shard=4/4
```

---

## 🏷 Test Tags & Priority

Every test must carry **at least one** of these tags:

| Tag | Purpose |
|---|---|
| `@smoke` | Core happy-path — fastest feedback loop |
| `@regression` | Edge cases, negative scenarios |
| `@e2e` | Full end-to-end user journeys |
| `@visual` | Screenshot / visual regression |
| `@P0` | Critical — must never fail |
| `@P1` | High priority |
| `@P2` | Medium priority |

```typescript
// Example
test('@smoke @P0 Login succeeds with valid credentials', async ({ loginPage }) => {
  await loginPage.login('standard_user', 'secret_sauce');
  await loginPage.verifyDashboardVisible();
});
```

---

## 🌍 Environments

Set `TTA_ENV` in your `.env` file — the framework resolves the correct base URL automatically:

| `TTA_ENV` | Base URL |
|---|---|
| `qa` *(default)* | `https://app.thetestingacademy.com` |
| `stg` | `https://staging.thetestingacademy.com` |
| `dev` / `local` | `http://localhost:3000` |
| `prod` | `https://app.thetestingacademy.com` |
| `api` | `https://restful-booker.herokuapp.com` |

Override any URL directly via env var (e.g. `BASE_URL=https://custom.host`).

---

## 📊 Reporting

Three reports generated on every run:

| Report | Location | Command to open |
|---|---|---|
| Playwright HTML | `playwright-report/` | `npm run test:report` |
| Allure | `allure-report/` | `npm run test:allure` |
| Custom TTA | `tta-report/index.html` | Open in browser |

### Failure Artifacts

When a test fails, these are automatically captured to `test-results/`:

```
test-results/
├── trace.zip          ← Step-by-step DOM timeline
├── screenshot-*.png   ← Full-page failure screenshot
├── video.webm         ← Test video recording
```

### Logging

```typescript
import { createLogger } from '@utils/logger';

const log = createLogger('CheckoutPage');
log.info('Filling shipping details');
log.warn('Slow response detected');
log.error('Payment failed', error);
```

Logs output to console (coloured) and `logs/combined.log` (plain text for CI artifacts).

---

## ✅ Quality Gates

Every code change must pass all four gates before committing:

```bash
npm run typecheck                         # 1. Zero TypeScript errors
npm run lint                              # 2. Zero ESLint errors
npm run format:check                      # 3. All files formatted
npx playwright test --grep @smoke         # 4. Smoke suite green
```

Run all at once:

```bash
npm run typecheck && npm run lint && npm run format:check && npx playwright test --grep @smoke
```

Full gate definitions live in [`rules/quality-gates.md`](rules/quality-gates.md).

---

## ⚙️ CI/CD Pipeline

Tests run automatically on every **push** and **pull request** to `main`.

```
Push / PR to main
       │
       ▼
┌──────────────────────────────────────┐
│     GitHub Actions Matrix (4 shards) │
├──────────┬──────────┬────────┬───────┤
│ Shard 1/4│ Shard 2/4│ 3/4   │ 4/4   │
│ ~25%     │ ~25%     │ ~25%  │ ~25%  │
└────┬─────┴────┬─────┴───┬───┴───┬───┘
     └──────────┴─────────┴───────┘
                    │
              Merge reports
                    │
           Publish artifacts
           (30-day retention)
```

Each shard installs browsers, runs its slice, and uploads an HTML report artifact.

---

## 🐳 Docker

Run tests in an isolated, reproducible container:

```bash
# Build the image
docker build -t tta-playwright .

# Run full suite
docker run --rm -v $(pwd)/test-results:/app/test-results tta-playwright

# Run a single shard
docker run --rm tta-playwright npx playwright test --shard=1/4
```

---

## 🤖 AI Assistant Support

This project ships rules for **9 AI coding assistants** — all enforcing the same quality gates:

| Tool | Rule File |
|---|---|
| Claude Code | [`CLAUDE.md`](CLAUDE.md) |
| GitHub Copilot | [`.github/copilot-instructions.md`](.github/copilot-instructions.md) |
| Cursor | [`.cursorrules`](.cursorrules) |
| Windsurf | [`.windsurfrules`](.windsurfrules) |
| Cline | [`.clinerules`](.clinerules) |
| Augment Code | [`augment-guidelines.md`](augment-guidelines.md) |
| Gemini CLI | [`GEMINI.md`](GEMINI.md) |
| Antigravity | [`.antigravity`](.antigravity) |
| Aider | [`.aider.conf.yml`](.aider.conf.yml) |

---

## 📜 Scripts Reference

| Script | What it does |
|---|---|
| `npm test` | Run full suite |
| `npm run test:headed` | Run with visible browser |
| `npm run test:ui` | Playwright interactive UI |
| `npm run test:debug` | Step-through debugger |
| `npm run test:chromium` | Chromium only |
| `npm run test:firefox` | Firefox only |
| `npm run test:webkit` | WebKit only |
| `npm run test:smoke` | Smoke suite |
| `npm run test:regression` | Regression suite |
| `npm run test:report` | Open HTML report |
| `npm run test:allure` | Generate + open Allure |
| `npm run typecheck` | TypeScript check |
| `npm run lint` | ESLint |
| `npm run lint:fix` | ESLint auto-fix |
| `npm run format` | Prettier auto-fix |
| `npm run format:check` | Prettier check |
| `npm run build` | Compile TypeScript |
| `npm run clean` | Remove all output folders |

---

<div align="center">

**Built with precision by [Saumitra Tripathi](https://github.com/st7394)**

[![GitHub](https://img.shields.io/badge/GitHub-st7394-181717?style=flat-square&logo=github)](https://github.com/st7394)

</div>
