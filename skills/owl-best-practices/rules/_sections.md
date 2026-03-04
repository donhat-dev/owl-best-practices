# Sections

This file defines all sections, their ordering, impact levels, and descriptions.
The section ID (in parentheses) is the filename prefix used to group rules.

---

## 1. Async & RPC Waterfalls (async)

**Impact:** CRITICAL  
**Description:** Waterfalls are the #1 performance killer. Each sequential await adds full network latency. Eliminating them yields the largest gains.

## 2. Asset Loading & Optional Libraries (bundle)

**Impact:** CRITICAL  
**Description:** Reducing initial bundle size improves Time to Interactive and Largest Contentful Paint.

## 3. Payload & Boundaries (payload)

**Impact:** HIGH  
**Description:** Minimize RPC payloads and data boundaries to reduce latency and browser work.

## 4. Events & Subscriptions (client)

**Impact:** HIGH  
**Description:** Avoid duplicated listeners and expensive handlers; prefer OWL hooks and explicit listener options.

## 5. OWL Architecture & Anti-Patterns (owl)

**Impact:** HIGH  
**Description:** Keep services as the source of truth, avoid silent failures, and use correct OWL/QWeb patterns.

## 6. Reactivity & State Reads (rerender)

**Impact:** MEDIUM  
**Description:** Reduce reactive churn and avoid accidental subscriptions to hot-changing state.

## 7. Rendering Performance (rendering)

**Impact:** MEDIUM  
**Description:** Optimizing the rendering process reduces the work the browser needs to do.

## 8. JavaScript Performance (js)

**Impact:** LOW-MEDIUM  
**Description:** Micro-optimizations for hot paths can add up to meaningful improvements.
