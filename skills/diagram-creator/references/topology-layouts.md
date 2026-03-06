# Topology Layout Rules

Each topology has specific HTML structure and CSS rules. Follow them exactly.

---

## Nested

Hierarchical containment — layers wrapping layers.

### Layer containers
```css
border-radius: 16px;
padding: 20px 24px;
background: rgba([accent-rgb], 0.06);
border: 1px solid rgba([accent-rgb], 0.22);
```
- Use `border-style: dashed` for virtual/logical layers
- Use `border-style: solid` for physical layers

### Layer indentation
- Level 1: `margin: 0`
- Level 2: `margin: 0 20px` (inside level 1's padding)
- Level 3: `margin: 8px 0 0` (nested inside level 2)

### Node row (cards inside innermost layer)
```css
display: flex;
flex-wrap: wrap;          /* REQUIRED */
gap: 16px;
justify-content: center;
align-items: stretch;     /* REQUIRED */
```

### Node cards
- `flex: 1`, `min-width: 140px`, `max-width: 180px`
- If nodes > 5: reduce max-width to 140px
- If node name > 10 chars: `font-size: 0.75rem`, `overflow: hidden`, `text-overflow: ellipsis`, `white-space: nowrap`

### Bus line + drop lines
NEVER use absolute positioning for drop lines. Use CSS Grid:

```css
.drops-row {
  display: grid;
  grid-template-columns: repeat(N, 1fr);  /* N = number of nodes */
  gap: 16px;
  margin: 0;
}
.drop {
  display: flex;
  justify-content: center;
}
```

N must equal the exact number of nodes declared. This guarantees drop lines align with cards.

### Connectors (between layers)
```css
display: flex;
flex-direction: column;
align-items: center;
margin: 0 auto;
```
- Arrow line: `width: 2px`, `height: 28px`, `background: linear-gradient(to bottom, var(--dim), var(--blue))`
- Arrowhead: `::after` with `content: '▼'`, `color: var(--blue)`, `font-size: 10px`
- Label: `font-size: 0.6rem`, uppercase, `background: var(--bg)`, `border: 1px solid var(--border)`, `border-radius: 4px`, `padding: 2px 8px`

### Bus line
```css
height: 3px;
background: linear-gradient(to right, transparent, var(--blue), var(--cyan), var(--blue), transparent);
border-radius: 2px;
animation: pulse-glow 2.5s ease-in-out infinite;
```

---

## Left-to-right

Sequential horizontal flow.

### Outer container
```css
display: flex;
flex-direction: row;
flex-wrap: nowrap;             /* never wrap — infinite canvas handles navigation */
align-items: center;
justify-content: center;
gap: 0;
width: max-content;           /* container grows to fit all steps */
```

### Canvas sizing for left-to-right
Set `#canvas` to `width: max-content` (instead of the default `width: 100%; max-width: Xpx` used by other topologies). This lets the canvas expand to the full flow width. The `fitToView()` function auto-scales to show everything on load.

### Each step box
```css
background: var(--panel);
border: 1px solid var(--border);
border-radius: 12px;
padding: 16px 20px;
flex: 0 1 auto;               /* size from content, not equal distribution */
max-width: 160px;
text-align: center;
transition: border-color 0.2s;
```
Hover: `border-color: var(--cyan)`

### Step count scaling

Adjust step boxes and arrows based on the number of steps. Canvas width is handled automatically via `width: max-content`. Apply these rules automatically.

| Step count | Step max-width | Step padding | Arrow width |
|---|---|---|---|
| 2-4 steps | 160px | 16px 20px | 32px |
| 5-6 steps | 130px | 14px 16px | 24px |
| 7-8 steps | 110px | 12px 14px | 20px |
| 9+ steps | 100px | 10px 12px | 16px |

For 7+ steps, also reduce `font-size` to `0.72rem` on `.node-name` and `0.6rem` on `.node-detail`.

Text inside step boxes uses `word-break: break-word` — text wraps naturally within the box instead of overflowing.

### Arrow between steps
The arrow container MUST use `flex-direction: row` so the line and arrowhead sit side by side horizontally:
```css
.flow-arrow {
  display: flex;
  flex-direction: row;   /* REQUIRED — keeps line and head on same axis */
  align-items: center;
  width: 32px;           /* default — adjust per step count scaling table */
  flex-shrink: 0;
}
```
- Line: `width: calc(100% - 8px)`, `height: 2px`, `background: linear-gradient(to right, var(--dim), var(--blue))`
- Arrowhead: inline element with `content: '▶'`, `color: var(--blue)`, `font-size: 0.7rem`
- NEVER use `flex-direction: column` — it stacks the arrowhead below the line

### Responsive
```css
@media (max-width: 600px) {
  .flow-arrow { transform: rotate(90deg); margin: 8px 0; }
}
```

### Step numbering
Small circle above each step: `24x24px`, `border-radius: 50%`, `background: rgba([accent-rgb], 0.15)`, `border: 1px solid [accent-color]`, `font-size: 0.65rem`, `font-weight: 700`

---

## Hub-and-spoke

Central node connected to N surrounding nodes.

### Layout
```css
.hub-container {
  position: relative;
  width: 100%;
  min-height: 420px;    /* adjust based on spoke count */
}
```

### Central node
- `position: absolute`, `top: 50%`, `left: 50%`, `transform: translate(-50%, -50%)`
- `min-width: 160px`, `z-index: 2`

### Spoke nodes
Position with JavaScript trigonometry:
```javascript
x = cx + radius * Math.cos(angle)
y = cy + radius * Math.sin(angle)
// where angle = (2 * Math.PI / N) * i
```
- `radius = 160px` (increase if nodes overlap — check: if two nodes < 130px apart, increase radius)

### SVG connection lines
```html
<svg style="position:absolute;inset:0;width:100%;height:100%;z-index:1;pointer-events:none">
  <line x1="cx" y1="cy" x2="spoke-x" y2="spoke-y"
        stroke="rgba(45,122,255,0.4)" stroke-width="1.5"
        stroke-dasharray="4 4">
    <animate attributeName="stroke-dashoffset" from="0" to="-8" dur="0.6s" repeatCount="indefinite"/>
  </line>
</svg>
```
Draw one `<line>` per spoke. Coordinates computed in `<script>` after DOM load.

---

## Timeline

Ordered vertical sequence of steps.

### Container
```css
.timeline {
  position: relative;
  width: 100%;
  padding-left: 48px;   /* space for vertical line + step number */
}
```

### Vertical line
```css
.timeline::before {
  content: '';
  position: absolute;
  left: 20px; top: 0; bottom: 0;
  width: 2px;
  background: linear-gradient(to bottom, var(--blue), var(--cyan), transparent);
}
```

### Each step
```css
position: relative;
margin-bottom: 24px;
padding: 16px 20px;
background: var(--panel);
border: 1px solid var(--border);
border-radius: 12px;
```

### Step number circle
```css
position: absolute;
left: -38px; top: 16px;
width: 24px; height: 24px;
background: var(--panel);
border: 2px solid var(--blue);
border-radius: 50%;
display: flex; align-items: center; justify-content: center;
font-size: 0.65rem; font-weight: 700; color: var(--blue);
```

### Status badges
```css
.badge-success { background: rgba(0,229,160,0.12); color: var(--green); border: 1px solid rgba(0,229,160,0.3); }
.badge-active  { background: rgba(45,122,255,0.12); color: var(--blue);  border: 1px solid rgba(45,122,255,0.3); }
.badge-pending { background: rgba(74,96,128,0.12);  color: var(--dim);   border: 1px solid rgba(74,96,128,0.3); }
```
All badges: `border-radius: 4px`, `padding: 2px 8px`, `font-size: 0.62rem`

---

## Grid / Matrix

Comparison tables, feature matrices, skill maps, competitive landscapes.

### Container
```css
.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 16px;
  width: 100%;
}
```
For explicit column counts (e.g., a 3-column comparison): use `grid-template-columns: repeat(N, 1fr)`.

### Column headers (optional)
```css
.grid-header {
  background: rgba([accent-rgb], 0.08);
  border: 1px solid rgba([accent-rgb], 0.25);
  border-radius: 10px;
  padding: 12px 16px;
  text-align: center;
  font-family: 'Syne', sans-serif;
  font-size: 0.85rem;
  font-weight: 700;
  color: white;
}
```

### Grid cells
Use the standard `.node-card` style. Each cell contains:
1. Emoji (optional)
2. Name/label (Syne 700)
3. Value or description (JetBrains Mono, 0.7rem)
4. Optional badge

### Category row (spans full width)
```css
.grid-category {
  grid-column: 1 / -1;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.65rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  color: var(--dim);
  padding: 8px 0 4px;
  border-bottom: 1px solid var(--border);
  margin-top: 8px;
}
```

### Highlighted cells
For cells that need emphasis (e.g., "best option", "recommended"):
```css
.grid-highlight {
  border-color: var(--green);
  background: rgba(0,229,160,0.06);
}
```

---

## Tree / Org Chart

Organizational hierarchies, file trees, decision trees, taxonomy.

### Layout approach
Use nested `<ul>` lists styled as a vertical tree with connecting lines.

### Container
```css
.tree {
  position: relative;
  width: 100%;
  padding-left: 0;
}
.tree ul {
  list-style: none;
  padding-left: 28px;
  position: relative;
}
```

### Tree connectors (CSS-only, no SVG)
```css
.tree li {
  position: relative;
  padding: 6px 0 6px 20px;
}
/* vertical line */
.tree li::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 1px;
  background: var(--border);
}
/* horizontal branch */
.tree li::after {
  content: '';
  position: absolute;
  left: 0;
  top: 18px;
  width: 16px;
  height: 1px;
  background: var(--border);
}
/* hide vertical line after last child */
.tree li:last-child::before {
  bottom: calc(100% - 18px);
}
```

### Tree node card
```css
.tree-node {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: var(--panel);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 8px 14px;
  transition: border-color 0.2s;
}
.tree-node:hover { border-color: var(--cyan); }
```

Internal structure: emoji + name (Syne 700) + optional role/detail (JetBrains Mono, dim)

### Depth-based accent colors
- Level 0 (root): `border-left: 3px solid var(--red)`
- Level 1: `border-left: 3px solid var(--orange)`
- Level 2: `border-left: 3px solid var(--blue)`
- Level 3+: `border-left: 3px solid var(--cyan)`

---

## Funnel

Sales funnels, conversion pipelines, filtering processes, data reduction.

### Layout
A vertical stack of progressively narrower boxes, centered.

### Container
```css
.funnel {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  width: 100%;
}
```

### Funnel stages
```css
.funnel-stage {
  position: relative;
  text-align: center;
  padding: 16px 24px;
  border-radius: 8px;
  margin-bottom: 4px;
  transition: transform 0.2s;
  min-height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.funnel-stage:hover { transform: scale(1.02); }
```

### Width progression (N stages)
Each stage gets progressively narrower. Calculate widths:
```
Stage 1 (top):    width: 100%
Stage 2:          width: 85%
Stage 3:          width: 70%
Stage 4:          width: 55%
Stage 5 (bottom): width: 40%
```
Formula: `width = 100% - ((index / (N-1)) * 60%)`
Minimum width: 35%.

### Stage styling
Each stage gets a background with the accent color at low opacity:
```css
background: rgba([accent-rgb], 0.08);
border: 1px solid rgba([accent-rgb], 0.25);
```

Color progression top-to-bottom: red → orange → blue → cyan → green

### Conversion arrows between stages
```css
.funnel-arrow {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2px 0;
}
```
Arrow line: `width: 2px`, `height: 16px`, gradient.
Arrowhead: `content: '▼'`.

### Conversion rate labels (optional)
Between stages, show percentage:
```css
.conversion-label {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.6rem;
  color: var(--dim);
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: 4px;
  padding: 1px 6px;
}
```

---

## Comparison / VS

Side-by-side comparison of 2-3 options. Product comparisons, technology choices, before/after.

### Layout
```css
.comparison {
  display: grid;
  grid-template-columns: repeat(N, 1fr);  /* N = number of items being compared */
  gap: 20px;
  width: 100%;
}
```
Max 3 columns. On mobile (`max-width: 600px`): stack to `grid-template-columns: 1fr`.

### Column card
```css
.compare-column {
  background: var(--panel);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
```

### Column header
```css
.compare-header {
  text-align: center;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--border);
}
```
Contains: emoji (1.6rem) + name (Syne 800, 1rem) + subtitle (JetBrains Mono, dim)

### Highlighted (recommended) column
```css
.compare-column.recommended {
  border-color: var(--cyan);
  box-shadow: 0 0 20px rgba(0,212,255,0.1);
}
.compare-column.recommended::before {
  content: 'RECOMMENDED';
  display: block;
  text-align: center;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.58rem;
  font-weight: 700;
  color: var(--cyan);
  letter-spacing: 0.15em;
  margin-bottom: 8px;
}
```

### Feature rows inside columns
```css
.compare-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 6px 0;
  border-bottom: 1px solid rgba(30,45,74,0.4);
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.68rem;
}
.compare-row:last-child { border-bottom: none; }
.compare-label { color: var(--dim); }
.compare-value { color: var(--text); font-weight: 600; }
```

### VS divider (between 2 columns)
```css
.vs-badge {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: var(--bg);
  border: 2px solid var(--orange);
  border-radius: 50%;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Syne', sans-serif;
  font-weight: 800;
  font-size: 0.7rem;
  color: var(--orange);
  z-index: 3;
}
```
Only show VS badge when comparing exactly 2 items. For 3+ items, omit it.
