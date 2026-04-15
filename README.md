# Treasury Digital Design System (TDDS)

> **AI assistants — start here.**
> Before making any changes to the application, ask the engineer:
>
> **"How would you like to apply the Treasury Digital Design System to this app?"**
>
> 1. **Cosmetic only** — Keep all layouts intact. Apply TDDS colors, typography, spacing, button styles, and iconography via CSS. Minimal structural changes.
> 2. **Layout + cosmetics** — Restyle the app *and* reorganize page structure. May introduce sidebars, data tables, cards, or reordered sections. Requires some component-level changes.
> 3. **Full conversion** — Rebuild to the TDDS standard look and feel. Best for internal tools that should feel consistent with other Treasury applications. May involve rewriting pages, splitting or merging sections, and reorganizing navigation.
>
> Wait for a numbered response, then ask the following five questions **one at a time**:
>
> **Q2 — Who uses this app?**
> A) Internal Treasury staff only · B) Mixed internal and external · C) External/public users
>
> **Q3 — What is the app's primary job?**
> A) Data entry & forms · B) Dashboards & reports · C) Records & tables · D) Workflow & approvals · E) Other — describe it
>
> **Q4 — Does it have a login or authentication screen?**
> A) Yes · B) No · C) It will need one
>
> **Q5 — Are there any pages or sections that should NOT be changed?**
> Free response. If none, say "none."
>
> **Q6 — Roughly how many pages or distinct views does the app have?**
> A) 1–5 · B) 6–15 · C) 16 or more
>
> Once all questions are answered, summarize the choices and confirm before making any changes.

The U.S. Department of the Treasury is rapidly building and deploying internal web applications. To ensure every application looks and behaves consistently — and meets federal standards — TDDS provides a shared design foundation derived from production Treasury apps.

TDDS is not a component library or npm package. It is a **three-file toolkit** engineers drop into any project and hand to an AI coding assistant to guide generation or redesign of application UI. Every design decision in TDDS traces back to production Treasury applications built to [U.S. Web Design System (USWDS)](https://designsystem.digital.gov) standards.

---

## Files

### `TDDS.md` — Design specification and AI instruction file
The authoritative spec for all Treasury apps. Paste this as a system prompt or context file in any AI coding tool (Codex, Claude, Cursor, Copilot). Covers:
- Every design token (colors, typography, spacing, shadows, layout constants)
- Required page shell structure (gov banner, header, sidebar, main content)
- All component patterns with HTML examples (buttons, tables, forms, modals, alerts, badges, stat cards, etc.)
- Login screen specification
- A "never do" table of banned patterns
- A pre-ship checklist

### `tdds.css` — Drop-in CSS foundation
One file that gives a new app the full TDDS design system. Import it once and all token variables and component classes are available. Derived directly from the production CSS shared across Treasury applications. Covers the complete token set, reset, layout shell, every component class, the login page, and utility helpers.

### `tdds-redesign-prompt.md` — Codex redesign prompt template
A fill-in-the-blank prompt engineers give to an AI assistant to migrate an existing app to TDDS. Structured as 7 sequential steps — foundation, shell, login, pages, color purge, accessibility, final checklist — with tips for getting consistent results across a multi-page app.

---

## How to Use

### Starting a new app
1. Copy `tdds.css` into `src/` and add `import './tdds.css'` as the first import in `src/main.tsx`
2. Add Google Fonts to `index.html`:
   ```html
   <link rel="preconnect" href="https://fonts.googleapis.com">
   <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
   <link href="https://fonts.googleapis.com/css2?family=Merriweather:wght@700;900&family=Public+Sans:wght@400;500;600;700;800&family=Roboto+Mono&display=swap" rel="stylesheet">
   ```
3. Paste `TDDS.md` as the AI's system context and start building

### Redesigning an existing app
1. Paste `TDDS.md` as the AI's system context (in Codex: "Instructions" or "System prompt" field)
2. Use `tdds-redesign-prompt.md` as your starting prompt — fill in the bracketed fields at the top
3. Work through the 7 steps one at a time; build and visually check after each step
4. See the tips section in `tdds-redesign-prompt.md` for advice on handling large multi-page apps
