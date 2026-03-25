---
name: validate-and-enhance
description: "Full-stack UI/UX + framework validation workflow. Audits any Next.js or React project against ui-ux-pro-max UX guidelines and next-best-practices, then produces a prioritised implementation plan and executes it. Use when: improving an existing app, pre-launch quality check, accessibility audit, performance review. Trigger phrases: validate my app, enhance my project, audit UI/UX, run a full review, check best practices."
license: MIT
compatibility: "Claude Code — requires ui-ux-pro-max and next-best-practices skills to be installed"
metadata:
  author: akshadjaiswal
  version: "1.1.0"
---

# Validate & Enhance — Full Audit + Implementation Workflow

You are running a complete end-to-end audit and enhancement of the current project. Work through all five phases below in order. Do not skip phases or jump ahead.

---

## PHASE 1 — Load Skill Knowledge

Invoke both sub-skills to load their full rule sets into context:

1. Run `/ui-ux-pro-max` — this loads:
   - 99 UX rules across 8 priority categories (Accessibility, Touch & Interaction, Performance, Layout & Responsive, Typography & Color, Animation, Style Selection, Charts & Data)
   - Pre-delivery checklist (visual quality, interaction states, light/dark mode, layout, accessibility)
   - Focus on extracting: accessibility rules, interaction state rules, layout rules, animation rules

2. Run `/next-best-practices` — this loads rules from these topic files:
   `image.md`, `font.md`, `error-handling.md`, `rsc-boundaries.md`, `metadata.md`,
   `bundling.md`, `async-patterns.md`, `hydration-error.md`, `suspense-boundaries.md`,
   `scripts.md`, `directives.md`, `runtime-selection.md`

> **Non-Next.js projects:** Skip step 2. For plain React use `/react-doctor` if installed; otherwise apply only the UX rules from step 1.

---

## PHASE 2 — Deep Codebase Exploration

Use the **Explore agent** to thoroughly read the project. Collect:

### Files to always read:
- Root layout (`app/layout.tsx` or `pages/_app.tsx`)
- Global CSS / Tailwind config (`globals.css`, `tailwind.config.js`)
- Framework config (`next.config.js` / `vite.config.ts`)
- `package.json` + lock file (`yarn.lock`, `pnpm-lock.yaml`, `package-lock.json`)
- All page/route files
- All shared/reusable components (inputs, buttons, modals, nav, cards)
- All animation components
- All state store files (Zustand, Redux, Context)
- Any API route handlers or service files with `console.log`

### Questions to answer during exploration:

**Images & Fonts**
- Are remote images loaded via `<img>` instead of `<Image>`?
- Are Google Fonts loaded via CSS `@import` instead of `next/font`?
- Do images have explicit `width` and `height` props (or `fill`) to prevent layout shifts?

**Error Handling**
- Do `error.tsx` and `not-found.tsx` exist?

**Forms & Accessibility**
- Do form inputs have visible `<label>` elements (not just placeholder)?
- Do interactive elements (buttons, dots, toggles) have visible focus rings?
- Do meaningful images have descriptive `alt` text? (decorative images must have `alt=""`)
- Do icon-only buttons have `aria-label` or visually-hidden text?
- Can all interactive elements be reached and activated by keyboard (tab navigation logical)?
- Are color contrast ratios ≥ 4.5:1 for body text and ≥ 3:1 for large text / UI components?
- Do touch/click targets meet the 44×44px minimum size?

**Interaction States**
- Are `alert()` calls used for error feedback instead of inline UI?
- Are error messages clear and specific (not generic "Something went wrong")?
- Do clickable elements have `cursor-pointer` applied?
- Do buttons show a loading/disabled state during async operations?
- Do hover states exist for all interactive elements (links, buttons, cards)?

**Animations**
- Does any animation skip `prefers-reduced-motion`?

**Layout & Viewport**
- Is `100vh` / `h-screen` used instead of `dvh`?
- Is there horizontal overflow / unwanted horizontal scroll at mobile widths?
- Is `z-index` managed with a consistent scale (no arbitrary `9999` values)?

**Performance & Code Quality**
- Are heavy components (charts, maps, editors) lazy-loaded with `next/dynamic`?
- Are `<script>` tags using `next/script` with the correct `strategy` prop?
- Are there unguarded `console.log` calls in production code?
- Is there a `data-theme` or equivalent on `<html>` to prevent flash of unstyled content?
- Do page-level `'use client'` components need a Server Component layout for `generateMetadata`?

---

## PHASE 3 — Violations Report

Cross-reference findings from Phase 2 against the loaded skill guidelines. Produce a violations table:

```
| # | Category | Skill Source | Violation (file:line) | Severity | Suggested Fix |
|---|----------|--------------|-----------------------|----------|---------------|
| 1 | Accessibility | ui-ux-pro-max | UsernameInput.tsx:60 — no <label>, only placeholder | High | Add <label htmlFor="username"> above input |
| 2 | Images | next-best-practices image.md | Slide14.tsx:105 — <img src={avatar_url}> | High | Replace with <Image> from next/image |
| 3 | Interaction | ui-ux-pro-max | SubmitButton.tsx:12 — no loading state on async click | Medium | Add disabled + spinner during fetch |
...
```

**Severity scale:**
- **Critical** — blocks accessibility / WCAG compliance / build
- **High** — visible UX regression or broken user flow
- **Medium** — best-practice miss, degrades experience
- **Low** — code quality / minor polish

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
- **Implementation order** — numbered list, Critical first, then High, then Medium, then Low
- **Critical files table** — all files being changed and what changes
- **What is NOT changed** — data fetching, routing, stores, themes, business logic explicitly listed
- **Verification checklist** — drawn from the template below, scoped to the violations found

### Verification Checklist Template
Include the relevant checks from this list based on what was actually fixed:

- [ ] Accessibility: Tab through all interactive elements — focus ring visible on each
- [ ] Accessibility: Screen reader announces form labels correctly
- [ ] Accessibility: Icon-only buttons announced with meaningful label
- [ ] Images: No `<img>` loading remote URLs — all use `<Image>` with correct sizing
- [ ] Images: No layout shift during image load (width/height or fill set)
- [ ] Fonts: No FOUT — fonts load with no layout shift
- [ ] Error states: `/nonexistent-page` shows `not-found.tsx`, not a blank page
- [ ] Animations: Reduced-motion OS setting disables all transitions and animations
- [ ] Interaction: All buttons show loading state during async ops
- [ ] Interaction: Error messages are specific and actionable
- [ ] Mobile: No horizontal scroll at 375px viewport width
- [ ] Build: `npm run build` exits 0 with no errors
- [ ] Lint: No new lint errors introduced

---

## PHASE 5 — Execute the Plan

**Exit plan mode.** Implement every fix in the exact order specified in Phase 4.

Follow these rules strictly:
- Read every file with the Read tool before editing it — never edit blind
- Use Edit tool for targeted changes, Write tool only for new files
- **Detect package manager** before running any script:
  - `yarn.lock` present → use `yarn`
  - `pnpm-lock.yaml` present → use `pnpm`
  - Otherwise → use `npm`
- Run build: `npm run build` / `yarn build` / `pnpm build`
  - If the build **fails** (non-zero exit): diagnose and fix immediately — do not move on
  - If the build **has warnings only**: note them in the summary but do not block completion
- Run lint: `npm run lint` / `yarn lint` / `pnpm lint` (only if ESLint config exists)
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
| 1.2 | components/Avatar.tsx | Replaced <img> with next/image |
| 2.1 | components/LoginForm.tsx | Added <label> for email input |
...

### Build Status
✓ npm run build — passed

### Verification Checklist
- [ ] [each item from the checklist scoped to fixes applied]
```

---

## Adapting to Non-Next.js Projects

- **Vite/React:** Skip `next.config.js`, `next/image`, `next/font`, `generateMetadata`, `next/script`. Focus on `react-router` patterns, `React.lazy` + `Suspense`, and Vite bundle config. All 25 UX/accessibility questions still apply.
- **Vue/Nuxt:** Replace `next-best-practices` with equivalent Nuxt guidelines. All UX rules still apply.
- **No framework config file found:** Skip that category of checks, note it in the violations report.
- **No TypeScript:** Skip type-related checks, still apply all UX and accessibility checks.

Always adapt — the UX guidelines from `ui-ux-pro-max` apply universally to every web project.
