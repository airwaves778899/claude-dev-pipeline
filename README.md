# claude-dev-pipeline

> A Claude Code plugin that orchestrates **7 specialized AI agents** to take your feature request all the way from requirements analysis to production deployment — with a human-in-the-loop checkpoint at every phase.

[中文說明](./README.zh.md)

---

## Why?

Writing a feature involves more than just code. You need:
- A clear spec that everyone agrees on
- An architecture decision before you write a single line
- Backend and frontend that actually fit together
- Tests that prove things work
- A code review that catches security holes
- A deployment config that actually runs

`claude-dev-pipeline` encodes that workflow as a Claude Code plugin. Each agent is an expert. You stay in control at every gate.

---

## The 7 Agents

| # | Agent | Role | Output |
|---|-------|------|--------|
| 0 | **Discovery** | Clarifies vague requirements via dialogue | Confirmed requirement |
| 1 | **Exploration** | Scans existing codebase in parallel | `.pipeline/exploration.md` |
| 2 | **PM** | Writes a structured PRD with user stories & acceptance criteria | `.pipeline/pm.md` |
| 3 | **Architect** | Proposes 2–3 architecture options with trade-offs | `.pipeline/architect.md` |
| 4a | **Backend** | Implements REST APIs, services, repositories in TypeScript | `src/backend/` |
| 4b | **Frontend** | Implements React UI, hooks, API client in TypeScript | `src/frontend/` |
| 5 | **QA** | Writes and runs unit, integration, and E2E tests | `tests/` |
| 6 | **Reviewer** | Audits code for security, bugs, and quality (confidence ≥ 80) | `.pipeline/review.md` |
| 7 | **DevOps** | Creates Dockerfile, docker-compose, GitHub Actions CI/CD | `deploy/` |

Phases 4a and 4b (Backend + Frontend) run **in parallel**.

---

## Installation

### Prerequisites

- [Claude Code](https://claude.ai/code) CLI or VS Code Extension installed

### Install the plugin

```bash
# 1. Clone this repository
git clone https://github.com/airwaves778899/claude-dev-pipeline.git

# 2. Register as a local marketplace
claude plugin marketplace add "C:\path\to\claude-dev-pipeline"

# 3. Install
claude plugin install claude-dev-pipeline

# 4. Verify
claude plugin list
# should show: ✓ claude-dev-pipeline  enabled
```

The plugin is now available in both the CLI and the VS Code Extension — no extra flags needed.

---

## Usage

### Start the full pipeline

```
/claude-dev-pipeline:dev-pipeline start "Add user authentication with email + password login"
```

### Run a single agent

```
/claude-dev-pipeline:dev-pipeline run --agent pm
/claude-dev-pipeline:dev-pipeline run --agent architect
/claude-dev-pipeline:dev-pipeline run --agent backend
```

### Resume from a specific phase

```
/claude-dev-pipeline:dev-pipeline run --from qa
```

### Other commands

```
/claude-dev-pipeline:dev-pipeline status   # Show current pipeline progress
/claude-dev-pipeline:dev-pipeline reset    # Clear .pipeline/ and start over
```

---

## Pipeline Flow

```
User requirement
       │
  [Discovery]  ← asks clarifying questions if vague
       │ confirmed requirement
  [Exploration] ← scans codebase in parallel (2 sub-agents)
       │ exploration.md
     [PM] ← you review & approve PRD
       │ pm.md
 [Architect] ← you choose 1 of 3 architecture options
       │ architect.md
  ┌────┴────┐
[Backend] [Frontend]  ← run in parallel
  └────┬────┘
      [QA]
       │ tests green
  [Reviewer] ← pipeline pauses if Critical issues > 3
       │ review.md clean
   [DevOps]
       │
  🎉 Done
```

---

## Intermediate Artifacts

All agents write to `.pipeline/` in your project root:

```
.pipeline/
├── pm.md           # PRD (Product Requirements Document)
├── architect.md    # Architecture decision + detailed design
├── exploration.md  # Codebase analysis
└── review.md       # Code review report
```

---

## Agent Models & Colors

| Agent | Model | Color |
|-------|-------|-------|
| PM | claude-sonnet | 🔵 Blue |
| Architect | claude-sonnet | 🟢 Green |
| Backend | claude-sonnet | 🟣 Purple |
| Frontend | claude-sonnet | 🟡 Yellow |
| QA | claude-sonnet | 🟠 Orange |
| Reviewer | claude-sonnet | 🔴 Red |
| DevOps | claude-sonnet | 🩵 Teal |

---

## Configuration

No configuration needed. The agents use sane defaults. If your project has a `CLAUDE.md` file, the Reviewer agent will automatically reference it for project-specific coding conventions.



---

## Stack Profiles

Use `--stack` to skip the tech stack configuration prompt:

```
/dev-pipeline start "Add payment processing" --stack python
/dev-pipeline start "Build mobile onboarding" --stack flutter
```

| Profile | Stack |
|---------|-------|
| `ts-node` (default) | TypeScript + Node.js + Express |
| `ts-react` | TypeScript + React + Vite |
| `python` | Python + FastAPI |
| `go` | Go + gin |
| `flutter` | Flutter + Dart |

See [`stack-profiles/`](./stack-profiles/) for full details and recommended packages.

---

## Knowledge Base (docs/)

The Architect Agent automatically creates a `docs/` directory in your project that persists across pipeline runs:

```
docs/
├── design-docs/
│   └── index.md              # Architecture decision log
├── tech-debt-tracker.md      # Tracked deferred items (visible, not hidden)
└── QUALITY_SCORE.md          # QA coverage results per layer
```

See [`templates/docs/`](./templates/docs/) for the template files.

---

## Git Auto-Commits

If your project is a git repository, the orchestrator automatically commits after each approved phase:

| Phase | Commit message |
|-------|----------------|
| PM | `pipeline: PM — add PRD for <feature>` |
| Architect | `pipeline: Architect — add architecture for <feature>` |
| Implementation | `pipeline: Implement <feature> (backend + frontend)` |
| QA | `pipeline: QA — add test suite for <feature>` |
| Reviewer | `pipeline: Reviewer — code review passed` |
| DevOps | `pipeline: DevOps — add deployment config` |---

## Contributing

Pull requests are welcome! Please open an issue first to discuss what you would like to change.

1. Fork the repo
2. Create your feature branch (`git checkout -b feature/better-pm-agent`)
3. Commit your changes (`git commit -m 'Improve PM agent acceptance criteria'`)
4. Push to the branch (`git push origin feature/better-pm-agent`)
5. Open a Pull Request

---

## License

[MIT](./LICENSE)

