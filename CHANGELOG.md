# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

---

## [3.0.0] — 2026-05-28

### Added
- **Security Agent** — OWASP Top 10 security scan with confidence scoring (≥ 80 threshold), runs between QA and Reviewer
- **Troubleshooter Agent** — activated by `/dev-pipeline fix "<issue>"` for targeted bug fixing without re-running the full pipeline
- **`/dev-pipeline fix "<issue>"`** — runs Troubleshooter Agent on a specific bug
- **`/dev-pipeline review-only`** — runs Security + Reviewer on an existing codebase
- **Auto PR creation** — DevOps Agent calls `gh pr create` after deployment config if `gh` CLI is available
- **`minClaudeCodeVersion` in plugin.json** — documents minimum Claude Code version requirement

### Changed
- Pipeline expanded from 7 to 9 agents (added Security, Troubleshooter)
- Security review now a dedicated phase between QA and Reviewer
- DevOps Agent finalizes with PR creation

---

## [2.0.0] — 2026-05-28

### Added
- **Stack profiles** — `ts-node`, `ts-react`, `python`, `go`, `flutter` with `--stack` flag
- **`{{TECH_STACK}}`, `{{BUILD_COMMAND}}`, `{{TEST_COMMAND}}`, `{{LINT_COMMAND}}`, `{{EXT}}`** placeholders replacing hardcoded TypeScript
- **Git auto-commits** — orchestrator commits after each approved phase
- **`docs/` knowledge base** — `design-docs/`, `tech-debt-tracker.md`, `QUALITY_SCORE.md`
- **`templates/docs/`** — scaffold templates for all knowledge base files
- **`stack-profiles/`** directory with recommended packages per stack

### Changed
- All agent prompts rewritten from Traditional Chinese to English
- QA Agent model downgraded to `haiku` for cost efficiency
- Architect Agent now creates `docs/` scaffold after detailed design

---

## [1.0.0] — 2026-05-28

### Added
- **PM Agent** — Produces structured PRD with user stories, acceptance criteria, and scope boundaries
- **Architect Agent** — Proposes 2–3 architecture options with trade-offs for user selection
- **Backend Agent** — Implements REST APIs, services, and repositories
- **Frontend Agent** — Implements UI with full loading/error/empty state handling
- **QA Agent** — Writes and runs unit, integration, and E2E tests (≥80% backend coverage)
- **Reviewer Agent** — Confidence-scored code review (reports only issues ≥ 80)
- **DevOps Agent** — Generates Dockerfile, docker-compose, GitHub Actions CI/CD
- **`/dev-pipeline` command** — Orchestrator with human-in-the-loop checkpoints
- Parallel Backend + Frontend execution
- MIT License, English + Traditional Chinese README

[Unreleased]: https://github.com/airwaves778899/claude-dev-pipeline/compare/v3.0.0...HEAD
[3.0.0]: https://github.com/airwaves778899/claude-dev-pipeline/compare/v2.0.0...v3.0.0
[2.0.0]: https://github.com/airwaves778899/claude-dev-pipeline/compare/v1.0.0...v2.0.0
[1.0.0]: https://github.com/airwaves778899/claude-dev-pipeline/releases/tag/v1.0.0
