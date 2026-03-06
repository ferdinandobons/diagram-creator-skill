# Mandatory Layout Safety Rules

These rules apply to EVERY topology. No exceptions.

## 1. Wrapper Overflow
```css
.wrapper { overflow-x: auto; min-width: 320px; }
```
Never let content escape the wrapper horizontally.

## 2. Flex Wrap Required
All flex rows must have `flex-wrap: wrap` unless the total content width is provably under 320px.

## 3. No Absolute Positioning for Repeated Elements
Never use `position: absolute` to place cards, nodes, or labels — UNLESS the topology is hub-and-spoke (which requires it for coordinate calculation).
For nested and left-to-right: use flex/grid only.

## 4. Drop Lines Use CSS Grid
(Nested topology only)
`grid-template-columns: repeat(N, 1fr)` where N = exact node count.
This is the only way to guarantee alignment with cards below.

## 5. Minimum Layer Height
All layers must have `min-height: 80px`. Prevents visual collapse.

## 6. Long Text Handling
If any node name or label exceeds 12 characters:
```css
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap;
```

## 7. Node Count Scaling (nested, hub-and-spoke, grid, tree, funnel, comparison)
| Node count | max-width | font-size | Emoji |
|---|---|---|---|
| 2-4 nodes | 180px | default | visible |
| 5-6 nodes | 150px | 0.78rem | visible |
| 7+ nodes | 130px | 0.72rem | hidden |

Apply automatically based on declared node count.

## 7b. Step Count Scaling (left-to-right only)
Left-to-right flows must scale step boxes, arrows, and canvas width to prevent wrapping on desktop. See `topology-layouts.md` for the full scaling table. Summary:

| Step count | Step max-width | Arrow width | Canvas max-width |
|---|---|---|---|
| 2-4 steps | 160px | 32px | 900px |
| 5-6 steps | 130px | 24px | 1200px |
| 7-8 steps | 110px | 20px | 1400px |
| 9+ steps | 100px | 16px | 1600px |

Apply automatically based on declared step count. The canvas max-width override applies to `#canvas` for left-to-right topology only.

## 8. SVG Restriction
SVG lines are for hub-and-spoke topology ONLY.
Never use SVG to draw lines in nested or left-to-right layouts.
Never use CSS box-shadow or border tricks to simulate diagonal lines.

## 9. Z-Index Discipline
| Element | z-index |
|---|---|
| body::before (grid background) | 0 |
| .wrapper | 1 |
| SVG lines (hub-and-spoke) | 1 |
| Cards / nodes | 2 |
| Tooltips / badges | 3 |

## 10. Responsive Minimum
The diagram must not break at viewport width 360px.
All content must reflow without horizontal scroll at that width.

## Output Requirements
- Single `.html` file
- All CSS in one `<style>` block
- All JS (if any) in one `<script>` block
- No external dependencies except Google Fonts (via `@import` in CSS)
- No CSS frameworks (Tailwind, Bootstrap, etc.)
- No JS libraries (jQuery, D3, etc.) — vanilla JS for hub-and-spoke coordinate calculation only
- Valid HTML5 — `<!DOCTYPE html>` must be the first line
- Must render correctly when opened directly in a browser without a local server
