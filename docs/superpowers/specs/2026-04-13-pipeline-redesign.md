# HyperForge Site — Pipeline Section Redesign

**Date:** 2026-04-13  
**Status:** Approved  
**File:** `Product/site/index.html`

---

## Summary

Five changes to `index.html`:

1. **Merge** Platform Features + Pipeline → single parallax workflow section
2. **Remove** Bot Library section (`#bots`)
3. **Remove** Why HyperForge section (`#different`) — keep Competitive Landscape
4. **Mobile-friendly** Competitive Landscape table
5. **Builder fee copy** — remove Phantom reference, add clear explanation of why the fee exists

---

## 1. Unified Parallax Workflow Section

Replaces both `#features` and `#pipeline`. Single `<section id="pipeline">`.

### Layout

Two-column grid, `align-items: start`:

| Column | Behaviour |
|--------|-----------|
| Left (timeline) | `position: sticky; top: 100px` — stays fixed while user scrolls |
| Right (panels) | Normal scroll — each panel is `min-height: 85vh` |

### Left column — sticky timeline

- Continuous vertical gradient line: `position: absolute; left: 10px; top: 22px; bottom: 22px; width: 2px`  
  Gradient: `#6366f1 → #22d3ee → #a78bfa → #34d399 → #f59e0b`
- 5 steps, each: number dot + tag + name + sub-label
- Active step: dot scales 1.25× + glow shadow; inactive steps opacity 0.35
- Clicking a step smooth-scrolls to its panel

### Right column — scrolling panels

5 panels, each `min-height: 85vh`, centered content:
- Tag, title, description, feature chips
- Dark UI preview card (mock `.forge` dashboard screenshot)
- Entrance animation: `opacity 0 → 1` + `translateY(30px → 0)` on IntersectionObserver trigger (threshold 0.4)

### 5 Steps — content

| # | Tag | Title | Chips | Card title |
|---|-----|-------|-------|------------|
| 1 | Strategy Builder · indigo | Create from 20+ templates | Trend, Mean Rev, Funding Harvest, Order Flow | strategy-builder.forge |
| 2 | Backtest · cyan | Validate on historical data | 3+ years data, Fee-adjusted, Monte Carlo | backtest-results.forge |
| 3 | Forward Test · purple | Paper trade in live markets | Real prices, Zero risk, vs Backtest comparison | forward-test.forge |
| 4 | Live Deploy · green | One click to mainnet | Non-custodial, Circuit breakers, 24/7 uptime | live-dashboard.forge |
| 5 | AI Optimization · amber | Gets smarter while you sleep | Hourly on Alpha, GLM-5.1, Zero manual work | ai-optimizer.forge |

### JS (IntersectionObserver)

```js
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      setActive(parseInt(e.target.dataset.panel)); // highlight left step
      e.target.classList.add('visible');           // fade-in panel
    }
  });
}, { threshold: 0.4 });
```

`setActive(i)`: toggle `.active` / `.inactive` on `.t-step` elements.

### Mobile (≤768px)

Left column: `position: relative; top: 0` — no longer sticky, displays above panels.  
Panels: `min-height: auto; padding: 24px 0`.

---

## 2. Remove Bot Library Section

Delete the full `<!-- Bot Types -->` section (`<section class="section course" id="bots">`).  
Also remove `#bots` from any nav links.

---

## 3. Remove Why HyperForge Section — Keep Competitive Landscape

- Delete the `.diff-grid` (4 diff-cards) and the section header ("Why HyperForge / The only 7/7 platform").
- Keep the Competitive Landscape table, promote it to a standalone section with its own header.

---

## 4. Mobile-Friendly Competitive Landscape

Current table has `min-width: 600px` inside `overflow-x: auto` — works but is cramped.

Replace with a **card-per-feature** layout on mobile (≤640px):

- Each row becomes a card: feature name at top, then 4 competitor columns as a 2×2 grid
- Competitor name + ✓/✗ per card cell
- On desktop (>640px): keep existing table layout

Alternative (simpler): reduce column padding, abbreviate competitor names to icons/short labels, and reduce font size so the table fits without horizontal scroll on 390px+ screens.

**Decision:** Use the simpler approach — reduce padding and font-size, abbreviate column headers on mobile. Avoids a full layout rewrite.

Mobile table adjustments:
```css
@media (max-width: 640px) {
  .comp-table th, .comp-table td { padding: 10px 8px; font-size: 11px; }
  .comp-table th:first-child, .comp-table td:first-child { padding-left: 12px; }
  /* Abbreviate headers via data-short attribute or just shorten text */
}
```

---

## 5. Builder Fee Copy

### Remove
The phrase `— Phantom charges 0.05%` from the builder fee explainer banner in the pricing section.

### Before
> Free users pay a 0.025% builder fee per order (the lowest on Hyperliquid — Phantom charges 0.05%). Paid tiers reduce or eliminate it. A $50K/month trader saves more than $149 in fees by upgrading to Alpha.

### After
> HyperForge is free to use. To keep it that way, free accounts pay a 0.025% builder fee per order — charged by Hyperliquid on your behalf and passed through at cost, the lowest rate on the platform. Paid tiers eliminate it entirely. A trader doing $50K/month saves over $149 in fees by upgrading to Alpha — more than the subscription costs.

Also remove the same Phantom reference from the `#different` diff-card (being deleted anyway).

---

## Files Changed

- `Product/site/index.html` — all changes above

## Nav

Remove `#bots` link if present in nav. Rename `#pipeline` link to `Pipeline` if not already.
