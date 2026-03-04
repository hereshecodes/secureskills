---
name: injection-prevention
description: Use when writing database queries, SQL, ORM code, or shell commands
---

## Injection Prevention

Never build queries or commands by concatenating user input. Use parameterized statements or ORM methods.

> Related: input-validation, security-context

### Rule 1: No String Concatenation in SQL

Concatenating user input into SQL is the most exploited vulnerability in web applications.

```sql
-- WRONG
SELECT * FROM users WHERE name = '" + userName + "';

-- RIGHT
SELECT * FROM users WHERE name = ?;
-- Bind userName as parameter
```

### Rule 2: Use ORM Query Builders

ORMs parameterize by default. Use them. Don't drop to raw SQL unless absolutely necessary.

```javascript
// WRONG — raw SQL with string interpolation
db.query(`SELECT * FROM users WHERE email = '${email}'`);

// RIGHT — parameterized query
db.query('SELECT * FROM users WHERE email = ?', [email]);
```

```python
# WRONG — f-string in raw SQL
cursor.execute(f"SELECT * FROM users WHERE name = '{name}'")

# RIGHT — parameterized
cursor.execute("SELECT * FROM users WHERE name = %s", (name,))
```

### Rule 3: No Shell Command Injection

Never pass user input directly to shell commands.

```javascript
// WRONG — user controls the command
exec(`convert ${userFilename} output.png`);

// RIGHT — use allowlists and escape
const safeName = path.basename(userFilename);
execFile('convert', [safeName, 'output.png']);
```

```python
# WRONG
os.system(f"ls {user_input}")

# RIGHT — use subprocess with argument list
subprocess.run(["ls", user_input], check=True)
```

### Rule 4: No LDAP Injection

Escape special characters in LDAP queries.

```
// WRONG
(&(uid={userInput})(userPassword={password}))

// RIGHT — escape LDAP special characters: * ( ) \ NUL
(&(uid={ldap_escape(userInput)})(userPassword={ldap_escape(password)}))
```

### Quick Reference

| Do | Don't |
|----|-------|
| Use parameterized queries | Concatenate user input into SQL |
| Use ORM query builders | Write raw SQL with interpolation |
| Use `execFile` with argument arrays | Use `exec` with string commands |
| Escape LDAP special characters | Pass raw input to directory queries |
| Validate input type before querying | Trust that input is the expected type |