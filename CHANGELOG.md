# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

---

## [1.0.0] — 2026-05-28

### Added
- **PM Agent** — Produces structured PRD with user stories, acceptance criteria, and scope boundaries
- **Architect Agent** — Proposes 2–3 architecture options with trade-offs for user selection
- **Backend Agent** — Implements REST APIs, services, and repositories in TypeScript strict mode
- **Frontend Agent** — Implements React UI, hooks, and API client with full loading/error/empty state handling
- **QA Agent** — Writes and runs unit, integration, and E2E tests (≥80% backend coverage)
- **Reviewer Agent** — Confidence-scored code review (reports only issues with confidence ≥ 80)
- **DevOps Agent** — Generates Dockerfile, docker-compose, and GitHub Actions CI/CD pipelines
- **`/dev-pipeline` command** — Orchestrator with human-in-the-loop checkpoints at every phase
- Parallel execution for Backend + Frontend phases
- Pipeline pauses automatically when Reviewer finds more than 3 Critical issues
- Intermediate artifacts stored in `.pipeline/` directory
- English README + Traditional Chinese README

[Unreleased]: https://github.com/airwaves778899/claude-dev-pipeline/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/airwaves778899/claude-dev-pipeline/releases/tag/v1.0.0
