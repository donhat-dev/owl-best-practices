---
title: Dependency-Based Parallelization
impact: CRITICAL
impactDescription: 2-10× improvement
tags: async, parallelization, dependencies
---

## Dependency-Based Parallelization

For operations with partial dependencies, start independent work early and only await what you need when you need it. This avoids hidden waterfalls.

**Incorrect (profile waits for config unnecessarily):**

```javascript
const [user, config] = await Promise.all([
  rpc('/my/user'),
  rpc('/my/config'),
])
const profile = await rpc('/my/profile', { userId: user.id })
```

**Correct (config and profile run in parallel):**

```javascript
const userPromise = rpc('/my/user')
const configPromise = rpc('/my/config')

const user = await userPromise
const profilePromise = rpc('/my/profile', { userId: user.id })

const [config, profile] = await Promise.all([
  configPromise,
  profilePromise,
])
```

Notes:
- This pattern applies to OWL `onWillStart()` data loading and to service actions that fan out into multiple RPC calls.
