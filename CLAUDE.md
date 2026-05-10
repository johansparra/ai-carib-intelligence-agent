# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

This repository is a **GitHub Copilot agent configuration platform** for intelligence analysis of Dynatrace and DataPower. It is composed entirely of markdown-based agent definitions, prompt files, and skills — there is no compiled code, package manager, or test runner. All files are configuration/documentation consumed by GitHub Copilot.

## Architecture

The repo defines three operational agents orchestrated through GitHub Copilot:

- **Dynatrace Agent** (`.github/agents/dyn-analyst/`) — Connects to Dynatrace Davis AI, runs DQL queries, detects anomalies. Uses skills in `.github/skills/dyn-queries/`.
- **DataPower Agent** (`.github/agents/dp-analyst/`) — Analyzes DataPower reports and generates professional insights. Uses skills in `.github/skills/dp-analysis/`. Operates independently with no dependencies on Dynatrace.
- **Structure Monitor** (`.github/agents/core-structure-monitor/`) — Automatically detects structural changes in the project and keeps all README.md files and Mermaid diagrams in sync. Configured via `.github/customizations/auto-sync.md`. Also manages automatic Git commits and pushes.

Two additional agents are present:
- **ai-team-dev** (`.github/agents/core-ai-team-dev.agent.md`) — A three-role dev team (Nova/Frontend, Sage/Backend, Milo/Visual) for feature implementation. Reads `PROJECT_BRIEF.md` before starting and writes progress to `docs/sprint-N/progress.md`.
- **Atlassian Requirements to Jira** (`.github/agents/core-atlassian-jira.agent.md`) — Transforms requirements documents into Jira epics and user stories via the Atlassian MCP Server. Requires explicit user approval before any create/update operation; enforces limits of 20 epics / 50 stories per batch.

The Chatbot Copilot acts as the central orchestrator: receives user requests, activates the appropriate agent (Dynatrace or DataPower), and assembles the final report.

## Key Rules (from copilot-instructions.md)

### Component Separation
Dynatrace and DataPower must remain fully independent. Changes to one must not affect the other. Shared logic belongs in `.github/toolboxes/`.

### Documentation via Structure Monitor
**Do not manually edit Mermaid diagrams in any README.md.** The Structure Monitor agent handles all diagram regeneration automatically when files or folders change. To trigger it manually, say: *"Sincroniza la documentación con los cambios actuales"* or run `/structure-monitor-sync` in Copilot.

### Atomic Commits
Each type of change should be a separate commit:
- Folder structure changes
- Logic/skills changes  
- Prompt changes

### Commit Message Conventions
- `docs: sync README.md diagrams`
- `docs: add new agent <name>`
- `docs: fix markdown formatting`
- `feat: add <component> skill`

### Markdown Formatting (enforced by Structure Monitor)
- Blank line required after `##` and `###` headings
- Lists following a `:` must be surrounded by blank lines (avoids MD032)

## Adding New Components

When adding a new agent, skill, or prompt:
1. Create the folder/file in the appropriate `.github/` subdirectory
2. Create a `README.md` for the new component
3. The Structure Monitor will detect the change and update all parent README.md files and Mermaid diagrams automatically
4. Commit structure and logic changes as separate atomic commits

## Atlassian MCP Requirement

The `core-atlassian-jira` agent requires the Atlassian MCP Server to be installed and configured in VS Code before use. Without it, the agent cannot connect to Jira.

## Naming Convention (prefix system)

All agents, skills and prompts follow a prefix system:

| Prefix | Domain | Examples |
| ------ | ------ | ------- |
| `dyn-` | Dynatrace | `dyn-analyst/`, `dyn-queries/`, `dyn-chatbot.prompt.md` |
| `dp-` | DataPower | `dp-analyst/`, `dp-analysis/`, `dp-analyst.prompt.md` |
| `ops-` | Operations (cross-domain) | `ops-incident-responder/`, `ops-daily-summary/`, `ops-incident/` |
| `core-` | Project infrastructure | `core-structure-monitor/`, `core-output-polisher/`, `core-gh-cli.md` |
