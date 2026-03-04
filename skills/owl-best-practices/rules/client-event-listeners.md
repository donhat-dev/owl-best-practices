---
title: Deduplicate Global Event Listeners
impact: LOW
impactDescription: single listener for N components
tags: client, owl, event-listeners, subscriptions
---

## Deduplicate Global Event Listeners

When many OWL components need the same global event (keydown/resize/visibilitychange), avoid registering duplicate listeners.

Prefer:
- `useExternalListener(target, event, handler)` for DOM events (auto-cleanup)
- `useBus(bus, event, handler)` for app-level events (auto-cleanup)

If you must share a single listener across instances, use a module-level registry.

**Incorrect (N instances = N listeners):**

```javascript
// Each instance registers its own listener (N instances = N listeners)
function registerShortcut(key, callback) {
  const handler = (e) => {
    if (e.metaKey && e.key === key) callback()
  }
  window.addEventListener('keydown', handler)
  return () => window.removeEventListener('keydown', handler)
}
```

When using the `useKeyboardShortcut` hook multiple times, each instance will register a new listener.

**Correct (N instances share one listener):**

```javascript
// Module-level registry: key -> Set(callback)
const callbacksByKey = new Map()

let listenerAttached = false
function attachListenerOnce() {
  if (listenerAttached) return
  listenerAttached = true

  window.addEventListener('keydown', (e) => {
    if (!e.metaKey) return
    const set = callbacksByKey.get(e.key)
    if (!set) return
    for (const cb of set) cb()
  })
}

export function registerShortcut(key, callback) {
  attachListenerOnce()
  if (!callbacksByKey.has(key)) callbacksByKey.set(key, new Set())
  callbacksByKey.get(key).add(callback)

  return () => {
    const set = callbacksByKey.get(key)
    if (!set) return
    set.delete(callback)
    if (set.size === 0) callbacksByKey.delete(key)
  }
}
```

In OWL components, call `registerShortcut()` in `setup()` and ensure the returned cleanup runs in `onWillUnmount()` (or prefer `useExternalListener` for per-component listeners).
