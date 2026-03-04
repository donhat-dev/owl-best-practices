---
title: CSS content-visibility for Long Lists
impact: HIGH
impactDescription: faster initial render
tags: rendering, css, content-visibility, long-lists
---

## CSS content-visibility for Long Lists

Apply `content-visibility: auto` to defer off-screen rendering.

**CSS:**

```css
.message-item {
  content-visibility: auto;
  contain-intrinsic-size: 0 80px;
}
```

**Example (OWL/QWeb list):**

```xml
<div class="message-list">
  <t t-foreach="messages" t-as="msg" t-key="msg.id">
    <div class="message-item">
      <span t-esc="msg.author"/>
      <span t-esc="msg.content"/>
    </div>
  </t>
</div>
```

For 1000 messages, browser skips layout/paint for ~990 off-screen items (10× faster initial render).
