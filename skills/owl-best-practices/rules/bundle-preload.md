---
title: Preload Based on User Intent
impact: MEDIUM
impactDescription: reduces perceived latency
tags: bundle, preload, user-intent, hover
---

## Preload Based on User Intent

Preload heavy bundles before they're needed to reduce perceived latency.

**Example (preload on hover/focus):**

```javascript
function preloadEditor() {
  void import('./heavy_editor')
}

// In OWL template:
// <button t-on-mouseenter="preloadEditor" t-on-focus="preloadEditor" t-on-click="onOpen">Open editor</button>
```

**Example (preload when feature flag is enabled):**

```javascript
import { onMounted } from '@odoo/owl'

export class App {
  setup() {
    onMounted(() => {
      if (this.env.flags && this.env.flags.editorEnabled) {
        void import('./heavy_editor')
      }
    })
  }
}
```
