# Dose Pop landing page rebuild: design plan

Date: 2026-10-16. Canonical host: https://dosepop.vishutdhar.com/

## Who, and the one job

- Reader: the adult child of a senior, usually on a phone, deciding whether this app is simple
  enough for a parent. Second reader: the senior, often with low vision, sent the link by family.
- Job: tap "Download on the App Store", or send the page to a parent.
- The point must land in the first phone screen at 390px: what it is (a medicine reminder),
  why it suits a parent (one big button, large print), the price, and the two actions.

## Tokens

| Name          | Value                  | Use |
|---------------|------------------------|-----|
| Ground        | #07080B                | page |
| Surface       | #0F1116                | phone frame, price band |
| Surface 2     | #15181F                | frame edge, pressed states |
| Heading ink   | #F4F5F6                | h1, h2, row titles (APCA Lc 101 on ground) |
| Body ink      | #CDD0D4                | body copy (Lc 78) |
| Muted         | #9BA1A8                | copyright line only (Lc 51, WCAG 7.7:1) |
| Dose teal     | #6DDFD3                | the one accent: the App Store button, focus ring, link underline |
| Hairline      | rgba(255,255,255,0.11) | row and section rules |

Dose teal is the icon's own hue. Sampling the 1024px icon puts 81% of its pixels at 170 to 180
degrees, mean #12A999 (hue 173.6). Kept at that hue and lightened in HLS (S 0.64, L 0.65) until
both brief rules pass with a margin: teal text on ground Lc 76, ground ink on a teal button
Lc 77. The fully saturated tint at the same contrast (#19E6D1) was rejected: it reads as neon
mint, which is the "black plus acid accent" default. The calmer tint reads as a pharmacy label
rather than a gadget. The icon's orange check is not used: one accent only.

## Type

Atkinson Hyperlegible Next, one variable woff2 (latin, 34 KB) covering 400 and 700, self
hosted with its OFL licence, preloaded, font-display swap. The Braille Institute drew it for
low vision readers. Its design goal is letterform distinction: characters that blur into each
other in most sans serifs (capital I, lower l and the figure 1; O and 0; b, d, p and q) are
drawn to stay distinct, with open counters. That is this audience exactly, and the face is
also the page's personality: it looks like clear signage, not like a startup.

Scale (brief tokens, with the audience adjustments for this site):
- body: clamp(1.125rem, 1.094rem + 0.126vw, 1.1953rem), 18 to 19.1px, line height 1.6
- lede: clamp(1.2656rem, 1.204rem + 0.253vw, 1.4063rem), 20.25 to 22.5px, line height 1.5
- every fluid value ramps from 390 to 1280 wide and is flat above, like the brief's h1 and h2
- h2: clamp(1.75rem, 1.439rem + 1.326vw, 2.5rem), 28 to 40px, weight 700, line height 1.12
- h1: clamp(2.125rem, 1.555rem + 2.431vw, 3.5rem), 34 to 56px, weight 700, line height 1.06;
  the price in the price row is set at this size
- row titles: 1.375rem (22px) weight 700
- nothing below 16px anywhere, footer included; tap targets 48px

## Wireframe (desktop, 1120px column, 7fr text : 5fr media, 64px gap)

```
 [icon] Dose Pop                                      For families    Support
 
 Medicine reminders with           |   Try it. Tap I Took It.
 one big button.                   |   .------------------------.
                                   |   |  8:41                  |
 When a medicine is due, Dose Pop  |   |  Today                 |
 reminds you ... $4.99, paid once, |   |  Friday, October 16    |
 no subscription.                  |   |  Metformin      9:00 AM |
                                   |   |  [ I Took It ] [ Skip ]|
 [ Download on the App Store ]     |   |                        |
 Send to a parent:  [Text] [Email] |   |                        |
 ================================== the phone is cut by this rule ===========
 Hard to miss. Easy to use.
 ---------------------------------- ----------------------------------------
 Sounds even on silent             | A second alert if a dose is missed
 ---------------------------------- ----------------------------------------
 Answer from the reminder          | Big buttons, large print
 ---------------------------------- ----------------------------------------
 A record of every dose            | A warning before you run out
 ---------------------------------- ----------------------------------------
 Any schedule                      | Doses on the Home Screen

 Let one family member keep watch. |          .------------.
 intro: off until turned on        |          | the real   |
 ----------------------------------|          | consent    |
 They see what was taken           |          | screen     |
 Dosages are never shown           |          |            |
 An email if a dose looks missed   |          |            |
 Stop sharing any time             |          '------------'
 ---------------------------------------------------------------------------
 $4.99    Paid once, no subscription. No account, no ads.  [ Download ... ]
 ---------------------------------------------------------------------------
 Dose Pop is a reminder, not medical advice ...
 ---------------------------------------------------------------------------
 Privacy  Terms  Support  support@freedom-terminal.com     [App Store badge]
 A Freedom Terminal product   (c) 2026 Dose Pop
```

Every block hangs on the same two vertical lines: the left text edge and the 7fr split. The
feature grid uses the hero's 7fr and 5fr columns, so its right column starts exactly where the
hero phone's column starts.

Phone (390px): one column with 20px gutters (the brief's gutter at that width). Header on one
line, h1 in two lines at 34px, lede, the full width button, the send row on one line, then the
phone at 60% of the screen width, cut by the rule. At Display Zoom widths (320px) the nav drops
under the brand and nothing scrolls sideways.

Alignment: everything left aligned on one grid line; no centred blocks except inside buttons.
Screens always sit in the right column, so the eye never zigzags.

## The one memorable element: the giant single tap

The hero object is a real capture of the Today screen from the current build, one medicine
pending, the green I Took It button at its real proportion. The phone rises out of the hero's
bottom rule, cropped where the screen goes empty, so it is never taller than the text beside
it and the button sits near the top third of the object.

The visitor can press the real button area on the picture. The picture swaps to the real
capture taken one tap later (Taken, with Undo), and a neutral touch mark answers the press.
Pressing Undo swaps it back. That is the whole product in one gesture, done by the reader.
It is user triggered, so it is the page's only motion and nothing moves until someone acts.
prefers-reduced-motion keeps the swap and drops the touch mark and the fade. Without script the
pending capture stands alone and the "Try it" line is not shown.

Everything else is quiet: hairline rows, no icons, no cards, no gradients, no shadows, no
section reveals, no hover transitions beyond a colour change on links.

## Review against the generic defaults, and what changed

- Black plus acid accent: the first pull was the store's mint #3ADFCB on near black. That is
  the default look. Changed to the calmer #6DDFD3 at the icon's hue, and the accent is spent on
  one button per screen, never on headline words.
- One accented word in the headline: the current page paints "Never miss" and "watch over you"
  mint. Removed. Headlines are one colour; the type itself carries them.
- Eyebrows and pill badges: the current page has "For iPhone and iPad", "Built for confidence",
  "For family backup" eyebrows and an "Optional and off by default" pill. All removed; the facts
  they carried moved into sentences.
- SaaS card kit: the current page is a nine card icon grid with rounded cards and a trust strip.
  Replaced by rows divided by hairlines, a title and one or two sentences each, no icons.
- Broadsheet hairlines everywhere: hairlines only separate rows inside a list and close a
  section; there are no boxed columns, no zero radius frames, no dense multi column text.
- Template chrome: no middle dot meta strings, no mono labels, no arrows on links, no numbered
  markers (none of the content is a sequence), no all caps.
- Two typefaces (Nunito from Google Fonts on the page, Inter on the legal pages): replaced by
  one self hosted face on every page, and no request leaves the site.
- Generic hero (big phone mockup centred under a headline): replaced by the cropped real screen
  with the press interaction, sized to the text block.
- App Store button: the current page draws its own button with the Apple logo, which Apple's
  marketing guidelines do not allow. The call to action is now a plain teal button that says
  "Download on the App Store"; the official black badge sits in the footer.

## Copy rules applied

Short sentences. The user's words: medicine, dose, pills, family. Every claim was checked
against the app source (DosePop main) and the live App Store listing (1.9, $4.99 paid up front,
iPhone and iPad): Sound on Silent (opt in, needs permission, plays through Silent and Focus,
volume floor), the follow up alert after a grace the user picks (default 30 minutes), the
notification actions I Took It, Snooze and Skip, Dynamic Type with stacked buttons at the
largest sizes, Caregiver Alerts (off until turned on, one person, names and taken or not,
dosages never shown, an email when a dose looks missed, stop any time and the link stops
working), history with taken times, the refill warning a week ahead, and the schedules (daily,
twice, three times, every other day, chosen weekdays, every few days, weeks or months, as
needed). The price and "paid once, no subscription" appear once in the hero lede and once in
the price row.

## Screenshot critique passes (1920, 1440, 390)

1. Four sections in a row put a bright phone on the right. Three supporting phones competed
   with the hero, which broke the one bold element rule. The reminder and record rows became
   one eight item grid with no image, and only the consent screen stayed as supporting proof.
   The refill and History captures are no longer shown (the files stay, their URLs work).
2. A figure's default 40px side margin pushed the hero phone off the grid line. Zeroed.
3. The skip link's bottom edge bled one device pixel at a fractional height. It now moves by
   its own height plus 4px (still height independent, never a fixed offset).
4. At 390 the header wrapped "Dose Pop" and "For families" onto two lines each. Items no
   longer wrap internally; at 320 the nav drops under the brand instead of overflowing.
5. The lede broke "I Took It" across lines, and "30 minutes." orphaned. Both are bound with
   non breaking spaces.
6. The gaps after the grid and before the price row measured 152 and 144px, over the 120px
   band limit, from the last row's bottom padding. The last row now has none; both gaps
   measure 120px at 1440.
7. The hero phone kept growing past 1280 and, at its cap, would have stood 6px taller than the
   text. Re-ramped to stop at 368px by 1280: 430px tall beside a 438px text block, flat to 1920.
8. The press ring in teal was faint on the light screen. It is now a neutral translucent touch
   mark, like the one iOS draws, so it reads without spending the accent.
9. "Send this page to a parent:" pushed Email onto its own line at 390. Shortened to "Send to a
   parent:"; the two buttons keep full accessible names.
10. Legal pages: the centred reading column did not share the home page's left grid line. It
    now hangs on that line at about 70 characters a line.
