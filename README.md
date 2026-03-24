# Claude Skills

A collection of Claude Code skills for full-stack development workflows. Compatible with the [Agent Skills](https://agentskills.io) open standard and discoverable on [skills.sh](https://skills.sh).

## Install All Skills

```bash
npx skills add akshadjaiswal/claude-skills
```

Or install a specific skill:

```bash
npx skills add akshadjaiswal/claude-skills --skill validate-and-enhance
```

---

## Skills

### `/validate-and-enhance`

**Full-stack UI/UX + Next.js audit and enhancement workflow.**

Runs a 5-phase deep audit of your project against 99 UX rules and Next.js best practices, generates a violations report, writes an implementation plan, and then executes all fixes automatically.

**What it does:**
- Phase 1: Loads `/ui-ux-pro-max` (99 UX rules) and `/next-best-practices` (Next.js patterns)
- Phase 2: Deep codebase exploration — reads every relevant file
- Phase 3: Generates a violations table with severity levels
- Phase 4: Enters plan mode and writes a full fix-by-fix implementation plan
- Phase 5: Executes every fix, runs `npm run build`, outputs a completion summary

**Use when:**
- Pre-launch quality check
- Improving an existing Next.js or React app
- Accessibility audit
- Performance review

**Trigger phrases:** `validate my app`, `enhance my project`, `audit UI/UX`, `run a full review`, `check best practices`

**Prerequisites:**

This skill internally calls `/ui-ux-pro-max` and `/next-best-practices`. Install them first:

```bash
# Install Anthropic's base skills (includes ui-ux-pro-max and next-best-practices)
npx skills add anthropics/skills

# Then install this skill
npx skills add akshadjaiswal/claude-skills
```

---

## Adding More Skills

More skills coming soon. This repo follows the [Agent Skills specification](https://agentskills.io/specification) — each skill is a directory with a `SKILL.md` file.

## License

MIT
