---
name: diagram-creator
description: Generate beautiful, production-ready architecture diagrams as self-contained HTML files from any input. Use this skill whenever the user wants to create a diagram, schema, flowchart, network diagram, system visualization, timeline, org chart, or any visual representation of concepts — technical or non-technical. Trigger when the user mentions "diagram", "schema", "architecture", "flow", "topology", "pipeline", "timeline", "visualize", "graph", or asks to turn a file into a visual diagram. Accepts ANY file type as input (.md, .txt, .json, .yaml, .csv, .pdf, .py, .ts, .toml, .env, .tf, .dockerfile, etc.) — Claude auto-detects the format and extracts relevant structure to diagram. Even a simple sentence like "diagram of how OAuth works" or "visualize this file" triggers this skill.
version: 1.0.0
---

# Diagram Creator

Generate stunning, self-contained HTML architecture diagrams with dark theme, professional typography, and smooth animations. Output is always a single `.html` file that opens directly in any browser — zero dependencies.

## Input Handling

Accept any of these inputs and auto-process them:

| Input type | How to handle |
|---|---|
| **Topic/description** | "Docker networking", "OAuth 2.0 flow" — use directly |
| **Markdown file** (.md) | Read and extract structure, headings, lists, relationships |
| **JSON / YAML** (.json, .yaml, .yml) | Parse structure — keys become nodes, nesting becomes layers |
| **Code files** (.py, .ts, .js, .go, .rs, etc.) | Extract classes, functions, imports — diagram the architecture |
| **Config files** (.toml, .env, .tf, .dockerfile, docker-compose.yml) | Map services, variables, dependencies |
| **CSV / data files** (.csv, .tsv) | Identify columns as entities, rows as relationships |
| **PDF** | Read content, extract the core structure to visualize |
| **Plain text** (.txt) | Parse as free-form description |

When receiving a file path, read the file, detect the extension, and intelligently extract what matters for the diagram. Don't ask the user to reformat — figure it out.

If the input is genuinely ambiguous (no clear structure to extract), ask ONE clarifying question about what to visualize. Then proceed autonomously.

## Step 0 — Choose the Topology

Pick exactly one topology based on what fits the content best:

| Topology | When to use | Examples |
|---|---|---|
| `nested` | Hierarchical containment, layers wrapping layers | Docker networking, K8s pods, OSI model, VPC subnets, org hierarchy |
| `left-to-right` | Sequential flow from A to Z | OAuth flow, CI/CD pipeline, user journey, request lifecycle |
| `hub-and-spoke` | Central node connected to N surrounding nodes | API gateway, microservices, load balancer, team structure |
| `timeline` | Ordered vertical sequence of events/steps | Deploy pipeline, project roadmap, historical events, boot sequence |

## Step 1 — Plan the Content

Before writing HTML, plan these elements in your thinking:

- **Topic**: one sentence describing what the diagram shows
- **Layers/Steps**: each level with name, description, and accent color
- **Nodes**: each box with emoji, name, and relevant metadata (IPs, ports, roles, dates — whatever fits the domain)
- **Connections**: what each arrow represents
- **Special Components**: callout boxes, badges, info panels
- **Legend Entries**: one per accent color used

## Step 2 — Build the HTML

Read the reference files for detailed CSS/layout specifications:

- `references/typography-and-colors.md` — fonts, palette, background
- `references/topology-layouts.md` — layout rules for each topology type
- `references/components.md` — node cards, badges, callouts, legend, animations
- `references/safety-rules.md` — mandatory layout constraints for all topologies

Follow every rule in those references exactly. The design system is battle-tested — don't improvise fonts, colors, or layout patterns.

## Step 3 — Output

- Single `.html` file, all CSS in `<style>`, all JS in `<script>`
- No external dependencies except Google Fonts via `@import`
- No CSS frameworks, no JS libraries (vanilla JS for hub-and-spoke only)
- Valid HTML5 with `<!DOCTYPE html>` as first line
- Must render correctly opened directly in a browser
- Responsive down to 360px viewport width

Save as `diagram-[topic-slug].html` in the current working directory. Open it in the browser with `open` (macOS) or `xdg-open` (Linux). Tell the user where it was saved.
