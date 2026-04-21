# Treasury Digital Design System — Agent Instructions

Before making any changes to the application, ask the engineer:

---

**"How would you like to apply the Treasury Digital Design System to this app?"**

1. **Cosmetic only** — Keep all layouts intact. Apply TDDS colors, typography, spacing, button styles, and iconography via CSS. Minimal structural changes.
2. **Layout + cosmetics** — Restyle the app *and* reorganize page structure. May introduce sidebars, data tables, cards, or reordered sections. Requires some component-level changes.
3. **Full conversion** — Rebuild to the TDDS standard look and feel. Best for internal tools that should feel consistent with other Treasury applications. May involve rewriting pages, splitting or merging sections, and reorganizing navigation.

---

Wait for a numbered response, then ask the following five questions **one at a time**. Wait for each answer before asking the next.

---

**Question 2 — Who uses this app?**
- A) Internal Treasury staff only
- B) Mixed internal and external users
- C) External or public users

*(Affects how strictly to apply the .gov banner, auth patterns, and formal tone.)*

---

**Question 3 — What is the app's primary job?**
- A) Data entry and forms
- B) Viewing dashboards and reports
- C) Managing records and tables
- D) Workflow and approvals
- E) Something else — describe it

*(Helps prioritize which TDDS patterns to focus on.)*

---

**Question 4 — Does it have a login or authentication screen?**
- A) Yes
- B) No
- C) It will need one

*(The .gov banner and login card are required on all auth screens.)*

---

**Question 5 — Are there any pages or sections that should NOT be changed?**

Free response. If none, say "none."

*(Prevents changes to embedded third-party tools, charts, maps, or sensitive areas.)*

---

**Question 6 — Roughly how many pages or distinct views does the app have?**
- A) 1–5
- B) 6–15
- C) 16 or more

*(Helps determine whether to work page-by-page or propose a global approach first.)*

---

Once all six questions are answered, summarize the engineer's choices and confirm before making any changes. Then use the files in this directory to guide your implementation:

- `TDDS.md` — Design system documentation and usage guidelines
- `tdds.css` — Core TDDS stylesheet
- `tdds-redesign-prompt.md` — Detailed redesign instructions and patterns

---

## Level 3 (Full Conversion) — Mandatory Standards

When the engineer selects **option 3 (full conversion)**, the following are non-negotiable regardless of what the existing app uses:

**Canonical branding assets** (copy from `assets/` in this repo into the target app's `public/`):
- `treasury-seal-white-simple.svg` — header logo, always 48×48px
- `favicon.svg` — browser tab icon, always referenced as `<link rel="icon" href="/favicon.svg" type="image/svg+xml">`
- `us_flag_small.png`, `icon-dot-gov.svg`, `icon-https.svg` — `.gov` banner

**USWDS icons** — Download the sprite during Step 1:
```bash
mkdir -p public/assets/img
curl -L "https://cdn.jsdelivr.net/npm/@uswds/uswds/packages/uswds-core/src/img/sprite.svg" \
  -o public/assets/img/sprite.svg
```
See the full icon list at https://designsystem.digital.gov/components/icon/. Never use emojis as icons.

**No gradients** — Do not use `linear-gradient` or `radial-gradient` on any background, card, panel, or UI element. Use flat TDDS color tokens only.
