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
flex-wrap: wrap;            /* REQUIRED for narrow screens */
align-items: center;
justify-content: center;
gap: 0;
width: 100%;
```

### Each step box
```css
background: var(--panel);
border: 1px solid var(--border);
border-radius: 12px;
padding: 16px 20px;
min-width: 120px;
max-width: 160px;
flex: 1;
text-align: center;
transition: border-color 0.2s;
```
Hover: `border-color: var(--cyan)`

### Arrow between steps
- `width: 32px`, `height: 2px`
- `background: linear-gradient(to right, var(--dim), var(--blue))`
- `::after` arrowhead: `content: '▶'`, `color: var(--blue)`

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
