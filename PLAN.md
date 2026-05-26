# Understand Anything — Clone, Audit, Deploy Plan

## Goal
Clone `Lum1104/Understand-Anything`, audit the codebase, get deployable UIs running, fix issues, deploy, and report.

## Repo Structure (pnpm monorepo)
- `homepage/` — Astro v6 marketing site (static, self-contained)
- `understand-anything-plugin/packages/core/` — Node lib with tree-sitter parsers (native deps)
- `understand-anything-plugin/packages/dashboard/` — React 19 + Vite SPA, has demo build mode
- `understand-anything-plugin/` — root skill package wrapping core

## Deployable Artifacts
1. **Homepage** (`homepage/`) → Astro static build → `dist/` → static hosting
2. **Dashboard Demo** (`packages/dashboard/`) → Vite demo build → `dist/` → static hosting with bundled demo graph

## Known Blockers
- Nested `pnpm-lock.yaml` inside `understand-anything-plugin/` confuses pnpm — remove it
- `tree-sitter-*` native grammar packages need C compiler + Python + node-gyp
- `packageManager` pins `pnpm@10.6.2` — must use exact version
- `sharp` + `esbuild` in `onlyBuiltDependencies` — usually fine, but tree-sitter grammars are riskiest

## Strategy
1. Fix nested lockfile
2. `pnpm install` (if tree-sitter fails, skip/ignore and keep working)
3. Build `homepage` and dashboard demo independently
4. Deploy both via `deploy_wfp`
5. Report exact status
