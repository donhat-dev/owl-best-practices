---
title: Subscribe to Derived State
impact: MEDIUM
impactDescription: reduces re-render frequency
tags: rerender, derived-state, media-query, optimization
---

## Subscribe to Derived State

Subscribe to derived boolean state instead of continuous values to reduce re-render frequency.

**Incorrect (re-renders on every pixel change):**

```javascript
export class Sidebar {
  setup() {
    this.state = useState({ width: window.innerWidth })
    window.addEventListener('resize', () => {
      this.state.width = window.innerWidth // re-renders constantly while resizing
    })
  }

  get isMobile() {
    return this.state.width < 768
  }
}
```

**Correct (re-renders only when boolean changes):**

```javascript
export class Sidebar {
  setup() {
    this.state = useState({ isMobile: window.matchMedia('(max-width: 767px)').matches })
    const mq = window.matchMedia('(max-width: 767px)')
    const onChange = () => { this.state.isMobile = mq.matches }
    mq.addEventListener('change', onChange)
    onWillUnmount(() => mq.removeEventListener('change', onChange))
  }
}
```
