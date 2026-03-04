---
title: Use Explicit Conditional Rendering
impact: LOW
impactDescription: prevents rendering 0 or NaN
tags: rendering, conditional, qweb, owl
---

## Use Explicit Conditional Rendering

In OWL templates, use explicit conditions (comparisons) for conditional blocks and avoid expression patterns that accidentally output `0` or `NaN`.

**Incorrect (expression may output 0):**

```xml
<!-- Avoid embedding boolean-ish expressions that may render numeric 0 -->
<div>
  <t t-out="count and count"/>
</div>
```

**Correct (use explicit condition):**

```xml
<div>
  <t t-if="count &gt; 0">
    <span class="badge" t-esc="count"/>
  </t>
</div>
```
