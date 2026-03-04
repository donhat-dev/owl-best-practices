---
title: Always Use t-key with t-foreach
impact: HIGH
impactDescription: prevents DOM reconciliation bugs and jank
tags: owl, qweb, templates, lists
---

## Always Use t-key with t-foreach

In OWL/QWeb templates, `t-foreach` without a stable `t-key` makes DOM reconciliation fragile (reordering, insert/delete, focus loss).

**Incorrect (no stable identity):**

```xml
<t t-foreach="items" t-as="item">
  <div><t t-esc="item.label"/></div>
</t>
```

**Correct (stable identity):**

```xml
<t t-foreach="items" t-as="item" t-key="item.id">
  <div><t t-esc="item.label"/></div>
</t>
```

Notes:
- `t-key` must be unique and stable across renders.
- Avoid using the loop index as a key for reorderable lists.
