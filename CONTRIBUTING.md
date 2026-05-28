# Contributing to claude-dev-pipeline

Thank you for your interest in improving this project! This guide explains how to contribute effectively.

---

## Ways to Contribute

- **Improve an agent prompt** — better instructions lead to better outputs
- **Add a new agent** — e.g. a Security agent, a Documentation agent
- **Fix a bug** — incorrect behavior, wrong output paths, broken commands
- **Improve documentation** — clearer README, better examples
- **Share example outputs** — real `.pipeline/` outputs from your projects

---

## Getting Started

1. **Fork** the repository
2. **Clone** your fork locally
   ```bash
   git clone https://github.com/your-username/claude-dev-pipeline.git
   cd claude-dev-pipeline
   ```
3. **Create a branch** with a descriptive name
   ```bash
   git checkout -b improve/pm-agent-acceptance-criteria
   ```

---

## Project Structure

```
claude-dev-pipeline/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata
├── agents/                  # One .md file per agent
│   ├── pm-agent.md          # PM Agent prompt
│   ├── architect-agent.md   # Architect Agent prompt
│   ├── backend-agent.md     # Backend Agent prompt
│   ├── frontend-agent.md    # Frontend Agent prompt
│   ├── qa-agent.md          # QA Agent prompt
│   ├── reviewer-agent.md    # Reviewer Agent prompt
│   └── devops-agent.md      # DevOps Agent prompt
├── commands/
│   └── dev-pipeline.md      # Main orchestrator command
└── examples/
    └── .pipeline/           # Sample agent outputs
```

---

## Agent Prompt Guidelines

Each agent file has a YAML frontmatter section and a markdown body:

```yaml
---
name: agent-name
description: One-line description used by the orchestrator to select this agent
tools: Glob, Grep, LS, Read, Write, ...
model: sonnet
color: blue
---
```

When editing agent prompts, please follow these principles:

- **Be specific about inputs and outputs** — agents must know exactly what files to read and where to write
- **Include completion conditions** — a clear checklist of what "done" means
- **Keep role separation clean** — PM doesn't make tech decisions; Architect doesn't write code
- **Use `TodoWrite`** for tracking progress within a session

---

## Submitting a Pull Request

1. Make your changes
2. Test manually with Claude Code (`claude plugin install .`)
3. Update `CHANGELOG.md` under the `[Unreleased]` section
4. Push your branch and open a Pull Request
5. Fill in the PR template

**Please open an Issue first** for significant changes (new agents, pipeline restructuring) so we can discuss the approach before you invest time building it.

---

## Code of Conduct

Be kind and constructive. We're all here to make AI-assisted development better.
