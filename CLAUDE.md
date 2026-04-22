# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Treasury Digital Design System (TDDS) — a three-file toolkit for building U.S. Department of the Treasury internal web applications conforming to USWDS standards. It is a specification and reference repository, not an application.

**Core deliverables:**
- `TDDS.md` — Authoritative design spec (tokens, components, rules)
- `tdds.css` — Drop-in CSS foundation (~1175 lines)
- `tdds-redesign-prompt.md` — 7-step migration template for existing apps
- `component-library/index.html` — Living single-page visual reference with copy snippets

## Working in this repo

There is no build step. Files are authored directly. To preview:
- Open `component-library/index.html` in a browser
- Use `npm start` (PORT=3001) if a local server is needed

## USWDS as the upstream fallback

TDDS is a Treasury-flavored layer on top of USWDS — it defines overrides, extensions, and Treasury-specific patterns. It is intentionally scoped and does not replicate every USWDS component.

**Rule:** When a component, pattern, atom, or style is needed and is not defined in TDDS (`tdds.css`, `TDDS.md`, or `component-library/index.html`), look to USWDS next before inventing anything custom.

**How to prompt the user:**
> "TDDS doesn't define a [thing]. USWDS has one at designsystem.digital.gov/components/[thing] — want me to pull it in and apply TDDS tokens, or use it as-is?"

**Decision logic:**
1. Defined in TDDS → use TDDS, no question needed.
2. Not in TDDS, exists in USWDS → offer the USWDS version (apply TDDS tokens) as the default. Only ask if there's a real reason to deviate.
3. Not in TDDS, not in USWDS → only then consider building custom. Say so explicitly: "Neither TDDS nor USWDS defines a [thing] — I'll build one to match TDDS conventions."

**"Make it myself" is a last resort**, not a first option. Hand-rolled components skip USWDS's 508 accessibility guarantees and keyboard/ARIA patterns.

**Package rule:** Never `npm install @uswds/uswds`. USWDS component HTML and CSS patterns can always be referenced and copied directly — it's the npm dependency that's banned, not the markup.

**Reference:** https://designsystem.digital.gov/components/

## Architecture and conventions

**CSS rules (non-negotiable):**
- Vanilla CSS only — no Tailwind, CSS Modules, SCSS, styled-components, emotion
- All values via CSS custom properties (`var(--usa-*)`) — never hardcode hex, fonts, or spacing
- BEM naming with `usa-` prefix for USWDS components
- No gradients (flat TDDS color tokens only)
- No emojis — use USWDS SVG sprites exclusively

**USWDS icons:** Always download the sprite via CDN during Step 1. Never install `@uswds/uswds` via npm.
```bash
mkdir -p public/assets/img
curl -L "https://cdn.jsdelivr.net/npm/@uswds/uswds/packages/uswds-core/src/img/sprite.svg" \
  -o public/assets/img/sprite.svg
```

**Canonical branding assets** (in `assets/` — copy to target app's `public/`):
- `treasury-seal-white-simple.svg` — header logo (48×48px)
- `favicon.svg` — browser tab icon (`<link rel="icon" href="/favicon.svg" type="image/svg+xml">`)
- `us_flag_small.png`, `icon-dot-gov.svg`, `icon-https.svg` — .gov banner assets

**Required app shell structure** (in order): `.gov banner → site header (64px, #162e51) → sidebar (240px) + main content`

**The .gov banner is legally required** on all .gov sites — never omit it.

## AI agent workflow

When helping engineers redesign or build a Treasury app, start by asking:

> **"How would you like to apply the Treasury Digital Design System to this app?"**
> 1. **Cosmetic only** — Keep all layouts intact. Apply TDDS colors, typography, spacing, button styles, and iconography via CSS. Minimal structural changes.
> 2. **Layout + cosmetics** — Restyle the app *and* reorganize page structure. May introduce sidebars, data tables, cards, or reordered sections. Requires some component-level changes.
> 3. **Full conversion** — Rebuild to the TDDS standard look and feel. Best for internal tools that should feel consistent with other Treasury applications. May involve rewriting pages, splitting or merging sections, and reorganizing navigation.

Wait for a numbered response, then ask the following five questions **one at a time**. Wait for each answer before asking the next.

**Q2 — Who uses this app?**
- A) Internal Treasury staff only
- B) Mixed internal and external users
- C) External or public users

*(Affects how strictly to apply the .gov banner, auth patterns, and formal tone.)*

**Q3 — What is the app's primary job?**
- A) Data entry and forms
- B) Viewing dashboards and reports
- C) Managing records and tables
- D) Workflow and approvals
- E) Something else — describe it

*(Helps prioritize which TDDS patterns to focus on.)*

**Q4 — Does it have a login or authentication screen?**
- A) Yes
- B) No
- C) It will need one

*(The .gov banner and login card are required on all auth screens.)*

**Q5 — Are there any pages or sections that should NOT be changed?**

Free response. If none, say "none."

*(Prevents changes to embedded third-party tools, charts, maps, or sensitive areas.)*

**Q6 — Roughly how many pages or distinct views does the app have?**
- A) 1–5
- B) 6–15
- C) 16 or more

*(Helps determine whether to work page-by-page or propose a global approach first.)*

Once all six questions are answered, summarize the engineer's choices and confirm before making any changes. Then use `TDDS.md`, `tdds.css`, and `tdds-redesign-prompt.md` to guide implementation.

For **Level 3**, follow the 7-step process in `tdds-redesign-prompt.md` exactly. Key steps: remove existing CSS frameworks, import `tdds.css`, add Google Fonts (Public Sans, Merriweather, Roboto Mono), copy branding assets, build the app shell with GovBanner + SiteHeader + Sidebar.

## File sync rule

`AGENTS.md` and the **AI agent workflow** section of this file (`CLAUDE.md`) contain the same 6-question intake script. They must stay identical.

**Rule:** Any time either file is edited — intake questions, answer options, parenthetical notes, or level descriptions — immediately apply the same change to the other file. Never let them drift.

## Reference apps (screenshots in `screenshots/`)

- `treasury-vault/` — 10 pages
- `treasury-software/` — 11 pages
- `treasury-ops-hub/` — 6 pages
