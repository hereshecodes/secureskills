# Contributing to SecureSkills

Thanks for wanting to help make AI-generated code more secure.

## Adding a New Skill

1. Create a folder in `skills/` with a kebab-case name
2. Add a `SKILL.md` file following the format below
3. Keep it under 100 lines. If it's longer, split it into two skills.
4. Every rule needs a WRONG and RIGHT code example
5. Examples must be framework-agnostic (no framework-specific imports)
6. Add a `> Related:` callout linking to relevant sister skills
7. Add a `Quick Reference` table at the bottom
8. Open a PR with a clear description of what the skill enforces

## Skill Format

```markdown
---
name: skill-name
description: When to trigger this skill
---

## Skill Title

One-line summary.

> Related: [other-skill], [another-skill]

### Rule Name

Why this matters (1 sentence).

\```language
// WRONG
insecure example

// RIGHT
secure example
\```

### Quick Reference

| Do | Don't |
|----|-------|
| ... | ... |
```

## Improving an Existing Skill

- Fix incorrect examples
- Add missing edge cases
- Improve clarity (shorter is better)
- Add examples in additional languages

## What We Don't Accept

- Framework-specific skills (make examples generic)
- Theory without code examples
- Skills over 100 lines
- Offensive security / exploitation techniques