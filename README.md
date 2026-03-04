# SecureSkills

**Security skills for AI coding agents. Install once, write secure code every time.**

SecureSkills is a collection of security-focused skills that make AI coding agents (Claude Code, Cursor, Windsurf, Codex) enforce secure coding patterns automatically. No configuration. No remembering to ask. The rules are always loaded.

## Install

```bash
npx skills add hereshecodes/secureskills
```

Or clone into your global skills folder:

```bash
# macOS/Linux
git clone https://github.com/hereshecodes/secureskills.git ~/.claude/skills/secureskills

# Windows
git clone https://github.com/hereshecodes/secureskills.git %USERPROFILE%\.claude\skills\secureskills
```

## Skills

| Skill | What It Enforces |
|-------|-----------------|
| [security-context](skills/security-context/SKILL.md) | Foundation. Secure-by-default mindset loaded on every task. |
| [injection-prevention](skills/injection-prevention/SKILL.md) | SQL injection, command injection, ORM misuse |
| [authentication](skills/authentication/SKILL.md) | Passwords, sessions, tokens, account lockout, MFA |
| [access-control](skills/access-control/SKILL.md) | IDOR, role checks, ownership verification, default-deny |
| [xss-csrf](skills/xss-csrf/SKILL.md) | Output encoding, anti-forgery tokens, safe rendering |
| [secrets-management](skills/secrets-management/SKILL.md) | No hardcoded secrets, env vars, vault patterns |
| [api-security](skills/api-security/SKILL.md) | Rate limiting, CORS, auth headers, input limits |
| [input-validation](skills/input-validation/SKILL.md) | Server-side validation, file uploads, type safety |
| [accessibility](skills/accessibility/SKILL.md) | WCAG 2.1 AA, semantic HTML, keyboard nav, screen readers |
| [security-headers](skills/security-headers/SKILL.md) | CSP, HSTS, X-Frame-Options, Referrer-Policy |
| [dependency-security](skills/dependency-security/SKILL.md) | Vulnerability scanning, pinned versions, supply chain |

## How It Works

Skills are markdown files with YAML frontmatter. Your AI agent reads them and follows the rules when the task matches the trigger description.

```
secureskills/
└── skills/
    ├── security-context/SKILL.md      ← Always loaded (foundation)
    ├── injection-prevention/SKILL.md  ← Loaded when writing queries
    ├── authentication/SKILL.md        ← Loaded when building auth
    ├── access-control/SKILL.md        ← Loaded when checking permissions
    └── ...
```

The **security-context** skill loads on every task and sets the baseline: default to secure, validate at boundaries, never trust client input. The other 10 skills trigger when you're working in their domain.

Skills cross-reference each other. When you're building an API endpoint, `api-security` loads and points you to `input-validation` and `authentication` for related rules.

## Every Rule Follows This Pattern

```
// WRONG — the insecure way
vulnerable code example

// RIGHT — the secure way
safe code example
```

No theory. No paragraphs. Just patterns your AI agent can follow.

## Framework Agnostic

Examples use JavaScript, Python, and generic SQL. The patterns apply to any stack:

- **Node.js / Express** — input validation, JWT handling, helmet headers
- **Python / Django / Flask** — ORM queries, CSRF tokens, secret management
- **React / Vue / Angular** — XSS prevention, safe rendering, accessibility
- **.NET / C#** — parameterized queries, anti-forgery, Identity framework
- **Go / Rust / Java** — the same principles, different syntax

## Per-Project or Global

**Global** (applies to all your projects):
```bash
git clone https://github.com/hereshecodes/secureskills.git ~/.claude/skills/secureskills
```

**Per-project** (committed to one repo):
```bash
git clone https://github.com/hereshecodes/secureskills.git .claude/skills/secureskills
```

**Team-wide** (everyone gets the same rules):
```bash
# Add as a git submodule
git submodule add https://github.com/hereshecodes/secureskills.git .claude/skills/secureskills
```

## Contributing

See [CONTRIBUTING.md](.github/CONTRIBUTING.md). We accept new skills, improved examples, and bug fixes. Keep skills under 100 lines. Every rule needs a code example.

## License

MIT

---

Built by [Danielle Hoopes](https://hereshecodes.com) | [Blog](https://hereshecodes.com/blog) | [GitHub](https://github.com/hereshecodes)