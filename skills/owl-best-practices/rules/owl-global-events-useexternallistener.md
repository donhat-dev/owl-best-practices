---
title: Prefer useExternalListener for Global DOM Events
impact: HIGH
impactDescription: avoids leaks and duplicate listeners
tags: owl, events, dom, lifecycle
---

## Prefer useExternalListener for Global DOM Events

For global DOM events (document/window), prefer `useExternalListener` so OWL handles cleanup.

**Incorrect (manual add/remove is easy to forget):**

```javascript
onMounted(() => {
  document.addEventListener('keydown', this.onKeyDown)
})
```

**Correct (auto-cleaned on unmount):**

```javascript
setup() {
  useExternalListener(window, 'keydown', (ev) => this.onKeyDown(ev))
}
```

Notes:
- For event listener options (passive/capture), you may need manual `addEventListener` with cleanup.
- Prefer one listener per surface when possible (see `client-event-listeners`).
