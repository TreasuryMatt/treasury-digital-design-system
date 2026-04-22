# TDDS Implementation Examples

Three levels of TDDS implementation exist to match the actual scope of each project. When an engineer opens the intake questionnaire and picks a level, this document tells you — the AI — what that level actually means in practice: what changes, what doesn't, how long it takes, and what "done" looks like.

---

## Level 1 — Cosmetic Only

**Definition:** Apply TDDS colors, typography, spacing tokens, and button styles via CSS. No structural changes to layouts, navigation, or component types. No new components introduced.

### When to choose it
The app's layout and navigation are already serviceable. The problem is purely visual — it looks nothing like a Treasury app. The engineer wants it to feel right without reorganizing anything.

Works across all starting frameworks: Bootstrap, Tailwind (see note below), plain CSS, or raw USWDS. For Tailwind specifically, a separate shadcn writeup is forthcoming.

### What changes
After a cosmetic pass, the engineer will see:
- **Color palette** — Navy, gold, and slate replace generic blues and grays via `--usa-primary`, `--usa-accent-cool`, and `--usa-gold`
- **Typography** — Public Sans (body), Merriweather (headings), Roboto Mono (code) replace whatever font stack existed before
- **Button styles** — Buttons adopt TDDS shape, weight, and color tokens
- **Spacing rhythm** — Consistent padding and margin via TDDS spacing tokens

### What does NOT change
- Page layout and grid structure
- Navigation order, hierarchy, and structure
- Component types — existing components are restyled, not replaced
- JavaScript and interactive behavior — nothing behavioral changes

### How to do it
Drop in `tdds.css` via a single `<link>` tag and let the cascade do the work:

```html
<link rel="stylesheet" href="/tdds.css">
```

That's it for the starting point. From there, resolve specificity conflicts (see below).

### Time estimate
**Half a day (2–4 hours)** for a mid-size app (6–15 views).

### Common gotcha: specificity wars
The most frequent problem — existing styles have higher specificity than `tdds.css` and win. Fix by:
1. Loading `tdds.css` last, after any framework stylesheets
2. Hunting down hardcoded hex values in inline styles and JS-generated classes and replacing them with `var(--usa-*)` tokens
3. Scoping overrides to the specific component rather than fighting with `!important`

### Done criteria
- No hardcoded hex values anywhere in the stylesheet — all colors flow through TDDS custom properties
- Typography is rendering correctly: Public Sans for body text, Merriweather for headings

### On the .gov banner
Recommended but optional at the cosmetic level. If the engineer is also adding the site header, the banner should come with it. If they're only doing a CSS pass with no structural work, the banner can wait for a higher-level pass.

### Reference example
The **TDDS component library** (`component-library/index.html`) is the best cosmetic-level example. It uses TDDS colors, typography, spacing, and component styles — but deliberately omits the site header, sidebar, and .gov banner. That's the cosmetic contract: the visual language without the structural shell.

---

## Level 2 — Layout + Cosmetics

**Definition:** Restyle the app AND reorganize page structure. Introduces TDDS layout components where needed (card grids, data tables, summary boxes). May involve sidebar navigation. All Level 1 cosmetic criteria must also be met.

### When to choose it
One or more of these is true:
- Content is unstructured — walls of text or divs with no card or table pattern
- Navigation is broken UX — users struggle to find things, reorganization is needed
- Responsive layout is missing — content stacks or overflows on resize

### What changes (beyond Level 1)
The most common structural addition is a **card grid** — content blocks reorganized into `usa-card` patterns. Other components commonly introduced (not just restyled):
- `usa-card` / card grid
- `usa-table` — structured data replacing unordered lists or divs
- `usa-summary-box` — key information highlighted in a callout

### Navigation: sidebar vs. top nav vs. both
There's no default. **Ask the engineer.** Some apps benefit from a sidebar; others keep top nav and add a sidebar alongside it; some apps with few links are fine with just top nav. The intake Q3 ("Does the app have navigation?") and the app's content structure determine the answer. Do not assume sidebar is always the right call at Layout level.

### Biggest risk: losing content
Layout work reorganizes sections, wraps content in new containers, and sometimes collapses or reorders views. The most common failure mode is a piece of content that was present before the pass that quietly disappears after. Always verify content parity before declaring done.

### Protected zones and exotic components
There are no hard off-limits areas at Layout level — anything can be touched as long as content is preserved. However: if you encounter an unusual or exotic component (non-standard widgets, custom visualizations, deeply nested interactive patterns), **check in with the engineer before restructuring it**. Don't assume you know how it works.

### Time estimate
**Half a day (2–4 hours)** for a mid-size app (6–15 views) — the same as cosmetic, because layout work at this level is mostly HTML restructuring, not rebuilding.

### Done criteria
All four must be true:
1. **App shell complete** — `.gov banner + site header + sidebar (if applicable) + main content` in place and in the correct DOM order
2. **No lost content** — every piece of content from the original app is still accessible
3. **Components introduced** — at least cards, tables, or sidenav used where appropriate
4. **Cosmetic bar met** — all Level 1 criteria satisfied (no hardcoded hex, typography correct)

### Reference example
None of the three reference apps (treasury-vault, treasury-software, treasury-ops-hub) are Layout-level examples — all three are Full conversions. Layout is the middle ground that doesn't yet have a canonical screenshot reference. When implementing at this level, use the component library for visual reference and the reference apps for structural aspiration.

---

## Level 3 — Full Conversion

**Definition:** Rebuild the app to the TDDS standard look and feel. Remove all existing CSS frameworks. Rebuild the HTML skeleton around the required app shell. Apply all cosmetic and layout changes. The result should feel identical to a canonical Treasury internal app.

### When to choose it
The app is used by **Treasury employees to do their jobs** — it's an internal operational tool. Full conversion is the baseline expectation for internal apps. The only time Full is overkill is for public-facing apps (see below).

### Order of operations
Follow the 7-step process in `tdds-redesign-prompt.md` exactly:

1. **Remove existing CSS frameworks** — uninstall Bootstrap, Tailwind, or whatever is loaded. Verify in the network tab that no old framework CSS is fetched.
2. **Import `tdds.css`** — single `<link>` tag, loaded first
3. **Add Google Fonts** — Public Sans, Merriweather, Roboto Mono
4. **Copy branding assets** — Treasury seal, favicon, .gov banner assets into `public/`
5. **Build the app shell** — `.gov banner → site header (64px, #162e51) → sidebar (240px) → main content`, in that DOM order
6. **Rebuild pages** — apply TDDS components page by page
7. **Review** — run the done checklist below

### What gets rebuilt (vs. Layout)
The key distinction between Layout and Full is the **structural HTML skeleton**. At Layout level, the existing HTML is reorganized. At Full level, the page-level shell is rebuilt from scratch around the required TDDS app shell structure.

### Required app shell components
All four are non-negotiable in a Full conversion:
- `.gov banner` — top of page, legally required on .gov sites
- **Site header** — 64px, `#162e51` navy, Treasury seal, app name
- **Sidebar** — 240px, left-side navigation
- **Main content area** — correct padding and max-width

### Time estimate
**1–2 days** for a mid-size app (6–15 views).

### Common mistake: skipping framework removal
The most frequent error — `tdds.css` is added but the old CSS framework (Bootstrap, etc.) is still loading. The two fight each other and the result is broken. The framework removal step (Step 1) is not optional. Verify in the browser network tab that the old framework is gone before moving to Step 2.

### Third-party embeds (charts, maps, iframes)
Flag them to the engineer and skip. Wrap the embed in a TDDS card or container, but don't touch the embed internals. Document what was skipped.

### When Full is overkill
**Public-facing apps.** External or public users don't interact with Treasury internal app patterns the same way employees do — the full internal shell (sidebar, Treasury seal header) may be inappropriate or confusing for them. For public-facing apps, use Cosmetic or Layout and apply USWDS patterns rather than the full TDDS internal shell.

### Done criteria — the review checklist (Step 7)
All four must pass:
1. **No hardcoded hex or fonts** — all values flow through TDDS custom properties; none in inline styles, component files, or JS
2. **App shell DOM order correct** — `.gov banner → site header → sidebar → main` in that order in the HTML
3. **No old framework CSS loaded** — verify in browser network tab; zero requests to Bootstrap/Tailwind/etc.
4. **All nav links work** — routing not broken by structural changes; test every sidebar link

### Canonical reference example
**`treasury-ops-hub`** — the 6-page operational hub is the clearest Full conversion example. Compact enough to study in full, structured enough to show all required shell components and patterns.

---

## Quick comparison

| | Cosmetic | Layout | Full |
|---|---|---|---|
| **CSS frameworks removed** | No | No | Yes |
| **Page skeleton rebuilt** | No | No | Yes |
| **New components introduced** | No | Yes | Yes |
| **Navigation reorganized** | No | Maybe | Yes |
| **App shell required** | No | Partially | Yes |
| **.gov banner** | Recommended | Required | Required |
| **Time (6–15 views)** | Half a day | Half a day | 1–2 days |
| **Reference example** | Component library | (none yet) | treasury-ops-hub |
