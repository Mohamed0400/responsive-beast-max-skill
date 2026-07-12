# Responsive Beast Max — Complete Rules & Instructions

**Extends:** `responsive-beast` (escalation model, three-layer system, sticky/scroll patterns, design forks).

**Adds:** Ultra-narrow (320–359px) stress testing, pixel-perfect final audits, touch-target enforcement, RTL storefront patterns, sheet/modal RTL safety, iOS input rules, and production-tested CSS class registry from Gold Standard storefront work.

**Skill directory:** `~/.cursor/skills/responsive-beast-max`

---

## When to use this skill (not base responsive-beast)

| Request | Skill |
|---------|-------|
| General responsive layout, breakpoints, sticky | `responsive-beast` |
| **Final audit**, **pixel perfect**, **320px**, **smallest screens**, touch targets, RTL micro-type, storefront polish | **`responsive-beast-max`** |
| "Make it work on every screen size down to 320" | **`responsive-beast-max`** |

Default to **Adaptive mode** — do not wait for user confirmation between audit and fix unless a genuine design fork exists.

---

## Modes

| Mode | Trigger | Workflow |
|------|---------|----------|
| **Pixel-perfect audit** | "final audit", "pixel perfect", "all screen sizes" | `workflows/pixel-perfect-audit.md` |
| **Transform existing** | "fix mobile", "make responsive", "audit" | `../responsive-beast/workflows/transform-existing.md` + this doc |
| **Build** | "mobile-first", "new page" | `../responsive-beast/workflows/build-responsive.md` + ultra-narrow section below |
| **Preview** | "show breakpoints", "live preview" | `scripts/preview.js` |

---

## The Four-Layer Responsive System (Max)

| Layer | Tool | Handles |
|-------|------|---------|
| Continuous | `clamp()`, fluid tokens (`--page-gutter`) | Typography, padding, gap — smooth scaling |
| Ultra-narrow | `@media (max-width: 359px)` | 320px stress test — structural overrides only here |
| Component | Container queries, BEM-style utility classes | Cards, filter panels, trust strips |
| Structural | `@media (min-width: …)` mobile-first | Nav transform, column count, sidebar |

### Escalation (Max)

```
Does content overflow at 320px?
  No  → clamp() / min-width:0 / overflow-wrap. Done.
  Yes → Is it structural (columns, hero split)?
    Yes → @media (max-width: 359px) single-column fallback
    No  → Touch targets, text wrap, shrink flex child (max-width %)
```

**Rule:** Always test **320px** even if primary mobile target is 375–390px. ~3% traffic + catches root flex/grid bugs.

---

## Global Foundation (apply first on every audit)

### Viewport & height

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover" />
```

```css
#root {
  min-height: 100dvh;
  width: 100%;
  min-width: 0;
  overflow-x: clip;
}

html, body {
  overflow-x: clip;   /* NOT overflow:hidden — hidden kills sticky */
  min-width: 0;
}

main, .page-shell {
  min-width: 0;
}
```

### Fluid gutters

```css
:root {
  --page-gutter: clamp(1rem, 2.5vw, 2.5rem);
  --page-max: min(120rem, calc(100% - 2 * var(--page-gutter)));
}

@media (max-width: 359px) {
  :root {
    --page-gutter: 0.75rem;  /* 12px — critical for 320px */
    --space-grid: 0.5rem;
  }
}
```

### Overflow clipping vs sticky

| Need | Use | Avoid |
|------|-----|-------|
| Prevent horizontal scroll | `overflow-x: clip` on html/body/#root | `overflow-x: hidden` on ancestors of sticky/fixed |
| Clip visual bleed | `overflow: clip` | `overflow: hidden` on layout parents of `position: sticky` |

### Flex/grid children

**Always** add `min-width: 0` on flex/grid children that contain dynamic text, prices, Arabic copy, or tabular numbers.

---

## Ultra-Narrow Breakpoint (≤359px)

Dedicated `max-width: 359px` block — **not** a replacement for mobile-first `min-width` queries.

### Hero (asymmetric 2-col → single column)

On wider mobile, keep editorial 2-column hero (`1.14fr / 0.68fr`). At ≤359px:

```css
@media (max-width: 359px) {
  .home-hero-body {
    grid-template-columns: 1fr;
    gap: 0.75rem;
  }
  .home-hero-visual {
    order: -1;              /* bullion image on top */
    max-width: 10.5rem;
    margin-inline: auto;
  }
  .home-hero-intro .type-display {
    font-size: clamp(1.2rem, 7vw, 1.55rem);
  }
}
```

### Trust strip (6 items, 2-col grid)

```css
.hero-trust-strip__label {
  overflow-wrap: anywhere;  /* licence numbers, long Arabic */
}
@media (max-width: 359px) {
  .hero-trust-strip__label {
    font-size: 0.625rem;
    line-height: 1.35;
  }
  html[dir='rtl'] .hero-trust-strip__label {
    font-size: 0.6875rem;   /* Arabic needs slightly larger micro type */
  }
}
```

### Inline CTAs beside inputs (e.g. weight calculator)

When a button sits beside an input at 320px, constrain the button — don't let it push the input off-screen:

```css
@media (max-width: 359px) {
  .prices-weight-cta {
    max-width: 42%;
    white-space: normal;
    text-align: center;
    line-height: 1.2;
    font-size: 0.625rem;
  }
  html[dir='rtl'] .prices-weight-cta {
    font-size: 0.6875rem;
  }
}
```

---

## Touch Targets (44px / 2.75rem)

**Minimum interactive size on mobile:** `2.75rem` (44px) per WCAG 2.5.5.

### Class registry

| Class | Applies to |
|-------|-----------|
| `.nav-icon-btn` | Header icon buttons (search, user, cart) — 44px mobile, 40px `lg+` |
| `.mobile-bottom-nav__item` | Bottom tab bar links |
| `.mobile-nav-link` / `.mobile-nav-sublink` | Drawer navigation |
| `.filter-panel__option` | Filter sheet category rows |
| `.filter-panel__chip` | Carat/metal filter chips |
| `.products-filter-trigger` | Mobile "Filters" toolbar button |
| `.touch-target` | Generic 44×44 |
| `.touch-target--row` | Full-width row min-height 44px |
| `[data-slot='sheet-close']` | Radix/shadcn sheet close — 44×44 hit area |
| `.branches-mobile-chip` | Horizontal branch switcher pills |

### Nav icon button pattern

```css
.nav-icon-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 2.75rem;
  min-width: 2.75rem;
  height: 2.75rem;
  width: 2.75rem;
}
@media (min-width: 1024px) {
  .nav-icon-btn {
    min-height: 2.5rem;
    min-width: 2.5rem;
    height: 2.5rem;
    width: 2.5rem;
  }
}
```

Search toggle when open: use text label «إغلاق» / "Close" on mobile (`min-h-11 min-w-[3.25rem]`), icon only on `lg+`.

---

## iOS & Form Inputs

```css
.contact-form-field,
.product-search-input {
  font-size: max(16px, 0.875rem);  /* prevents Safari zoom on focus */
}
```

Use `type="text"` + `inputMode="search"` for search fields — avoids duplicate native clear (X) buttons alongside custom clear.

---

## RTL / Arabic Rules

### Logical properties (mandatory for new CSS)

| Physical (avoid) | Logical (use) |
|------------------|---------------|
| `left` / `right` | `inset-inline-start` / `inset-inline-end` |
| `ml-` / `mr-` / `pl-` / `pr-` | `ms-` / `me-` / `ps-` / `pe-` |
| `text-left` | `text-start` |
| `border-l` / `border-r` | `border-s` / `border-e` |

### Sheet / modal close (RTL-safe)

```tsx
// Close button
className="absolute top-4 end-4"   // NOT right-4

// Sheet header — room for close button
<SheetHeader className="pe-12 text-start" />
```

### Section kickers (filter panels, static pages)

Use `.page-kicker` instead of raw `uppercase tracking-[0.18em]`:

```css
.page-kicker {
  font-size: 0.6875rem;
  font-weight: 700;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #3f6f00;
}
html[dir='rtl'] .page-kicker {
  letter-spacing: 0;
  text-transform: none;
  font-size: 0.75rem;
}
```

### RTL micro-type overrides

Global RTL body line-height boost can break compact UI. Override explicitly:

```css
/* Bottom nav — must stay compact */
html[dir='rtl'] .mobile-bottom-nav__label {
  font-size: 0.6875rem !important;
  line-height: 1.2 !important;
  font-weight: 700 !important;
  letter-spacing: 0 !important;
}
```

### Long unbreakable strings

Apply to licence numbers, addresses, chip labels:

```css
overflow-wrap: anywhere;  /* or break-word for less aggressive breaks */
min-width: 0;
```

---

## Mobile Bottom Navigation

### Height & safe areas

```css
:root {
  --bottom-nav-content: 4rem;  /* tall enough for icon + Arabic label */
  --bottom-nav-height: calc(var(--bottom-nav-content) + env(safe-area-inset-bottom, 0px));
}
.mobile-bottom-nav {
  padding-bottom: env(safe-area-inset-bottom, 0px);
}
.main-with-bottom-nav {
  padding-bottom: var(--bottom-nav-height);
}
```

### visualViewport pin (iOS keyboard / browser chrome)

Sync fixed bottom nav to `visualViewport` offset when keyboard opens — prevents nav floating mid-screen or clipping.

### Labels

- `text-overflow: ellipsis; white-space: nowrap; text-align: center`
- Short i18n keys for cramped tabs (`nav.productsShort`)
- `touch-action: manipulation` on items

---

## Mobile Drawer Nav Hierarchy

**Problem:** Parent and child both styled "active green" — no hierarchy.

**Fix:**
- Parent route (`/prices`) when on sub-route → **muted label only** (`.mobile-nav-group__label`), not active link style
- Sub-routes → indented with `border-inline-start` + `.mobile-nav-sublink`
- Only the exact matching sub-link gets `--active` background

---

## Search Panel Rules

1. **One clear control** — remove duplicate X buttons (panel side close + input native clear)
2. Mobile header: text «إغلاق» instead of icon-only close
3. Toolbar variant: larger input (`min-h-[3rem]`), text «مسح» clear button
4. Panel: `.product-search-panel { max-width: 100%; box-sizing: border-box; }`

---

## Filter Sheet (Products / Filters)

```tsx
<SheetContent side={isAr ? 'left' : 'right'} className="w-full max-w-md p-0">
  <SheetHeader className="border-b px-5 py-4 pe-12 text-start">
    <SheetTitle className="pe-1">{t('filtersHeading')}</SheetTitle>
  </SheetHeader>
  <FilterPanel showHeading={false} />  {/* avoid duplicate title + X overlap */}
</SheetContent>
```

Filter section headings → `.page-kicker` + `.filter-panel__section-title`.

---

## Static Storefront Pages (`/about`, `/contact`, `/branches`)

```css
.storefront-static-page {
  min-height: 100dvh;
}
.storefront-static-page__tail {
  padding-bottom: calc(clamp(1rem, 3vw, 1.75rem) + env(safe-area-inset-bottom));
}
```

### Branches mobile

- Horizontal chip scroller: `.branches-mobile-switcher` with `overflow-x: auto`, hidden scrollbar
- Chips: `min-height: 2.75rem`, `white-space: nowrap`, `flex-shrink: 0`
- Hero chips stack vertical on `max-width: 639px`
- No auto map popup on mobile — single card + chip switcher

### About licence

- Trust logos via `HeroTrustIcon` + `HeroTrustStrip` patterns
- Licence number: **text only** (no PDF link)
- `.about-licence-number` + `overflow-wrap: anywhere`

### Contact

- All inputs: `.contact-form-field` with 16px min font
- Form max-width: `--page-max-form`

---

## Prices Page Mobile

- Hero: compact type at `text-xl` → `sm:text-4xl`
- KWD ounce cards: `grid-cols-2 gap-2` — keep at 320px
- Chart metal tabs: `overflow-x-auto` horizontal scroll, `sm:flex-wrap`
- Weight input: `min-w-0 flex-1`, `text-base` (16px)
- CTA beside input: `.prices-weight-cta` ultra-narrow rules above

---

## Pixel-Perfect Audit Workflow

### Priority viewports (Max order)

| Priority | Width | Why |
|----------|-------|-----|
| 1 | **320** | Stress test — if it passes, 360+ will |
| 2 | 360 | Most common Android |
| 3 | 375 | iPhone SE / older |
| 4 | 390 | iPhone 12–15 |
| 5 | 768 | iPad portrait |
| 6 | 1024 | Laptop / large tablet |
| 7 | 1440 | Desktop reference |

**Also:** drag continuously 280px → 2560px — don't jump between presets only.

### 12-Point Pixel-Perfect Check (per viewport)

1. **Zero horizontal scroll** — `document.documentElement.scrollWidth <= clientWidth`
2. **No text collision** — headlines, badges, prices don't overlap
3. **Touch targets ≥ 44px** on mobile widths
4. **Inputs ≥ 16px** font — no iOS zoom
5. **Sticky/fixed elements** — bottom nav, ticker, headers not clipped
6. **Safe areas** — `env(safe-area-inset-*)` on fixed footers
7. **RTL mirror** — sheet side, close button `end-4`, indent `margin-inline-start`
8. **Sheet/modal** — title not under close X; `pe-12` on header
9. **Images/media** — `max-width: 100%`, no overflow from bullion/hero assets
10. **Flex/grid wrap** — filter chips wrap; toolbar doesn't overflow
11. **Z-index** — search panel, sheets, chart fullscreen above nav
12. **Reduced motion** — `prefers-reduced-motion: reduce` respected

### Cross-page sweep checklist

After foundation CSS, verify each route:

- [ ] `/` — hero (2-col → 1-col at 359), trust strip, ticker, bottom nav
- [ ] `/products` — filter trigger, chips, sort select, filter sheet RTL
- [ ] `/products/:slug` — gallery, qty controls, add-to-cart
- [ ] `/prices` — calculator row, chart tabs, metal cards
- [ ] `/cart` — line items, club pricing, checkout CTA
- [ ] `/branches` — chip scroller, single mobile card
- [ ] `/about` — licence strip, trust cards wrap
- [ ] `/contact` — form fields 16px, message textarea
- [ ] `/register` — KYC fields stack on narrow

### Fix priority order

1. Foundation — overflow-x clip, min-width:0, viewport meta, dvh
2. Ultra-narrow — 359px structural fallbacks
3. Touch targets — 44px classes
4. RTL — logical properties, sheet headers, kicker overrides
5. Component-specific — search, filters, nav hierarchy
6. Polish — spacing tokens, reduced motion, typography clamp
7. Build verify — `npm run build` must pass

### Verification output template

```markdown
## Pixel-Perfect Audit — [Project]

**Tested:** 320, 360, 375, 768, 1440 (+ drag 280→2560)
**RTL:** ar + en checked on [pages]

### Fixed
- [category]: [what changed]

### Build
- [ ] `npm run build` passes

### Remaining (if any)
- [issue] at [viewport] — [reason deferred]
```

---

## Tools

### Live multi-viewport preview

```bash
node ~/.cursor/skills/responsive-beast-max/scripts/preview.js http://localhost:5173
node ~/.cursor/skills/responsive-beast-max/scripts/preview.js http://localhost:5173 --breakpoints 320,360,375,768,1440
```

### Snapshots (before/after)

```bash
node ~/.cursor/skills/responsive-beast-max/scripts/snapshot.js http://localhost:5173 --breakpoints 320,360,375,768,1440
node ~/.cursor/skills/responsive-beast-max/scripts/snapshot.js http://localhost:5173 --before
# ... make changes ...
node ~/.cursor/skills/responsive-beast-max/scripts/snapshot.js http://localhost:5173
```

**Windows note:** `snapshot.js` heredoc may fail in Git Bash (`<< was unexpected`). Fallback:
1. DevTools → Responsive → drag to 320px → Capture full-size screenshot
2. Repeat at 360, 375, 768, 1440
3. Or run snapshot from PowerShell / WSL

---

## Inherited from responsive-beast (still mandatory)

Load `~/.cursor/skills/responsive-beast/references/ai-failure-patterns.md` before outputting CSS.

Top failures to re-check every audit:

1. `100vh` → use `100dvh` / `100svh`
2. Desktop-first `max-width` queries for layout → mobile-first `min-width`
3. Missing `min-width: 0` on flex children
4. `overflow: hidden` killing sticky
5. `transform` on ancestor breaking `position: fixed`
6. Input font < 16px on iOS
7. Missing safe-area insets
8. Missing `align-self: start` on sticky in flex/grid
9. Optimizing only 1440px — **always include 320px**

---

## CSS Audit Block Template

Append to project global CSS (e.g. `index.css`) after component styles:

```css
/* --------------------------------------------------------------------------
 * Responsive Beast Max — [project] audit block
 * Viewports: 320px → ultrawide | Touch: 44px | RTL: logical properties
 * -------------------------------------------------------------------------- */

@media (max-width: 359px) {
  :root { --page-gutter: 0.75rem; }
  /* project-specific ultra-narrow overrides */
}

/* Touch targets, overflow-wrap, sheet close, form 16px — see class registry above */
```

---

## Design Forks (ask user only when ambiguous)

| Pattern | Options |
|---------|---------|
| Hero on 360–639px | A) Keep 2-col asymmetric B) Single column always below 640px |
| Bottom nav item count | A) 4 items B) 5 items with smaller labels |
| Filter on mobile | A) Bottom sheet B) Side sheet (RTL-aware) |

Default choices from Gold Standard work: **2-col hero until 359px**, **5-item bottom nav**, **side sheet** (`left` in RTL).

---

## Reference Index

| File | Contents |
|------|----------|
| [responsive-beast-max.md](responsive-beast-max.md) | This file — full rules |
| [workflows/pixel-perfect-audit.md](workflows/pixel-perfect-audit.md) | Step-by-step final audit |
| `../responsive-beast/references/ai-failure-patterns.md` | Pre-flight scan |
| `../responsive-beast/references/testing-checklist.md` | 10-point base check |
| `../responsive-beast/references/sticky-scroll-patterns.md` | Bottom nav, visualViewport |
| `../responsive-beast/references/modern-css-patterns.md` | clamp, container queries |
