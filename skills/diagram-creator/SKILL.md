---
name: diagram-creator
description: Generate beautiful, production-ready architecture diagrams as self-contained HTML files from any input. Use this skill whenever the user wants to create a diagram, schema, flowchart, network diagram, system visualization, timeline, org chart, or any visual representation of concepts — technical or non-technical. Trigger when the user mentions "diagram", "schema", "architecture", "flow", "topology", "pipeline", "timeline", "visualize", "graph", or asks to turn a file into a visual diagram. Accepts ANY file type as input (.md, .txt, .json, .yaml, .csv, .pdf, .py, .ts, .toml, .env, .tf, .dockerfile, etc.) — Claude auto-detects the format and extracts relevant structure to diagram. Even a simple sentence like "diagram of how OAuth works" or "visualize this file" triggers this skill.
metadata:
  version: 1.0.0
---

# Diagram Creator

Generate stunning, self-contained HTML diagrams with professional typography, smooth animations, and an infinite canvas you can pan and zoom like Miro. Output is always a single `.html` file that opens directly in any browser — zero dependencies. Supports 5 themes and 8 topology layouts.

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

If the input is genuinely ambiguous (no clear structure to extract), ask clarifying questions before proceeding. See "Pre-generation Questions" below.

## Pre-generation Questions

When the input doesn't make the answer obvious, ask up to 3 quick questions before generating. Ask them all in one message, not one at a time. If the input is already clear (e.g., "diagram of Docker bridge networking"), skip questions and proceed.

Questions to consider (ask only what's needed):

1. **What to visualize?** — "What's the main thing you want this diagram to show?" (only if input is vague like "make a diagram")
2. **Topology preference?** — "Which layout fits best?" Offer the options with one-line descriptions. (only if multiple topologies could work equally well)
3. **Theme?** — "Any theme preference? dark (default), light, corporate, neon, minimal" (only if context doesn't make it obvious)

Once you have the answers, proceed autonomously through all remaining steps without further questions.

## Step 0 — Choose the Topology

Pick exactly one topology based on what fits the content best:

| Topology | When to use | Examples |
|---|---|---|
| `nested` | Hierarchical containment, layers wrapping layers | Docker networking, K8s pods, OSI model, VPC subnets |
| `left-to-right` | Sequential flow from A to Z | OAuth flow, CI/CD pipeline, user journey, request lifecycle |
| `hub-and-spoke` | Central node connected to N surrounding nodes | API gateway, microservices, load balancer, team structure |
| `timeline` | Ordered vertical sequence of events/steps | Deploy pipeline, project roadmap, historical events |
| `grid` | Matrix/table comparison, feature grids | Feature comparison, skill map, competitive landscape |
| `tree` | Hierarchical branching, org charts | Org chart, file tree, decision tree, taxonomy |
| `funnel` | Progressive narrowing, conversion stages | Sales funnel, data pipeline, filtering process |
| `comparison` | Side-by-side 2-3 options | Product vs product, technology choices, before/after |

## Step 1 — Plan the Content

Before writing HTML, plan these elements in your thinking:

- **Topic**: one sentence describing what the diagram shows
- **Layers/Steps**: each level with name, description, and accent color
- **Nodes**: each box with emoji, name, and relevant metadata (IPs, ports, roles, dates — whatever fits the domain)
- **Connections**: what each arrow represents
- **Special Components**: callout boxes, badges, info panels
- **Legend Entries**: one per accent color used

## Step 1.5 — Choose the Theme

If the user specifies a theme, use it. Otherwise default to **dark**.

| Theme | Best for | Keywords |
|---|---|---|
| `dark` (default) | Technical diagrams, dev docs | "dark", default |
| `light` | Presentations, documentation sites | "light", "chiaro", "presentation" |
| `corporate` | Business plans, pitch decks | "corporate", "business", "professional" |
| `neon` | Creative, gaming, social media | "neon", "cyberpunk", "vibrant" |
| `minimal` | Clean docs, technical specs | "minimal", "clean", "simple" |

See `references/themes.md` for the exact `:root` values and overrides per theme.

## Step 2 — Build the HTML

Read the reference files for detailed CSS/layout specifications:

- `references/typography-and-colors.md` — fonts, palette, background
- `references/themes.md` — theme system with 5 color schemes
- `references/topology-layouts.md` — layout rules for each of the 8 topologies
- `references/components.md` — node cards, badges, callouts, legend, animations
- `references/safety-rules.md` — mandatory layout constraints for all topologies
- `references/canvas.md` — infinite canvas with pan & zoom (Miro-like navigation)

Follow every rule in those references exactly. The design system is battle-tested — don't improvise fonts, colors, or layout patterns.

**Always include the infinite canvas system** from `references/canvas.md`. This lets users pan (click+drag) and zoom (scroll wheel) the diagram freely, like a whiteboard.

## Step 3 — Output

- Single `.html` file, all CSS in `<style>`, all JS in `<script>`
- No external dependencies except Google Fonts via `@import`
- No CSS frameworks, no JS libraries (vanilla JS for hub-and-spoke coordinates and canvas pan/zoom)
- Valid HTML5 with `<!DOCTYPE html>` as first line
- Must render correctly opened directly in a browser
- Responsive down to 360px viewport width

Save as `diagram-[topic-slug].html` in the current working directory. Open it in the browser with `open` (macOS) or `xdg-open` (Linux). Tell the user where it was saved.
