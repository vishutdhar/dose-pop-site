# Dose Pop landing page, pass 2: the app's own colours, depth, motion

Date: 2026-10-16. Builds on the pass 1 plan (2026-10-16-landing-rebuild-design.md), which still
governs type, proportions, copy, the price row, the footer and the accessibility floor.

Owner request: "can we match the colors of the app in the landing page and add some 3d stuff to
them? also some animations?"

## 1. The palette, sampled from the app

Every value below comes from the DosePop app repo (main, fbda331c) or from the real Today capture
the hero uses. The app follows the system appearance: every screen colour is an adaptive light and
dark pair (`Color(light:dark:)` in AppColors.swift), the asset catalog AccentColor has a dark
variant, and nothing forces one appearance (no `preferredColorScheme`, no `UIUserInterfaceStyle`).
So the page ships both schemes and follows `prefers-color-scheme`. Light is the default, because
the app opens light on a default iPhone and every store screenshot is light.

| Token | Light | Dark | Source |
|---|---|---|---|
| Ground | #FAFAF9 | #1C1917 | `ppBackground` / `ppBackgroundDark` (AppColors.swift), the `ppAdaptiveBackground` every main screen sets; #FAFAF9 sampled from the Today capture at (40, 200) |
| Surface | #F5F5F4 | #292524 | `ppSurface` / `ppSurfaceDark`, the card fill; #F5F5F4 sampled inside the dose card |
| Heading ink | #1C1917 | #FAFAF9 | `ppTextPrimary`; dark uses the app's own warm white |
| Body ink | #44403C | #E7E5E4 | derived inside the app's stone family (the app uses #1C1917, #292524, #57534E, #78716C, #A8A29E, #F5F5F4, #FAFAF9) to clear the body bar with room for low vision readers |
| Muted | #78716C | #A8A29E | `ppTextSecondary` / `ppTextTertiary`; copyright line and the day strip labels only |
| Accent | #0F766E fill, #0D9488 line | #5EEAD4 fill, #14B8A6 line | AccentColor.colorset (#0D9488 light, #14B8A6 dark); `PrimaryButtonStyle` fills with `ppPrimaryDark` #0F766E and white text, so the page's one primary button does exactly what the app's does; dark uses `ppPrimaryLight` #5EEAD4 for the fill because it is the only app teal that dark ink clears on |
| Secondary | #16A34A | #16A34A | `ppSuccess`, the I Took It button (`SuccessButtonStyle`), sampled at (200, 1270) in the capture; used only for the taken mark on the day strip |
| Hairline | rgba(28, 25, 23, 0.14) | rgba(255, 255, 255, 0.14) | dark is the app's `ppCardBorder`; light is the same stroke in the app's ink |

Not used on the page chrome: the orange pending colour (`ppWarningText` #B45309, `ppWarning`
#F59E0B) and the coral `ppAccent` #F97316. They stay inside the screenshot. Two chromatic colours
only: teal and green.

APCA (0.0.98G-4g) and WCAG 2 ratios, measured with a script over the composited values:

| Pair | Light | Dark | Bar |
|---|---|---|---|
| Heading ink on ground | Lc 101.3 (16.7:1) | Lc 103.2 (16.7:1) | 90 |
| Body ink on ground | Lc 90.8 (9.8:1) | Lc 90.0 (13.9:1) | 75 |
| Accent text (link hover) on ground | #0F766E Lc 73.8 (5.2:1) | #5EEAD4 Lc 79.7 (11.8:1) | 60 |
| Ink on the accent button | white on #0F766E Lc 82.2 (5.5:1) | #1C1917 on #5EEAD4 Lc 79.7 (11.8:1) | 75 |
| Muted on ground | Lc 70.3 (4.6:1) | Lc 51.2 (6.9:1) | fine print |
| Quiet button border | 0.48 ink, 3.1:1 | 0.40 white, 3.8:1 | 3:1 non text |
| Green taken mark on ground | 3.2:1 | 5.3:1 | 3:1 non text |

Rejected: #0D9488 as the button fill (white text Lc 70, under 75) and #14B8A6 in dark (dark ink
Lc 54). Accent coverage stays under 5% of any screen: the primary button, link underlines, the
focus ring and one dose mark.

## 2. Depth: one scene, three planes, one contact point

CSS perspective and preserve-3d only. No WebGL, no generated images. The only new image files are
measured cuts of the existing real captures:

- `today-plate`: the Today capture with the dose card removed. The card sat on flat app
  background (#FAFAF9 at every pixel from row 1460 down, and between the date card's bottom edge
  at row 861 and the card shadow), so the plate is that background filled over rows 862 to 1460.
  Nothing is invented; it is the screen with its one card lifted out.
- `dose-card-pending` and `dose-card-taken`: the card itself, 1224 by 485 at (48, 918) in each
  capture. The two captures differ only inside that box (pixel diff bounding box 1128 by 343 at
  96, 1060), so one plate serves both states.

The planes, back to front:

1. Background: a day strip. Hour lines for the morning, 6 AM to 11 AM, with the 9 AM line level
   with the dose card, and one dose mark on it: a teal ring (the app's `ppPending` is teal) that
   fills green when the visitor takes the dose. Hairlines and muted labels, set back by 35% of
   the phone's width (119px at full size), then scaled by the same factor so the labels still
   render at 16px and the lines land where they are laid out. The phone occludes the lines;
   they reappear on its far side. On a phone the strip runs out to the right of the phone and
   its labels sit at the far end.
2. Subject: the phone, standing on the hero rule. Front face in the app's ink, a real side edge
   (a second face turned 90 degrees, so the edge shows as the phone turns), and the clean plate as
   its screen. The rule is the floor: the phone body runs below it and is cut by it.
3. Foreground: the dose card, lifted off the screen toward the reader by 16% of the phone's
   width (54px at full size). Its shadow lies on the
   screen under it (offset down, blurred), so as the scene turns the card visibly separates from
   its own shadow. At the opening pose the card's near edge passes in front of the phone's bezel.

The shared contact point is the phone's base at the hero rule. Every rotation, scroll or pointer,
pivots there, so the planes move as one scene and nothing floats loose from the floor.

### The one 3D treatment: the phone stands in perspective and straightens as you scroll

- Opening pose: turned 16 degrees toward the copy and leaning back 7 degrees.
- Scroll: over the first stretch of scroll (until the hero's bottom passes 35% of the viewport,
  measured once per resize), the turn and lean ease to zero. The card stays lifted, so the depth
  is still there when the phone faces the reader square. Every pixel of that travel changes the
  pose; nothing pins.
- Pointer on desktop (fine pointer with hover only): the whole scene tilts up to 6 degrees about
  the contact point, eased toward the pointer each frame, so the day strip, the phone and the
  card shift against each other. Touch devices get the scroll pose only.
- The press: the I Took It target lives on the lifted card, so the real button area is pressable
  at every pose. A press pushes the card toward the screen and back, swaps to the real Taken
  capture (Undo swaps back), shows the neutral touch mark, and fills the day strip's dose mark.

## 3. Motion score

| Section | Moment | Trigger | What moves | Duration |
|---|---|---|---|---|
| Hero | Planes settle | page load | day strip comes forward 100px and fades in; phone rises out of the rule; the card lifts off the screen and its shadow appears | 0 to 1150 ms |
| Hero | Copy appears | page load, after the planes | h1, lede, button row, send row and caption fade up 8px, staggered | 500 to 1240 ms |
| Hero | Phone straightens | scroll | turn 16 to 0 degrees, lean 7 to 0 degrees, the day strip drifts 3.5% of the phone's width (12px) slower than the page | follows scroll |
| Hero | Scene tilt | pointer, desktop | rig rotates up to 6 degrees about the contact point | eased, about 300 ms to settle |
| Hero | Press | click or tap on I Took It or Undo | card pushed in and back, picture swap, touch mark, dose mark fills | 380 ms |
| Hard to miss | Rows wipe in along their hairline | each row entering view, once | the hairline draws left to right, then the title and text wipe open in the same direction | 560 ms line, 520 ms text |
| Let one family member keep watch | The sharing screen straightens | screenshot entering view, once | turned 18 degrees in perspective, it squares up to the reader | 900 ms |
| Price | The price arrives | price row entering view, once | $4.99 rises into its line from below a mask, then the terms fade in | 640 ms, terms at 160 ms |
| Buttons | Answer the hand | hover, press | 1px lift on hover, 2% smaller on press | 140 ms |

Links never animate. No pinned sections, no scroll cue, no counters, no overlays or grain.
Transform, opacity and clip-path only; every transition names its properties.

Budget: the script is one inline block, under 6 KB minified. Scroll and pointer listeners are
passive and only schedule a frame; the frame reads `scrollY` and the cached pointer position and
writes three custom properties on the stage. Rects are measured once on load and on resize.

## 4. Reduced motion

`prefers-reduced-motion: reduce` turns every animation and transition off and leaves every element
in its final state. The scene keeps its depth: the phone holds the opening pose (turned and
leaning), the card stays lifted with its shadow, the day strip stays behind. Scroll and pointer do
nothing. The press still swaps the pictures and fills the dose mark, with no push and no touch
mark. Without script the page shows the same static composition, with the pending card and no
press target.

## 5. What changed against pass 1 and why

- Ground from near black to the app's warm white, because the app is a light first app and the
  owner asked for its colours. Dark returns only for visitors whose system is dark, using the
  app's own dark palette.
- The accent from the icon tint #6DDFD3 to the app's real button teal #0F766E. The icon tint is
  not a colour the app shows on screen.
- One hero picture became three planes. Pass 1 allowed one motion moment; pass 2's brief asks for
  a load sequence, the scroll pose and section reveals, each tied to its content.

## 6. Screenshot critique passes

Captures: ~/Code/dose-pop-site-captures/2026-10-16-pass2/ (strips at 1440 and 390, the same
with reduced motion, the hero at 1920, load and reveal frames, legal pages in both schemes,
iOS Safari frames).

1. The hour labels sat 17px from the lede at 1440. The strip now starts 48px from the copy
   column (40px at 861 to 1023), and the lines sit closer together so 6 AM is level with the
   end of the lede rather than its middle.
2. On the phone layout the lines began left of the phone and showed as slivers in the
   gutter. The strip now starts behind the phone.
3. The strip's scroll drift was a fixed 24px, which left the 9 AM line 19px below the card
   once the phone straightened on a phone. The drift now scales with the phone; measured
   card centre to 9 AM line is within 5px at every pose at 1440 and 390.
4. The feature rows' text appeared before its hairline drew: a clip-path cannot transition
   to none, so it jumped. Both ends of the wipe are now inset() values.
5. At 900 wide the 9 AM mark slid under the phone's edge. The phone's width now ramps from
   240px at 819 to 340px at 1284 (pass 1: 368px, ramping faster than its column), and the mark
   sits 46px after its label: it clears the phone by 31px at 861 and 65px at 1280.
6. Headless WebKit painted the strip unshrunk, its labels running off the right edge. It
   ignores perspective entirely in that mode (a 200px perspective changed nothing), while
   its computed geometry matched Chromium exactly. iOS Safari in the simulator renders the
   scene in true 3D with the labels inside the edge, so no change was made for it.

## 7. Verification

- The press works at every pose: clicked at the projected centre of the button, and through
  the accessible button, at scroll poses 0, 0.5 and 1, with the pointer at both corners and
  the centre, in Chromium and WebKit at 1440 and 390; hit tested on the lifted card in iOS
  Safari. Keyboard: Tab reaches the target after Email, Enter takes the dose, Space undoes it.
- No horizontal scroll from 320 to 1920 wide on any page, in either scheme.
- html-validate clean on every page; no console messages in a fresh browser.
- Lighthouse, local server: home 100 in all four categories on mobile and desktop, and on
  mobile with the dark scheme forced; privacy, terms and support 100 on mobile. Mobile
  total blocking time 0 ms, layout shift 0, LCP 1.7 s (the hero plate).
- The inline script is 2.4 KB minified.
