---
title: Minimize RPC Payloads and Data Boundaries
impact: HIGH
impactDescription: reduces data transfer size
tags: payload, rpc, serialization, odoo
---

## Minimize RPC Payloads and Data Boundaries

In Odoo, RPC responses and service state become UI inputs. Over-fetching increases latency, memory usage, and render work. Request and store only fields the UI actually uses.

**Incorrect (fetches/keeps everything):**

```javascript
// Fetches a large record but UI uses only a few fields.
const record = await this.rpc('/my/model/read', { ids: [id] })
this.state.record = record
```

**Correct (request/store only what you render):**

```javascript
const record = await this.rpc('/my/model/read', {
  ids: [id],
  fields: ['name', 'email'],
})

this.state.record = {
  name: record.name,
  email: record.email,
}
```

Notes:
- Prefer field lists for ORM/RPC reads.
- Avoid storing full server payloads in reactive state if only a small slice is rendered.
