# Treasury Digital Design System (TDDS)
## AI Engineering Guide · U.S. Department of the Treasury

This document is the authoritative design specification for all Treasury internal web applications. Feed it as context to any AI coding assistant (Codex, Claude, Copilot, Cursor, etc.) before generating or redesigning application UI.

All Treasury apps **must** conform to this specification. It is derived from production apps built to USWDS (U.S. Web Design System) standards.

---

## 1. Foundational Rules

These are non-negotiable. Follow them in every file you generate.

### CSS Architecture
- **Use vanilla CSS only.** No Tailwind, no styled-components, no CSS Modules, no Sass/SCSS.
- **All styles live in two files:** `src/index.css` (design system + global) and `src/App.css` (app-specific overrides).
- **Use CSS custom properties (`var(--...)`) for every color, spacing, font, shadow, and radius.** Never hardcode hex values or pixel values that have a token equivalent.
- **Class naming: BEM-style with `usa-` prefix** for USWDS components (`usa-button`, `usa-table`, `usa-modal`), plain BEM for app-specific components (`.stat-card`, `.rollup-program__header`).
- **No inline `style=` props** in JSX except for truly dynamic values (e.g. a color computed at runtime from data). Static styles always go in CSS.

### USWDS as the upstream fallback

TDDS is a Treasury-flavored layer on top of [USWDS](https://designsystem.digital.gov/components/). It defines overrides, extensions, and Treasury-specific patterns — it does not replicate every USWDS component.

**When a component, atom, pattern, or style is needed that is not defined in TDDS, look to USWDS before building anything custom.** Apply TDDS color, spacing, and typography tokens to the USWDS markup.

Decision order for any AI agent working in a Treasury app:
1. **Defined in TDDS** → use it, no question needed.
2. **Not in TDDS, exists in USWDS** → offer it: *"TDDS doesn't define a [thing]. USWDS has one — want me to pull it in with TDDS tokens, or use it as-is?"*
3. **Not in TDDS, not in USWDS** → build custom and say so: *"Neither TDDS nor USWDS defines a [thing] — I'll build one to match TDDS conventions."*

Do **not** `npm install @uswds/uswds`. USWDS HTML/CSS patterns can be referenced and copied directly — it's the npm package that's banned, not the markup.

### Required Assets
Every app must include `tdds.css` (or an equivalent `index.css` with all TDDS tokens). Do **not** install the `@uswds/uswds` npm package — use the hand-coded TDDS implementation.

The TDDS repo ships the following canonical assets in its `assets/` directory. Copy all of them into your app's `public/` folder at the start of every Level 3 (full conversion) build:

| File | Use |
|---|---|
| `assets/treasury-seal-white-simple.svg` | Header logo — 48×48px, white, left side of `.usa-header` |
| `assets/favicon.svg` | Browser tab icon — reference in `<link rel="icon">` |
| `assets/us_flag_small.png` | US flag in `.gov` banner |
| `assets/icon-dot-gov.svg` | "Official .gov" icon in `.gov` banner disclosure |
| `assets/icon-https.svg` | "Secure .gov" icon in `.gov` banner disclosure |

Never substitute a different logo, seal, or favicon. These are the canonical Treasury branding assets.

---

## 2. Design Tokens

All tokens are defined as CSS custom properties on `:root`. Import `tdds.css` and use them everywhere.

### Colors

```css
/* Primary — Treasury blue */
--usa-primary-lighter:    #d9e8f6   /* tinted backgrounds, hover states */
--usa-primary-light:      #73b3e7   /* secondary text on dark bg, decorative */
--usa-primary:            #005ea2   /* buttons, links, active borders */
--usa-primary-vivid:      #0050d8   /* focus rings */
--usa-primary-dark:       #1a4480   /* hover on primary */
--usa-primary-darker:     #162e51   /* header bg, page titles, modal headers */

/* Secondary — alert red */
--usa-secondary:          #d83933
--usa-secondary-dark:     #b50909

/* Accent cool — cyan */
--usa-accent-cool-lighter: #e1f3f8
--usa-accent-cool:        #00bde3
--usa-accent-cool-dark:   #28a0cb
--usa-accent-cool-darker: #07648d

/* Accent warm — orange */
--usa-accent-warm-light:  #ffbc78
--usa-accent-warm:        #fa9441
--usa-accent-warm-dark:   #c05600

/* Status */
--usa-success:            #00a91c
--usa-success-dark:       #008817
--usa-success-lighter:    #ecf3ec
--usa-warning:            #ffbe2e
--usa-warning-dark:       #e5a000
--usa-warning-darker:     #936f38
--usa-warning-lighter:    #faf3d1
--usa-error:              #d54309
--usa-error-dark:         #b50909
--usa-error-lighter:      #f4e3db

/* Neutrals */
--usa-base-lightest:      #f0f0f0   /* page background */
--usa-base-lighter:       #dfe1e2   /* borders, dividers */
--usa-base-light:         #a9aeb1
--usa-base:               #71767a   /* secondary text */
--usa-base-dark:          #565c65   /* muted text */
--usa-base-darker:        #3d4551
--usa-base-darkest:       #1b1b1b
--usa-ink:                #1b1b1b   /* body text */
--usa-white:              #ffffff

/* Gov banner */
--gov-banner-bg:          #f0f0f0
--gov-banner-text:        #1b1b1b
```

**Quick reference — when to use which blue:**
| Context | Token |
|---|---|
| Page/section title text | `--usa-primary-darker` |
| Header background, modal header | `--usa-primary-darker` |
| Button background, links | `--usa-primary` |
| Button hover | `--usa-primary-dark` |
| Focus ring | `--usa-primary-vivid` |
| Hover row bg, tinted panel | `--usa-primary-lighter` |
| Icon / decorative on dark bg | `--usa-primary-light` |

### Typography

```css
--font-sans:  'Public Sans', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif
--font-serif: 'Merriweather', Georgia, 'Times New Roman', serif
--font-mono:  'Roboto Mono', 'Bitstream Vera Sans Mono', monospace
```

**Font usage rules:**
- **Public Sans** — all body text, labels, buttons, nav, UI chrome
- **Merriweather serif** — page titles (`.usa-page-title`), stat values (`.stat-value`), empty state headings, login card title only
- **Roboto Mono** — code snippets, IDs, technical strings

**Type scale:**
| Role | Size | Weight | Font |
|---|---|---|---|
| Page title | 28px | 900 | serif |
| Section title | 17px | 800 | sans |
| Card header | 15px | 700 | sans |
| Stat value | 36px | 900 | serif |
| Body | 14.5–16px | 400 | sans |
| Label | 14px | 700 | sans |
| Hint / caption | 13px | 400 | sans |
| Badge / tag / chip | 11–12px | 700 | sans, uppercase |
| Table header | 11px | 700 | sans, uppercase |

### Spacing

All spacing uses an 8px base unit.

```css
--space-05:  4px
--space-1:   8px
--space-105: 12px
--space-2:   16px
--space-205: 20px
--space-3:   24px
--space-4:   32px
--space-5:   40px   /* page padding */
--space-6:   48px
--space-7:   56px
--space-8:   64px
```

Never use arbitrary pixel values like `margin: 10px` or `padding: 18px`. Always round to the nearest token.

### Layout Constants

```css
--sidebar-width: 240px
--max-content:   1200px   /* max-width for .usa-page */
--radius:        4px       /* standard border-radius */
--shadow-1:      0 1px 4px 0 rgba(0,0,0,.1)    /* cards, inputs */
--shadow-2:      0 4px 12px 0 rgba(0,0,0,.12)  /* hovered cards */
--shadow-3:      0 8px 24px 0 rgba(0,0,0,.14)  /* modals, login card */
```

---

## 3. Required Page Shell

Every authenticated page uses this exact layout structure. Do not deviate.

```
┌─────────────────────────────────────────────────────┐
│  .usa-banner  (.gov strip, gray, collapsible)        │
├─────────────────────────────────────────────────────┤
│  .usa-header  (64px, bg: #162e51, Treasury seal)     │
├──────────────────────┬──────────────────────────────┤
│  .usa-sidenav-       │  .usa-main                   │
│   container          │  (flex:1, overflow-y:auto,   │
│  (240px, white,      │   bg: #f0f0f0)               │
│   border-right)      │                              │
│                      │   .usa-page                  │
│  .usa-sidenav        │   (padding:40px, max-w:1200) │
│  (nav links w/       │                              │
│   active left border)│   .usa-page-header           │
│                      │   (border-bottom:2px primary)│
│                      │                              │
│                      │   [page content]             │
└──────────────────────┴──────────────────────────────┘
```

**React structure:**
```tsx
// AppLayout.tsx
<div className="app-shell">           {/* flex column, 100vh */}
  <a className="usa-skipnav" href="#main-content">Skip to main content</a>
  <GovBanner />                        {/* .usa-banner */}
  <SiteHeader />                       {/* .usa-header */}
  <div className="app-body">           {/* flex, flex:1, overflow:hidden */}
    <Sidebar />                        {/* .usa-sidenav-container */}
    <main id="main-content" className="usa-main">
      <Outlet />                       {/* React Router outlet */}
    </main>
  </div>
</div>

// Every page component
<div className="usa-page">
  <div className="usa-page-header">
    <div>
      <h1 className="usa-page-title">Page Title</h1>
      <p className="usa-page-subtitle">Optional subtitle</p>
    </div>
    <div className="flex gap-2">
      {/* Action buttons */}
    </div>
  </div>
  {/* Page content */}
</div>
```

---

## 4. Required Components

### Gov Banner (`.usa-banner`)
- The `.gov` strip **must** appear at the very top of every authenticated page and on the login page.
- Background: `var(--gov-banner-bg)` (#f0f0f0), 12px text, collapsible with "How you know" guidance.
- The US flag icon is an inline CSS gradient — do not use an image file.

### Site Header (`.usa-header`)
```
background: var(--usa-primary-darker)   height: 64px
Left side: Treasury seal (48×48px SVG) + agency name (serif, 11px, uppercase, primary-light) + app name (sans, 20px, 800 weight, white)
Right side: notification bell (optional) + user name + role pill + account dropdown
```

**Logo:** Always use `public/treasury-seal-white-simple.svg` (copied from `assets/` in this repo). Never use a different seal, a PNG substitute, a placeholder, or an emoji. The `<img>` element must be `width="48" height="48" alt="U.S. Department of the Treasury seal"`.

### Sidebar (`.usa-sidenav-container`)
```
width: 240px   background: white   border-right: 1px solid base-lighter
```
- Navigation links use `.usa-sidenav a` — 14.5px, 4px left border (transparent → primary on active/hover)
- Mark the active link with `class="usa-current"` on the `<a>` element (USWDS canonical)
- Group nav items under `.sidenav-section` with optional `.sidenav-section-label` (11px, uppercase, gray)
- Sub-navigation: nest `<ul class="usa-sidenav__sublist">` inside a `.usa-sidenav__item`; supports up to 3 levels

**Basic sidebar:**
```html
<nav class="usa-sidenav-container" aria-label="Primary navigation">
  <div class="sidenav-section">
    <ul class="usa-sidenav">
      <li class="usa-sidenav__item">
        <a href="/dashboard" class="usa-current">Dashboard</a>
      </li>
      <li class="usa-sidenav__item"><a href="/projects">Projects</a></li>
      <li class="usa-sidenav__item"><a href="/reports">Reports</a></li>
    </ul>
  </div>
</nav>
```

**Grouped sections with sub-navigation:**
```html
<nav class="usa-sidenav-container" aria-label="Primary navigation">
  <div class="sidenav-section">
    <p class="sidenav-section-label">Overview</p>
    <ul class="usa-sidenav">
      <li class="usa-sidenav__item">
        <a href="/projects" class="usa-current">Projects</a>
        <ul class="usa-sidenav__sublist">
          <li class="usa-sidenav__item"><a href="/projects/active">Active</a></li>
          <li class="usa-sidenav__item">
            <a href="/projects/archived" class="usa-current">Archived</a>
          </li>
        </ul>
      </li>
    </ul>
  </div>
  <div class="sidenav-section">
    <p class="sidenav-section-label">System</p>
    <ul class="usa-sidenav">
      <li class="usa-sidenav__item"><a href="/settings">Settings</a></li>
    </ul>
  </div>
</nav>
```

### Page Header (`.usa-page-header`)
```css
border-bottom: 2px solid var(--usa-primary)
margin-bottom: var(--space-4)
padding-bottom: var(--space-3)
display: flex; align-items: flex-end; justify-content: space-between
```

---

## 5. Component Patterns

### Stat Cards

Use for key metrics at the top of dashboard pages.

```html
<div class="stat-grid">
  <div class="stat-card">           <!-- add .warn, .danger, or .success for colored top border -->
    <div class="stat-label">TOTAL PROJECTS</div>
    <div class="stat-value">42</div>
  </div>
</div>
```

- Grid: `repeat(auto-fill, minmax(170px, 1fr))`
- Top border colors: default=primary, `.warn`=warning-dark, `.danger`=error, `.success`=success
- Stat value: Merriweather 36px 900 weight, color `primary-darker` (or `.warn`/`.danger`/`.success` modifier)

### Mini Stat Cards

A horizontal summary bar for 4–6 compact metrics. Use when you need more items in a row than `.stat-grid` allows, or want a banner-style strip rather than individual cards.

```html
<div class="mini-stat-row">
  <div class="mini-stat-card">             <!-- add .warn, .success, or .danger for left accent -->
    <div class="stat-label">TOTAL RESOURCES</div>
    <div class="mini-stat-value">13</div>
    <div class="mini-stat-sublabel">8 Federal / 5 Contractor</div>  <!-- optional -->
  </div>
</div>
```

- All cards in one flex row; vertical rules separate them automatically; no background of their own
- Left-border accent: `.warn`=warning-dark, `.success`=success, `.danger`=error, default=`base-lighter` (gray)
- Value: Public Sans 32px 800 weight, `base-darkest`
- Sublabel: 12px `base-dark`, optional context line below the value

### Detailed Stat Cards

Use below a Mini Stat Row to highlight 2–4 actionable focus areas. Each card pairs a metric headline with a brief explanation and one CTA button.

```html
<div class="detail-stat-grid">
  <div class="detail-stat-card danger">    <!-- add .warn, .success, or .danger for left accent -->
    <div class="stat-label">SUGGESTED FOCUS</div>
    <div class="detail-stat-card__heading">1 overloaded resource</div>
    <div class="detail-stat-card__body">Rebalance assignments before pulling in new work.</div>
    <div><button class="usa-button usa-button--outline">Review utilization</button></div>
  </div>
</div>
```

- Grid: `repeat(auto-fill, minmax(260px, 1fr))`
- Left-border accent same system as Mini Stat Cards; default=primary blue
- Heading: Public Sans 26px 700 weight, `base-darkest`
- Body: 14px `base-dark`; CTA uses `usa-button--outline`

### Data Tables

```html
<div class="table-wrap">
  <table class="usa-table">
    <thead>
      <tr><th>Column</th></tr>   <!-- auto: primary-darker bg, white text, uppercase, 11px -->
    </thead>
    <tbody>
      <tr><td>Cell</td></tr>     <!-- auto: hover = primary-lighter bg -->
    </tbody>
  </table>
</div>
```

### Filter Bar

```html
<div class="filter-bar">
  <div class="filter-bar__search">
    <Icon name="search" />
    <input class="usa-input" placeholder="Search…" />
  </div>
  <select class="usa-select">…</select>
  <button class="usa-button usa-button--outline usa-button--sm">Clear</button>
</div>
```

### Buttons

```html
<!-- Primary action -->
<button class="usa-button usa-button--primary">Save</button>

<!-- Secondary / cancel -->
<button class="usa-button usa-button--secondary">Cancel</button>

<!-- Outlined -->
<button class="usa-button usa-button--outline">Export</button>

<!-- Danger -->
<button class="usa-button usa-button--danger">Delete</button>

<!-- Small -->
<button class="usa-button usa-button--primary usa-button--sm">View</button>

<!-- Link-style -->
<button class="usa-button usa-button--unstyled">Details</button>
```

### Tags / Badges

```html
<span class="usa-tag usa-tag--blue">Active</span>
<span class="usa-tag usa-tag--green">Approved</span>
<span class="usa-tag usa-tag--red">Denied</span>
<span class="usa-tag usa-tag--yellow">Pending</span>
<span class="usa-tag usa-tag--gray">Cancelled</span>
<span class="usa-tag usa-tag--cyan">In Review</span>
```

For RAG (Red/Amber/Green) project status, use `RagBadge` component with `status` prop: `green` | `yellow` | `red` | `gray`.

### Alerts

```html
<div class="usa-alert usa-alert--success" role="alert">
  <strong>Success.</strong> Changes saved.
</div>
<div class="usa-alert usa-alert--error" role="alert">
  <strong>Error.</strong> Something went wrong.
</div>
<div class="usa-alert usa-alert--warning">…</div>
<div class="usa-alert usa-alert--info">…</div>
```

### Modal

```html
<div class="usa-modal-overlay" role="dialog" aria-modal="true">
  <div class="usa-modal">
    <div class="usa-modal__header">
      <h2 class="usa-modal__heading">Dialog Title</h2>
      <button class="usa-modal__close" aria-label="Close">✕</button>
    </div>
    <div class="usa-modal__body">
      <!-- form content -->
    </div>
    <div class="usa-modal__footer">
      <button class="usa-button usa-button--secondary">Cancel</button>
      <button class="usa-button usa-button--primary">Confirm</button>
    </div>
  </div>
</div>
```

Modal header always uses `background: var(--usa-primary-darker)` with white text.

### Forms

```html
<div class="usa-form-group">
  <label class="usa-label" for="field-id">Field Label</label>
  <input id="field-id" class="usa-input" type="text" />
  <span class="usa-hint">Optional helper text.</span>
  <!-- on error: -->
  <span class="usa-error-message">This field is required.</span>
</div>
```

- Inputs: 2px border `base`, focus: 2px `primary-vivid` + `box-shadow: 0 0 0 3px rgba(0,80,216,.25)`
- Two-column form: wrap in `<div class="form-grid">`

### Cards

```html
<div class="usa-card">
  <div class="usa-card__header">
    <span class="usa-card__header-title">Section Title</span>
    <button class="usa-button usa-button--sm usa-button--outline">Action</button>
  </div>
  <div class="usa-card__body">…</div>
  <div class="usa-card__footer">…</div>
</div>
```

### Loading & Empty States

```html
<!-- Loading -->
<div class="page-loading">
  <span class="usa-spinner" aria-hidden="true"></span>
  Loading…
</div>

<!-- Empty state -->
<div class="empty-state">
  <div class="empty-state__icon">📂</div>
  <h3>No items found</h3>
  <p>Try adjusting your filters.</p>
</div>
```

---

## 6. Login Screen

The login page is **outside** the authenticated shell — no sidebar, no header.

```html
<div class="login-page">               <!-- full-viewport, primary-darker bg, diagonal texture overlay -->
  <div class="login-card">             <!-- white card, max-width 440px, shadow-3 -->

    <!-- .gov strip -->
    <div class="login-card__banner">
      [US Flag inline SVG/gradient]
      <strong>An official website of the United States government</strong>
    </div>

    <!-- Branding header -->
    <div class="login-card__header">   <!-- primary-darker bg, white text -->
      <div class="login-card__agency">U.S. Department of the Treasury</div>
      <div class="login-card__title">[App Name]</div>
      <div class="login-card__subtitle">Secure PIV / CAIA authentication required</div>
    </div>

    <!-- Form -->
    <div class="login-card__body">
      <form>
        <div class="usa-form-group">
          <label class="usa-label" for="caia-id">CAIA ID</label>
          <input id="caia-id" class="usa-input" autoComplete="username" required />
          <span class="usa-hint">Dev mode: mock CAIA login is enabled.</span>
        </div>
        [error alert if login fails]
        <button class="usa-button usa-button--primary" style="width:100%;justify-content:center">
          Sign In
        </button>
      </form>
    </div>

    <!-- Footer -->
    <div class="login-card__footer">
      For access issues, contact your system administrator.
    </div>

  </div>
</div>
```

**Login screen rules:**
- Background: `var(--usa-primary-darker)` with a subtle diagonal stripe texture (`repeating-linear-gradient` at 45deg, rgba white at ~1.5% opacity)
- Card: white, max-width 440px, `var(--shadow-3)`, centered vertically and horizontally
- Card header: `var(--usa-primary-darker)` background, white text
- Agency label: `var(--usa-primary-light)`, 11px, 700 weight, uppercase, letter-spacing 0.1em
- App name: `var(--font-serif)`, 26px, 900 weight, white
- Subtitle: `var(--usa-primary-lighter)`, 14px
- The `.gov` banner strip appears at the top of the card (not page), using `var(--gov-banner-bg)`

---

## 7. Transitions & Animation

- Hover effects: `transition: background .15s, box-shadow .15s, border-color .15s`
- Card lift: `transform: translateY(-2px)` on hover for clickable cards
- Loading spinner: `animation: spin .7s linear infinite`
- No CSS animations beyond these. No fade-ins, slide-ins, or keyframe animations on page load.

---

## 8. Icons

TDDS uses the USWDS icon set exclusively. **Never use emojis as icons** — not in nav, not in empty states, not in buttons, not anywhere in the UI.

**Source:** The full USWDS icon library is at [https://designsystem.digital.gov/components/icon/](https://designsystem.digital.gov/components/icon/). Do not install `@uswds/uswds` as a full dependency. Instead, during setup, download the USWDS sprite and place it at `public/assets/img/sprite.svg`:

```bash
# One-time setup — download the USWDS sprite
curl -L "https://cdn.jsdelivr.net/npm/@uswds/uswds/packages/uswds-core/src/img/sprite.svg" \
  -o public/assets/img/sprite.svg
```

This approach is preferred over copying from `node_modules` because it stays current as USWDS adds icons.

**Usage pattern:**
```html
<!-- Inline SVG (preferred for accessibility) -->
<svg class="usa-icon" aria-hidden="true" focusable="false">
  <use href="/assets/img/sprite.svg#search"></use>
</svg>

<!-- Always pair with a visible or sr-only label -->
<button aria-label="Search">
  <svg class="usa-icon" aria-hidden="true" focusable="false">
    <use href="/assets/img/sprite.svg#search"></use>
  </svg>
</button>
```

**Sizing:** Use the `.nav-icon` class (18×18px) for nav/UI icons, or set width/height explicitly using spacing tokens.

**Full icon list:** [USWDS icon library](https://designsystem.digital.gov/components/icon/) — consult this page to find icon names before using them.

---

## 9. Accessibility Requirements

- **Skip link:** `<a class="usa-skipnav" href="#main-content">Skip to main content</a>` — first element inside `<body>`, visually hidden until focused.
- **Focus rings:** `outline: 3px solid var(--usa-primary-vivid); outline-offset: 3px` on all interactive elements. Never `outline: none` without a custom focus style.
- **Screen reader text:** Use `.sr-only` for icon-only buttons. Always pair icons with visible or SR-only labels.
- **ARIA:** Use `role="dialog"`, `aria-modal="true"` on modals. Use `aria-label` on icon buttons. Use `aria-expanded` on toggles.
- **Semantic HTML:** Use `<main>`, `<nav>`, `<header>`, `<button>` (not `<div>` with onClick for interactive elements).
- **Color contrast:** All text passes WCAG AA minimum. Do not invent new color combinations — use only tokens from section 2.

---

## 10. The "Never Do" List

These patterns are **banned** in Treasury apps.

| ❌ Never | ✅ Instead |
|---|---|
| Hardcode a color: `color: #005ea2` | Use `color: var(--usa-primary)` |
| Use Tailwind classes: `className="text-blue-600 p-4"` | Use TDDS CSS classes and tokens |
| Apply inline styles for static layout: `style={{ padding: 24, background: '#f0f0f0' }}` | Add a CSS class |
| Use arbitrary spacing: `margin: 18px` | Use `margin: var(--space-2)` (16px) or `var(--space-3)` (24px) |
| Use a different font: `font-family: Inter, Roboto` | Use only `var(--font-sans)`, `var(--font-serif)`, or `var(--font-mono)` |
| Install `@uswds/uswds` npm package | Use `tdds.css` directly |
| Use CSS-in-JS, styled-components, or emotion | Vanilla CSS in index.css |
| Use Tailwind or utility-first CSS | TDDS component classes |
| Skip the `.gov` banner | Always include it — it's legally required on .gov sites |
| Skip the `.usa-skipnav` link | Accessibility requirement |
| Create a new color not in the token set | Use the closest existing semantic token |
| Omit `aria-label` on icon-only buttons | Always label interactive elements |
| Use `outline: none` | Replace with custom `outline: 3px solid var(--usa-primary-vivid)` |
| Skip the login card `.gov` strip | It must appear on the login page too |
| Put the Treasury seal or agency name in a different font | Always serif + uppercase for agency, sans-serif for app name |
| Use a gradient on a background, card, or panel: `background: linear-gradient(...)` | Use flat `var(--usa-*)` color tokens — gradients look vibe-coded and are not TDDS |
| Use an emoji as a UI icon (🔔 📂 ✕ etc.) | Use USWDS SVG icons via sprite — see Section 8 |
| Use a different logo or placeholder for the Treasury seal | Always use `public/treasury-seal-white-simple.svg` from the TDDS assets |
| Use a different favicon | Always use `public/favicon.svg` from the TDDS assets |

---

## 11. Reference Apps

The following Treasury applications have been built with TDDS. Use their screenshots as visual reference when applying the design system.

### Treasury Vault — Secure file sharing and asset management
| Page | Screenshot |
|---|---|
| Login | `screenshots/treasury-vault/01-login.png` |
| Dashboard | `screenshots/treasury-vault/02-dashboard.png` |
| Manage Files | `screenshots/treasury-vault/03-files-manage.png` |
| Asset Library | `screenshots/treasury-vault/04-asset-library.png` |
| Clients | `screenshots/treasury-vault/05-clients.png` |
| Groups | `screenshots/treasury-vault/06-groups.png` |
| System Users | `screenshots/treasury-vault/07-system-users.png` |
| User Roles | `screenshots/treasury-vault/08-user-roles.png` |
| Options | `screenshots/treasury-vault/09-options.png` |
| Integrations | `screenshots/treasury-vault/10-integrations.png` |

### Treasury Software Access Manager — License and software request portal
| Page | Screenshot |
|---|---|
| Login | `screenshots/treasury-software/01-login.png` |
| Software Catalog | `screenshots/treasury-software/02-catalog.png` |
| My Requests | `screenshots/treasury-software/03-my-requests.png` |
| My Licenses | `screenshots/treasury-software/04-my-licenses.png` |
| Approvals | `screenshots/treasury-software/05-approvals.png` |
| Notifications | `screenshots/treasury-software/06-notifications.png` |
| Admin — Dashboard | `screenshots/treasury-software/07-admin-dashboard.png` |
| Admin — Software | `screenshots/treasury-software/08-admin-software.png` |
| Admin — Users | `screenshots/treasury-software/09-admin-users.png` |
| Admin — Licenses | `screenshots/treasury-software/10-admin-licenses.png` |
| Admin — Reports | `screenshots/treasury-software/11-admin-reports.png` |

### Treasury Operations Hub — Project and resource management
| Page | Screenshot |
|---|---|
| Login | `screenshots/treasury-ops-hub/01-login.png` |
| Dashboard | `screenshots/treasury-ops-hub/02-dashboard.png` |
| Projects | `screenshots/treasury-ops-hub/03-projects.png` |
| Resources | `screenshots/treasury-ops-hub/04-resources.png` |
| Resource Requests | `screenshots/treasury-ops-hub/05-resource-requests.png` |
| Notifications | `screenshots/treasury-ops-hub/06-notifications.png` |

---

## 12. File Checklist for a New App

Before shipping any new Treasury app, verify:

- [ ] `tdds.css` imported in `main.tsx` / `index.tsx`
- [ ] Google Fonts preconnect + Public Sans, Merriweather, Roboto Mono loaded in `index.html`
- [ ] `public/treasury-seal-white-simple.svg` copied from TDDS `assets/` — used in `.usa-header`
- [ ] `public/favicon.svg` copied from TDDS `assets/` — referenced as `<link rel="icon" href="/favicon.svg">`
- [ ] `public/assets/img/sprite.svg` downloaded from USWDS CDN — used for all icons
- [ ] `.usa-banner` present on every authenticated page and on the login page
- [ ] `.usa-skipnav` is the first element in the page shell
- [ ] Site header shows `treasury-seal-white-simple.svg` logo, "U.S. Department of the Treasury" in serif uppercase, and app name
- [ ] Sidebar is 240px, white background, `border-right: 1px solid var(--usa-base-lighter)`
- [ ] All colors come from `var(--usa-*)` tokens — no hardcoded hex in JSX or CSS
- [ ] No gradients on any background, card, panel, or container
- [ ] No emojis anywhere in the UI — use USWDS SVG icons via sprite
- [ ] No Tailwind, styled-components, or CSS Modules
- [ ] Login page uses `.login-page` / `.login-card` pattern
- [ ] All interactive elements have focus styles
- [ ] Loading states use `.page-loading` + `.usa-spinner`
- [ ] Empty states use `.empty-state` pattern
