# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project shape

This repo is a **single static page**: one [index.html](index.html) file containing the full markup, an inline `<style>` block, and an inline `<script>` block. There is no build step, no bundler, no package manager, no tests, and no server-side code. External dependencies are limited to Google Fonts (Cormorant Garamond + Inter) and Unsplash image URLs.

When making changes, keep the single-file structure unless explicitly asked to split it. Do not introduce a build pipeline, framework, or `package.json` on your own initiative.

## Running locally

The form posts to a remote endpoint, so you need an HTTP origin (not `file://`) for the booking form to work end-to-end. **Always serve on port 8080** for this project — the user wants a single predictable URL across sessions:

```bash
python -m http.server 8080
# then open http://localhost:8080/
```

If 8080 is already in use, ask before falling back to a different port. For visual-only changes, opening `index.html` directly in a browser is fine.

## Booking form — how it works

- Submits via `fetch` (JSON, `POST`) to `https://formsubmit.co/ajax/angch@tertiaryinfotech.com` — a third-party form-to-email service.
- The payload object's keys are the **human-readable field names** ("Full Name", "Preferred Date", …) so they arrive in the email as labeled rows. Don't rename them to camelCase.
- Metadata fields `_subject`, `_template: "table"`, and `_captcha: "false"` are part of FormSubmit's API, not data fields — leave them in the payload.
- **First-time activation**: the very first POST to a new recipient address triggers a one-time confirmation email from FormSubmit. Until that link is clicked, no submissions deliver. This is expected and not a bug.
- On `data.success === "true"`, the form is hidden and `#formSuccess` is shown; on any failure the inline `#formError` banner appears and the submit button is re-enabled.

## Design system (in `:root` CSS variables)

- Palette: `--blush`, `--blush-soft`, `--blush-deep`, `--gold`, `--gold-deep`, `--ink`, `--ink-soft`, `--cream`. Reuse these — don't hardcode hex values for new elements.
- Type: `--serif` (Cormorant Garamond, italic-leaning, used for all `h1`–`h4` and prices) + `--sans` (Inter, body and UI).
- Reusable atoms: `.eyebrow` (uppercase tracked label), `.gold-rule` (centered hairline with gold dots), `.section-head` (centered heading block), `.btn` + `.btn-primary` / `.btn-outline`.

## Patterns to preserve

- **Scroll reveal**: any element with class `.reveal` is observed by an `IntersectionObserver` and gets `.in` added when it scrolls into view. New visible components should opt in by adding `.reveal` rather than implementing their own animation.
- **Package preselect**: package CTA buttons carry `data-package="Classic|Signature|Luxe"`. The JS sets `#package` `<select>` to that value and scrolls to the form. **If you add or rename a package, the `data-package` string and the matching `<option value="…">` must stay in sync.**
- **Featured package variant** (`.package.featured`) inverts the palette to dark + gold. Several rules in the packages section override colors specifically when `.featured` is present — when adding a new property to `.package`, check whether `.package.featured` needs a matching override.
- **Sticky nav** toggles the `.scrolled` class past 40px of scroll, swapping the bar from transparent (over hero) to a frosted cream backdrop. Nav link colors are theme-swapped via `nav.scrolled .nav-links a`.
- **Mobile nav** below 720px becomes a slide-in drawer (`.nav-links.open`); the hamburger toggle is hidden above that breakpoint.
- **Parallax backgrounds** (`background-attachment: fixed`) are used on `.hero` and `.booking`, but explicitly switched to `scroll` below 720px to avoid iOS rendering jank — keep that override if you adjust those sections.

## Breakpoints

Two media queries: `max-width: 960px` (collapses two-column layouts and the 3-up packages grid) and `max-width: 960px` followed by `max-width: 720px` (single-column form, drawer nav, smaller section padding). Match these rather than introducing new breakpoints.
