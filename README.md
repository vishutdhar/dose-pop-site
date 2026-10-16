# Dose Pop site

The landing page, support page, privacy policy and terms for Dose Pop, the medicine
reminder for iPhone and iPad.

Canonical site: https://dosepop.vishutdhar.com/ (Vercel). The same files are also served
by GitHub Pages at https://vishutdhar.github.io/dose-pop-site/; every page's canonical link
points at the Vercel host.

## Files

- `index.html` and `styles.css`: the landing page. Plain HTML and CSS in the app's own
  colours, light by default and dark when the visitor's system is dark, as the app is.
  The only script is inline: one line in the head that marks the page for motion unless
  the visitor asked for less, and one block at the end that lets a visitor press I Took It
  on the hero, turns the hero phone as the page scrolls, tilts the scene toward a mouse
  pointer, and plays each section's entrance once. With reduced motion, or without
  script, everything is shown in its final state.
- `privacy.html`, `terms.html`, `support.html`: load `styles.css` for the shared header,
  footer and tokens, and `legal.css` for the reading layout.
- `fonts/`: Atkinson Hyperlegible Next, one variable woff2 (latin) with its licence,
  `OFL.txt`. Nothing loads from a third party.
- `assets/`: real app screenshots, the app icon sizes, the App Store badge and the
  social card. The hero is built from three cuts of two real Today captures:
  `today-plate-20261016` (the screen with the dose card lifted out; the card sat on plain
  app background, so the plate is that background filled in) and
  `dose-card-pending-20261016` and `dose-card-taken-20261016` (the card itself, before and
  after one tap). `docs/og.html` renders the social card (see below).
- `vercel.json`: clean URLs (`/privacy` serves `privacy.html`), no trailing slash, and a
  one year immutable cache on `assets/` and `fonts/`.
- `docs/superpowers/specs/2026-10-16-landing-rebuild-design.md`: the design plan.
- `docs/superpowers/specs/2026-10-16-app-colours-depth-motion.md`: the second pass: every
  colour and where in the app it came from, the hero scene, and the motion score.

## Rules for changes

- Files under `assets/` and `fonts/` are cached for a year as immutable. Never change one
  in place: save the new version under a new name (the date suffix, for example
  `today-pending-20261016.webp`) and update the references.
- Every claim on the page must be true of the shipped app. Check the app repo before adding
  one, and update the page only after a feature is live in the App Store.
- The hero scene is positioned from the capture's pixels: the card's place on the screen
  (see the comment above `.hero-media` in `styles.css`) and the press target inside the
  card (the comment above `.tap-target`). A new hero capture needs both measured again,
  and the plate and card cut again.
- Colours come from the app. Take a new value from `AppColors.swift` or the asset catalog
  in the app repo, and check its contrast before using it (the spec lists the bar).

## Run it locally

Any static server works. `python3 -m http.server 8000` from this folder serves every page
by its `.html` name; the extensionless links (`/privacy`) need a server with clean URLs,
such as Vercel or GitHub Pages.

## Social card

`docs/og.html` is a 1200 by 630 page that loads `styles.css` and reuses the hero scene, so it
always matches the page. Serve the folder, capture `docs/og.html` at that size in the light
scheme, save it under a new name in `assets/` (the current one is `og-light-20261016.png`),
then point `og:image` and `twitter:image` in `index.html` at the new file.
