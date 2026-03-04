---
title: Defer Await Until Needed
impact: HIGH
impactDescription: avoids blocking unused code paths
tags: async, await, conditional, optimization
---

## Defer Await Until Needed

Move `await` operations into the branches where they're actually used to avoid blocking code paths that don't need them. This is especially valuable when the awaited work is an RPC call.

**Incorrect (blocks both branches):**

```javascript
async function handleRequest(userId, skipProcessing) {
  const userData = await rpc('/my/user_data', { userId })
  
  if (skipProcessing) {
    // Returns immediately but still waited for userData
    return { skipped: true }
  }
  
  // Only this branch uses userData
  return processUserData(userData)
}
```

**Correct (only blocks when needed):**

```javascript
async function handleRequest(userId, skipProcessing) {
  if (skipProcessing) {
    // Returns immediately without waiting
    return { skipped: true }
  }
  
  // Fetch only when needed
  const userData = await rpc('/my/user_data', { userId })
  return processUserData(userData)
}
```

**Another example (early return optimization):**

```javascript
// Incorrect: always fetches permissions
async function updateResource(resourceId, userId) {
  const permissions = await rpc('/my/permissions', { userId })
  const resource = await rpc('/my/resource', { resourceId })
  
  if (!resource) {
    return { error: 'Not found' }
  }
  
  if (!permissions.canEdit) {
    return { error: 'Forbidden' }
  }
  
  return rpc('/my/resource/update', { resourceId, permissions })
}

// Correct: fetches only when needed
async function updateResource(resourceId, userId) {
  const resource = await rpc('/my/resource', { resourceId })
  
  if (!resource) {
    return { error: 'Not found' }
  }
  
  const permissions = await rpc('/my/permissions', { userId })
  
  if (!permissions.canEdit) {
    return { error: 'Forbidden' }
  }
  
  return rpc('/my/resource/update', { resourceId, permissions })
}
```

This optimization is especially valuable when the skipped branch is frequently taken, or when the deferred operation is expensive.
