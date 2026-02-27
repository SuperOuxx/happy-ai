# Repository Guidelines

## Project Structure & Module Organization
This repository is a Yarn v1 monorepo (`package.json` workspaces) with five packages under `packages/`:

- `happy-app`: Expo app (mobile + web) with source in `packages/happy-app/sources/` and Tauri desktop files in `packages/happy-app/src-tauri/`.
- `happy-cli`: CLI client in `packages/happy-cli/src/` with executable entrypoints in `packages/happy-cli/bin/`.
- `happy-server`: Fastify/Prisma backend in `packages/happy-server/sources/` and database schema/migrations in `packages/happy-server/prisma/`.
- `happy-agent`: Remote agent CLI in `packages/happy-agent/src/`.
- `happy-wire`: Shared schemas/types in `packages/happy-wire/src/`.

Root-level `docs/`, `scripts/`, and `patches/` contain repo-wide docs, automation, and patch-package patches.

## Build, Test, and Development Commands
Use Yarn from the repo root.

- `yarn install`: install all workspace dependencies.
- `yarn web`: run `happy-app` in Expo web mode.
- `yarn cli`: run the `happy-cli` workspace `cli` script.
- `yarn workspace happy-server dev`: start backend dev server (kills port 3005 first).
- `yarn workspace happy-server test`: run server tests with Vitest.
- `yarn workspace happy-cli test`: build first, then run Vitest (required by package script).
- `yarn workspace @slopus/happy-wire build`: typecheck + build shared types package.

## Coding Style & Naming Conventions
Code is mostly TypeScript. Follow local package style instead of reformatting unrelated files (spacing/semicolons differ across packages).

- Prefer descriptive `camelCase` for variables/functions and `PascalCase` for types/components.
- Keep filenames consistent with existing patterns (e.g., `*.test.ts`, `*.spec.ts`, `*.integration.test.ts`).
- Use workspace-local `tsconfig.json` and `vitest.config.ts` as the source of truth.
- There is no repo-wide lint/format command; avoid introducing formatting-only churn.

## Testing Guidelines
Vitest is used across packages (`vitest.config.ts` in each workspace). Tests are commonly colocated with source files in `src/` or `sources/`.

- Run targeted package tests via `yarn workspace <pkg> test`.
- Add/keep tests for behavior changes, especially shared protocol/schema logic in `happy-wire` and CLI/server APIs.
- For `happy-app`, prefer unit tests for pure logic and utilities; UI changes should include manual verification notes.

## Commit & Pull Request Guidelines
Recent history follows Conventional Commit style (`feat(...)`, `fix(...)`, `ref:`). Prefer:

- `type(scope): concise summary` (example: `fix(happy-app): hide internal tool in selector`)

PRs should include:

- What changed and why (1-2 paragraphs).
- Affected package(s) (`happy-app`, `happy-server`, etc.).
- Test evidence (commands run and result).
- Screenshots/video for UI changes (`happy-app` web/mobile views).
- Linked issue(s) when applicable.

## Security & Configuration Tips
Do not commit secrets or local `.env*` values. Use existing examples (for example `packages/happy-cli/.envrc.example`) and keep environment-specific changes out of PRs unless required.
