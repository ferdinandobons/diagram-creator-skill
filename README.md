# Diagram Creator — Claude Code Skill

Turn any input into beautiful, production-ready architecture diagrams. Self-contained HTML files with dark theme, professional typography, smooth animations, and zero dependencies.

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

## 4 Topology Layouts

| Layout | Best for | Examples |
|---|---|---|
| **Nested** | Hierarchical containment | Docker networking, K8s pods, VPCs, OSI model |
| **Left-to-right** | Sequential flows | OAuth flow, CI/CD pipeline, user journey |
| **Hub-and-spoke** | Central node + surrounding nodes | API gateway, microservices, load balancer |
| **Timeline** | Ordered vertical steps | Deploy pipeline, project roadmap, historical events |

## Output Features

- Dark theme with professional typography (Syne + JetBrains Mono)
- Animated connections and entrance effects
- Fully responsive (works down to 360px)
- Single HTML file, zero external dependencies
- Color-coded layers with auto-generated legends
- Hover effects on all interactive elements

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
│       │   ├── topology-layouts.md        # Layout rules per topology
│       │   ├── components.md              # Cards, badges, callouts, animations
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
