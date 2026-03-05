# Typography & Color System

## Fonts

Import both fonts via `@import` at the top of the `<style>` block:

```
https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;700&family=Syne:wght@400;700;800
```

| Element | Font | Size | Weight | Extra |
|---|---|---|---|---|
| Title (h1) | Syne | 1.6rem | 800 | One keyword highlighted in `var(--cyan)` |
| Subtitle (.subtitle) | JetBrains Mono | 0.75rem | 400 | uppercase, letter-spacing: 0.1em, color: var(--dim) |
| Layer labels | JetBrains Mono | 0.65rem | 700 | uppercase, letter-spacing: 0.15em |
| Card/node names | Syne | 0.85rem | 700 | color: white |
| Secondary info | JetBrains Mono | 0.65-0.72rem | 400 | |
| Arrow labels | JetBrains Mono | 0.6rem | 400 | uppercase, letter-spacing: 0.08em |

Never use Inter, Roboto, Arial, system-ui, or any sans-serif fallback as primary font.

## Color Palette

Define exactly these CSS custom properties on `:root`. Do not modify values.

```css
:root {
  --bg:     #0a0e1a;   /* page background */
  --panel:  #0f1528;   /* card / inner panel background */
  --border: #1e2d4a;   /* default card border */
  --blue:   #2d7aff;   /* primary accent */
  --cyan:   #00d4ff;   /* secondary accent */
  --green:  #00e5a0;   /* positive / exposed / success */
  --orange: #ff8c42;   /* host layer / warning / NAT */
  --red:    #ff4d6a;   /* external / internet / danger */
  --text:   #c8d8f0;   /* body text */
  --dim:    #4a6080;   /* muted labels */
}
```

### Layer color assignment

- Outermost layer → `var(--red)`
- Second layer → `var(--orange)`
- Third layer → `var(--blue)`
- Fourth layer → `var(--cyan)` (if needed)
- Node hover border → `var(--cyan)`
- Exposed ports / positive states → `var(--green)`

## Background

```css
body {
  background: var(--bg);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px 20px;
  overflow-x: hidden;
}

body::before {
  content: '';
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(rgba(45,122,255,0.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(45,122,255,0.04) 1px, transparent 1px);
  background-size: 40px 40px;
  pointer-events: none;
  z-index: 0;
}

.wrapper {
  position: relative;
  z-index: 1;
  width: 100%;
  max-width: 900px;
}
```
