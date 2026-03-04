---
title: Choose the Right Lifecycle Hook
impact: HIGH
impactDescription: avoids blocking renders and DOM timing bugs
tags: owl, lifecycle, hooks
---

## Choose the Right Lifecycle Hook

Use lifecycle hooks for the right kind of work:

- `onWillStart`: async data needed before first render
- `onMounted`: DOM-dependent work (refs, measurements, third-party widgets)
- `onWillUpdateProps`: keep controlled local buffers in sync with incoming props
- `onWillUnmount`: cleanup for manual listeners/timers

**Incorrect (DOM access before mount):**

```javascript
setup() {
  this.rootRef = useRef('root')
  const rect = this.rootRef.el.getBoundingClientRect() // el may be null
}
```

**Correct (DOM access in onMounted):**

```javascript
setup() {
  this.rootRef = useRef('root')
  onMounted(() => {
    const rect = this.rootRef.el.getBoundingClientRect()
    this.onMeasured(rect)
  })
}
```

Notes:
- Avoid long `onWillStart` chains; use `async-parallel` and `async-defer-await`.
