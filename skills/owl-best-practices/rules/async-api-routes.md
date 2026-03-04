---
title: Start Promises Early, Await Late
impact: CRITICAL
impactDescription: 2-10× improvement
tags: async, promises, waterfalls, parallelization
---

## Start Promises Early, Await Late

When you have independent async work (RPC calls, permissions checks, config loads), start it immediately and await as late as possible.

**Incorrect (config waits for auth, data waits for both):**

```javascript
async function loadDashboard(userId) {
  const permissions = await rpc('/my/permissions', { userId })
  const config = await rpc('/my/config')
  const data = await rpc('/my/data', { userId })
  return { permissions, config, data }
}
```

**Correct (auth and config start immediately):**

```javascript
async function loadDashboard(userId) {
  const permissionsPromise = rpc('/my/permissions', { userId })
  const configPromise = rpc('/my/config')

  const permissions = await permissionsPromise
  const dataPromise = rpc('/my/data', { userId: permissions.userId })

  const [config, data] = await Promise.all([configPromise, dataPromise])
  return { permissions, config, data }
}
```

For more complex dependency chains, combine this with `async-dependencies`.
