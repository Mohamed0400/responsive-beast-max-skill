# Workflow: Pixel-Perfect Final Audit

Use when the user asks for a **final**, **pixel-perfect**, or **all screen sizes** responsive pass — especially down to **320px**.

**Prerequisite:** Read [responsive-beast-max.md](../responsive-beast-max.md) in full.

**Do not wait** for user confirmation between steps unless a design fork is ambiguous.

---

## Step 0: Pre-flight

Load `~/.cursor/skills/responsive-beast/references/ai-failure-patterns.md`.

Confirm dev server URL (e.g. `http://localhost:5173`). Note RTL default (`ar` / `dir="rtl"`) if applicable.

Optional baseline:

```bash
node ~/.cursor/skills/responsive-beast-max/scripts/snapshot.js <url> --breakpoints 320,360,375,768,1440 --before
```

---

## Step 1: Foundation Scan (code, not visual)

Check these in global CSS / layout shell **before** opening DevTools:

| Check | Pass criteria |
|-------|---------------|
| Viewport meta | `width=device-width`, `viewport-fit=cover` |
| Height | `#root` uses `100dvh`, not bare `100vh` |
| Overflow | `html, body, #root` → `overflow-x: clip` |
| Gutters | `--page-gutter: clamp(...)` + `0.75rem` at `max-width: 359px` |
| Flex children | `min-width: 0` on `main`, `.page-shell`, dynamic text containers |
| Forms | `font-size: max(16px, 1rem)` on inputs |
| Safe areas | `env(safe-area-inset-bottom)` on fixed bottom nav |
| RTL | Logical properties (`end-`, `start-`, `ms-`, `me-`) on new positioning |

**If any fail → fix foundation first.** Do not proceed to component polish until foundation passes.

---

## Step 2: Ultra-Narrow Block (≤359px)

Add or verify project-specific `@media (max-width: 359px)` overrides:

1. **Hero / above-fold** — if 2-column below 640px, add 1-column fallback at 359px with visual `order: -1`
2. **Trust/badge grids** — `overflow-wrap: anywhere`, smaller micro type
3. **Input + button rows** — constrain button `max-width: 42%`, allow `white-space: normal`
4. **Licence / address text** — `min-width: 0`, `overflow-wrap: anywhere`

---

## Step 3: Touch Target Pass

Audit every interactive element at mobile widths:

- Header icons → `.nav-icon-btn` (44px)
- Bottom nav items → `min-height: 2.75rem`
- Drawer links → `min-height: 2.75rem`
- Filter sheet options/chips → `.filter-panel__option` / `.filter-panel__chip`
- Sheet close → `[data-slot='sheet-close']` 44×44
- Toolbar buttons (filters, sort) → `min-h-11` / `.touch-target--row`
- Horizontal chips (branches, chart tabs) → scrollable OR `min-height: 2.75rem`

---

## Step 4: RTL Pass

Toggle `dir="rtl"` / Arabic locale and re-check:

- [ ] Sheet opens from correct side (`left` in RTL)
- [ ] Close button `end-4` — title not under X (`pe-12` on header)
- [ ] Filter section headings use `.page-kicker` (no broken uppercase tracking)
- [ ] Bottom nav labels readable, not clipped (RTL font-size override)
- [ ] Indent borders use `border-inline-start`, `margin-inline-start`
- [ ] Long Arabic strings wrap (trust strip, licence, addresses)

---

## Step 5: Cross-Page Visual Sweep

At each priority viewport (**320, 360, 375, 768, 1440**), check:

### `/` Home
- Hero layout (2-col → 1-col at 359)
- Trust strip 2-col grid
- Ticker + language switcher not overflowing
- Bottom nav not clipping labels

### `/products`
- Filter trigger + active chips wrap
- Sort select full width on mobile
- Filter sheet: title, close, scrollable panel
- Product grid cards — prices, carat labels, add button

### `/prices`
- Weight calculator input + CTA row at 320px
- Chart metal tabs horizontal scroll
- KWD cards `grid-cols-2` intact

### `/cart`, `/products/:slug`
- Line items, qty controls, CTAs
- No horizontal overflow from prices

### `/branches`, `/about`, `/contact`
- Static page gutters
- Branch chip scroller
- Contact form 16px inputs
- About licence/trust cards wrap

### Global chrome
- Search panel (one clear, no duplicate X)
- Mobile drawer nav hierarchy (parent muted on sub-routes)
- Fixed bottom nav + `visualViewport` behavior

**Drag test:** Slowly resize 280px → 2560px on home and products — note any in-between breaks.

---

## Step 6: 12-Point Check (per viewport)

From responsive-beast-max.md:

1. Zero horizontal scroll
2. No text collision
3. Touch targets ≥ 44px
4. Inputs ≥ 16px
5. Sticky/fixed not clipped
6. Safe areas on fixed footers
7. RTL mirror correct
8. Sheet/modal header spacing
9. Images/media contained
10. Flex/grid wrap behavior correct
11. Z-index stacking (search, sheets, chart fullscreen)
12. `prefers-reduced-motion` respected

---

## Step 7: Implement Fixes

**Priority order:**

1. Foundation (overflow, gutters, min-width:0)
2. Ultra-narrow structural fallbacks
3. Touch targets
4. RTL logical properties + overrides
5. Component-specific (search, filters, nav, static pages)
6. Typography/spacing polish

**Rules:**
- Minimal diff — match existing conventions
- Prefer CSS classes in global audit block over scattered one-offs
- Reuse class registry from responsive-beast-max.md
- Do not introduce `overflow: hidden` on sticky ancestors

Run `npm run build` (or project equivalent) after changes.

---

## Step 8: Verify & Report

### Automated (optional)

```bash
node ~/.cursor/skills/responsive-beast-max/scripts/snapshot.js <url> --breakpoints 320,360,375,768,1440
```

### Manual (required if snapshots fail on Windows)

DevTools → Responsive → full-page screenshot at 320, 360, 375, 768, 1440.

### Report template

```markdown
## Pixel-Perfect Audit — [Project]

**Viewports tested:** 320, 360, 375, 768, 1440 (+ drag 280→2560)
**RTL:** [pages checked in ar]

### Foundation
- [what was added/fixed]

### Ultra-narrow (≤359px)
- [hero, trust, CTA rows, etc.]

### Touch targets
- [components updated]

### RTL
- [sheet, kickers, bottom nav, etc.]

### Pages verified
- [x] / [x] /products ...

### Build
- [x] passes / [ ] N/A

### Remaining
- none / [list]
```

Offer live preview:

```bash
node ~/.cursor/skills/responsive-beast-max/scripts/preview.js <url> --breakpoints 320,360,375,768,1440
```

---

## Success Criteria

- [ ] Foundation rules applied
- [ ] 320px passes without horizontal scroll
- [ ] 44px touch targets on mobile interactive elements
- [ ] RTL sheet/search/filter patterns correct
- [ ] Cross-page sweep completed
- [ ] Build passes
- [ ] Audit report delivered
