# TDDS Redesign Prompt Template
## Instructions for engineers using Codex / Claude / Cursor / Copilot

Copy the prompt below, fill in the `[BRACKETED]` fields, and paste it as your AI assistant's system or user prompt. Attach or paste the contents of `TDDS.md` and `tdds.css` alongside it.

---

## How to Use

1. Open your AI coding tool (Codex, Claude Code, Cursor, GitHub Copilot, etc.)
2. Start a new session / chat
3. Paste `TDDS.md` as context (or as a system prompt / "rules" file)
4. Then paste the prompt below, filling in the bracketed sections
5. Work through the checklist section by section — don't try to do everything in one shot

---

## The Prompt

```
You are redesigning a U.S. Department of the Treasury internal web application to conform
to the Treasury Digital Design System (TDDS). The full specification is in TDDS.md and
the complete CSS file is tdds.css.

APPLICATION DETAILS
-------------------
App name: [e.g. "Treasury Forms Portal"]
Framework: [e.g. "React 18 + TypeScript + React Router v6"]
Current CSS approach: [e.g. "Tailwind CSS" / "inline styles" / "Bootstrap" / "custom CSS"]
Entry point: [e.g. "src/main.tsx"]
Main CSS file(s): [e.g. "src/index.css, src/App.css"]

TASK
----
Redesign this application to match TDDS exactly. Work through the following steps in order.
Complete each step fully before moving to the next. After each step, confirm what was changed.

STEP 1 — Foundation
• Remove any existing CSS framework (Tailwind, Bootstrap, etc.) from package.json and all imports.
• Copy tdds.css into src/ and import it as the first import in src/main.tsx (or index.tsx).
• Add Google Fonts to index.html <head>:
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Merriweather:wght@700;900&family=Public+Sans:wght@400;500;600;700;800&family=Roboto+Mono&display=swap" rel="stylesheet">
• Rename any existing src/index.css to src/App.css (keep app-specific styles there).
• Confirm: does the app build without errors?

STEP 2 — App Shell
• Create (or rewrite) AppLayout.tsx to match this structure exactly:
    <div className="app-shell">
      <a className="usa-skipnav" href="#main-content">Skip to main content</a>
      <GovBanner />
      <SiteHeader appName="[App Name]" />
      <div className="app-body">
        <Sidebar />
        <main id="main-content" className="usa-main">
          <Outlet />
        </main>
      </div>
    </div>
• Create GovBanner component: .usa-banner with US flag (inline CSS gradient), text 
  "An official website of the United States government", collapsible "How you know" section.
• Create SiteHeader component: .usa-header (bg #162e51, 64px), Treasury seal SVG on left, 
  agency name in serif uppercase + app name in sans 800 weight, user info + logout on right.
• Create Sidebar component: .usa-sidenav-container (240px, white, border-right), nav links 
  using .usa-sidenav a with active left-border pattern.
• Confirm: shell renders correctly with banner, header, sidebar, and outlet area.

STEP 3 — Login Page
• Rewrite the login page component to use:
    <div className="login-page">
      <div className="login-card">
        <div className="login-card__banner">[flag gradient] An official website...</div>
        <div className="login-card__header">
          <div className="login-card__agency">U.S. Department of the Treasury</div>
          <div className="login-card__title">[App Name]</div>
          <div className="login-card__subtitle">Secure PIV / CAIA authentication required</div>
        </div>
        <div className="login-card__body">[form with .usa-form-group, .usa-label, .usa-input]</div>
        <div className="login-card__footer">For access issues, contact your system administrator.</div>
      </div>
    </div>
• The login page must NOT use the AppLayout shell — it stands alone.
• Confirm: login page shows dark blue background with card, matches TDDS spec.

STEP 4 — Pages: Migrate One Page at a Time
For EACH page in the application:
• Wrap content in <div className="usa-page">
• Add .usa-page-header with .usa-page-title (serif 28px) and optional .usa-page-subtitle
• Replace any non-TDDS colors (hardcoded hex, Tailwind classes) with var(--usa-*) tokens
• Replace inline style={{ color: ... }} or style={{ background: ... }} with CSS classes
• Replace any non-TDDS button styles with .usa-button variants
• Replace status/type indicators with .usa-tag or .badge variants from TDDS
• Replace any data table with .usa-table inside .table-wrap

STEP 5 — Color & Style Purge
Search the entire src/ directory for:
• Any hex color value not wrapped in var() — replace with the closest TDDS token
• Any font-family that is not var(--font-sans), var(--font-serif), or var(--font-mono)
• Any margin or padding using values not in the spacing scale (4, 8, 12, 16, 20, 24, 32, 40, 48, 56, 64px)
• className strings containing Tailwind class names (text-*, bg-*, p-*, m-*, flex-*, etc.)
• style={{ ... }} props containing static layout properties — move them to CSS classes

STEP 6 — Accessibility Pass
• Confirm .usa-skipnav is the first element in the page shell
• Add aria-label to all icon-only buttons
• Add role="dialog" aria-modal="true" to all modals
• Confirm focus rings are present (outline: 3px solid var(--usa-primary-vivid))
• Replace any <div onClick> interactive elements with proper <button> elements

STEP 7 — Final Checklist
Run through the TDDS file checklist (Section 10 of TDDS.md). Fix any remaining items.
Then confirm:
□ App builds and runs without errors
□ Login page looks like TDDS spec
□ Every page has .usa-banner + .usa-header + sidebar
□ No hardcoded colors remain in JSX or CSS
□ All buttons use .usa-button classes
□ Tables use .usa-table
□ Forms use .usa-form-group / .usa-label / .usa-input

IMPORTANT RULES (follow strictly throughout all steps)
------------------------------------------------------
• NEVER use Tailwind, styled-components, CSS Modules, or emotion — only vanilla CSS
• NEVER hardcode a hex color — always use var(--usa-*) tokens from TDDS
• NEVER use inline style= for static layout values (colors, spacing, fonts)
• NEVER skip the .gov banner — it is legally required on all .gov sites
• NEVER use a font other than Public Sans, Merriweather, or Roboto Mono
• NEVER install @uswds/uswds npm package — tdds.css replaces it
• ALWAYS use semantic HTML: <button> for actions, <nav> for navigation, <main> for content
• ALWAYS include focus styles on interactive elements
```

---

## Tips for Best Results

**Break it into sessions.** Run Steps 1–3 in one session (shell + login), then do one page per session in Step 4. Trying to migrate a 20-page app in one prompt produces inconsistent results.

**Show the AI one page at a time.** For Step 4, paste the current page's JSX and say: "Migrate this page to TDDS. Here is the current code: [paste code]". This is more reliable than asking it to find and fix all pages at once.

**After each step, build and look.** Run `npm run dev` and visually check. If something looks wrong, paste a screenshot or describe what's off before continuing.

**For the filter bar and tables**, always provide the data shape (column names, filter fields) so the AI can use the right TDDS patterns. Don't let it guess.

**Watch for regression on step 5.** The color purge step is where AIs often reintroduce inline styles. After running it, grep for `style={{` in your JSX and review every hit.

---

## Quick Color Reference (Most Common Mistakes)

| You might see | Replace with |
|---|---|
| `#162e51` or `rgb(22,46,81)` | `var(--usa-primary-darker)` |
| `#005ea2` | `var(--usa-primary)` |
| `#1a4480` | `var(--usa-primary-dark)` |
| `#d9e8f6` | `var(--usa-primary-lighter)` |
| `#f0f0f0` | `var(--usa-base-lightest)` |
| `#1b1b1b` | `var(--usa-ink)` |
| `#71767a` | `var(--usa-base)` |
| `#dfe1e2` | `var(--usa-base-lighter)` |
| `#00a91c` | `var(--usa-success)` |
| `#d54309` | `var(--usa-error)` |
| `#ffbe2e` | `var(--usa-warning)` |
