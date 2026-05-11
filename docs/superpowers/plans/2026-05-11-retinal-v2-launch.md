# Retinal v2 Launch Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the current v1 `index.html` with the v2 design (new nav, page-frame borders, mobile menu, sticky side-nav) and add the `retinal-logo.png` asset.

**Architecture:** Two-file update. Copy the logo PNG asset to the repo root, then write the v2 prototype HTML to `index.html`. The v2 prototype is already production-ready — no Tweaks panel to strip.

**Tech Stack:** Plain HTML5 / CSS3 / vanilla JS. Google Fonts (Inter). GitHub Pages via `www.retinal.info`.

---

### Task 1: Copy the logo asset

**Files:**
- Create: `retinal-logo.png` (binary copy)

- [ ] **Step 1: Copy `retinal-logo.png` to the repo root**

```bash
copy "C:\Users\hishs\Downloads\Retinal Site v2\design_handoff_retinal_homepage\retinal-logo.png" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\retinal-logo.png"
```

- [ ] **Step 2: Verify the file exists and is non-zero**

```bash
dir "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\retinal-logo.png"
```

Expected: file listed with a non-zero size (the source is a 380×380 PNG, should be tens of KB).

---

### Task 2: Replace index.html with the v2 prototype

**Files:**
- Modify: `index.html` (complete rewrite)

Source: `C:\Users\hishs\Downloads\Retinal Site v2\design_handoff_retinal_homepage\index.html`
Destination: `C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html`

- [ ] **Step 1: Copy the v2 prototype to index.html**

```bash
copy "C:\Users\hishs\Downloads\Retinal Site v2\design_handoff_retinal_homepage\index.html" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
```

- [ ] **Step 2: Verify the new elements are present**

Search for these strings in `index.html` — all must be found:

```bash
findstr /C:"page-frame" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
findstr /C:"side-nav" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
findstr /C:"mobile-menu" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
findstr /C:"hamburger" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
findstr /C:"retinal-logo.png" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
findstr /C:"frame-left" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
findstr /C:"finalcta" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
```

Expected: each command returns at least one matching line.

- [ ] **Step 3: Verify no Tweaks panel remnants**

```bash
findstr /C:"unpkg.com" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
findstr /C:"text/babel" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
findstr /C:"tweaks-root" "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io\index.html"
```

Expected: no output (zero matches) for all three.

---

### Task 3: Commit and push

**Files:**
- Commit: `index.html`, `retinal-logo.png`

- [ ] **Step 1: Stage both files**

```bash
cd "C:\Users\hishs\Desktop\Projects\Website\szethgdgt.github.io"
git add index.html retinal-logo.png
```

- [ ] **Step 2: Confirm what's staged**

```bash
git status
```

Expected: both `index.html` (modified) and `retinal-logo.png` (new file) listed under "Changes to be committed".

- [ ] **Step 3: Commit**

```bash
git commit -m "Launch Retinal v2

Redesigned nav with logo PNG, numbered links, and circular arrow CTA.
Added page-frame hairline borders, sticky side-nav indicator, mobile
hamburger menu, and updated section numbering. New retinal-logo.png asset."
```

- [ ] **Step 4: Push to main**

```bash
git push origin main
```

Expected output contains: `main -> main`

- [ ] **Step 5: Confirm push succeeded**

```bash
git log --oneline -3
```

Expected: top commit is the v2 launch commit.
