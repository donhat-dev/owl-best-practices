---
title: Keep Services as Source of Truth
impact: HIGH
impactDescription: prevents state drift and enables undo/redo patterns
tags: owl, services, architecture, state
---

## Keep Services as Source of Truth

In Odoo OWL apps, prefer a service-owned reactive state for shared/editor state. Components read from the service and call service actions for mutations.

**Incorrect (duplicate shared state inside a component):**

```javascript
export class EditorCanvas {
  setup() {
    this.editor = this.env.workflowEditor
    this.state = useState({ nodes: [...this.editor.state.graph.nodes] })
  }
}
```

**Correct (read reactive service state; mutate via actions):**

```javascript
export class EditorCanvas {
  setup() {
    this.editor = this.env.workflowEditor
    this.editorState = useState(this.editor.state)
  }

  onMoveNode(id, pos) {
    this.editor.actions.moveNode(id, pos)
  }
}
```

Notes:
- Keep UI-local state in components (focus, ephemeral drag state) only.
- Put cross-component state (graph, selection, viewport) in a service.
