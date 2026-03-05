# Diagram Creator Skill

A Claude Code skill that generates production-ready architecture diagrams as self-contained HTML files.

## About This Project

This is a Claude Code skill repository. The skill reads any file or topic description and generates a beautiful HTML diagram with professional typography, animations, and an infinite canvas.

## Key Directories

- `skills/diagram-creator/SKILL.md` — Core skill instructions
- `skills/diagram-creator/references/` — Design system specifications (DO NOT modify without testing)
- `skills/diagram-creator/examples/` — Reference implementations
- `assets/` — Images for README
- `.claude-plugin/` — Marketplace manifest

## Commands

No build step. This is a content-only skill repo.

## Standards

- Output must be a single `.html` file with all CSS in `<style>` and all JS in `<script>`
- No external dependencies except Google Fonts via `@import`
- Must render correctly when opened directly in a browser
- Responsive down to 360px viewport width
- Always include the infinite canvas system from `references/canvas.md`

## Notes

- The design system in `references/` is battle-tested — follow it exactly
- When adding a new topology, update: `references/topology-layouts.md`, `SKILL.md` topology table, and add an example
- SKILL.md should stay under 500 lines — move detailed specs to `references/`
