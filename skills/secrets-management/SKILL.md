---
name: secrets-management
description: Use when handling API keys, passwords, connection strings, or credentials
---

## Secrets Management

No secret should ever appear in source code, logs, or error messages. Use environment variables or a vault.

> Related: authentication, dependency-security, security-context

### Rule 1: Never Hardcode Secrets

Not in code. Not in config files committed to git. Not in comments.

```javascript
// WRONG — API key in source code
const apiKey = 'sk-1234567890abcdef';

// RIGHT — from environment variable
const apiKey = process.env.API_KEY;
if (!apiKey) throw new Error('API_KEY environment variable is required');
```

```python
# WRONG — password in settings file
DATABASE_PASSWORD = "hunter2"

# RIGHT — from environment
DATABASE_PASSWORD = os.environ["DATABASE_PASSWORD"]
```

### Rule 2: Add Secrets to .gitignore

Prevent accidental commits. These files should never be tracked.

```gitignore
# MUST be in .gitignore
.env
.env.local
.env.production
*.pem
*.key
credentials.json
service-account.json
```

### Rule 3: Use Different Secrets Per Environment

Dev, staging, and production must use separate credentials.

```bash
# WRONG — same API key everywhere
API_KEY=sk-production-key  # used in dev too

# RIGHT — environment-specific secrets
# .env.development
API_KEY=sk-dev-key-safe-to-rotate

# Production: set via hosting provider, vault, or CI/CD secrets
```

### Rule 4: Rotate Secrets on Exposure

If a secret appears in a commit, log, or error message, it's compromised. Rotate immediately.

```bash
# If a secret was committed:
# 1. Rotate the secret FIRST (revoke old key, generate new)
# 2. Then clean git history (but assume the old secret is compromised)
# Cleaning history alone is NOT sufficient — bots scrape commits in real time
```

### Rule 5: Never Log Secrets

Sanitize log output. Mask sensitive values.

```javascript
// WRONG — logs the full connection string
logger.info(`Connecting to ${connectionString}`);

// RIGHT — mask sensitive parts
logger.info(`Connecting to ${connectionString.replace(/\/\/.*@/, '//***@')}`);
```

```python
# WRONG — logs the API key
logging.info(f"Using key: {api_key}")

# RIGHT — log that a key exists, not the value
logging.info(f"Using key: {api_key[:4]}...{api_key[-4:]}")
```

### Quick Reference

| Do | Don't |
|----|-------|
| Use environment variables | Hardcode secrets in source |
| Add `.env` to `.gitignore` | Commit secret files to git |
| Use separate secrets per environment | Share production keys in dev |
| Rotate immediately on exposure | Assume cleaning git history is enough |
| Mask secrets in logs | Log full connection strings or keys |
| Validate that required secrets exist at startup | Silently fail with undefined values |