# Changelog

## 1.1.0 (unreleased)

### Accessibility: WCAG 2.1 AA

- **accessibility**: rewritten around WCAG 2.1 AA. New rules for disabled controls that explain why, dialogs that trap and return focus, dismissible and hoverable tooltips, accessible tables and grids, errors that name the field and the fix, non-text contrast and visible focus, and session timeouts. March rules (labels, button names, semantic HTML, color, headings, keyboard, live regions, reduced motion, contrast, touch targets) are kept, merged to stay under 100 lines.
- **accessibility-testing** (new): axe-core with `@axe-core/playwright` in CI, Lighthouse, a keyboard pass, a screen reader pass (NVDA, VoiceOver), zoom and reflow at 200% and 400%, contrast, PDF and media checks, and what automated tools can't catch.
- **accessibility/wcag-2.1-aa.md** (new): all 50 WCAG 2.1 Level A and AA success criteria, each mapped to the rule or test that covers it. A starting point for a VPAT / Accessibility Conformance Report.

## 1.0.0 (2026-03-03)

- Initial release: 11 security skills for AI coding agents.
