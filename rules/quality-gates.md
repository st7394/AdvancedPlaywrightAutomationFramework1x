# Quality Gates

Every time code is added or modified in this project, the following checks must pass in order:

## 1. Type Check
```bash
npm run typecheck
```
- Runs `tsc --noEmit`
- Must pass with zero errors before any commit or PR

## 2. Lint Check
```bash
npm run lint
```
- Runs ESLint across all TypeScript files
- No errors allowed. Warnings are acceptable but should be resolved

## 3. Format Check
```bash
npm run format:check
```
- Runs Prettier in check mode
- All files must be formatted consistently
- Auto-fix with `npm run format` before committing

## 4. Smoke Tests
```bash
npx playwright test --grep @smoke
```
- Must pass on Chromium before merging to main
- Covers core happy-path flows only

---

## Run All Gates in One Command
```bash
npm run typecheck && npm run lint && npm run format:check && npx playwright test --grep @smoke
```

---

## AI Assistant Rule Files

| Tool | File | Auto-loaded |
|---|---|---|
| Claude Code | `CLAUDE.md` | Yes |
| GitHub Copilot | `.github/copilot-instructions.md` | Yes |
| Cursor | `.cursorrules` | Yes |
| Windsurf | `.windsurfrules` | Yes |
| Cline | `.clinerules` | Yes |
| Augment Code | `augment-guidelines.md` | Yes |
| Gemini CLI | `GEMINI.md` | Yes |
| Antigravity | `.antigravity` | Yes |
| Aider | `.aider.conf.yml` | Yes |

## Rules for AI Agents

These rules apply to all AI assistants (Claude, Copilot, Cursor, Windsurf, Cline, Augment, Gemini, Antigravity, Aider) working in this repository:

1. **After every file change** — run `npm run typecheck`. Fix all type errors before proceeding.
2. **After every file change** — run `npm run lint`. Fix all lint errors. Do not suppress rules without justification.
3. **After every file change** — run `npm run format:check`. Run `npm run format` to auto-fix if it fails.
4. **After adding new test files or page objects** — run `npx playwright test --grep @smoke` to confirm nothing is broken.
5. **Never commit** code that fails any of the above checks.
6. **Never skip** type safety with `// @ts-ignore` or `as any` unless absolutely unavoidable and commented with a reason.
7. **Never disable** ESLint rules inline (`// eslint-disable`) without a comment explaining why.
8. **All new test functions** must be tagged with at least one of: `@smoke`, `@regression`, `@e2e`, `@P0`, `@P1`, `@P2`.
9. **All new Page Object locators** must use arrow functions to avoid stale element references.
10. **All new files** must be placed in the correct `src/` subfolder per the project structure.
