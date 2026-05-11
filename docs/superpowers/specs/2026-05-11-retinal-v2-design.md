# Retinal Homepage v2 — Design Spec
**Date:** 2026-05-11  
**Status:** Approved

---

## Goal

Replace the current `index.html` (v1 launch) with the v2 design prototype, and add the `retinal-logo.png` asset. The v2 prototype is production-ready — no Tweaks panel to strip.

---

## Source Files

- `C:\Users\hishs\Downloads\Retinal Site v2\design_handoff_retinal_homepage\index.html` — complete production HTML
- `C:\Users\hishs\Downloads\Retinal Site v2\design_handoff_retinal_homepage\retinal-logo.png` — cobalt target/aperture logomark PNG (380×380, transparent background)

---

## What Changes vs v1

### New CSS tokens
- `--frame-left: 72px` (56px on ≤768px) — drives the logo cell width and page-frame hairline position

### New: Page-frame borders (`.page-frame`)
- `position: fixed; inset: 0; z-index: 60; pointer-events: none`
- `::before` — 1px vertical ink line at `left: var(--frame-left)`
- `::after` — 1px vertical ink line at `right: 0`
- Both pseudo-elements hidden at `max-width: 1279px` (only show at wide viewports)

### Redesigned nav
- Two-cell layout: logo cell (width = `--frame-left`, contains 44px PNG) + nav-body (wordmark + links + CTA)
- Numbered nav links: `<span class="num">01</span>How it works` etc. — number is muted by default, both number and label turn cobalt on hover
- CTA is now a `<button class="nav-cta">` with text + 32px ink-bordered circle containing a right-arrow SVG. Hover: circle fills ink, nudges 2px right
- Bottom border is always 1px solid `--ink` (was conditional `.scrolled` class in v1)
- Old `.logo-mark` CSS ring+dot is replaced by `<img src="retinal-logo.png">`

### New: Mobile hamburger menu
- `<button class="hamburger">` (36px ink circle, 3 hairlines) replaces nav links on ≤768px
- `<div class="mobile-menu">` — fixed full-width drawer, slides in from top (`.open` toggles `translateY(-100%) → 0`)
- `<div class="mobile-scrim">` — semi-transparent ink overlay behind the menu, closes on tap
- Menu contains 4 numbered links (22px / 500) + full-width solid-ink "Get early access" button
- Accessible: hamburger has `aria-label` + `aria-expanded`; menu has `aria-hidden`

### New: Sticky side-nav indicator (`.side-nav`)
- `position: fixed; right: 24px; top: 50%; translateY(-50%); z-index: 40`
- Visible only at `min-width: 1024px`
- 7 entries: Intro, ROI, How it works, Pricing, Testimonials, FAQ, Get started
- Each entry: 10px / 500 / 0.22em uppercase muted, followed by a 16px hairline
- Active entry (determined by scroll, threshold 35vh from top): turns cobalt, hairline stretches to 28px
- Clicking an entry calls `scrollIntoView({ behavior: 'smooth' })`

### Updated section heads
- **ROI**: left column of `.roi-head` is now an empty `<div>` (no secnum)
- **Pricing**: now a 2-column `.price-head` grid (secnum "02" left, eyebrow + h2 right) — was centred single-column in v1
- **Testimonials**: secnum "03", h2 "What tradespeople say." (no eyebrow, has trailing period)
- **FAQ**: secnum "04", h2 "Common questions" (no eyebrow in secnum column)
- **Footer**: `id="finalcta"` added; eyebrow text is now "Get started" (was "06 — Get started")

### New JS
Replaces the single-script block in v1 with a three-part script:
1. `IntersectionObserver` reveal (same as v1)
2. Mobile menu open/close (hamburger click, scrim click, link/cta click closes)
3. Side-nav active state: on scroll + resize, find the last section whose `offsetTop ≤ scrollY + innerHeight * 0.35`; toggle `.active` on the matching side-nav link

### What does NOT change
- Hero SVG linework (same coordinates, same dense opacity values: grid 0.14, rays 0.55, arcs 0.8, contrast filter)
- Hero copy content
- ROI stat content and `.stat-grid` CSS
- How it works steps content (secnum "01")
- Pricing card content
- Testimonials quotes
- FAQ questions and answers
- Footer links and legal text
- All design tokens except the addition of `--frame-left`

---

## New Asset

`retinal-logo.png` must be copied to the repo root (`C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\retinal-logo.png`) so the `<img src="retinal-logo.png">` reference in the nav resolves correctly on GitHub Pages.

---

## Output

Two files committed and pushed to `main`:
1. `index.html` — full v2 replacement
2. `retinal-logo.png` — logo asset
