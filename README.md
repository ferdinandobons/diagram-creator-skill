# Diagram Creator — Claude Code Skill

Turn any input into beautiful, production-ready diagrams on an infinite canvas. Self-contained HTML files with 5 themes, 8 topology layouts, pan & zoom navigation, professional typography, smooth animations, and zero dependencies.

Built by [Ferdinando Bons](https://github.com/ferdinandobons).

**Contributions welcome!** Found a way to improve the skill or want to add a new topology? [Open a PR](#contributing).

## What It Does

Give it a topic, a description, or **any file** — and it generates a polished architecture diagram as a single `.html` file you can open directly in your browser.

```
"Create a diagram of OAuth 2.0 flow"          → diagram-oauth2-flow.html
"Visualize this docker-compose.yml"            → diagram-docker-compose.html
"Diagram my Kubernetes networking"             → diagram-k8s-networking.html
"Turn this business plan into a visual schema" → diagram-business-plan.html
```

## Supported Input Types

| Input | How it's processed |
|---|---|
| **Text description** | Used directly as diagram specification |
| **Markdown** (.md) | Extracts structure from headings, lists, relationships |
| **JSON / YAML** (.json, .yaml) | Parses structure — keys become nodes, nesting becomes layers |
| **Code files** (.py, .ts, .js, .go, etc.) | Extracts classes, functions, imports — diagrams the architecture |
| **Config files** (.toml, .env, .tf, docker-compose.yml) | Maps services, variables, dependencies |
| **CSV** (.csv) | Identifies entities and relationships from columns |
| **PDF** (.pdf) | Reads content, extracts core structure |
| **Plain text** (.txt) | Parses as free-form description |

## 8 Topology Layouts

| Layout | Best for | Examples |
|---|---|---|
| **Nested** | Hierarchical containment | Docker networking, K8s pods, VPCs, OSI model |
| **Left-to-right** | Sequential flows | OAuth flow, CI/CD pipeline, user journey |
| **Hub-and-spoke** | Central node + surrounding nodes | API gateway, microservices, load balancer |
| **Timeline** | Ordered vertical steps | Deploy pipeline, project roadmap, historical events |
| **Grid / Matrix** | Comparison tables, feature grids | Feature comparison, skill map, competitive landscape |
| **Tree / Org Chart** | Hierarchical branching | Org chart, file tree, decision tree, taxonomy |
| **Funnel** | Progressive narrowing | Sales funnel, conversion pipeline, data filtering |
| **Comparison / VS** | Side-by-side 2-3 options | Product vs product, tech choices, before/after |

## 5 Themes

| Theme | Best for |
|---|---|
| **Dark** (default) | Technical diagrams, dev docs, README screenshots |
| **Light** | Presentations, documentation sites, print-friendly |
| **Corporate** | Business plans, pitch decks, stakeholder presentations |
| **Neon** | Creative projects, gaming, social media screenshots |
| **Minimal** | Clean documentation, technical specs |

## Output Features

- **Infinite canvas** — pan (click+drag) and zoom (scroll wheel) like Miro/draw.io
- Professional typography (Syne + JetBrains Mono)
- Animated connections and entrance effects
- Fully responsive (works down to 360px)
- Single HTML file, zero external dependencies
- Color-coded layers with auto-generated legends
- Hover effects on all interactive elements
- Keyboard shortcuts: `+` zoom in, `-` zoom out, `0` reset

## Installation

### Option 1: CLI Install (Recommended)

```bash
npx skills add ferdinandobons/diagram-creator-skill
```

### Option 2: Claude Code Plugin

```bash
/plugin marketplace add ferdinandobons/diagram-creator-skill
/plugin install diagram-creator
```

### Option 3: Clone and Copy

```bash
git clone https://github.com/ferdinandobons/diagram-creator-skill.git
cp -r diagram-creator-skill/skills/diagram-creator ~/.claude/skills/
```

### Option 4: Git Submodule

```bash
git submodule add https://github.com/ferdinandobons/diagram-creator-skill.git .claude/diagram-creator
```

### Option 5: SkillKit (Multi-Agent)

```bash
npx skillkit install ferdinandobons/diagram-creator-skill
```

## Usage

Once installed, just ask Claude naturally:

```
"Create a diagram of my microservices architecture"
"Visualize this docker-compose.yml"
"Turn this business plan into a schema"
"Diagram the OAuth 2.0 flow"
"Make a timeline of the deployment pipeline"
```

Or pass a file directly:

```
"Diagram this file: /path/to/architecture.md"
"Visualize /path/to/docker-compose.yml"
```

## Skill Structure

```
diagram-creator-skill/
├── .claude-plugin/
│   └── plugin.json              # Marketplace manifest
├── skills/
│   └── diagram-creator/
│       ├── SKILL.md             # Core skill instructions
│       ├── references/
│       │   ├── typography-and-colors.md   # Fonts, palette, background
│       │   ├── themes.md                  # 5 color themes (dark, light, corporate, neon, minimal)
│       │   ├── topology-layouts.md        # Layout rules for all 8 topologies
│       │   ├── components.md              # Cards, badges, callouts, animations
│       │   ├── canvas.md                  # Infinite canvas with pan & zoom
│       │   └── safety-rules.md            # Mandatory layout constraints
│       └── examples/
│           ├── oauth2-flow.md             # Left-to-right example
│           ├── kubernetes-networking.md   # Nested example
│           └── saas-microservices.md      # Hub-and-spoke example
└── README.md
```

## Examples

### Nested — Kubernetes Pod Networking
![Nested topology](examples/screenshots/nested.png)

### Left-to-right — OAuth 2.0 Flow
![Left-to-right topology](examples/screenshots/left-to-right.png)

### Hub-and-spoke — SaaS Microservices
![Hub-and-spoke topology](examples/screenshots/hub-and-spoke.png)

### Timeline — Napoleon's Story
![Timeline topology](examples/screenshots/timeline.png)

## Contributing

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/new-topology`)
3. Make your changes
4. Submit a Pull Request

### Adding a new topology

1. Add layout rules in `references/topology-layouts.md`
2. Add an example in `examples/`
3. Update the topology table in `SKILL.md`
4. Test with at least 3 different inputs

## License

MIT
