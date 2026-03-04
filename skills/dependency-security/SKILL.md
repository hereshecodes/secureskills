---
name: dependency-security
description: Use when adding packages, updating dependencies, or reviewing lock files
---

## Dependency Security

Every package you install is code you didn't write running in your application. Audit before you add. Update regularly.

> Related: secrets-management, security-context

### Rule 1: Audit Before You Install

Check download count, maintenance status, and known vulnerabilities before adding a dependency.

```bash
# WRONG — install without checking
npm install random-helper-lib

# RIGHT — check first
# 1. Does it have recent commits? (Abandoned packages don't get security patches)
# 2. Does it have known vulnerabilities? (npm audit, snyk, socket.dev)
# 3. Is the download count reasonable? (Very low = untested. Very high = high-value target)
# 4. Does it need to be a dependency? (Can you write 10 lines instead?)
npm info random-helper-lib
npm audit
npm install random-helper-lib
```

### Rule 2: Pin Major Versions

Prevent unexpected breaking changes and supply chain attacks via compromised minor/patch releases.

```json
// WRONG — accepts any compatible version (risky)
"dependencies": {
  "express": "^4.18.0"
}

// RIGHT — pin to specific version
"dependencies": {
  "express": "4.18.2"
}
```

```python
# WRONG — unpinned
requests

# RIGHT — pinned
requests==2.31.0
```

### Rule 3: Run Vulnerability Scans Regularly

Don't wait for a breach to find out your dependencies are vulnerable.

```bash
# JavaScript
npm audit
npx snyk test

# Python
pip-audit
safety check

# .NET
dotnet list package --vulnerable

# Ruby
bundle audit check

# Go
govulncheck ./...
```

### Rule 4: Keep Lock Files Committed

Lock files ensure everyone uses the exact same dependency versions. Never .gitignore them.

```gitignore
# WRONG — ignoring lock files
package-lock.json
yarn.lock

# RIGHT — lock files MUST be committed
# (Don't add them to .gitignore)
```

### Rule 5: Remove Unused Dependencies

Every dependency is attack surface. If you're not using it, remove it.

```bash
# Find unused packages
npx depcheck

# Python
pip-extra-reqs --requirements-file requirements.txt .

# Then remove them
npm uninstall unused-package
```

### Rule 6: Watch for Typosquatting

Attackers publish malicious packages with names similar to popular ones.

```bash
# WRONG — typo installs malicious package
npm install expres      # missing 's'
npm install lodahs      # transposed letters
pip install reqeusts    # typo

# RIGHT — double-check the package name
npm install express     # correct
pip install requests    # correct
```

### Quick Reference

| Do | Don't |
|----|-------|
| Audit packages before installing | Blindly install dependencies |
| Pin major versions in production | Use `^` or `~` ranges for critical deps |
| Run `npm audit` / `pip-audit` regularly | Wait for incidents to check vulnerabilities |
| Commit lock files | Gitignore package-lock.json or yarn.lock |
| Remove unused dependencies | Keep packages "just in case" |
| Verify package names carefully | Rush through install commands |
| Prefer well-maintained packages | Use abandoned or unmaintained libraries |