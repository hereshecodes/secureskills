---
name: accessibility
description: Use when writing frontend UI, HTML, components, forms, dialogs, tooltips, tables, or grids. Enforces WCAG 2.1 AA (ADA Title II, Section 508).
---

## Accessibility (WCAG 2.1 AA)

If it's not usable by keyboard, screen reader, and zoom, it's broken. Criteria map: [wcag-2.1-aa.md](wcag-2.1-aa.md).

> Related: accessibility-testing, input-validation, xss-csrf

### Rule 1: Disabled Controls Explain Why
A `disabled` button can't take focus, so its tooltip reason never reaches keyboard or screen reader users.
```html
<!-- WRONG --> <button disabled title="Term is closed">Edit grades</button>
<!-- RIGHT: focusable, announced as unavailable, reason is real text. Click handler ignores it. -->
<button aria-disabled="true" aria-describedby="edit-why">Edit grades</button>
<p id="edit-why">Grades are locked because the term is closed.</p>
```

### Rule 2: Dialogs Trap Focus and Return It
Focus moves in, stays in, Esc closes, focus returns to the trigger. Same for drawers and menus. Angular Material's MatDialog and MatMenu do this by default; keep `restoreFocus` on.
```javascript
// WRONG: focus stays on the page behind and is lost on close
dialog.hidden = false;
// RIGHT: showModal() traps focus and handles Esc; return focus explicitly
const trigger = document.activeElement; dialog.showModal();
dialog.addEventListener('close', () => trigger.focus(), { once: true });
```

### Rule 3: Tooltips Are Dismissible, Hoverable, Persistent
Show on hover AND focus, stay while the pointer is on the tip, close on Esc.
```javascript
// WRONG: hover only; vanishes before the pointer reaches it
trigger.onmouseenter = show; trigger.onmouseleave = hide;
// RIGHT (trigger has aria-describedby="tip"; tip has role="tooltip")
let t; const keep = () => { clearTimeout(t); show(); }, later = () => { t = setTimeout(hide, 300); };
for (const el of [trigger, tip]) { el.onmouseenter = keep; el.onmouseleave = later; }
trigger.onfocus = keep; trigger.onblur = hide;
document.addEventListener('keydown', (e) => { if (e.key === 'Escape') hide(); });
```

### Rule 4: Tables and Grids Announce Structure and State
Headers, sort direction, selection, and row counts must be announced, not only drawn. Custom grids: `role="grid"`, `columnheader`, `aria-selected`, arrow keys.
```html
<!-- WRONG --> <div class="row head"><div onclick="sort()">Name ▲</div></div>
<!-- RIGHT -->
<table><caption>Students</caption>
  <tr><th scope="col" aria-sort="ascending"><button>Name</button></th><th scope="col">Selected</th></tr>
  <tr><td>Ana Ruiz</td><td><input type="checkbox" aria-label="Select Ana Ruiz" checked></td></tr></table>
<p role="status">Showing 24 of 310 students</p>
```

### Rule 5: Errors Name the Field, the Problem, and the Fix
Color alone is not an error. Delete, dispose, and payment actions get a confirm step or Undo.
```html
<!-- WRONG --> <input id="dob" class="invalid">
<!-- RIGHT -->
<label for="dob">Date of birth</label> <input id="dob" autocomplete="bday" aria-invalid="true" aria-describedby="dob-err">
<p id="dob-err" role="alert">Date of birth must look like 04/09/2012.</p>
```

### Rule 6: Visible Focus and Contrast
Text 4.5:1. Input borders, checkboxes, meaningful icons, and focus rings 3:1.
```css
/* WRONG */ :focus { outline: none; }  input { border: 1px solid #ddd; } /* 1.4:1 */
/* RIGHT */ :focus-visible { outline: 3px solid #1a5fb4; outline-offset: 2px; }
input { border: 1px solid #767676; } /* 4.5:1 on white */
```

### Rule 7: Real Elements, Real Names, Real Page Structure
`<button>` and `<a>` are keyboard accessible for free. Pages need `lang`, `<title>`, a skip link, landmarks, sequential headings.
```html
<!-- WRONG --> <div onclick="save()">Save</div> <button><svg>…</svg></button> <a href="/r.pdf">Click here</a>
<!-- RIGHT -->
<html lang="en"><title>Gradebook: Period 3 | District Portal</title> <a href="#main">Skip to main content</a>
<nav aria-label="Main">…</nav> <main id="main"><h1>Gradebook</h1><h2>Recent activity</h2>
  <button type="button" onclick="save()">Save</button> <button type="button" aria-label="Close"><svg aria-hidden="true">…</svg></button>
  <img src="trend.png" alt="Average rose from 78 to 84"> <img src="rule.svg" alt=""> <a href="/r.pdf">Period 3 report (PDF)</a></main>
```

### Rule 8: Users Control Time and Motion
Warn before a timeout and let users extend it. Anything that moves or auto-updates can be paused.
```javascript
// WRONG: session silently expires and the form is lost
setTimeout(logout, 20 * 60_000);
// RIGHT: warn 2 minutes ahead with a "Stay signed in" button; honor prefers-reduced-motion
setTimeout(() => warnDialog('Your session ends in 2 minutes.', extendSession), 18 * 60_000);
```

### Quick Reference
| Do | Don't |
|----|-------|
| Label every input; visible label text is inside the accessible name; add `autocomplete` | Placeholder as the only label |
| Fluid layout in `rem`; works at 200% text and 320px wide; never lock orientation | Fixed `px` widths and heights on text containers |
| Change context only on an explicit action (button, submit) | Navigate or submit on focus or `select` change |
| Same navigation order and control names on every page | Rename the same action from page to page |
| Generated PDFs: tagged, title, `lang`, reading order. HTML emails follow these rules | Ship untagged PDF exports |
| 44x44px touch targets (stricter than AA) | Tiny tap targets |
