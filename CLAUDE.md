# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static HTML5/CSS3/JavaScript splash page for HoldingCo LLC, a U.S.-based cryptocurrency asset holding company. The site is designed for KYB (Know Your Business) due diligence purposes and is fully WCAG 2.2 AA compliant with no external dependencies.

## Development

### Running Locally

No build tools required. Use Python's built-in HTTP server:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080` in your browser. The page will display with system fonts and no external network requests.

Alternatively, use `npx serve .` if Node is available.

### Architecture

- **Single file**: `index.html` contains all HTML, CSS (in `<style>` block), and JavaScript (in `<script>` block at the bottom)
- **No dependencies**: No npm packages, no build tools, no external libraries
- **Minimal external requests**: System fonts only. No Google Fonts, no analytics, no tracking scripts
- **CSS custom properties**: All colors, spacing, and typography are defined in `:root` as CSS variables — edit these, not scattered property values

## Accessibility & WCAG 2.2 AA Compliance

This is a hard requirement. All changes must maintain:

- **Color contrast**: 4.5:1 minimum for body text, 3:1 for large/bold text and UI components (verified against `#0D1117` dark background)
- **Focus indicators**: `:focus-visible` outline (cyan, 2px) on all interactive elements — never remove focus styles
- **Keyboard navigation**: All interactive elements must be reachable via Tab and Escape keys
- **Screen reader support**: Semantic HTML5 landmarks, `aria-labelledby`, `aria-label`, `aria-live`, `role` attributes where needed
- **Responsive**: No horizontal scroll at 320px viewport, layout reflows at 640px breakpoint

Before committing changes, verify with:
1. Manual tab through entire page (all interactive elements get cyan outline)
2. Test with browser zoom at 200% and 400%
3. Run WAVE or axe DevTools browser extension to catch regressions
4. Test dismissal of cookie banner with Escape key

## Color System

Theme colors are in the `:root` CSS block:

| Token | Hex | Purpose |
|---|---|---|
| `--color-bg` | `#0D1117` | Page background |
| `--color-text-primary` | `#E6EDF3` | Body text (14.5:1 contrast) |
| `--color-text-secondary` | `#8B949E` | Captions/muted text (4.6:1 contrast) |
| `--color-accent-cyan` | `#22D3EE` | Headings, links, focus rings (9.0:1 contrast) |
| `--color-accent-orange` | `#FB923C` | Accents, badges (5.5:1 contrast) |

All verified WCAG AA against the dark background. **Do not change color values without rechecking contrast ratios.**

## Cookie Banner

The privacy/cookie banner at the bottom of the page:
- Uses `sessionStorage` (not `localStorage`) to store a dismissal flag — intentional, as this avoids any persistent storage
- Starts hidden and slides in after 800ms via JavaScript
- Announces itself to screen readers via `aria-live="polite"` and `aria-atomic="true"` (live region)
- Slides out when dismissed via button or Escape key
- Returns focus to the skip link after dismissal

**Do not change this to use `localStorage`** — that would create a persistent cookie-like behavior, contradicting the privacy policy.

## Page Sections

The HTML uses semantic landmarks:
- `<header role="banner">` — logo + navigation
- `<main id="main-content">` — all content sections
- `<section aria-labelledby="...">` — each major section (hero, about, compliance, privacy-policy)
- `<footer role="contentinfo">` — company info and contact

Sections are identified by CSS `id` attributes, which the in-page nav links reference and the JavaScript copyright year updates.

## Responsive Design

- **Mobile first**: Single-column layout at 320px and up
- **640px breakpoint**: Card grid switches from 1 column to 2 columns; banner layout changes from column to row
- **Fluid typography**: Hero title uses `clamp()` to scale between `2rem` and `2.75rem` based on viewport width
- **No fixed widths**: All content uses `padding-inline` and `max-width` for responsive breathing room

## Copyright Year

The footer copyright year is injected via JavaScript (`new Date().getFullYear()`). The HTML placeholder `<span id="copyright-year"></span>` is replaced on page load. **Do not hardcode the year.**

## No Forms

This is purely informational — there are no form fields, input fields, or user submissions of any kind. The only interactive elements are navigation links and the cookie banner dismiss button.

---

**Created**: April 20, 2026  
**Status**: WCAG 2.2 AA Compliant  
**Theme**: Dark mode (cyan + orange accents, white text, system sans-serif)
