---
title: Defer State Reads to Usage Point
impact: MEDIUM
impactDescription: avoids unnecessary subscriptions
tags: rerender, owl, browser-state, optimization
---

## Defer State Reads to Usage Point

Don't keep fast-changing or external browser state in reactive component state if you only need it inside a user action.

**Incorrect (stores external state just to use in a click):**

```javascript
export class ShareButton {
  setup() {
    this.state = useState({ ref: new URLSearchParams(window.location.search).get('ref') })
  }

  onShare() {
    this.props.onShare({ ref: this.state.ref })
  }
}
```

**Correct (read on demand at usage point):**

```javascript
export class ShareButton {
  onShare() {
    const ref = new URLSearchParams(window.location.search).get('ref')
    this.props.onShare({ ref })
  }
}
```
