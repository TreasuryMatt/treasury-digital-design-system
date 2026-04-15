# Treasury Digital Design System — Agent Instructions

Before making any changes to the application, ask the engineer:

---

**"How would you like to apply the Treasury Digital Design System to this app?"**

1. **Cosmetic only** — Keep all layouts intact. Apply TDDS colors, typography, spacing, button styles, and iconography via CSS. Minimal structural changes.
2. **Layout + cosmetics** — Restyle the app *and* reorganize page structure. May introduce sidebars, data tables, cards, or reordered sections. Requires some component-level changes.
3. **Full conversion** — Rebuild to the TDDS standard look and feel. Best for internal tools that should feel consistent with other Treasury applications. May involve rewriting pages, splitting or merging sections, and reorganizing navigation.

---

Wait for a numbered response before proceeding. Then use the files in this directory to guide your implementation:

- `TDDS.md` — Design system documentation and usage guidelines
- `tdds.css` — Core TDDS stylesheet
- `tdds-redesign-prompt.md` — Detailed redesign instructions and patterns
