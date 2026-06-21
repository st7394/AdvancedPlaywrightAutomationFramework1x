# Rules

This folder contains the quality gate definitions for this project.

All AI assistants and contributors must follow these rules before committing any code.

## Files

| File | Purpose |
|---|---|
| [quality-gates.md](quality-gates.md) | Full quality gate definitions and AI agent rules |

## The 4 Gates

Every change must pass these in order:

```bash
npm run typecheck                       # 1. Zero TypeScript errors
npm run lint                            # 2. Zero ESLint errors
npm run format:check                    # 3. All files formatted
npx playwright test --grep @smoke       # 4. Smoke suite green
```

## Where Each AI Reads Its Rules

| Tool | File |
|---|---|
| Claude Code | `CLAUDE.md` |
| GitHub Copilot | `.github/copilot-instructions.md` |
| Cursor | `.cursorrules` |
| Windsurf | `.windsurfrules` |
| Cline | `.clinerules` |
| Augment Code | `augment-guidelines.md` |
| Gemini CLI | `GEMINI.md` |
| Antigravity | `.antigravity` |
| Aider | `.aider.conf.yml` |

All of the above enforce the same rules defined in [quality-gates.md](quality-gates.md).
