---
name: accessibility
description: Use when writing frontend UI, HTML, components, or forms
---

## Accessibility (WCAG 2.1 AA)

Accessibility is not optional. Every component, every page, every form. If it's not usable for everyone, it's broken.

> Related: input-validation, xss-csrf, security-context

### Rule 1: Every Input Needs a Label

No exceptions. Screen readers cannot describe unlabeled inputs.

```html
<!-- WRONG — input with no label -->
<input type="email" placeholder="Email">

<!-- RIGHT — label with matching for/id -->
<label for="email">Email</label>
<input id="email" type="email" autocomplete="email" aria-required="true">
```

### Rule 2: Buttons Describe Their Action

Icon-only buttons must have an aria-label. "X" is not a description.

```html
<!-- WRONG — screen reader says "button" -->
<button><svg>...</svg></button>

<!-- RIGHT — screen reader says "Close dialog" -->
<button aria-label="Close dialog"><svg aria-hidden="true">...</svg></button>
```

### Rule 3: Use Semantic HTML

Use the right element for the job. Divs are not buttons. Spans are not links.

```html
<!-- WRONG — div pretending to be a button -->
<div class="btn" onclick="save()">Save</div>

<!-- RIGHT — actual button (keyboard accessible, focusable, announced correctly) -->
<button type="button" onclick="save()">Save</button>
```

### Rule 4: Color Is Not the Only Indicator

15-20% of users have some form of color vision deficiency.

```html
<!-- WRONG — only color indicates error -->
<span style="color: red">Error</span>

<!-- RIGHT — icon + color + role for screen readers -->
<span role="alert" class="error">
  <svg aria-hidden="true">...</svg> Error: Email is required
</span>
```

### Rule 5: Maintain Heading Hierarchy

Never skip heading levels. One h1 per page.

```html
<!-- WRONG — skips h2 -->
<h1>Dashboard</h1>
<h3>Recent Activity</h3>

<!-- RIGHT — sequential levels -->
<h1>Dashboard</h1>
<h2>Recent Activity</h2>
```

### Rule 6: All Interactive Elements Are Keyboard Accessible

Tab, Enter, Escape, Arrow keys. If you can click it, you can keyboard it.

```javascript
// WRONG — only handles click
element.addEventListener('click', handler);

// RIGHT — handles click AND keyboard
element.addEventListener('click', handler);
element.addEventListener('keydown', (e) => {
  if (e.key === 'Enter' || e.key === ' ') handler(e);
});
// Or better: use <button> which handles this automatically
```

### Rule 7: Announce Dynamic Content

When content changes without a page load, screen readers need to know.

```html
<!-- Status updates -->
<div role="status" aria-live="polite">3 results found</div>

<!-- Error alerts -->
<div role="alert">Payment failed. Please try again.</div>
```

### Rule 8: Respect Motion Preferences

Some users get physically ill from animations.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

### Quick Reference

| Do | Don't |
|----|-------|
| Label every form input | Use placeholder as the only label |
| Add aria-label to icon-only buttons | Leave buttons without accessible names |
| Use `<button>` and `<a>` for interactions | Use `<div onclick>` or `<span onclick>` |
| Pair color with icons/text | Use color as the only indicator |
| Keep heading hierarchy sequential | Skip heading levels |
| Support keyboard navigation | Make click-only interactions |
| Use `aria-live` for dynamic updates | Silently change content |
| Respect `prefers-reduced-motion` | Auto-play animations |
| Minimum 44x44px touch targets | Make tiny interactive elements |
| 4.5:1 contrast ratio for text | Use low-contrast text |