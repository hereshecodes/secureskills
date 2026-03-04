---
name: security-headers
description: Use when configuring HTTP responses, middleware, or server settings
---

## Security Headers

Every HTTP response should include security headers. They're your last line of defense and take minutes to add.

> Related: xss-csrf, api-security, security-context

### Rule 1: Content Security Policy (CSP)

Blocks inline scripts, unauthorized sources, and most XSS attacks.

```
// WRONG — no CSP (any script from anywhere can run)

// RIGHT — restrictive CSP
Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self'; connect-src 'self'; frame-ancestors 'none';
```

### Rule 2: Strict-Transport-Security (HSTS)

Forces HTTPS. Prevents downgrade attacks.

```
// WRONG — no HSTS (browser may request HTTP first)

// RIGHT — enforce HTTPS for 1 year, include subdomains
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

### Rule 3: X-Content-Type-Options

Prevents MIME type sniffing. Browser respects the declared Content-Type.

```
X-Content-Type-Options: nosniff
```

### Rule 4: X-Frame-Options

Prevents clickjacking by blocking your site from being embedded in iframes.

```
// Block all framing
X-Frame-Options: DENY

// Or allow only same-origin framing
X-Frame-Options: SAMEORIGIN
```

### Rule 5: Referrer-Policy

Controls how much URL information is sent to other sites.

```
// WRONG — sends full URL including query params
Referrer-Policy: no-referrer-when-downgrade

// RIGHT — only sends origin, not full path
Referrer-Policy: strict-origin-when-cross-origin
```

### Rule 6: Permissions-Policy

Restricts which browser features your site can use.

```
Permissions-Policy: camera=(), microphone=(), geolocation=(), payment=()
```

### Implementation Examples

```javascript
// Node.js / Express — use helmet
const helmet = require('helmet');
app.use(helmet());

// Or set manually
app.use((req, res, next) => {
  res.setHeader('X-Content-Type-Options', 'nosniff');
  res.setHeader('X-Frame-Options', 'DENY');
  res.setHeader('Referrer-Policy', 'strict-origin-when-cross-origin');
  res.setHeader('Permissions-Policy', 'camera=(), microphone=(), geolocation=()');
  res.setHeader('Strict-Transport-Security', 'max-age=31536000; includeSubDomains');
  next();
});
```

```python
# Django — use django-csp and SecurityMiddleware
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_CONTENT_TYPE_NOSNIFF = True
X_FRAME_OPTIONS = 'DENY'
```

### Quick Reference

| Header | Value | Purpose |
|--------|-------|---------|
| Content-Security-Policy | `default-src 'self'` | Blocks unauthorized scripts/resources |
| Strict-Transport-Security | `max-age=31536000` | Forces HTTPS |
| X-Content-Type-Options | `nosniff` | Prevents MIME sniffing |
| X-Frame-Options | `DENY` | Prevents clickjacking |
| Referrer-Policy | `strict-origin-when-cross-origin` | Limits referrer info |
| Permissions-Policy | `camera=(), microphone=()` | Restricts browser features |