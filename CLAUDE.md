# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

The Agency is a collection of 154+ specialized AI agent personas defined as markdown files. Agents are tool-agnostic source files that get converted into formats for Claude Code, GitHub Copilot, Cursor, Aider, Windsurf, Antigravity, Gemini CLI, OpenCode, OpenClaw, and Qwen.

## Common Commands

### Lint agents
```bash
./scripts/lint-agents.sh                    # lint all agent files
./scripts/lint-agents.sh path/to/agent.md   # lint specific file(s)
```

### Convert agents to tool-specific formats
```bash
./scripts/convert.sh --tool claude-code     # convert for a specific tool
./scripts/convert.sh --tool cursor --parallel --jobs 4
```

### Install converted agents locally
```bash
./scripts/install.sh --tool claude-code     # install for a specific tool
./scripts/install.sh --interactive          # auto-detect installed tools
```

## Agent File Format

Every agent is a markdown file with YAML frontmatter:

```yaml
---
name: Agent Name
description: One-line specialty description
color: colorname or "#hexcode"
emoji: (optional)
vibe: (optional) One-line personality hook
services: (optional)
  - name: Service Name
    url: https://...
    tier: free|freemium|paid
---
```

**Required frontmatter fields:** `name`, `description`, `color`

**Recommended body sections:** Identity, Core Mission, Critical Rules. The linter warns if these are missing. Body must be 50+ words.

## Repository Structure

Agents are organized by category directory: `engineering/`, `design/`, `marketing/`, `specialized/`, `sales/`, `product/`, `paid-media/`, `project-management/`, `testing/`, `support/`, `spatial-computing/`, `game-development/`, `academic/`.

Non-agent directories:
- `strategy/` — NEXUS multi-agent orchestration framework (playbooks, runbooks, coordination templates)
- `examples/` — Multi-agent collaboration demonstrations
- `integrations/` — Generated tool-specific outputs (gitignored; regenerate via `convert.sh`)
- `scripts/` — `convert.sh`, `install.sh`, `lint-agents.sh`

## CI/CD

The GitHub Actions workflow (`.github/workflows/lint-agents.yml`) runs `lint-agents.sh` on changed agent files in PRs. It auto-detects which files were modified and only lints those.

## Architecture Notes

- Agents are content (markdown), not code. The conversion pipeline (`convert.sh`) transforms them into tool-specific formats.
- Generated integration files under `integrations/` are gitignored. Users run `convert.sh` locally to produce them.
- The NEXUS orchestration layer in `strategy/` defines three operating modes (Full/Sprint/Micro) for coordinating multiple agents together, with phase-based playbooks and quality gates.
- `lint-agents.sh` defines the canonical list of agent directories in its `AGENT_DIRS` array. When adding a new category, update this array, plus the directory loops in `convert.sh` and `install.sh`.
