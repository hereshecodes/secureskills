---
name: authentication
description: Use when implementing login, signup, password reset, or session handling
---

## Authentication

Use established auth libraries. Don't build your own. Enforce strong defaults.

> Related: access-control, secrets-management, security-context

### Rule 1: Never Roll Your Own Auth

Use your framework's auth system. It handles edge cases you haven't thought of.

```javascript
// WRONG — custom password check
if (user.password === inputPassword) { login(); }

// RIGHT — use bcrypt/argon2 with established library
const valid = await bcrypt.compare(inputPassword, user.passwordHash);
if (valid) { createSession(user); }
```

### Rule 2: Hash Passwords Properly

Use bcrypt, argon2, or scrypt. Never MD5, SHA-1, or SHA-256 alone for passwords.

```python
# WRONG — fast hash, no salt
password_hash = hashlib.sha256(password.encode()).hexdigest()

# RIGHT — slow hash with salt
password_hash = bcrypt.hashpw(password.encode(), bcrypt.gensalt(rounds=12))
```

### Rule 3: Store Tokens in HttpOnly Cookies

Never store auth tokens in localStorage or sessionStorage. They're accessible via XSS.

```javascript
// WRONG — accessible to any script on the page
localStorage.setItem('token', jwt);

// RIGHT — HttpOnly cookie, not accessible via JavaScript
res.cookie('session', token, {
  httpOnly: true,
  secure: true,
  sameSite: 'strict',
  maxAge: 3600000
});
```

### Rule 4: Implement Account Lockout

Prevent brute force attacks. Lock after repeated failures.

```javascript
// WRONG — unlimited login attempts
if (!validPassword) return res.status(401).send('Wrong password');

// RIGHT — track failures, lock account
if (user.failedAttempts >= 5) {
  return res.status(423).send('Account locked. Try again in 15 minutes.');
}
```

### Rule 5: Secure Password Reset

Reset tokens must be single-use, time-limited, and random.

```javascript
// WRONG — predictable token
const token = user.id + Date.now();

// RIGHT — cryptographically random, expires in 1 hour
const token = crypto.randomBytes(32).toString('hex');
await saveResetToken(user.id, token, Date.now() + 3600000);
```

### Quick Reference

| Do | Don't |
|----|-------|
| Use framework auth (Passport, NextAuth, Identity) | Build custom auth |
| Hash with bcrypt/argon2 (cost 12+) | Use MD5, SHA-1, or SHA-256 for passwords |
| Store tokens in HttpOnly secure cookies | Store tokens in localStorage |
| Lock accounts after 5 failed attempts | Allow unlimited login attempts |
| Use cryptographic random for reset tokens | Use predictable token values |
| Require current password to change password | Allow password change without verification |