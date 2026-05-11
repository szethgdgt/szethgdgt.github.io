# Design Spec — Web Guidelines Audit Patch

**Date:** 2026-05-11  
**File in scope:** `index.html` (single-file site)  
**Source audit:** `audit-web-design-guidelines.md`  
**Approach:** Single-pass edit to `index.html`; all fixes committed together.  
**Out of scope:** Placeholder `href="#"` links (Privacy, Terms, Contact, "Hear our agent") — left as-is by user decision.

---

## 1. Accessibility

### 1a. Skip link
Add a visually hidden skip link as the very first element inside `<body>`, before `<div class="page-frame">`:

```html
<a href="#top" class="skip-link">Skip to main content</a>
```

CSS — visually hidden until focused:

```css
.skip-link {
  position: absolute;
  top: -100%;
  left: 0;
  padding: 8px 16px;
  background: var(--accent);
  color: #fff;
  font-size: 13px;
  font-weight: 500;
  z-index: 9999;
  transition: top .2s;
}
.skip-link:focus { top: 0; }
```

### 1b. Decorative quote marks
Add `aria-hidden="true"` to each of the three `.quote-mark` divs (lines 925, 930, 935):

```html
<div class="quote-mark" aria-hidden="true">"</div>
```

### 1c. Mobile menu focus trap
When the menu opens, trap Tab and Shift-Tab within focusable elements inside `#mMenu`. On Escape, close the menu. Restore focus to `#hamburger` on close.

Focusable targets: all `<a>` and `<button>` elements inside `#mMenu`.

Logic added to the existing mobile menu JS block:

```js
// collect focusable els inside menu
const menuFocusable = () => [...mMenu.querySelectorAll('a, button')];

// trap focus
mMenu.addEventListener('keydown', (e) => {
  if (!mMenu.classList.contains('open')) return;
  const els = menuFocusable();
  const first = els[0], last = els[els.length - 1];
  if (e.key === 'Tab') {
    if (e.shiftKey && document.activeElement === first) {
      e.preventDefault(); last.focus();
    } else if (!e.shiftKey && document.activeElement === last) {
      e.preventDefault(); first.focus();
    }
  }
  if (e.key === 'Escape') closeMenu();
});

// move focus into menu on open, restore on close
// openMenu(): after classList.add, call menuFocusable()[0]?.focus()
// closeMenu(): after classList.remove, call hamburger.focus()
```

---

## 2. Focus States

Add a global `:focus-visible` reset first so no element is left without a ring, then add scoped overrides where the default ring placement looks wrong:

```css
/* Global baseline */
:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
}

/* Nav links — offset increased so ring clears the tight line-height */
.nav-links a:focus-visible {
  outline-offset: 4px;
  border-radius: 2px;
}

/* CTA button — ring on the outer wrapper */
.nav-cta:focus-visible {
  outline-offset: 5px;
  border-radius: 4px;
}

/* Buttons — ring hugs the border */
.btn:focus-visible {
  outline-offset: 2px;
}

/* FAQ summary — ring clears the +/− icon */
.faq summary:focus-visible {
  outline-offset: 4px;
  border-radius: 2px;
}
```

---

## 3. Animation — `prefers-reduced-motion` guards

### 3a. CSS guards
Wrap `scroll-behavior`, `.reveal` transitions, and `.mobile-menu` transition inside a `no-preference` media query. The inverted `reduce` query collapses them:

```css
@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }

  .reveal {
    opacity: 1;
    transform: none;
    transition: none;
  }

  .mobile-menu {
    transition: none;
  }
}
```

This is additive — the existing rules stay; the reduce block overrides only when needed.

### 3b. JS guard
Replace the bare `scrollIntoView({ behavior: 'smooth' })` call with a motion-aware version:

```js
const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
t.scrollIntoView({ behavior: prefersReduced ? 'auto' : 'smooth', block: 'start' });
```

---

## 4. Images

Add explicit `width` and `height` to the logo `<img>` to reserve layout space and prevent CLS:

```html
<img src="retinal-logo.png" alt="Retinal" width="44" height="44" />
```

Values match the `.logo-mark` CSS dimensions (44 × 44 px).

---

## 5. Dark Mode

Add `color-scheme: light` in two places so native UI elements (scrollbars, `<details>` markers, form controls) stay light-themed regardless of system preference:

**`<head>`:**
```html
<meta name="color-scheme" content="light" />
```

**CSS `:root`:**
```css
:root {
  color-scheme: light;
  /* existing vars … */
}
```

---

## 6. Safe Areas

Extend `.nav` and `.page-frame` to respect device safe-area insets (notch / Dynamic Island):

```css
.nav {
  padding-top: env(safe-area-inset-top);
}

.page-frame {
  top: env(safe-area-inset-top);
}
```

`env()` falls back to `0` on devices without notches — zero visual change on desktop.

---

## 7. Touch

Add `-webkit-tap-highlight-color: transparent` to the global reset so all `<button>` and `<a>` elements suppress the default iOS tap flash, which conflicts with the custom hover/active styling:

```css
button, a {
  -webkit-tap-highlight-color: transparent;
}
```

---

## Change Summary

| # | Category | Type | Location |
|---|---|---|---|
| 1a | Accessibility | HTML + CSS | Before `<div class="page-frame">` |
| 1b | Accessibility | HTML attr | `.quote-mark` divs ×3 |
| 1c | Accessibility | JS | Mobile menu event block |
| 2 | Focus states | CSS | After existing button/link rules |
| 3a | Animation | CSS | New `prefers-reduced-motion` block |
| 3b | Animation | JS | Side-nav click handler |
| 4 | Images | HTML attr | Logo `<img>` |
| 5 | Dark mode | HTML + CSS | `<head>` meta + `:root` |
| 6 | Safe areas | CSS | `.nav`, `.page-frame` |
| 7 | Touch | CSS | Global reset block |

No new files created. No placeholder links changed.
