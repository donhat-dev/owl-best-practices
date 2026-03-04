---
title: Do Not Mix t-model with Manual Input Handlers
impact: HIGH
impactDescription: prevents desync and hard-to-debug input bugs
tags: owl, qweb, forms, t-model
---

## Do Not Mix t-model with Manual Input Handlers

`t-model` is two-way binding. Mixing it with `t-on-input` or manual value writes often causes double updates and desynchronization.

**Incorrect (two sources of truth):**

```xml
<input
  t-model="state.query"
  t-on-input="onInput"
/>
```

```javascript
onInput(ev) {
  this.state.query = ev.target.value
}
```

**Correct (pick one):**

```xml
<!-- Option A: keep t-model only -->
<input t-model="state.query"/>
```

```xml
<!-- Option B: handle input manually -->
<input t-att-value="state.query" t-on-input="onInput"/>
```

Notes:
- Use `t-model.*` modifiers (`.trim`, `.number`, `.lazy`) when needed.
