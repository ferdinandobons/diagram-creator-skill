# AGENTS.md

Guidelines for AI agents working in this repository.

## Repository Overview

This repository contains an **Agent Skill** for AI agents. Skills install to `~/.claude/skills/` for Claude Code.

- **Name**: Diagram Creator
- **GitHub**: [ferdinandobons/diagram-creator-skill](https://github.com/ferdinandobons/diagram-creator-skill)
- **Creator**: Ferdinando Bonsegna
- **License**: MIT

## Repository Structure

```
diagram-creator-skill/
├── .claude-plugin/
│   └── plugin.json              # Claude Code plugin marketplace manifest
├── .github/
│   ├── FUNDING.yml
│   ├── ISSUE_TEMPLATE/
│   ├── PULL_REQUEST_TEMPLATE/
│   └── workflows/
├── assets/
│   └── demo.png                 # Hero image for README
├── skills/
│   └── diagram-creator/
│       ├── SKILL.md             # Core skill instructions
│       ├── references/          # Design system specs
│       │   ├── typography-and-colors.md
│       │   ├── themes.md
│       │   ├── topology-layouts.md
│       │   ├── components.md
│       │   ├── canvas.md
│       │   └── safety-rules.md
│       └── examples/            # Example diagram specs
│           ├── oauth2-flow.md
│           ├── kubernetes-networking.md
│           └── saas-microservices.md
├── AGENTS.md
├── CLAUDE.md
├── CONTRIBUTING.md
├── LICENSE
├── README.md
└── VERSIONS.md
```

## Skill Structure

The skill consists of:
- **SKILL.md**: Main instructions with input handling, pre-generation questions, topology selection, content planning, theme selection, HTML building steps
- **references/**: Immutable design system specs (typography, colors, themes, layouts, components, canvas, safety rules)
- **examples/**: Reference implementations showing expected output for different topologies

## Build / Lint / Test

Skills are content-only (no build step). To validate:
- YAML frontmatter has valid `name` and `description`
- `name` field matches directory name exactly
- SKILL.md is under 500 lines
- Reference files exist and are complete

## Key Rules

- Output is always a single self-contained `.html` file
- Never use external JS/CSS libraries (except Google Fonts)
- Always include the infinite canvas system (pan & zoom)
- Follow the design system exactly — don't improvise fonts, colors, or layout
- 5 themes: dark (default), light, corporate, neon, minimal
- 8 topologies: nested, left-to-right, hub-and-spoke, timeline, grid, tree, funnel, comparison
