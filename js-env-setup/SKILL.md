---
name: js-env-setup
description: Set up the right Node version and package manager when working in a JavaScript or TypeScript project. Use before running node, installing/adding/removing/updating dependencies, running package scripts or tests, setting up a new JS/TS project, or writing install steps for CI, Dockerfiles or READMEs. Selects the Node version with nvm instead of the system default, and defaults to pnpm when the project doesn't specify a package manager.
---

# JS Node version and package manager

Two things go wrong when an agent uses whatever `node` and package manager happen to be on the machine: the wrong Node version breaks builds or changes lockfiles, and mixing package managers leaves two lockfiles and diverging `node_modules`. Settle both before running anything.

## 1. Node version (use nvm)

The default `node` on the PATH is often not the version the project wants, so select it explicitly.

1. Find the wanted version: `.nvmrc`, `.node-version`, `engines.node` in `package.json`, or `.tool-versions`.
2. Load nvm in the shell (it's a shell function, not a binary, so it may not be loaded in non-interactive shells): `source ~/.nvm/nvm.sh`.
3. Run `nvm use` in the project root. It reads `.nvmrc`. If the version isn't installed, run `nvm install` and then `nvm use`. If only `engines.node` exists, use `nvm install <version>` / `nvm use <version>` with a matching version.
4. If no version is specified, use the latest LTS (`nvm use --lts`, installing it if needed) and tell the user. Don't write a new `.nvmrc` unless asked.
5. Confirm with `node -v` before continuing. Each shell call may start fresh, so run `nvm use` in the same command as the one that needs it (e.g. `source ~/.nvm/nvm.sh && nvm use && pnpm test`).

If nvm isn't installed, use the project's other version manager if it has one (fnm, volta, asdf), otherwise continue with the system Node and tell the user about any version mismatch.

## 2. Package manager (pnpm by default)

Use whichever one the project already uses. If it doesn't specify one, use **pnpm**.

1. Use what the project declares: the `packageManager` field in `package.json`, or a lockfile (`pnpm-lock.yaml`, `yarn.lock`, `package-lock.json`, `bun.lock`/`bun.lockb`). Docs or CI config naming a tool also count.
2. If nothing is declared (new project, no lockfile), use pnpm. Enable it with `corepack enable pnpm` if it's missing, and set `packageManager` in `package.json` so the choice is recorded.
3. If pnpm can't be installed in the environment, fall back to npm and tell the user.

## Rules

- Never run another manager's install command in an existing project, even for one package.
- If lockfiles conflict, follow `packageManager`, don't delete anything, and tell the user.
- In CI and Dockerfiles use the frozen-lockfile install (e.g. `pnpm install --frozen-lockfile`) and pin the same Node version the project uses locally.
- In monorepos, run from the workspace root and use the manager's workspace/filter option.
- Briefly tell the user which Node version and package manager you used, and why when you had to choose by default.
