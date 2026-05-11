# Retinal Site Launch Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the placeholder `index.html` with the production Retinal marketing site by cleaning up the design prototype (stripping the Tweaks panel, baking in Dense linework).

**Architecture:** Single self-contained `index.html` — no build step, no framework. All CSS is inline in `<style>`, all JS is inline in `<script>`, hero and footer SVGs are inline in the HTML. The source prototype is `C:\Users\hishs\Downloads\Retinal Site\design_handoff_retinal_site\Retinal.html`.

**Tech Stack:** Plain HTML5 / CSS3 / vanilla JS. Google Fonts (Inter). GitHub Pages via `www.retinal.info` CNAME.

---

### Task 1: Strip the Tweaks panel and bake in Dense linework

**Files:**
- Modify: `index.html` (full rewrite from the prototype)

Source file: `C:\Users\hishs\Downloads\Retinal Site\design_handoff_retinal_site\Retinal.html`

This task is a series of exact edits to the prototype. Apply them in order.

---

**Step 1a: Update the hero-art div to add the Dense contrast filter**

- [ ] In the hero section, find:
```html
<div class="hero-art" id="heroArt" aria-hidden="true">
```
Change to:
```html
<div class="hero-art" id="heroArt" aria-hidden="true" style="filter: contrast(1.05)">
```

---

**Step 1b: Bake in Dense opacity on the SVG grid group**

- [ ] Find:
```html
<g class="lw lw-grid" stroke="var(--accent)" stroke-opacity="0.07" stroke-width="1">
```
Change to:
```html
<g class="lw lw-grid" stroke="var(--accent)" stroke-opacity="0.14" stroke-width="1">
```

---

**Step 1c: Bake in Dense opacity on the SVG rays group**

- [ ] Find:
```html
<g class="lw lw-rays" stroke-width="1" stroke-opacity="0.32">
```
Change to:
```html
<g class="lw lw-rays" stroke-width="1" stroke-opacity="0.55">
```

---

**Step 1d: Bake in Dense opacity on the SVG arcs group**

- [ ] Find:
```html
<g class="lw lw-arcs" stroke-width="1" stroke-opacity="0.55">
```
Change to:
```html
<g class="lw lw-arcs" stroke-width="1" stroke-opacity="0.8">
```

---

**Step 1e: Remove the Tweaks CSS block**

- [ ] Find and delete this entire `<style>` block (it begins right after `</script>` at the end of the main JS block):
```html
<style>
  /* Linework density modes */
  body[data-lines="minimal"] .lw-grid,
  ...
  body[data-pose="poster"] .stat-num { font-size: 112px; }
</style>
```
Delete everything from `<style>` (the one with the `/* Linework density modes */` comment) through its closing `</style>`. The main stylesheet earlier in `<head>` stays.

---

**Step 1f: Remove the three UMD script tags**

- [ ] Find and delete these three consecutive lines:
```html
<script src="https://unpkg.com/react@18.3.1/umd/react.development.js" integrity="sha384-hD6/rw4ppMLGNu3tX5cjIb+uRZ7UkRJ6BPkLpg4hAu/6onKUg4lLsHAs9EBPT82L" crossorigin="anonymous"></script>
<script src="https://unpkg.com/react-dom@18.3.1/umd/react-dom.development.js" integrity="sha384-u6aeetuaXnQ38mYT8rp6sbXaQe3NL9t+IBXmnYxwkUI2Hw4bsp2Wvmx4yRQF1uAm" crossorigin="anonymous"></script>
<script src="https://unpkg.com/@babel/standalone@7.29.0/babel.min.js" integrity="sha384-m08KidiNqLdpJqLq95G/LEi8Qvjl/xUYll3QILypMoQ65QorJ9Lvtp2RXYGBFj1y" crossorigin="anonymous"></script>
```

---

**Step 1g: Remove the tweaks-panel.jsx script tag**

- [ ] Find and delete:
```html
<script type="text/babel" src="tweaks-panel.jsx"></script>
```

---

**Step 1h: Remove the tweaks mount div**

- [ ] Find and delete:
```html
<div id="tweaks-root"></div>
```

---

**Step 1i: Remove the inline Babel script block**

- [ ] Find and delete the entire block starting with:
```html
<script type="text/babel">
  const TWEAK_DEFAULTS = /*EDITMODE-BEGIN*/{
```
…through its closing `</script>` tag. This is the last `<script>` in the file.

---

**Step 1j: Save the result as index.html**

- [ ] Write the edited content to:
```
C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html
```
This replaces the existing placeholder file.

---

**Step 1k: Verify the file has no Tweaks remnants**

- [ ] Search the saved `index.html` for each of these strings — none should appear:
  - `tweaks-root`
  - `text/babel`
  - `unpkg.com`
  - `EDITMODE`
  - `data-lines`
  - `data-pose`

---

### Task 2: Visual verification

**Files:** (read-only)
- Open: `index.html` in a browser

- [ ] **Open index.html in a browser** (double-click the file or use a local server). Verify:
  - Page loads with warm off-white background (`#f9f8f6`), not black
  - Cobalt SVG linework fills the hero (right-side arcs, diagonals, dots visible) — dense/prominent
  - Hero headline "Never miss a job / because you'll / *never* miss a call." renders with the italic cobalt "never"
  - Sticky nav shows logo ring + "RETINAL" wordmark + three nav links + "Get early access" button
  - Scrolling past the hero reveals ROI stats (dark cell with £2,400, two light cells), how-it-works steps, pricing card, testimonials, FAQ accordion, dark footer CTA
  - FAQ accordion opens/closes on click with the `+` rotating to `×`
  - Sections fade in on scroll (reveal animation)
  - No Tweaks panel appears in the bottom-right corner
  - No console errors (open DevTools → Console)

---

### Task 3: Commit and push

**Files:**
- Commit: `index.html`

- [ ] **Stage and commit:**
```bash
git add index.html
git commit -m "Launch Retinal marketing site

Replace placeholder with full production site: nav, hero with Dense SVG
linework, ROI stats, how-it-works, pricing, testimonials, FAQ, footer CTA.
Plain HTML/CSS/JS, no framework. Tweaks panel stripped."
```

- [ ] **Push to main:**
```bash
git push origin main
```

- [ ] **Verify GitHub Pages deployment** — visit `https://www.retinal.info` (allow ~1 min for Pages to rebuild). The full site should be live.
