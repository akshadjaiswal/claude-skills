---
name: validate-and-enhance
description: "Full-stack UI/UX + framework validation workflow. Audits any Next.js or React project against ui-ux-pro-max UX guidelines and next-best-practices, then produces a prioritised implementation plan and executes it. Use when: improving an existing app, pre-launch quality check, accessibility audit, performance review. Trigger phrases: validate my app, enhance my project, audit UI/UX, run a full review, check best practices."
license: MIT
compatibility: "Claude Code — requires ui-ux-pro-max and next-best-practices skills to be installed"
metadata:
  author: akshadjaiswal
  version: "1.0.0"
---

# Validate & Enhance — Full Audit + Implementation Workflow

You are running a complete end-to-end audit and enhancement of the current project. Work through all five phases below in order. Do not skip phases or jump ahead.

---

## PHASE 1 — Load Skill Knowledge

First, load both skill knowledge bases by invoking them:

1. Run `/ui-ux-pro-max` to load the full UX guidelines dataset (99 rules, accessibility, motion, forms, feedback, viewport, focus, etc.)
2. Run `/next-best-practices` to load the Next.js best-practices dataset (image, font, metadata, error boundaries, RSC, bundling, etc.)

> If this is not a Next.js project, skip `/next-best-practices` and instead use the most relevant framework skill available (e.g. `/react-doctor` for plain React).

---

## PHASE 2 — Deep Codebase Exploration

Use the **Explore agent** to thoroughly read the project. Collect:

### Files to always read:
- Root layout (`app/layout.tsx` or `pages/_app.tsx`)
- Global CSS / Tailwind config (`globals.css`, `tailwind.config.js`)
- Framework config (`next.config.js` / `vite.config.ts`)
- `package.json` (dependencies + scripts)
- All page/route files
- All shared/reusable components (inputs, buttons, modals, nav, cards)
- All animation components
- All state store files (Zustand, Redux, Context)
- Any API route handlers or service files with `console.log`

### Questions to answer during exploration:
- Are remote images loaded via `<img>` instead of `<Image>`?
- Are Google Fonts loaded via CSS `@import` instead of `next/font`?
- Do `error.tsx` and `not-found.tsx` exist?
- Do form inputs have visible `<label>` elements (not just placeholder)?
- Do interactive elements (buttons, dots, toggles) have visible focus rings?
- Are `alert()` calls used for error feedback?
- Does any animation skip `prefers-reduced-motion`?
- Is `100vh` / `h-screen` used instead of `dvh`?
- Are heavy components (charts, maps, editors) lazy-loaded with `next/dynamic`?
- Are there unguarded `console.log` calls in production code?
- Is there a `data-theme` or equivalent on `<html>` to prevent flash of unstyled content?
- Do page-level `'use client'` components need a Server Component layout for `generateMetadata`?

---

## PHASE 3 — Violations Report

Cross-reference findings from Phase 2 against the loaded skill guidelines. Produce a violations table:

```
| # | Skill Source | Rule | Violation Found (file:line) | Severity |
|---|-------------|------|----------------------------|----------|
| 1 | ui-ux-pro-max #43 | Form inputs need labels | UsernameInput.tsx:60 — no <label>, only placeholder | High |
| 2 | next-best-practices image.md | Use next/image for remote images | Slide14.tsx:105 — <img src={avatar_url}> | High |
...
```

Only list actual violations found in the code — do not list rules that are already followed correctly.

---

## PHASE 4 — Enter Plan Mode and Write the Implementation Plan

**Enter plan mode now.** Write the full implementation plan before touching any file.

For each violation, write a dedicated fix section containing:

1. **Fix ID** (e.g. `1.1`, `1.2`, `2.1`)
2. **File path** (exact absolute or relative path)
3. **What to change** — clear description
4. **Before/After code snippets** — exact lines to replace
5. **Which skill guideline** this satisfies
6. **Risk level** — Low / Medium / High
7. **Dependencies** — list any fix that must be done first

Then write:
- **Implementation order** — numbered list, critical fixes first
- **Critical files table** — all files being changed and what changes
- **What is NOT changed** — explicit list of preserved functionality
- **Verification checklist** — manual tests to confirm all fixes work

---

## PHASE 5 — Execute the Plan

**Exit plan mode.** Implement every fix in the exact order specified in Phase 4.

Follow these rules strictly:
- Read every file with the Read tool before editing it — never edit blind
- Use Edit tool for targeted changes, Write tool only for new files
- After all edits are complete, run `npm run build` (or `yarn build` / `pnpm build`)
- If the build fails, diagnose and fix immediately — do not move on
- Run `npm run lint` if an ESLint config exists or was just created
- Do not add any feature, refactor, or improvement not listed in the plan
- Do not break existing data fetching, routing, stores, themes, or business logic

---

## Output Format

At the end, print a completion summary:

```
## Enhancement Complete ✓

### Changes Applied
| Fix | File | Description |
|-----|------|-------------|
| 1.1 | next.config.js | Added image remotePatterns |
...

### Build Status
✓ npm run build — passed

### Verification Checklist
- [ ] [each item from the checklist]
```

---

## Adapting to Non-Next.js Projects

- **Vite/React:** Skip `next.config.js`, `next/image`, `next/font`, `generateMetadata`. Focus on `react-router` patterns, lazy imports via `React.lazy`, and Vite bundle config.
- **Vue/Nuxt:** Replace `next-best-practices` with equivalent Nuxt guidelines. Same UX rules apply.
- **No framework config file found:** Skip that category of checks, note it in the report.
- **No TypeScript:** Skip type-related checks, still apply all UX and accessibility checks.

Always adapt — the UX guidelines from `ui-ux-pro-max` apply universally to every web project.
