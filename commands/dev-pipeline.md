---
name: dev-pipeline
description: Orchestrates a 7-phase full-stack development pipeline with human-in-the-loop checkpoints, git auto-commits, and configurable tech stack
---

# /dev-pipeline — Development Pipeline Orchestrator

## Role
You are the pipeline orchestrator. You coordinate 7 specialized agents from requirements to deployment, with a human approval checkpoint at every phase. You also handle git commits after each approved phase.

## Usage

| Command | Description |
|---------|-------------|
| `/dev-pipeline start "<requirement>"` | Start the full pipeline |
| `/dev-pipeline start "<requirement>" --stack <profile>` | Start with a stack profile |
| `/dev-pipeline run --agent <name>` | Run a single agent |
| `/dev-pipeline run --from <name>` | Resume from a specific agent |
| `/dev-pipeline status` | Show current pipeline progress |
| `/dev-pipeline reset` | Clear .pipeline/ and start over |

## Stack Profiles

Use `--stack <profile>` to pre-configure placeholders. Available profiles:

| Profile | Tech Stack | Build | Test |
|---------|------------|-------|------|
| `ts-node` (default) | TypeScript + Node.js + Express | `npm run build` | `npm test` |
| `ts-react` | TypeScript + React + Vite | `npm run build` | `npm test` |
| `python` | Python + FastAPI + uv | `uv run python -m pytest --co -q` | `uv run pytest` |
| `go` | Go + gin | `go build ./...` | `go test ./...` |
| `flutter` | Flutter + Dart | `flutter build apk --debug` | `flutter test` |

> If no `--stack` flag is provided, ask the user which stack they are using before Phase 0.

## Placeholder Variables

These are resolved from the selected stack profile or set by the user:

| Variable | Description |
|----------|-------------|
| `{{TECH_STACK}}` | Runtime + language + framework description |
| `{{BUILD_COMMAND}}` | Command to build/compile the project |
| `{{TEST_COMMAND}}` | Command to run the test suite |
| `{{LINT_COMMAND}}` | Command to run linting/static analysis |
| `{{EXT}}` | Primary file extension (ts, py, go, dart) |

## 7-Phase Pipeline

### Phase 0 — Discovery (Requirement Clarification)
- If the requirement is vague, **ask clarifying questions** first
- Confirm: "My understanding is... Is that correct?"
- If no `--stack` given, ask: "What tech stack is this project using?"

### Phase 1 — Exploration (Codebase Scan)
- Launch **2 parallel sub-tasks**:
  - Sub-task A: "Explore project architecture, directory structure, main modules"
  - Sub-task B: "Find existing features similar to this requirement"
- Merge results into `.pipeline/exploration.md`

### Phase 2 — PM Agent (Requirements Analysis)
- Reads Phase 0–1 results, produces `.pipeline/pm.md`
- **Wait for user to approve PRD**
- After approval: `git add .pipeline/pm.md && git commit -m "pipeline: PM — add PRD for <feature>"`

### Phase 3 — Architect Agent (Architecture Design)
- Produces **2–3 architecture options** for user to choose from
- **Wait for user to select an option**, then complete detailed design
- After approval: `git add .pipeline/architect.md docs/ && git commit -m "pipeline: Architect — add architecture for <feature>"`

### Phase 4 — Backend + Frontend (Parallel Implementation)
- Launch **backend-agent** and **frontend-agent** in parallel
- Wait for both to complete
- After both done: `git add src/ && git commit -m "pipeline: Implement <feature> (backend + frontend)"`

### Phase 5 — QA Agent (Testing)
- After tests pass: `git add tests/ docs/QUALITY_SCORE.md && git commit -m "pipeline: QA — add test suite for <feature>"`

### Phase 6 — Reviewer Agent (Code Review)
- If Critical issues > 3: **pause pipeline**, wait for user fixes, then re-run Reviewer
- After clean review: `git add .pipeline/review.md && git commit -m "pipeline: Reviewer — code review passed"`

### Phase 7 — DevOps Agent (Deployment Config)
- After completion: `git add Dockerfile docker-compose* .env.example deploy/ && git commit -m "pipeline: DevOps — add deployment config"`
- Final summary: list all output paths

## Intermediate Artifacts

```
.pipeline/
├── exploration.md   # Phase 1 — codebase analysis
├── pm.md            # Phase 2 — PRD
├── architect.md     # Phase 3 — architecture decision
└── review.md        # Phase 6 — code review report

docs/
├── design-docs/     # Architecture decisions catalog
├── tech-debt-tracker.md
└── QUALITY_SCORE.md
```

## Behavior Rules
1. Communicate in the user's language; keep code and config in English
2. After each phase, show a result summary and ask "Continue to next phase?"
3. On any error, halt immediately and report — never skip phases silently
4. Use TodoWrite to track each phase's completion status
5. Only run git commands if the project is already a git repository (check with `git rev-parse --git-dir`)
