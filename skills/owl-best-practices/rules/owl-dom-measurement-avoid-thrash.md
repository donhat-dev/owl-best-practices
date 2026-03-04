---
title: Avoid DOM Measurement Thrash in Hot Paths
impact: HIGH
impactDescription: prevents forced reflow and layout jank
tags: owl, dom, performance, svg
---

## Avoid DOM Measurement Thrash in Hot Paths

DOM reads like `getBoundingClientRect()`, SVG `getTotalLength()`, and `getPointAtLength()` can force layout. Avoid calling them per mousemove/wheel/frame.

**Incorrect (measure every time):**

```javascript
function onDrag(ev) {
  const rect = this.rootRef.el.getBoundingClientRect()
  this.onDragAt(ev.clientX - rect.left, ev.clientY - rect.top)
}
```

**Correct (separate reads from writes; reuse/caches):**

```javascript
let cachedRect = null

onMounted(() => {
  cachedRect = this.rootRef.el.getBoundingClientRect()
})

function onDrag(ev) {
  const rect = cachedRect || this.rootRef.el.getBoundingClientRect()
  this.onDragAt(ev.clientX - rect.left, ev.clientY - rect.top)
}
```

Notes:
- Re-measure on resize/zoom changes, not on every event.
- For SVG path measurements, reuse a single offscreen SVG element and cache results by a stable key.
