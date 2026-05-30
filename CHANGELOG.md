# Changelog

All notable changes to this project are documented here.
This project adheres to [Semantic Versioning](https://semver.org/).

## [0.1.0] — 2026-05-30

### Added
- `kio-scaffold` plugin marketplace.
- `harness` plugin (v0.1.0).
- `/harness:scaffold` skill — bootstraps a project's AI work environment:
  - Interview-first flow (asks project name, stack, rule scope before generating).
  - Generates `CLAUDE.md` (project map, kept short), `.claude/rules/`
    (`project-structure`, `code-style`, `testing`, `security`), and a role-based
    folder layout (`src/ docs/ tests/ .dev/ .claude/ out/`).
  - User→Project→Folder scope inheritance; folder-level `CLAUDE.md` as optional override.
  - Optional force-push blocking hook (double safety).
  - Diagnose/simplify mode for already-set-up projects.
- Bundled template assets and `references/conventions.md`.
