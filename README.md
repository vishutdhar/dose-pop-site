# Dose Pop site

The landing page, support page, privacy policy and terms for Dose Pop, the medicine
reminder for iPhone and iPad.

Canonical site: https://dosepop.vishutdhar.com/ (Vercel). The same files are also served
by GitHub Pages at https://vishutdhar.github.io/dose-pop-site/; every page's canonical link
points at the Vercel host.

## Files

- `index.html` and `styles.css`: the landing page. Plain HTML and CSS; the only script is
  the small inline one that lets a visitor press I Took It on the hero screenshot.
- `privacy.html`, `terms.html`, `support.html`: load `styles.css` for the shared header,
  footer and tokens, and `legal.css` for the reading layout.
- `fonts/`: Atkinson Hyperlegible Next, one variable woff2 (latin) with its licence,
  `OFL.txt`. Nothing loads from a third party.
- `assets/`: real app screenshots, the app icon sizes, the App Store badge and the
  social card. `docs/og.html` renders the social card (see below).
- `vercel.json`: clean URLs (`/privacy` serves `privacy.html`), no trailing slash, and a
  one year immutable cache on `assets/` and `fonts/`.
- `docs/superpowers/specs/2026-10-16-landing-rebuild-design.md`: the design plan.

## Rules for changes

- Files under `assets/` and `fonts/` are cached for a year as immutable. Never change one
  in place: save the new version under a new name (the date suffix, for example
  `today-pending-20261016.webp`) and update the references.
- Every claim on the page must be true of the shipped app. Check the app repo before adding
  one, and update the page only after a feature is live in the App Store.
- The hero press target is positioned from the screenshot's pixels (see the comment above
  `.tap-target` in `styles.css`). A new hero capture needs those numbers measured again.

## Run it locally

Any static server works. `python3 -m http.server 8000` from this folder serves every page
by its `.html` name; the extensionless links (`/privacy`) need a server with clean URLs,
such as Vercel or GitHub Pages.

## Social card

`docs/og.html` is a 1200 by 630 page built from the same tokens. Capture it at that size and save
it as `assets/og-<date>.png`, then point `og:image` and `twitter:image` in `index.html` at
the new file.
