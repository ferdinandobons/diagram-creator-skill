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
    <button id="download-png" title="Download as PNG">⬇ PNG</button>
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

#zoom-reset,
#download-png {
  width: auto;
  padding: 0 12px;
  font-size: 0.6rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

#download-png {
  margin-left: 8px;
  border-color: var(--green);
  color: var(--green);
}
#download-png:hover {
  border-color: var(--green);
  background: rgba(0,229,160,0.08);
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
    translateX = Math.max(0, (cw - dw) / 2);
    translateY = Math.max(20, (ch - dh) / 2);
    applyTransform();
  }

  // Initial centering after content renders
  requestAnimationFrame(() => {
    requestAnimationFrame(centerCanvas);
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
    scale = 1;
    centerCanvas();
  });

  // Keyboard shortcuts
  window.addEventListener('keydown', (e) => {
    if (e.key === '=' || e.key === '+') { scale = Math.min(MAX_SCALE, scale + ZOOM_STEP); applyTransform(); }
    if (e.key === '-') { scale = Math.max(MIN_SCALE, scale - ZOOM_STEP); applyTransform(); }
    if (e.key === '0') { scale = 1; centerCanvas(); }
  });

  // Download as PNG
  document.getElementById('download-png').addEventListener('click', async () => {
    const btn = document.getElementById('download-png');
    btn.textContent = '...';
    btn.disabled = true;

    try {
      // Temporarily reset transform for clean capture
      const prevTransform = canvas.style.transform;
      canvas.style.transform = 'none';
      canvas.style.position = 'relative';

      // Use html2canvas-like approach with native canvas API
      const rect = canvas.getBoundingClientRect();
      const dpr = window.devicePixelRatio || 2;
      const w = canvas.scrollWidth;
      const h = canvas.scrollHeight;

      // Create offscreen canvas
      const offscreen = document.createElement('canvas');
      offscreen.width = w * dpr;
      offscreen.height = h * dpr;
      const ctx = offscreen.getContext('2d');
      ctx.scale(dpr, dpr);

      // Render via SVG foreignObject
      const svgData = `
        <svg xmlns="http://www.w3.org/2000/svg" width="${w}" height="${h}">
          <foreignObject width="100%" height="100%">
            <div xmlns="http://www.w3.org/1999/xhtml">
              ${canvas.outerHTML}
            </div>
          </foreignObject>
        </svg>`;

      const svgBlob = new Blob([svgData], { type: 'image/svg+xml;charset=utf-8' });
      const url = URL.createObjectURL(svgBlob);
      const img = new Image();

      img.onload = () => {
        // Draw background color first
        const bgColor = getComputedStyle(document.body).backgroundColor || '#0a0e1a';
        ctx.fillStyle = bgColor;
        ctx.fillRect(0, 0, w, h);
        ctx.drawImage(img, 0, 0, w, h);

        // Trigger download
        const a = document.createElement('a');
        a.download = document.title.replace(/[^a-z0-9]/gi, '-').toLowerCase() + '.png';
        a.href = offscreen.toDataURL('image/png');
        a.click();

        URL.revokeObjectURL(url);
        canvas.style.transform = prevTransform;
        canvas.style.position = 'absolute';
        btn.textContent = '\u2B07 PNG';
        btn.disabled = false;
      };

      img.onerror = () => {
        // Fallback: use simpler approach without foreignObject
        // Create a clean SVG export instead
        canvas.style.transform = prevTransform;
        canvas.style.position = 'absolute';
        btn.textContent = '\u2B07 PNG';
        btn.disabled = false;

        // Fallback: open print dialog (user can save as PDF/image)
        window.print();
      };

      img.src = url;
    } catch (err) {
      canvas.style.transform = prevTransform;
      canvas.style.position = 'absolute';
      btn.textContent = '\u2B07 PNG';
      btn.disabled = false;
      window.print();
    }
  });
})();
```

## Behavior

- **Pan**: click and drag anywhere on the canvas (cursor changes to grab/grabbing)
- **Zoom**: scroll wheel zooms toward cursor position
- **Touch**: single-finger drag to pan (mobile/tablet)
- **Buttons**: +/−/Reset controls in bottom-right corner
- **Keyboard**: `+` zoom in, `-` zoom out, `0` reset view
- **Download**: click the green "PNG" button to download the diagram as a PNG image
- **Initial state**: diagram is centered in the viewport at scale 1

## Integration Notes

- The `.wrapper` class now lives on `#canvas` instead of being a direct child of body
- `body` no longer uses `justify-content: center` — the canvas JS handles centering
- The grid background (`body::before`) stays fixed and is NOT affected by pan/zoom — this creates a nice parallax effect
- All existing topology JS (hub-and-spoke coordinate calculation) runs before the canvas JS
- The canvas system works identically across all themes
