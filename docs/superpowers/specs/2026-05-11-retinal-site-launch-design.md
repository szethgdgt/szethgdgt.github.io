# Retinal Marketing Site — Launch Design
**Date:** 2026-05-11  
**Status:** Approved

---

## Goal

Replace the current placeholder `index.html` ("RETINAL / soon come") with the full Retinal marketing site, using the completed design prototype from the handoff bundle at `C:\Users\hishs\Downloads\Retinal Site\design_handoff_retinal_site\`.

---

## Source

`Retinal.html` — a self-contained, production-quality HTML/CSS/JS prototype. The design is final (high-fidelity per the handoff README). No visual changes are being made.

---

## What Changes

**Strip the Tweaks panel** (design-time tool, not for production):

1. Remove the `<style>` block beginning `/* Linework density modes */` — **except** the Dense variant overrides, which must be preserved (see below).
2. Remove the three UMD `<script>` tags loading React, ReactDOM, and Babel.
3. Remove `<script type="text/babel" src="tweaks-panel.jsx">`.
4. Remove the inline `<script type="text/babel">` block at the bottom.
5. Remove `<div id="tweaks-root"></div>`.

**Bake in Dense linework** (chosen final value):

Rather than using `data-lines="dense"` on `<body>`, inline the Dense overrides directly on the SVG `<g>` elements' `stroke-opacity` attributes, then discard the CSS overrides and the `data-*` mechanism entirely. The Dense values are:
- `.lw-grid` → `stroke-opacity="0.14"`
- `.lw-rays` → `stroke-opacity="0.55"`
- `.lw-arcs` → `stroke-opacity="0.8"`
- Hero art `<div class="hero-art">` → add `style="filter: contrast(1.05)"` (the Dense mode also adds a contrast bump to the whole SVG container)

**Pose:** Surgical (default — no `data-pose` attribute needed, no CSS override needed).

**Accent:** Cobalt `#3d6fd4` (already the CSS variable default).

---

## What Does NOT Change

- All HTML structure, copy, and sections (nav, hero, ROI, how it works, pricing, testimonials, FAQ, footer).
- All CSS design tokens, typography, spacing, layout, responsive rules.
- All JS (scroll reveal `IntersectionObserver`, sticky nav scroll listener).
- Hero and footer inline SVG linework (coordinates, classes, structure).
- The `CNAME` file (`www.retinal.info`) stays as-is.

---

## Output

A single file: `index.html` in the repo root, committed and pushed to `main` for GitHub Pages to serve at `retinal.info`.

---

## CTAs

"Get early access" and "Get started" buttons remain inert `<button>` elements. Lead-capture wiring is out of scope for this launch.

---

## Constraints (from brief)

- Plain HTML / CSS / JS — no framework, no build step.
- Google Fonts (Inter) only — no other external dependencies.
- Hero and footer SVG inline in the HTML.
- No React/Babel UMD tags in the shipped file.
