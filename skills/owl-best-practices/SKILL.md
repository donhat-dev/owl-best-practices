---
name: owl-best-practices
description: Best practices for performance and maintainability in Odoo OWL (Odoo 16/17+). Use when writing, reviewing, or refactoring OWL components, QWeb templates, client actions, and service-driven state (EventBus/services/env). Includes OWL/Odoo anti-patterns like missing `t-key`, mixing `t-model` with manual input handlers, and Fail-First service access.
---

# OWL Best Practices

Practical rules for Odoo OWL apps with a focus on high-throughput UIs (canvas editors, large lists, heavy event handling) and service-oriented state.

## Quick Reference

- **Async/RPC**: `async-parallel`, `async-defer-await`, `client-rpc-dedup`
- **OWL architecture**: `owl-service-source-of-truth`, `owl-no-optional-chaining-services`, `owl-global-events-useexternallistener`
- **Templates**: `owl-template-t-key-required`, `owl-t-model-do-not-mix-input-handler`
- **Canvas/perf**: `owl-canvas-raf-throttle`, `owl-dom-measurement-avoid-thrash`, `rendering-content-visibility`
- **JS hot paths**: `js-index-maps`, `js-set-map-lookups`, `js-batch-dom-css`

## How to Use

- Start with `rules/_sections.md` to pick the highest-impact area.
- Read individual rules in `rules/*.md` for the "bad vs good" examples.
