# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static single-page website for **Smile Care Dental Clinic** in Da Nang, Vietnam (smilecaredanang.com). There is no build tool, no framework, and no package manager — the entire site is one file.

## Files

- `index.html` — the entire site: HTML structure, embedded CSS (`<style>`), and embedded JS (`<script>`), ~810 lines
- `*.webp`, `smile.jpg` — images referenced directly from `index.html`

## Development

No build step. Open `index.html` directly in a browser, or serve it locally:

```bash
python3 -m http.server 8000
```

Deployment: push `index.html` and all image files to the static host. Everything is self-contained.

## Architecture

**Single HTML file** with three embedded sections:

1. `<style>` — all CSS, using CSS custom properties (`--color-primary`, `--font-display`, etc.) defined in `:root`
2. HTML body — sections in order: `#hero` → trust bar → `#services` → `#prices` → `#about` → gallery → `#reviews` → `#contact` → footer + floating contact buttons
3. `<script>` — vanilla JS at the bottom of `<body>`:
   - Scroll-sensitive nav: toggles `.scrolled` on `#mainNav` when `scrollY > 60`
   - Mobile menu toggle via `#navToggle` / `#navMenu`
   - Language switcher (EN/VI)
   - Reveal-on-scroll via `IntersectionObserver` on `[data-reveal]` elements
   - Booking form intercept (`#bookingForm`)

## Bilingual content (EN/VI)

Every user-visible string carries two data attributes:

```html
<span data-en="Book an Appointment" data-vi="Đặt Lịch Hẹn">Book an Appointment</span>
```

The JS language switcher iterates all `[data-en],[data-vi]` elements and sets `el.innerHTML = el.dataset[lang]`. The default content in the element itself is the English version. When adding or editing copy, **both** `data-en` and `data-vi` attributes must be updated, and the element's visible content should match `data-en`.

Special handling in the switcher:
- `INPUT`/`TEXTAREA`: sets `.placeholder`
- `OPTION`: sets `.textContent`
- Everything else: sets `.innerHTML` (safe for elements like `<h1>` that contain `<em>` tags)

## Design tokens

All colours, fonts, radii, shadows, and transitions are CSS variables in `:root`. Prefer using the existing tokens rather than hard-coding values:

| Variable | Value |
|---|---|
| `--color-primary` | `#1a3a8f` (navy blue) |
| `--color-accent` | `#b5202a` (red) |
| `--color-accent2` | `#00aad4` (cyan) |
| `--color-success` | `#0e8a5c` (green) |
| `--font-display` | DM Serif Display |
| `--font-body` | Plus Jakarta Sans |
| `--font-vi` | Be Vietnam Pro (applied via `:lang(vi),[data-vi]`) |
