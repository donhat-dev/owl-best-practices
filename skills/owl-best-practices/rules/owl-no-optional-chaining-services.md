---
title: Avoid Optional Chaining for Critical Services (Fail-First)
impact: HIGH
impactDescription: surfaces wiring errors early
tags: owl, fail-fast, services, debugging
---

## Avoid Optional Chaining for Critical Services (Fail-First)

If a component/service requires a dependency, fail loudly when it's missing. Optional chaining (`?.`) can hide wiring bugs and produce partial UI state.

**Incorrect (silently accepts missing service/state):**

```javascript
const node = this.env.workflowEditor?.state?.graph?.nodes?.find((n) => n.id === id)
if (!node) return
```

**Correct (assert and crash early):**

```javascript
const editor = this.env.workflowEditor
if (!editor) {
  throw new Error('workflowEditor service is required')
}

const graph = editor.state.graph
if (!graph) {
  throw new Error('workflowEditor.state.graph is required')
}
```

Notes:
- Use explicit guards for truly optional inputs (user-provided callbacks, feature flags).
- For required dependencies, prefer explicit errors over silent no-ops.
