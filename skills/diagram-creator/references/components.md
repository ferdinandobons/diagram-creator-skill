# Components & Animations

## Node Card (all topologies)

```css
.node-card {
  background: var(--panel);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 16px 20px;
  transition: border-color 0.2s;
}
.node-card:hover { border-color: var(--cyan); }
```

Internal structure (top to bottom):
1. Emoji icon: `display: block`, `font-size: 1.4rem`, `margin-bottom: 8px` (omit if node count >= 7)
2. `.node-name`: Syne 700, 0.85rem, white
3. `.node-detail`: JetBrains Mono, 0.7rem, var(--cyan) — for IPs, dates, roles, or any metadata
4. Port badge, status badge, or descriptive note

## Port / Status Badge

```css
.badge-port {
  display: inline-block;
  background: rgba(0,229,160,0.12);
  color: var(--green);
  border: 1px solid rgba(0,229,160,0.3);
  border-radius: 4px;
  padding: 2px 6px;
  font-size: 0.62rem;
}
```

For internal-only nodes: `<div style="font-size:0.65rem; color:var(--dim)">internal only</div>`

## Layer Label

```html
<div class="layer-label">Layer Name · metadata</div>
```

```css
.layer-label::before {
  content: '';
  display: inline-block;
  width: 6px; height: 6px;
  border-radius: 50%;
  background: [accent-color];
  box-shadow: 0 0 8px [accent-color];
  margin-right: 8px;
  vertical-align: middle;
}
```

## Callout / Info Box

```css
.callout {
  background: rgba(0,212,255,0.05);
  border: 1px solid rgba(0,212,255,0.2);
  border-radius: 10px;
  padding: 12px 18px;
  font-size: 0.72rem;
  color: var(--text);
  text-align: center;
  margin-top: 12px;
}
.callout strong { color: var(--cyan); }
```

## Warning / Mechanism Badge

```css
.badge-mechanism {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background: rgba(255,140,66,0.12);
  border: 1px solid rgba(255,140,66,0.35);
  color: var(--orange);
  border-radius: 8px;
  padding: 6px 14px;
  font-size: 0.72rem;
  font-weight: 600;
}
```

## External Client Card

```css
.ext-client {
  background: rgba(255,77,106,0.08);
  border: 1px solid rgba(255,77,106,0.25);
  border-radius: 10px;
  padding: 10px 18px;
  font-size: 0.75rem;
  color: var(--red);
  text-align: center;
}
.ext-client span {
  display: block;
  color: var(--dim);
  font-size: 0.62rem;
  margin-top: 2px;
}
```

---

## Animations

### Entrance (all topologies)
```css
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}
```
Apply: `animation: fadeUp 0.5s ease both; animation-delay: calc(0.1s * [index]);`
Maximum delay: 0.5s. Do not animate more than 8 elements individually.

### Bus line pulse (nested only)
```css
@keyframes pulse-glow {
  0%, 100% { box-shadow: 0 0 6px rgba(45,122,255,0.3); }
  50%       { box-shadow: 0 0 16px rgba(0,212,255,0.7); }
}
```
`animation: pulse-glow 2.5s ease-in-out infinite;`

### SVG dash (hub-and-spoke only)
```html
<animate attributeName="stroke-dashoffset" from="0" to="-8" dur="0.6s" repeatCount="indefinite"/>
```

No other animations. No bounce, spin, scale, or color-shift effects.

---

## Legend

Render at the bottom of `.wrapper`, outside all layer containers.

```css
.legend {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 28px;
}
.legend-item {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.65rem;
  color: var(--dim);
  letter-spacing: 0.05em;
}
.legend-dot {
  width: 8px; height: 8px;
  border-radius: 50%;
  flex-shrink: 0;
}
```

One entry per accent color used. Dot `box-shadow` must match the accent color.

Example: `<div class="legend-dot" style="background:var(--red);box-shadow:0 0 6px var(--red)"></div>`
