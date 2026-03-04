---
title: Dynamic Imports for Heavy Components
impact: CRITICAL
impactDescription: directly affects TTI and LCP
tags: bundle, dynamic-import, code-splitting, owl
---

## Dynamic Imports for Heavy Components

Lazy-load large libraries that are not needed for the initial screen (code editors, charting, heavy SVG/icon sets). In OWL, this usually means:

- `import()` on demand, or
- injecting a script tag (CDN) and sharing a module-level loader promise.

**Incorrect (loads heavy editor on every screen):**

```javascript
import { initEditor } from './heavy_editor'

export class CodePanel {
  async setup() {
    await initEditor() // always loaded
  }
}
```

**Correct (load on demand with a shared loader):**

```javascript
let loaderPromise = null

export async function loadEditor() {
  if (loaderPromise) return loaderPromise
  loaderPromise = import('./heavy_editor').then((m) => m)
  return loaderPromise
}

export class CodePanel {
  async onOpen() {
    const editor = await loadEditor()
    editor.mount(this.containerRef.el)
  }
}
```
