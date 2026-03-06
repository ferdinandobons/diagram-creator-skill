# Infinite Canvas — Pan & Zoom

Every diagram gets an infinite canvas that lets the user navigate freely, like Miro or draw.io. This is implemented with vanilla JS — no libraries.

## HTML Structure

Wrap the entire diagram inside a canvas container:

```html
<body>
  <!-- Grid background stays fixed via body::before -->
  <div id="canvas-container">
    <div id="canvas" class="wrapper">
      <!-- ALL diagram content goes here -->
    </div>
  </div>
  <div class="canvas-controls">
    <button id="zoom-in" title="Zoom in">+</button>
    <button id="zoom-reset" title="Reset view">Reset</button>
    <button id="zoom-out" title="Zoom out">−</button>
  </div>
</body>
```

## CSS

```css
#canvas-container {
  position: fixed;
  inset: 0;
  overflow: hidden;
  cursor: grab;
  z-index: 1;
}

#canvas-container.grabbing {
  cursor: grabbing;
}

#canvas {
  position: absolute;
  transform-origin: 0 0;
  /* Remove centering from body — the canvas handles positioning */
  /* width and max-width are kept for content sizing */
}

/* Override body to not center — canvas does it */
body {
  padding: 0;
  justify-content: flex-start;
  align-items: flex-start;
}

/* Controls */
.canvas-controls {
  position: fixed;
  bottom: 20px;
  right: 20px;
  display: flex;
  gap: 4px;
  z-index: 10;
}

.canvas-controls button {
  width: 36px;
  height: 36px;
  border: 1px solid var(--border);
  background: var(--panel);
  color: var(--text);
  border-radius: 8px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.85rem;
  font-weight: 700;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: border-color 0.2s, background 0.2s;
}

.canvas-controls button:hover {
  border-color: var(--cyan);
  background: rgba(0,212,255,0.08);
}

#zoom-reset {
  width: auto;
  padding: 0 12px;
  font-size: 0.6rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}
```

## JavaScript

Place this at the end of the `<script>` block, after any topology-specific JS (like hub-and-spoke coordinate calculations).

```javascript
// Infinite Canvas — Pan & Zoom
(function() {
  const container = document.getElementById('canvas-container');
  const canvas = document.getElementById('canvas');

  let scale = 1;
  let translateX = 0;
  let translateY = 0;
  let isPanning = false;
  let startX, startY;

  const MIN_SCALE = 0.2;
  const MAX_SCALE = 3;
  const ZOOM_STEP = 0.1;

  function applyTransform() {
    canvas.style.transform = `translate(${translateX}px, ${translateY}px) scale(${scale})`;
  }

  function centerCanvas() {
    const cw = container.clientWidth;
    const ch = container.clientHeight;
    const dw = canvas.scrollWidth * scale;
    const dh = canvas.scrollHeight * scale;
    translateX = (cw - dw) / 2;
    translateY = Math.max(20, (ch - dh) / 2);
    applyTransform();
  }

  // Fit entire diagram into the viewport, scaling down if needed
  function fitToView() {
    const cw = container.clientWidth;
    const ch = container.clientHeight;
    const dw = canvas.scrollWidth;
    const dh = canvas.scrollHeight;
    const scaleX = (cw - 40) / dw;
    const scaleY = (ch - 40) / dh;
    scale = Math.min(scaleX, scaleY, 1);
    scale = Math.max(scale, MIN_SCALE);
    centerCanvas();
  }

  // Initial fit after content renders
  requestAnimationFrame(() => {
    requestAnimationFrame(fitToView);
  });

  // Pan — mouse
  container.addEventListener('mousedown', (e) => {
    if (e.button !== 0) return; // left click only
    isPanning = true;
    startX = e.clientX - translateX;
    startY = e.clientY - translateY;
    container.classList.add('grabbing');
  });

  window.addEventListener('mousemove', (e) => {
    if (!isPanning) return;
    translateX = e.clientX - startX;
    translateY = e.clientY - startY;
    applyTransform();
  });

  window.addEventListener('mouseup', () => {
    isPanning = false;
    container.classList.remove('grabbing');
  });

  // Pan — touch
  container.addEventListener('touchstart', (e) => {
    if (e.touches.length === 1) {
      isPanning = true;
      startX = e.touches[0].clientX - translateX;
      startY = e.touches[0].clientY - translateY;
    }
  }, { passive: true });

  container.addEventListener('touchmove', (e) => {
    if (!isPanning || e.touches.length !== 1) return;
    translateX = e.touches[0].clientX - startX;
    translateY = e.touches[0].clientY - startY;
    applyTransform();
  }, { passive: true });

  container.addEventListener('touchend', () => { isPanning = false; });

  // Zoom — scroll wheel (zoom toward cursor)
  container.addEventListener('wheel', (e) => {
    e.preventDefault();
    const rect = container.getBoundingClientRect();
    const mouseX = e.clientX - rect.left;
    const mouseY = e.clientY - rect.top;

    const prevScale = scale;
    if (e.deltaY < 0) {
      scale = Math.min(MAX_SCALE, scale + ZOOM_STEP);
    } else {
      scale = Math.max(MIN_SCALE, scale - ZOOM_STEP);
    }

    // Adjust translate so zoom centers on cursor position
    const ratio = scale / prevScale;
    translateX = mouseX - ratio * (mouseX - translateX);
    translateY = mouseY - ratio * (mouseY - translateY);

    applyTransform();
  }, { passive: false });

  // Button controls
  document.getElementById('zoom-in').addEventListener('click', () => {
    scale = Math.min(MAX_SCALE, scale + ZOOM_STEP);
    applyTransform();
  });

  document.getElementById('zoom-out').addEventListener('click', () => {
    scale = Math.max(MIN_SCALE, scale - ZOOM_STEP);
    applyTransform();
  });

  document.getElementById('zoom-reset').addEventListener('click', () => {
    fitToView();
  });

  // Keyboard shortcuts
  window.addEventListener('keydown', (e) => {
    if (e.key === '=' || e.key === '+') { scale = Math.min(MAX_SCALE, scale + ZOOM_STEP); applyTransform(); }
    if (e.key === '-') { scale = Math.max(MIN_SCALE, scale - ZOOM_STEP); applyTransform(); }
    if (e.key === '0') { fitToView(); }
  });

})();
```

## Behavior

- **Pan**: click and drag anywhere on the canvas (cursor changes to grab/grabbing)
- **Zoom**: scroll wheel zooms toward cursor position
- **Touch**: single-finger drag to pan (mobile/tablet)
- **Buttons**: +/−/Reset controls in bottom-right corner
- **Keyboard**: `+` zoom in, `-` zoom out, `0` fit to view
- **Initial state**: diagram is auto-scaled to fit entirely within the viewport (scales down if content is wider/taller than viewport, stays at scale 1 if it fits)

## Integration Notes

- The `.wrapper` class now lives on `#canvas` instead of being a direct child of body
- `body` no longer uses `justify-content: center` — the canvas JS handles centering
- The grid background (`body::before`) stays fixed and is NOT affected by pan/zoom — this creates a nice parallax effect
- All existing topology JS (hub-and-spoke coordinate calculation) runs before the canvas JS
- The canvas system works identically across all themes
