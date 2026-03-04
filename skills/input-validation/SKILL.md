---
name: input-validation
description: Use when processing user input, form data, file uploads, or query parameters
---

## Input Validation

Validate everything that enters your system. Server-side. Every time. Client-side validation is UX, not security.

> Related: injection-prevention, xss-csrf, api-security, security-context

### Rule 1: Validate Server-Side (Always)

Client-side validation can be bypassed with one curl command.

```javascript
// WRONG — only client-side validation
<input type="email" required>  // Attacker skips this entirely

// RIGHT — validate on the server
function createUser(req, res) {
  const { email, name } = req.body;
  if (!email || !isValidEmail(email)) return res.status(400).json({ error: 'Valid email required' });
  if (!name || name.length > 100) return res.status(400).json({ error: 'Name required, max 100 chars' });
  // proceed
}
```

### Rule 2: Whitelist, Don't Blacklist

Reject everything except what you expect. Blacklists always miss something.

```javascript
// WRONG — trying to block bad characters
if (input.includes('<script>')) reject();

// RIGHT — only allow expected format
if (!/^[a-zA-Z0-9\s\-]{1,100}$/.test(input)) reject();
```

### Rule 3: Validate Type, Length, and Range

Every field has constraints. Enforce them.

```python
# WRONG — no validation
age = request.form['age']
save_user(age=age)

# RIGHT — validate type, range, and length
age = request.form.get('age')
if not age or not age.isdigit() or not (0 <= int(age) <= 150):
    return bad_request('Age must be a number between 0 and 150')
save_user(age=int(age))
```

### Rule 4: Validate File Uploads

Check extension, MIME type, and file signature. Never trust the filename.

```javascript
// WRONG — trusts the file extension
if (file.originalname.endsWith('.jpg')) saveFile(file);

// RIGHT — check MIME type AND magic bytes
const allowedMimes = ['image/jpeg', 'image/png', 'image/webp'];
if (!allowedMimes.includes(file.mimetype)) reject();

const fileBuffer = fs.readFileSync(file.path);
const type = await fileTypeFromBuffer(fileBuffer);
if (!type || !allowedMimes.includes(type.mime)) reject();
```

### Rule 5: Sanitize Before Storage, Escape Before Display

Two different operations. Both required.

```javascript
// Sanitize on input (remove dangerous content)
const cleanHtml = DOMPurify.sanitize(userInput);
await saveToDb(cleanHtml);

// Escape on output (prevent XSS in different contexts)
element.textContent = storedValue;  // HTML context
```

### Quick Reference

| Do | Don't |
|----|-------|
| Validate server-side on every request | Rely on client-side validation alone |
| Whitelist expected formats | Blacklist known bad patterns |
| Enforce type, length, and range limits | Accept any value from the client |
| Check file MIME type AND magic bytes | Trust file extensions |
| Sanitize on input, escape on output | Skip either step |
| Return clear error messages | Silently accept bad input |