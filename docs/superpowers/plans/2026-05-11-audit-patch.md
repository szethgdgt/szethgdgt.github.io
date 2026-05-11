# Audit Patch Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Patch `index.html` to fix all high/medium/low issues from `audit-web-design-guidelines.md` except placeholder `href="#"` links (out of scope by user decision).

**Architecture:** Single-file site — all CSS lives in one `<style>` block in `<head>`, all JS in one `<script>` block before `</body>`. Changes are additive (new rules/attributes appended or inserted); no existing rules are deleted. Seven independent tasks, each committed separately.

**Tech Stack:** Vanilla HTML/CSS/JS. No build step. Open `index.html` directly in a browser to verify.

**Spec:** `docs/superpowers/specs/2026-05-11-audit-patch-design.md`

---

### Task 1: `<head>` — color-scheme meta tag + `:root` CSS property

**Files:**
- Modify: `index.html` (head meta block ~line 4–9; `:root` block ~line 11–20)

- [ ] **Step 1: Add `<meta name="color-scheme">` after the viewport meta**

Find this line in `<head>`:
```html
<meta name="viewport" content="width=device-width,initial-scale=1" />
```
Insert immediately after it:
```html
<meta name="color-scheme" content="light" />
```

- [ ] **Step 2: Add `color-scheme: light` to `:root`**

Find the `:root` block:
```css
:root {
  --bg: #f9f8f6;
```
Change to:
```css
:root {
  color-scheme: light;
  --bg: #f9f8f6;
```

- [ ] **Step 3: Verify**

Open `index.html` in a browser with system dark mode enabled (macOS: System Settings → Appearance → Dark; Windows: Settings → Personalisation → Dark).  
Expected: scrollbar, `<details>` markers in the FAQ, and all text remain on the light palette — nothing inverts.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "fix: declare color-scheme light to prevent dark-mode native-element inversion"
```

---

### Task 2: CSS — global reset additions (tap highlight + global `:focus-visible`)

**Files:**
- Modify: `index.html` (`<style>` block, after the existing `a { color: inherit; text-decoration: none; }` rule ~line 60)

- [ ] **Step 1: Add touch tap-highlight reset and global focus-visible rule**

Find this exact rule:
```css
a { color: inherit; text-decoration: none; }
```
Insert immediately after it:
```css
button, a { -webkit-tap-highlight-color: transparent; }

:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
}
```

- [ ] **Step 2: Verify tap highlight**

Open `index.html` on an iOS device or in Chrome DevTools with a touch-enabled mobile profile.  
Tap the nav CTA button and hamburger.  
Expected: no blue/grey flash on tap.

- [ ] **Step 3: Verify focus ring baseline**

Open `index.html` in a desktop browser. Press Tab repeatedly from the top of the page.  
Expected: a blue (`#3d6fd4`) 2px outline appears on every focused element.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "fix: add tap-highlight reset and global focus-visible ring"
```

---

### Task 3: CSS — scoped `:focus-visible` overrides per component

**Files:**
- Modify: `index.html` (`<style>` block — append after the rules added in Task 2)

- [ ] **Step 1: Add scoped overrides**

Directly after the `:focus-visible` block added in Task 2, insert:
```css
.nav-links a:focus-visible {
  outline-offset: 4px;
  border-radius: 2px;
}
.nav-cta:focus-visible {
  outline-offset: 5px;
  border-radius: 4px;
}
.btn:focus-visible {
  outline-offset: 2px;
}
.faq summary:focus-visible {
  outline-offset: 4px;
  border-radius: 2px;
}
```

- [ ] **Step 2: Verify each component**

Tab to each component in the browser:
- Nav links (`How it works`, `Pricing`, `About`, `FAQ`) — ring sits 4px outside with rounded corners.
- Nav CTA (`Get early access`) — ring sits 5px outside with rounded corners.
- Pricing `Get started` button and hero `Get early access` button — ring hugs the border at 2px.
- FAQ `<summary>` rows — ring sits 4px outside with rounded corners.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "fix: add scoped focus-visible overrides for nav, buttons, and FAQ summary"
```

---

### Task 4: CSS — safe-area insets for `.nav` and `.page-frame`

**Files:**
- Modify: `index.html` (`<style>` block — `.nav` rule ~line 63, `.page-frame` rule ~line 38)

- [ ] **Step 1: Add safe-area padding to `.nav`**

Find:
```css
  .nav {
    position: sticky; top: 0; z-index: 55;
    background: var(--bg);
    display: flex; align-items: stretch;
    padding: 0;
    border-bottom: 1px solid var(--ink);
  }
```
Change `padding: 0;` to:
```css
    padding: env(safe-area-inset-top) 0 0;
```

- [ ] **Step 2: Add safe-area top to `.page-frame`**

Find:
```css
  .page-frame {
    position: fixed; inset: 0;
    pointer-events: none;
    z-index: 60;
  }
```
Add `top: env(safe-area-inset-top);` so it becomes:
```css
  .page-frame {
    position: fixed; inset: 0;
    top: env(safe-area-inset-top);
    pointer-events: none;
    z-index: 60;
  }
```

- [ ] **Step 3: Verify**

In Chrome DevTools, open Device Toolbar, select **iPhone 14 Pro** (has Dynamic Island). Scroll to top.  
Expected: nav bar does not clip under the Dynamic Island notch area; the page-frame vertical lines start below the safe area.  
On a standard desktop viewport: no visual change (env() falls back to 0).

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "fix: add safe-area-inset-top to nav and page-frame for notched devices"
```

---

### Task 5: CSS — `prefers-reduced-motion` block

**Files:**
- Modify: `index.html` (`<style>` block — append before the closing `</style>` tag)

- [ ] **Step 1: Add the reduced-motion override block**

Find the closing `</style>` tag and insert immediately before it:
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

- [ ] **Step 2: Verify in Chrome DevTools**

Open DevTools → Rendering panel (three-dot menu → More tools → Rendering) → set **Emulate CSS media feature prefers-reduced-motion** to `reduce`.

Check:
- All `.reveal` elements are immediately visible (opacity 1, no translateY) — no content hidden waiting for scroll.
- Mobile menu (resize to ≤768px, click hamburger) appears/disappears instantly, no slide animation.
- Clicking a side-nav link does not smooth-scroll (verified in Task 6 JS fix).

Remove the emulation when done.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "fix: guard reveal, mobile-menu, and scroll-behavior with prefers-reduced-motion"
```

---

### Task 6: JS — `prefers-reduced-motion` guard on `scrollIntoView`

**Files:**
- Modify: `index.html` (`<script>` block — side-nav click handler ~line 1074)

- [ ] **Step 1: Replace the bare smooth scroll call**

Find this exact JS in the `<script>` block:
```js
  sideLinks.forEach(a => {
    a.addEventListener('click', () => {
      const t = document.getElementById(a.dataset.target);
      if (t) t.scrollIntoView({ behavior: 'smooth', block: 'start' });
    });
  });
```
Replace with:
```js
  sideLinks.forEach(a => {
    a.addEventListener('click', () => {
      const t = document.getElementById(a.dataset.target);
      if (!t) return;
      const reduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
      t.scrollIntoView({ behavior: reduced ? 'auto' : 'smooth', block: 'start' });
    });
  });
```

- [ ] **Step 2: Verify**

In Chrome DevTools Rendering panel, set `prefers-reduced-motion` to `reduce`.  
Click any side-nav link (desktop, ≥1024px viewport).  
Expected: page jumps instantly to the section — no smooth scroll animation.

Remove the emulation. Click a side-nav link again.  
Expected: smooth scroll works normally.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "fix: guard side-nav scrollIntoView with prefers-reduced-motion check"
```

---

### Task 7: HTML — skip link element, `aria-hidden` on quote marks, logo img dimensions

**Files:**
- Modify: `index.html` (body ~line 627; quote-mark divs ~lines 925, 930, 935; logo img ~line 81)

- [ ] **Step 1: Add CSS for the skip link**

In the `<style>` block, find the `.btn` rule block. Insert before it:
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

- [ ] **Step 2: Add skip link HTML element**

Find the first line of `<body>`:
```html
<div class="page-frame" aria-hidden="true"></div>
```
Insert immediately before it:
```html
<a href="#top" class="skip-link">Skip to main content</a>
```

- [ ] **Step 3: Add `aria-hidden` to the three quote-mark divs**

Find each of the three instances of:
```html
        <div class="quote-mark">"</div>
```
Replace each with:
```html
        <div class="quote-mark" aria-hidden="true">"</div>
```
There are exactly three — one per `.quote` block in the `#about` section.

- [ ] **Step 4: Add `width` and `height` to the logo `<img>`**

Find:
```html
    <img src="retinal-logo.png" alt="Retinal" />
```
Replace with:
```html
    <img src="retinal-logo.png" alt="Retinal" width="44" height="44" />
```

- [ ] **Step 5: Verify skip link**

Open `index.html` in a browser. Press Tab once without clicking anything first.  
Expected: a blue bar reading "Skip to main content" appears at the top of the viewport.  
Press Enter — page should jump to the hero section.  
Tab away — the bar disappears (returns off-screen).

- [ ] **Step 6: Verify aria-hidden**

Open DevTools → Accessibility panel → inspect one of the `.quote` elements.  
Expected: the `.quote-mark` div shows `aria-hidden: true`; it will not appear in the accessibility tree.

- [ ] **Step 7: Verify logo CLS**

Open DevTools → Network tab → throttle to Slow 3G.  
Reload the page.  
Expected: the nav height does not shift when `retinal-logo.png` finishes loading (space is pre-reserved).

- [ ] **Step 8: Commit**

```bash
git add index.html
git commit -m "fix: add skip link, aria-hidden on decorative quote marks, logo img dimensions"
```

---

### Task 8: JS — focus trap + focus management for mobile menu

**Files:**
- Modify: `index.html` (`<script>` block — mobile menu section ~lines 1044–1067)

- [ ] **Step 1: Replace the mobile menu JS block**

Find the entire mobile menu block:
```js
  // Mobile menu
  const hamburger = document.getElementById('hamburger');
  const mMenu = document.getElementById('mMenu');
  const mScrim = document.getElementById('mScrim');
  const closeMenu = () => {
    mMenu.classList.remove('open');
    mScrim.classList.remove('open');
    hamburger.setAttribute('aria-expanded', 'false');
    mMenu.setAttribute('aria-hidden', 'true');
  };
  const openMenu = () => {
    mMenu.classList.add('open');
    mScrim.classList.add('open');
    hamburger.setAttribute('aria-expanded', 'true');
    mMenu.setAttribute('aria-hidden', 'false');
  };
  hamburger.addEventListener('click', () => {
    if (mMenu.classList.contains('open')) closeMenu();
    else openMenu();
  });
  mScrim.addEventListener('click', closeMenu);
  mMenu.querySelectorAll('a, .mm-cta').forEach(el => {
    el.addEventListener('click', closeMenu);
  });
```
Replace it entirely with:
```js
  // Mobile menu
  const hamburger = document.getElementById('hamburger');
  const mMenu = document.getElementById('mMenu');
  const mScrim = document.getElementById('mScrim');

  const menuFocusable = () => [...mMenu.querySelectorAll('a, button')];

  const closeMenu = () => {
    mMenu.classList.remove('open');
    mScrim.classList.remove('open');
    hamburger.setAttribute('aria-expanded', 'false');
    mMenu.setAttribute('aria-hidden', 'true');
    hamburger.focus();
  };
  const openMenu = () => {
    mMenu.classList.add('open');
    mScrim.classList.add('open');
    hamburger.setAttribute('aria-expanded', 'true');
    mMenu.setAttribute('aria-hidden', 'false');
    const focusable = menuFocusable();
    if (focusable.length) focusable[0].focus();
  };

  hamburger.addEventListener('click', () => {
    if (mMenu.classList.contains('open')) closeMenu();
    else openMenu();
  });
  mScrim.addEventListener('click', closeMenu);
  mMenu.querySelectorAll('a, .mm-cta').forEach(el => {
    el.addEventListener('click', closeMenu);
  });

  // Focus trap
  mMenu.addEventListener('keydown', (e) => {
    if (!mMenu.classList.contains('open')) return;
    const els = menuFocusable();
    const first = els[0];
    const last = els[els.length - 1];
    if (e.key === 'Tab') {
      if (e.shiftKey && document.activeElement === first) {
        e.preventDefault();
        last.focus();
      } else if (!e.shiftKey && document.activeElement === last) {
        e.preventDefault();
        first.focus();
      }
    }
    if (e.key === 'Escape') closeMenu();
  });
```

- [ ] **Step 2: Verify focus moves into menu on open**

Resize browser to ≤768px. Tab to the hamburger button and press Enter (or Space).  
Expected: menu slides open AND focus moves to the first link inside the menu (`How it works`).

- [ ] **Step 3: Verify focus trap**

With menu open, press Tab repeatedly.  
Expected: focus cycles through `How it works` → `Pricing` → `About` → `FAQ` → `Get early access` (mm-cta button) → back to `How it works`. Focus never escapes to page content behind the overlay.

Press Shift-Tab from the first item.  
Expected: focus jumps to the last item (`Get early access` button).

- [ ] **Step 4: Verify Escape closes and restores focus**

With menu open and focus inside, press Escape.  
Expected: menu closes and focus returns to the hamburger button.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "fix: add focus trap, Escape-to-close, and focus restore to mobile menu"
```

---

## Self-Review Checklist

- [x] **Spec coverage:** color-scheme (Task 1) ✓, tap-highlight + global focus-visible (Task 2) ✓, scoped focus-visible (Task 3) ✓, safe areas (Task 4) ✓, prefers-reduced-motion CSS (Task 5) ✓, prefers-reduced-motion JS (Task 6) ✓, skip link + aria-hidden + img dims (Task 7) ✓, focus trap (Task 8) ✓. Placeholder links explicitly out of scope.
- [x] **Placeholder scan:** All steps contain exact code. No TBDs.
- [x] **Type consistency:** `menuFocusable()` name used consistently in Task 8. `reduced` variable name in Task 6 is local and not reused elsewhere.
