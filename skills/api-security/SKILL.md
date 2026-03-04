---
name: api-security
description: Use when building REST APIs, GraphQL endpoints, or webhooks
---

## API Security

Every API endpoint needs authentication, rate limiting, input validation, and proper CORS configuration.

> Related: authentication, input-validation, access-control, security-context

### Rule 1: Authenticate Every Endpoint

Default to requiring auth. Explicitly mark public endpoints.

```javascript
// WRONG — endpoint is open to anyone
app.get('/api/users', async (req, res) => {
  res.json(await getUsers());
});

// RIGHT — require authentication
app.get('/api/users', requireAuth, async (req, res) => {
  res.json(await getUsers());
});
```

### Rule 2: Rate Limit All Endpoints

Prevent abuse and brute force attacks. Apply stricter limits to auth endpoints.

```javascript
// WRONG — no rate limiting
app.post('/api/login', loginHandler);

// RIGHT — rate limited (stricter on auth routes)
app.post('/api/login', rateLimit({ windowMs: 15 * 60 * 1000, max: 5 }), loginHandler);
app.use('/api/', rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }));
```

### Rule 3: Configure CORS Restrictively

Never allow all origins in production.

```javascript
// WRONG — allows any website to call your API
app.use(cors({ origin: '*' }));

// RIGHT — whitelist specific origins
app.use(cors({
  origin: ['https://yourdomain.com', 'https://app.yourdomain.com'],
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  credentials: true
}));
```

### Rule 4: Limit Request Size

Prevent denial-of-service via large payloads.

```javascript
// WRONG — no size limit (default can be very large)
app.use(express.json());

// RIGHT — explicit size limit
app.use(express.json({ limit: '1mb' }));
app.use(express.urlencoded({ limit: '1mb', extended: true }));
```

### Rule 5: Don't Leak Error Details in Production

Internal errors reveal architecture to attackers.

```javascript
// WRONG — sends stack trace to client
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message, stack: err.stack });
});

// RIGHT — generic message to client, full details to logs
app.use((err, req, res, next) => {
  logger.error(err);
  res.status(500).json({ error: 'Internal server error' });
});
```

### Rule 6: Validate Content-Type

Reject requests with unexpected content types.

```javascript
// WRONG — accepts anything
app.post('/api/data', handler);

// RIGHT — enforce expected content type
app.post('/api/data', (req, res, next) => {
  if (!req.is('application/json')) {
    return res.status(415).json({ error: 'Content-Type must be application/json' });
  }
  next();
}, handler);
```

### Quick Reference

| Do | Don't |
|----|-------|
| Require auth on all endpoints by default | Leave endpoints open accidentally |
| Rate limit (stricter on auth routes) | Allow unlimited requests |
| Whitelist CORS origins | Use `origin: '*'` in production |
| Limit request body size | Accept unlimited payload sizes |
| Return generic errors to clients | Send stack traces to clients |
| Validate Content-Type headers | Accept any content type |