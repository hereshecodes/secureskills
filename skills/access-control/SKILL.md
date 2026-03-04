---
name: access-control
description: Use when writing authorization logic, route guards, or resource access
---

## Access Control

Every protected resource must verify that the requesting user has permission. Default to deny.

> Related: authentication, api-security, security-context

### Rule 1: Verify Resource Ownership

Never trust route parameters alone. Verify the user owns or can access the resource.

```javascript
// WRONG — anyone can access any user's data
app.get('/users/:id/settings', async (req, res) => {
  const settings = await getSettings(req.params.id);
  res.json(settings);
});

// RIGHT — verify ownership
app.get('/users/:id/settings', async (req, res) => {
  if (req.params.id !== req.user.id && !req.user.isAdmin) {
    return res.status(403).json({ error: 'Forbidden' });
  }
  const settings = await getSettings(req.params.id);
  res.json(settings);
});
```

### Rule 2: Default to Deny

Protect everything by default. Explicitly mark public routes.

```python
# WRONG — protect individual routes (easy to forget one)
@app.route('/admin')
@login_required
def admin(): ...

# RIGHT — protect everything, whitelist public routes
@app.before_request
def require_auth():
    public = ['/login', '/signup', '/health']
    if request.path not in public and not current_user.is_authenticated:
        return redirect('/login')
```

### Rule 3: Check Permissions in Multiple Layers

Don't rely on route middleware alone. Check in the service layer too.

```javascript
// WRONG — only checked at route level
router.delete('/posts/:id', requireAdmin, deletePost);

// RIGHT — checked at route AND service level
router.delete('/posts/:id', requireAdmin, deletePost);

async function deletePost(postId, requestingUser) {
  const post = await getPost(postId);
  if (!post) throw new NotFoundError();
  if (!requestingUser.isAdmin && post.authorId !== requestingUser.id) {
    throw new ForbiddenError();
  }
  await removePost(postId);
}
```

### Rule 4: Use Role-Based or Policy-Based Access

Don't scatter permission checks as ad-hoc if statements. Centralize them.

```javascript
// WRONG — ad-hoc checks everywhere
if (user.role === 'admin' || user.role === 'editor') { ... }

// RIGHT — centralized policy
const policies = {
  'posts:delete': (user, post) => user.isAdmin || post.authorId === user.id,
  'users:manage': (user) => user.isAdmin,
};

function authorize(action, user, resource) {
  return policies[action]?.(user, resource) ?? false;
}
```

### Rule 5: Never Expose Internal IDs Unnecessarily

Use UUIDs for public-facing identifiers. Sequential IDs reveal data volume and are easily enumerated.

```javascript
// WRONG — sequential IDs in URLs
GET /api/invoices/1042

// RIGHT — UUIDs in URLs
GET /api/invoices/a1b2c3d4-e5f6-7890-abcd-ef1234567890
```

### Quick Reference

| Do | Don't |
|----|-------|
| Verify ownership on every request | Trust route parameters |
| Default to deny, whitelist public routes | Protect routes individually (easy to miss) |
| Check permissions in routes AND services | Rely on a single middleware layer |
| Centralize permission logic | Scatter if-statements across codebase |
| Use UUIDs for public-facing IDs | Expose sequential database IDs |