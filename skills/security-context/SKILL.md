---
name: security-context
description: Use ALWAYS when writing any code. Sets the secure-by-default mindset for all development tasks.
---

## Security Context

Every piece of code starts from a secure baseline. These principles apply to all tasks, all languages, all frameworks.

### Principles

1. **Default to secure.** Insecure patterns require explicit justification. Never assume "we'll add security later."

2. **Validate at system boundaries.** Every place data enters your system (user input, API responses, file uploads, database results from untrusted sources) is a validation point.

3. **Never trust the client.** Client-side validation is UX. Server-side validation is security. Always do both.

4. **Least privilege.** Grant the minimum access needed. Default to deny. Whitelist, don't blacklist.

5. **Log actions, never credentials.** Record what happened and who did it. Never log passwords, tokens, API keys, or PII.

6. **Fail closed.** When something goes wrong, deny access. Don't fail into an open/permissive state.

7. **Defense in depth.** Don't rely on a single layer. Validate in the controller AND the service. Check permissions at the route AND the database query.

### Code Review Checklist

Before any code is complete, verify:

- [ ] User input is validated server-side
- [ ] Database queries use parameterized statements
- [ ] Authentication is checked on protected routes
- [ ] Authorization verifies resource ownership
- [ ] Secrets are not hardcoded
- [ ] Error messages don't leak internal details
- [ ] Sensitive data is not logged
- [ ] HTTP responses include security headers

### When Unsure

If a secure approach isn't clear, flag it. Write a `// TODO: SECURITY — verify this is safe` comment and explain the concern. A visible question is better than a hidden vulnerability.