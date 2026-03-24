# Claude Skills

Production-ready Claude Code skills for full-stack development — UI/UX audits, Next.js best practices, and automated enhancement workflows.

Compatible with the [Agent Skills](https://agentskills.io) open standard · Discoverable on [skills.sh](https://skills.sh)

---

## Quick Start

```bash
# 1. Install base skills (required dependencies)
npx skills add anthropics/skills

# 2. Install these skills
npx skills add akshadjaiswal/claude-skills

# 3. Open Claude Code and type:
#    "validate my app"
```

---

## Available Skills

### `validate-and-enhance`

Audits your entire Next.js or React project against 99 UX rules and Next.js best practices, generates a prioritised violations report, writes an implementation plan, and executes every fix automatically.

**Use when:**
- "validate my app" / "run a full review"
- "audit UI/UX" / "enhance my project"
- "check best practices" / "audit my project"
- Pre-launch quality check before shipping
- Accessibility compliance review
- Performance review for an existing app

**Categories covered:**

| Category | Priority | What it checks |
|----------|----------|----------------|
| Accessibility | Critical | Form labels, focus rings, ARIA attributes |
| Images & Fonts | High | `next/image` vs `<img>`, `next/font` vs CSS `@import` |
| Error Handling | High | `error.tsx`, `not-found.tsx` existence |
| Animations | High | `prefers-reduced-motion` guards |
| Performance | Medium-High | Lazy loading, `next/dynamic` for heavy components |
| Layout & Viewport | Medium | `dvh` vs `100vh`, responsive breakpoints |
| Code Quality | Medium | `console.log` guards, `generateMetadata`, theme flash |

**How it works (5 phases):**

1. **Load** — Invokes `/ui-ux-pro-max` (99 UX rules) and `/next-best-practices` (Next.js patterns)
2. **Explore** — Deep codebase scan: reads every page, component, config, and store file
3. **Report** — Produces a violations table with file:line references and severity levels
4. **Plan** — Enters plan mode and writes a fix-by-fix implementation plan with before/after code
5. **Execute** — Implements every fix in order, runs `npm run build`, outputs a completion summary

**Example output:**

```
## Enhancement Complete ✓

### Changes Applied
| Fix | File                    | Description                          |
|-----|-------------------------|--------------------------------------|
| 1.1 | next.config.js          | Added image remotePatterns            |
| 1.2 | components/Avatar.tsx   | Replaced <img> with next/image        |
| 2.1 | components/LoginForm.tsx | Added <label> for email input        |
| 3.1 | app/globals.css         | Added prefers-reduced-motion guard   |

### Build Status
✓ npm run build — passed
```

**Prerequisites:** Requires `/ui-ux-pro-max` and `/next-best-practices` (installed via `npx skills add anthropics/skills`)

---

## Installation

### via npx (recommended)

```bash
# Install all skills in this repo
npx skills add akshadjaiswal/claude-skills

# Install a specific skill
npx skills add akshadjaiswal/claude-skills --skill validate-and-enhance

# Install globally (available in all projects)
npx skills add akshadjaiswal/claude-skills -g
```

### via Claude Code plugin marketplace

```
/plugin marketplace add akshadjaiswal/claude-skills
```

---

## Skill Structure

Each skill in this repo lives at `skills/<skill-name>/SKILL.md` and follows the [Agent Skills specification](https://agentskills.io/specification):

```
claude-skills/
└── skills/
    └── validate-and-enhance/
        └── SKILL.md
```

More skills coming soon. Contributions welcome.

---

## License

MIT · [Akshad Jaiswal](https://github.com/akshadjaiswal)
