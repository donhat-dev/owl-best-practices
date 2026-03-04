---
title: Deduplicate RPC Requests
impact: MEDIUM-HIGH
impactDescription: fewer redundant round trips
tags: client, rpc, caching, promises
---

## Deduplicate RPC Requests

In OWL apps, multiple components/services may request the same data at the same time (mount, panel open, selection change). Deduplicate in-flight requests so you don't issue duplicate RPC calls.

**Incorrect (duplicate concurrent requests):**

```javascript
async function loadUser(id) {
  return this.rpc('/my/user/get', { id })
}

// Called twice before the first resolves -> two requests
const a = loadUser(42)
const b = loadUser(42)
```

**Correct (share one in-flight promise):**

```javascript
const inflight = new Map()

export function dedupRpc(key, fn) {
  if (inflight.has(key)) return inflight.get(key)
  const p = Promise.resolve()
    .then(fn)
    .finally(() => inflight.delete(key))
  inflight.set(key, p)
  return p
}

async function loadUser(id) {
  return dedupRpc(`user:${id}`, () => this.rpc('/my/user/get', { id }))
}
```

Notes:
- Deduplicate only when requests are truly identical (same params, same permissions).
- Combine with field selection (see `payload-minimize-rpc`) to keep responses small.
