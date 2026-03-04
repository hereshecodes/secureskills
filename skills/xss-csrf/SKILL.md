---
name: xss-csrf
description: Use when rendering user content, building forms, or handling POST requests
---

## XSS & CSRF Prevention

Never render unsanitized user content. Always include anti-forgery tokens on state-changing requests.

> Related: input-validation, security-headers, security-context

### Rule 1: Never Insert Raw User Content Into HTML

Use your framework's auto-escaping. If you bypass it, sanitize first.

```javascript
// WRONG — innerHTML with user content
element.innerHTML = userComment;

// RIGHT — use textContent (auto-escapes)
element.textContent = userComment;

// If you MUST render HTML, sanitize first
import DOMPurify from 'dompurify';
element.innerHTML = DOMPurify.sanitize(userComment);
```

```python
# WRONG — marking user input as safe in Jinja2
{{ user_comment | safe }}

# RIGHT — auto-escaped by default
{{ user_comment }}
```

### Rule 2: Escape Output Based on Context

HTML context, attribute context, JavaScript context, and URL context all need different escaping.

```javascript
// WRONG — user input in an attribute without escaping
`<img src="${userUrl}" alt="${userName}">`

// RIGHT — encode for HTML attributes
`<img src="${encodeURI(userUrl)}" alt="${escapeHtml(userName)}">`
```

### Rule 3: Include Anti-Forgery Tokens on All Forms

Every POST, PUT, DELETE request must include a CSRF token.

```html
<!-- WRONG — form without CSRF token -->
<form method="POST" action="/delete">
  <button type="submit">Delete</button>
</form>

<!-- RIGHT — CSRF token included -->
<form method="POST" action="/delete">
  <input type="hidden" name="_csrf" value="{{csrfToken}}">
  <button type="submit">Delete</button>
</form>
```

### Rule 4: Set SameSite on Cookies

Prevent CSRF by restricting cookie behavior.

```javascript
// WRONG — no SameSite attribute
res.cookie('session', token);

// RIGHT — SameSite restricts cross-origin requests
res.cookie('session', token, {
  sameSite: 'strict',
  httpOnly: true,
  secure: true
});
```

### Rule 5: Set Content Security Policy

CSP is your last line of defense against XSS. It blocks inline scripts and unauthorized sources.

```
// WRONG — no CSP header (any script can run)

// RIGHT — restrictive CSP
Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:;
```

### Quick Reference

| Do | Don't |
|----|-------|
| Use framework auto-escaping | Insert raw user content with innerHTML |
| Sanitize with DOMPurify if rendering HTML | Mark user input as "safe" in templates |
| Include CSRF tokens on all forms | Submit state-changing requests without tokens |
| Set `SameSite: strict` on cookies | Leave cookies without SameSite attribute |
| Set Content Security Policy header | Allow inline scripts from any source |