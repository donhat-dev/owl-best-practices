---
title: Conditional Module Loading
impact: HIGH
impactDescription: loads large data only when needed
tags: bundle, conditional-loading, lazy-loading
---

## Conditional Module Loading

Load large data or modules only when a feature is activated.

**Example (lazy-load animation frames):**

```javascript
export class AnimationPlayer {
  setup() {
    this.state = useState({ enabled: false, frames: null })
  }

  async onEnable() {
    this.state.enabled = true
    if (this.state.frames) return
    try {
      const mod = await import('./animation_frames')
      this.state.frames = mod.frames
    } catch {
      this.state.enabled = false
    }
  }
}
```

Notes:
- Keep the initial view fast; activate heavy modules only when the user needs the feature.
