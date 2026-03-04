---
title: Use Passive Event Listeners for Scrolling Performance
impact: MEDIUM
impactDescription: eliminates scroll delay caused by event listeners
tags: client, event-listeners, scrolling, performance, touch, wheel
---

## Use Passive Event Listeners for Scrolling Performance

Add `{ passive: true }` to touch and wheel event listeners to enable immediate scrolling. Browsers normally wait for listeners to finish to check if `preventDefault()` is called, causing scroll delay.

If you call `preventDefault()` (common for custom zoom/pan on a canvas), you must use `{ passive: false }`.

**Incorrect:**

```javascript
const handleTouch = (e) => console.log(e.touches[0].clientX)
const handleWheel = (e) => console.log(e.deltaY)

document.addEventListener('touchstart', handleTouch)
document.addEventListener('wheel', handleWheel)

// Cleanup on unmount
// document.removeEventListener('touchstart', handleTouch)
// document.removeEventListener('wheel', handleWheel)
```

**Correct:**

```javascript
const handleTouch = (e) => console.log(e.touches[0].clientX)
const handleWheel = (e) => console.log(e.deltaY)

document.addEventListener('touchstart', handleTouch, { passive: true })
document.addEventListener('wheel', handleWheel, { passive: true })

// If you call preventDefault in a wheel/touch handler:
// document.addEventListener('wheel', onWheel, { passive: false })
```

In OWL components, prefer `useExternalListener(target, eventName, handler)` and keep the listener options explicit when performance-sensitive.

**Use passive when:** tracking/analytics, logging, any listener that doesn't call `preventDefault()`.

**Don't use passive when:** implementing custom swipe gestures, custom zoom controls, or any listener that needs `preventDefault()`.
