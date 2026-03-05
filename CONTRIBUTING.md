# Contributing to Diagram Creator Skill

Thank you for your interest in contributing to the Diagram Creator skill. This guide covers everything you need to know to submit a quality contribution.

## Table of Contents

- [Requesting a Feature or New Topology](#requesting-a-feature-or-new-topology)
- [Adding a New Topology](#adding-a-new-topology)
- [Improving the Existing Skill](#improving-the-existing-skill)
- [Naming Conventions](#naming-conventions)
- [Skill Quality Checklist](#skill-quality-checklist)
- [Submitting Your Contribution](#submitting-your-contribution)

---

## Requesting a Feature or New Topology

If you have an idea but are not ready to implement it yourself:

1. Open a **GitHub Issue** using the appropriate template.
2. For new topologies, describe the layout pattern, provide a real-world use case, and include a rough sketch or reference image if possible.
3. For other features, explain the problem you are solving and the expected behavior.

The maintainers will review, label, and prioritize your request.

---

## Adding a New Topology

Adding a topology requires changes across multiple files. Follow these steps carefully.

### 1. Define Layout Rules

Create or update the layout specification in:

```
skills/diagram-creator/references/topology-layouts.md
```

Your layout definition must include:
- Container structure and nesting rules
- Spacing and alignment specifications
- Connection/arrow routing logic
- Responsive behavior at smaller viewports

### 2. Add an Example

Create a new example file in:

```
skills/diagram-creator/examples/
```

The example should demonstrate the topology with a realistic use case (not a trivial "hello world" diagram). Use an existing example as a template for the expected format.

### 3. Update SKILL.md

Add the new topology to the topology table in `skills/diagram-creator/SKILL.md`. Include the topology name, a short description, and its ideal use cases.

### 4. Test with Multiple Inputs

Before submitting, verify that the topology produces correct output with **at least three different inputs** that vary in:
- Number of nodes (small, medium, large)
- Content length (short labels vs. long descriptions)
- Connection complexity

Open each generated HTML file directly in a browser and confirm it renders correctly, the infinite canvas works, and the layout does not break.

---

## Improving the Existing Skill

When making improvements to the existing skill:

1. **Read the existing skill thoroughly.** Start with `SKILL.md`, then review the relevant files in `references/` and `examples/`.
2. **Test changes locally.** Generate diagrams before and after your change to confirm the improvement and catch regressions.
3. **Keep changes minimal.** A focused, well-tested change is far more likely to be accepted than a sweeping rewrite. If your improvement touches multiple areas, consider splitting it into separate pull requests.
4. **Respect the design system.** The files in `references/` define a battle-tested design system. If you need to modify them, explain why in your PR description.

---

## Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Skill directory | lowercase with hyphens | `diagram-creator` |
| Skill `name` field | must match directory name exactly | `name: diagram-creator` |
| Reference files | lowercase with hyphens, `.md` extension | `topology-layouts.md` |
| Example files | lowercase with hyphens, `.md` extension | `oauth2-flow.md` |

---

## Skill Quality Checklist

Before submitting, verify every item:

- [ ] **YAML frontmatter is valid** — `name` and `description` fields are present and correctly formatted
- [ ] **`name` matches directory** — The `name` field in SKILL.md frontmatter matches the skill directory name exactly
- [ ] **Description is clear** — The `description` field explains what the skill does in one or two sentences
- [ ] **Instructions are actionable** — SKILL.md contains concrete, step-by-step instructions that an AI agent can follow without ambiguity
- [ ] **No sensitive data** — No API keys, passwords, personal information, or proprietary content is included anywhere
- [ ] **SKILL.md is under 500 lines** — Detailed specs belong in `references/`, not in the main instruction file
- [ ] **Output is self-contained** — Generated HTML files include all CSS and JS inline with no external dependencies (except Google Fonts)

---

## Submitting Your Contribution

### 1. Fork and Branch

1. Fork this repository.
2. Create a descriptive branch from `main`:
   - `feat/topology-swimlane` for new topologies
   - `fix/timeline-overlap` for bug fixes
   - `docs/update-examples` for documentation changes

### 2. Commit and Push

Write clear commit messages that explain *what* changed and *why*. Keep commits atomic — one logical change per commit.

### 3. Open a Pull Request

Use the appropriate PR template:

| Template | Use When |
|----------|----------|
| `new-topology.md` | Adding a new topology layout |
| `skill-update.md` | Modifying skill instructions, references, or behavior |
| `documentation.md` | Updating README, examples, or other documentation |

Fill out every section of the template. PRs with incomplete templates may be delayed.

### 4. Review Process

- A maintainer will review your PR, typically within a few days.
- Address any requested changes promptly.
- Once approved, your contribution will be merged into `main`.

---

Thank you for helping make the Diagram Creator skill better.
