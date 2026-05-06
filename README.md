# Aurelia Bridal

A single-page website for **Aurelia Bridal Photography** — wedding and bridal photography services with online booking.

The whole site lives in [index.html](index.html): markup, an inline `<style>` block, and an inline `<script>` block. No build step, no bundler, no package manager.

## Sections

- **Hero** — full-bleed parallax intro
- **About** — brand story
- **Services** — bridal portraits, engagement, full-day wedding coverage
- **Packages** — Classic / Signature / Luxe, with one featured tier
- **Gallery** — curated image grid
- **Testimonials** — client quotes
- **Booking form** — name, date, package, message → submitted via [FormSubmit](https://formsubmit.co/)

## Running locally

The booking form posts to a remote endpoint, so an HTTP origin is required (not `file://`):

```bash
python -m http.server 8080
```

Then open <http://localhost:8080/>.

For visual-only changes, opening `index.html` directly in a browser is fine.

## Booking form

- Submits via `fetch` (JSON `POST`) to `https://formsubmit.co/ajax/<recipient>` — a third-party form-to-email service.
- Payload keys are human-readable field names (e.g. `"Full Name"`, `"Preferred Date"`) so they arrive as labeled rows in the email.
- `_subject`, `_template: "table"`, and `_captcha: "false"` are FormSubmit metadata, not data fields.
- **First-time activation**: the very first POST to a new recipient triggers a one-time confirmation email from FormSubmit. Until that link is clicked, no submissions deliver.

## Design system

Defined as CSS variables in `:root`:

- **Palette** — `--blush`, `--blush-soft`, `--blush-deep`, `--gold`, `--gold-deep`, `--ink`, `--ink-soft`, `--cream`
- **Typography** — `--serif` (Cormorant Garamond) for headings and prices, `--sans` (Inter) for body and UI
- **Atoms** — `.eyebrow`, `.gold-rule`, `.section-head`, `.btn` + `.btn-primary` / `.btn-outline`

## Patterns

- `.reveal` — elements fade in via `IntersectionObserver` when scrolled into view
- Package CTAs carry `data-package="Classic|Signature|Luxe"` — clicking preselects the form's package dropdown and scrolls down
- `.package.featured` inverts the palette to dark + gold for the highlighted tier
- Sticky nav swaps from transparent to frosted cream past 40px of scroll
- Mobile nav (≤720px) is a slide-in drawer
- Parallax (`background-attachment: fixed`) on hero and booking sections, switched to `scroll` on mobile to avoid iOS jank

## Breakpoints

`960px` (collapses two-column layouts and the 3-up packages grid) and `720px` (single-column form, drawer nav, smaller padding).
