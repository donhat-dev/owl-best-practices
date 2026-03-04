---
title: Defer Non-Critical Third-Party Libraries
impact: MEDIUM
impactDescription: loads after hydration
tags: bundle, third-party, analytics, defer
---

## Defer Non-Critical Third-Party Libraries

Analytics, logging, and error tracking don't block user interaction. Load them after the UI is interactive (e.g., in `onMounted`, after first paint, or during idle time).

**Incorrect (loads on startup unconditionally):**

```javascript
import { initAnalytics } from './analytics'

export class App {
  setup() {
    initAnalytics() // runs on every app load
  }
}
```

**Correct (defer until mounted/idle):**

```javascript
import { onMounted } from '@odoo/owl'

export class App {
  setup() {
    onMounted(() => {
      const run = () => import('./analytics').then((m) => m.initAnalytics())
      if ('requestIdleCallback' in window) {
        window.requestIdleCallback(run)
      } else {
        setTimeout(run, 0)
      }
    })
  }
}
```
