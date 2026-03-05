# Theme System

The diagram supports 5 themes. The user can request a theme by name, or you pick the best default based on context (dark for technical, light for presentations, corporate for business docs).

If the user doesn't specify a theme, default to **dark**.

## How Themes Work

All themes use the same CSS variable names. Only the `:root` values change. The rest of the CSS (layout, typography, components) stays identical. Apply the theme by replacing the `:root` block.

The background grid pattern opacity and card styles adapt per theme via the variables below.

---

## Theme: Dark (default)

Best for: technical diagrams, developer docs, README screenshots.

```css
:root {
  --bg:     #0a0e1a;
  --panel:  #0f1528;
  --border: #1e2d4a;
  --blue:   #2d7aff;
  --cyan:   #00d4ff;
  --green:  #00e5a0;
  --orange: #ff8c42;
  --red:    #ff4d6a;
  --text:   #c8d8f0;
  --dim:    #4a6080;
  --grid-opacity: 0.04;
  --title-color: #ffffff;
}
```

Grid: `rgba(45,122,255, var(--grid-opacity))`

---

## Theme: Light

Best for: presentations, documentation sites, print-friendly output.

```css
:root {
  --bg:     #f8f9fc;
  --panel:  #ffffff;
  --border: #e2e6f0;
  --blue:   #2563eb;
  --cyan:   #0891b2;
  --green:  #059669;
  --orange: #d97706;
  --red:    #dc2626;
  --text:   #334155;
  --dim:    #94a3b8;
  --grid-opacity: 0.06;
  --title-color: #0f172a;
}
```

Grid: `rgba(37,99,235, var(--grid-opacity))`

Additional overrides for light theme:
```css
.node-card { box-shadow: 0 1px 3px rgba(0,0,0,0.08); }
.node-card:hover { box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
.callout { background: rgba(8,145,178,0.05); border-color: rgba(8,145,178,0.2); }
```

---

## Theme: Corporate

Best for: business plans, pitch decks, stakeholder presentations.

```css
:root {
  --bg:     #f1f5f9;
  --panel:  #ffffff;
  --border: #cbd5e1;
  --blue:   #1e40af;
  --cyan:   #0369a1;
  --green:  #15803d;
  --orange: #b45309;
  --red:    #b91c1c;
  --text:   #1e293b;
  --dim:    #64748b;
  --grid-opacity: 0.03;
  --title-color: #0f172a;
}
```

Grid: `rgba(30,64,175, var(--grid-opacity))`

Additional overrides:
```css
body::before { display: none; } /* no grid background — cleaner look */
.node-card { border-radius: 8px; box-shadow: 0 1px 2px rgba(0,0,0,0.06); }
h1 { font-size: 1.4rem; }
```

---

## Theme: Neon

Best for: creative projects, gaming, cyberpunk aesthetics, social media screenshots.

```css
:root {
  --bg:     #0a0a0f;
  --panel:  #12121a;
  --border: #2a2a3d;
  --blue:   #6366f1;
  --cyan:   #22d3ee;
  --green:  #34d399;
  --orange: #fb923c;
  --red:    #f43f5e;
  --text:   #e2e8f0;
  --dim:    #6b7280;
  --grid-opacity: 0.06;
  --title-color: #ffffff;
}
```

Grid: `rgba(99,102,241, var(--grid-opacity))`

Additional overrides:
```css
.node-card:hover { box-shadow: 0 0 20px rgba(34,211,238,0.2); }
.bus-line { background: linear-gradient(to right, transparent, var(--blue), var(--cyan), #a78bfa, var(--blue), transparent); }
h1 .highlight { text-shadow: 0 0 20px rgba(34,211,238,0.5); }
```

---

## Theme: Minimal

Best for: clean documentation, technical specs, when the diagram should fade into the background.

```css
:root {
  --bg:     #ffffff;
  --panel:  #fafafa;
  --border: #e5e5e5;
  --blue:   #3b82f6;
  --cyan:   #06b6d4;
  --green:  #10b981;
  --orange: #f59e0b;
  --red:    #ef4444;
  --text:   #404040;
  --dim:    #a3a3a3;
  --grid-opacity: 0;
  --title-color: #171717;
}
```

Additional overrides:
```css
body::before { display: none; } /* no grid */
.node-card { border-radius: 6px; padding: 12px 16px; }
.node-card:hover { border-color: var(--blue); } /* blue instead of cyan */
@keyframes fadeUp { from { opacity: 0; } to { opacity: 1; } } /* no translateY — subtler */
```

---

## Applying a Theme

In the SKILL.md workflow, the theme is selected in Step 1 (planning). The user may say:
- "light theme" / "tema chiaro"
- "corporate style" / "for a presentation"
- "neon" / "cyberpunk"
- "minimal" / "clean"

Map these to the theme name, then apply the corresponding `:root` block and any additional overrides. Everything else (layout, components, typography families) stays the same.
