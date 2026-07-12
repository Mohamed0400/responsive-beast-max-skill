---
name: responsive-beast-max
description: "Maximum responsive design skill — extends responsive-beast with pixel-perfect final audits, ultra-narrow 320–359px stress testing, 44px touch targets, RTL/logical-property rules, iOS input safety, sheet/modal RTL patterns, and production CSS class registry. Use for final responsive audits, pixel-perfect polish, smallest-screen fixes, storefront/mobile nav/filter/search work, or when the user mentions responsive-beast-max, 320px, touch targets, or pixel perfect."
argument-hint: "[audit|pixel-perfect|build|preview]"
---

# Responsive Beast Max

**Skill directory:** `~/.cursor/skills/responsive-beast-max`

**Full rules (read first on every audit):** [responsive-beast-max.md](responsive-beast-max.md)

Extends `responsive-beast` with everything missing for **production storefronts** — especially **320px stress tests**, **pixel-perfect audits**, **RTL Arabic**, and **44px touch targets**.

## Quick Start

| User says | Do this |
|-----------|---------|
| "final audit", "pixel perfect", "all screen sizes", "320px" | Read [workflows/pixel-perfect-audit.md](workflows/pixel-perfect-audit.md) + [responsive-beast-max.md](responsive-beast-max.md) → fix → verify |
| "fix mobile", "make responsive" | Read `../responsive-beast/workflows/transform-existing.md` + ultra-narrow section in responsive-beast-max.md |
| "build mobile-first" | Read `../responsive-beast/workflows/build-responsive.md` + foundation rules in responsive-beast-max.md |
| "preview breakpoints" | `node scripts/preview.js <url> --breakpoints 320,360,375,768,1440` |

**Default: Adaptive** — audit, fix, and verify without waiting for confirmation unless a design fork is genuinely ambiguous.

---

## Max vs Base — What's Added

| Base responsive-beast | Beast Max adds |
|----------------------|----------------|
| 375/768/1440 testing | **320px mandatory** + 360/390 priority |
| General touch target mention | **`.nav-icon-btn`, filter/sheet class registry** |
| overflow:hidden warning | **`overflow-x: clip` foundation block** |
| RTL mentioned in checklist | **Logical properties, sheet `end-4`, `page-kicker`, bottom-nav RTL overrides** |
| Transform audit workflow | **Pixel-perfect 12-point check + cross-page sweep** |
| — | **Search/filter/bottom-nav/static-page patterns from production** |

---

## Non-Negotiable Foundation (every project)

Before component fixes, ensure:

1. `viewport-fit=cover` in meta
2. `#root { min-height: 100dvh; min-width: 0; overflow-x: clip }`
3. `html, body { overflow-x: clip }` — never `hidden` on sticky ancestors
4. `--page-gutter: clamp(1rem, 2.5vw, 2.5rem)` → `0.75rem` at `max-width: 359px`
5. `min-width: 0` on `main`, `.page-shell`, flex/grid text children
6. Inputs: `font-size: max(16px, 0.875rem)`

---

## Ultra-Narrow Rule (≤359px)

Dedicated `max-width: 359px` block for **structural** changes only:

- Hero: 2-col → 1-col, visual `order: -1`
- Trust strip: smaller type + `overflow-wrap: anywhere`
- Inline CTAs beside inputs: `max-width: 42%`, `white-space: normal`
- Tighter gutters: `--page-gutter: 0.75rem`

Do **not** replace mobile-first `min-width` breakpoints with desktop-first `max-width` layout.

---

## Touch Targets (44px)

Apply `.nav-icon-btn`, `.touch-target`, `.filter-panel__option`, `.filter-panel__chip`, `.products-filter-trigger`, `[data-slot='sheet-close']` — see class registry in [responsive-beast-max.md](responsive-beast-max.md).

---

## Pixel-Perfect Audit (summary)

Full workflow: [workflows/pixel-perfect-audit.md](workflows/pixel-perfect-audit.md)

**Viewports:** 320 → 360 → 375 → 768 → 1440, then drag 280→2560.

**Pages to sweep (storefront):** `/`, `/products`, `/prices`, `/cart`, `/branches`, `/about`, `/contact`.

**Fix order:** foundation → ultra-narrow → touch → RTL → components → build.

---

## RTL Checklist (quick)

- [ ] `end-4` / `start-3` not `right-4` / `left-3`
- [ ] Sheet `side={isAr ? 'left' : 'right'}`
- [ ] `SheetHeader` has `pe-12`
- [ ] `.page-kicker` for section labels (not raw uppercase tracking)
- [ ] `html[dir='rtl']` overrides on compact UI (bottom nav labels)
- [ ] `overflow-wrap: anywhere` on licence numbers, addresses

---

## Tools

```bash
# Live preview (all breakpoints side by side)
node ~/.cursor/skills/responsive-beast-max/scripts/preview.js http://localhost:5173 --breakpoints 320,360,375,768,1440

# Before/after snapshots
node ~/.cursor/skills/responsive-beast-max/scripts/snapshot.js http://localhost:5173 --breakpoints 320,360,375,768,1440 --before
```

**Windows:** snapshot heredoc may fail in Git Bash — use DevTools full-page screenshots or PowerShell/WSL. See responsive-beast-max.md.

---

## Required Reading by Mode

| Mode | Read |
|------|------|
| Pixel-perfect audit | `responsive-beast-max.md`, `workflows/pixel-perfect-audit.md`, `../responsive-beast/references/ai-failure-patterns.md` |
| Transform | `../responsive-beast/workflows/transform-existing.md`, `responsive-beast-max.md` (foundation + ultra-narrow) |
| Build | `../responsive-beast/workflows/build-responsive.md`, `../responsive-beast/references/modern-css-patterns.md` |
| Sticky/bottom nav | `../responsive-beast/references/sticky-scroll-patterns.md` |
| Verify | `../responsive-beast/references/testing-checklist.md` + 12-point check in responsive-beast-max.md |

---

## Gotchas (Max-specific)

1. **Two-column hero at 360px but not 320px** — use 359px fallback, not 639px only
2. **Duplicate search X buttons** — `type="text"` + `inputMode="search"`, one clear control
3. **Filter sheet title under X in RTL** — `end-4` + `pe-12`, `showHeading={false}` in sheet body
4. **Prices parent + sub both green in mobile nav** — parent = muted group label only
5. **Bottom nav clipping Arabic** — `--bottom-nav-content: 4rem`, RTL label override
6. **iOS keyboard displacing fixed nav** — sync to `visualViewport`
7. **Auditing only 375px** — always include **320px**

---

## Success Criteria

- [ ] No horizontal scroll at 320, 360, 375, 768, 1440
- [ ] Touch targets ≥ 44px on mobile widths
- [ ] RTL sheets/headers/close buttons correct
- [ ] Inputs ≥ 16px on iOS
- [ ] `npm run build` passes (if applicable)
- [ ] Audit summary presented with viewports tested
