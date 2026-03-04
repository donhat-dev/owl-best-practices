---
title: Promise.all() for Independent Operations
impact: CRITICAL
impactDescription: 2-10× improvement
tags: async, parallelization, promises, waterfalls
---

## Promise.all() for Independent Operations

When async operations have no interdependencies, execute them concurrently using `Promise.all()`. This is especially important for Odoo RPC calls.

**Incorrect (sequential execution, 3 round trips):**

```javascript
const user = await this.rpc('/my/user')
const posts = await this.rpc('/my/posts')
const comments = await this.rpc('/my/comments')
```

**Correct (parallel execution, 1 round trip):**

```javascript
const [user, posts, comments] = await Promise.all([
  this.rpc('/my/user'),
  this.rpc('/my/posts'),
  this.rpc('/my/comments'),
])
```
