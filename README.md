# Responsive Beast Max

**Maximum responsive design** — extends [responsive-beast](../responsive-beast/SKILL.md) with pixel-perfect audits, 320px stress testing, touch targets, and RTL storefront patterns.

## Files

| File | Purpose |
|------|---------|
| [SKILL.md](SKILL.md) | Agent entry point — read this first |
| [responsive-beast-max.md](responsive-beast-max.md) | **Complete rules & instructions** (all patterns from production audits) |
| [workflows/pixel-perfect-audit.md](workflows/pixel-perfect-audit.md) | Final audit step-by-step |
| [scripts/](scripts/) | `preview.js`, `snapshot.js` (copied from responsive-beast) |

## Invoke

```
/responsive-beast-max audit
/responsive-beast-max pixel-perfect
```

Or: "run responsive-beast-max final audit on the storefront"

## Origin

Built from Gold Standard storefront (`Gold-Standard-Front`) responsive work:
- Hero asymmetric 2-col + 359px fallback
- Bottom nav visualViewport + Arabic labels
- Search/filter sheet RTL fixes
- Touch target class registry
- Static pages, branches, prices calculator ultra-narrow rules
