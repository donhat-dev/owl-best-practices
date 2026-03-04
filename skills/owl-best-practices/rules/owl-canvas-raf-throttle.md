---
title: Throttle Hot Event Handlers with requestAnimationFrame
impact: HIGH
impactDescription: reduces event storm re-renders and DOM work
tags: owl, canvas, raf, events
---

## Throttle Hot Event Handlers with requestAnimationFrame

For wheel, mousemove, and drag handlers, batch work into a single `requestAnimationFrame` tick.

**Incorrect (runs on every event):**

```javascript
onWheel(ev) {
  ev.preventDefault()
  this.editor.actions.zoomTo(this.editor.state.ui.viewport.zoom * 1.1)
}
```

**Correct (one update per frame):**

```javascript
let frame = null

onWheel(ev) {
  ev.preventDefault()
  if (frame) return
  frame = requestAnimationFrame(() => {
    frame = null
    const zoom = this.editor.state.ui.viewport.zoom
    this.editor.actions.zoomTo(zoom * 1.1)
  })
}
```

Notes:
- Keep wheel/touch listeners correct: if you call `preventDefault`, register with `{ passive: false }`.
- Consider batching multi-step mutations with history batching if you have undo/redo.
