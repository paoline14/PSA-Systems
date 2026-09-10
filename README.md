# PSA Laguna — office systems landing page

A static page with two cards. Clicking either card opens that system.

## Deploy

No build step. Upload the folder, or from the Vercel CLI:

    cd landing
    vercel deploy --prod

Any static host works — it is one HTML file plus an `assets/` folder.

## Set the two addresses first

Both links live in one place, near the bottom of the HTML:

    const SYSTEMS = {
      rooms:   "https://trainingroom-ten.vercel.app",
      vehicle: "https://psa-vehicle-scheduling.vercel.app",
    };

**Check both before this goes out.** The room address came from a screenshot
you sent; the vehicle one came from a note in your vehicle repo
(`UPLOAD-THESE.md`). Neither was verified against a live deployment. A landing
page whose links 404 is worse than no landing page.

The host name printed under each card comes from the same map, so correcting a
URL fixes both the link and the label.

## Two versions

**`index.html`** — uses the clips you supplied as card previews. They play on
hover or keyboard focus, never on load, so nobody pays for two autoplaying
videos on a phone. If a clip fails to load, the poster frame stays and the card
still works.

**`index-no-video.html`** — the same page with drawn panels instead. Nothing to
buffer, no text baked into an image, legible at any size, and about 1.2 MB
lighter. See the note below on why this exists.

Pick one and rename it `index.html`.

## About the clips

Both current clips are clean: no generator watermark, and the on-screen text
reads correctly rather than as the garbled lettering the earlier versions had.
The small PSA globe in the corner of the room clip still has scrambled text
around its ring, which is minor at card size but visible if you look.

Posters are the first frame of the room clip and frame 145 of the vehicle clip
— the vehicle title overlay dissolves partway through, so a late frame gives a
cleaner still. A card is never blank while its clip loads, and if a clip fails
to load the poster simply stays.

`assets/rooms-photo.jpg` is a photograph of the actual room, cropped to the
card size, kept in case you would rather use a real picture than an
illustration. To switch, point the room card's `<img>` at it and delete the
`<video>` line beside it.

## Files

    index.html              clips version
    index-no-video.html     drawn-panel version
    assets/
      rooms.mp4, rooms-poster.jpg
      vehicle.mp4, vehicle-poster.jpg
      rooms-photo.jpg          real photo of the room, unused by default
      psa-logo.png, bagong-pilipinas.png

## Notes

- Trajan Pro is self-hosted in `assets/fonts/`, not fetched from a font
  service. That keeps the page working on an office machine with no internet:
  the two woff2 files ship with the site and load from the same origin, so
  there is no third party to reach and nothing to fail. 55KB and 34KB, from
  the supplied .ttf and .otf.
- Trajan has no lowercase — lowercase letters render as small capitals, which
  is exactly how the official letterhead sets "Republic of the Philippines".
  The text is typed normally and the typeface does the rest.
- `font-display: swap` shows the fallback serif immediately rather than holding
  the masthead blank while the font arrives.
- The whole card is the link. A small button inside a large clickable panel
  gives two targets for one action.
- Keyboard: cards are focusable, focus ring is visible, and focus starts the
  preview the same way hover does.
- `prefers-reduced-motion` suppresses the lift and the clips.
- Both official marks sit either side of the office name, and the three
  together form one `.letterhead` group. Keeping them grouped is what lets the
  header be `justify-content: space-between` — the letterhead holds the left
  edge, the clock the right — without the Bagong Pilipinas mark drifting into
  the middle of the row.
- The masthead is full width, unlike the cards below it, which stay boxed to
  1040px. The letterhead is the office identifying itself and belongs at the
  edge of the page; a centred container would leave both it and the clock
  adrift in the middle of a wide monitor.
- The clock is pinned to `Asia/Manila`, not the machine's own zone. An office
  laptop left on a foreign timezone would otherwise show a confident, wrong
  "Philippine Standard Time", which is worse than showing nothing. What it
  displays is the *device* clock formatted for Manila — right to the second on
  anything syncing time normally, but not an authoritative source. This page is
  static, so there is no server to ask for the real time.
- The time line reserves its height (`min-height: 1.4em`) and uses tabular
  figures, so nothing shifts when the script first fills it in and the line
  does not twitch as the seconds turn.
- The four systems sit in one row. Fixed columns rather than `auto-fit`: left
  to itself the grid breaks three-and-one at some widths, and a lone card on its
  own row reads as an afterthought rather than one of four.
- The content column runs to the same edges as the masthead, so the row of
  systems lines up with the letterhead instead of sitting in a narrower column
  of its own. Capped at 1720px, because on a very wide monitor four unbounded
  cards become four billboards. The lede keeps its own 58ch limit so the prose
  does not stretch with the container, and the footer matches so its line
  starts under the first card.
- The row steps down 4 → 2 → 1 rather than shrinking to four thin columns.
  Below about 1280px a quarter of the width is narrower than the card photo
  needs, and a squeezed card is worse than a second row.
- HRIS and Inventory carry an "In development" badge and no link, and they do
  not lift on hover — a card that rises under the cursor and then does nothing
  when clicked is a small broken promise. Inventory has its supplied artwork;
  HRIS has a drawn placeholder, because there is no HRIS artwork yet and putting
  another system's banner there would label the wrong picture.
- The animated backdrop is a port of a React/Tailwind component to plain CSS
  and one script. This page has neither React nor Tailwind, and pulling either
  in for a background would undo the point of it being one file that renders on
  an office machine with no internet.
- It paints the middle band only. The masthead and footer stay white, so the
  letterhead and the clock keep their contrast and only the cards sit against
  the blue — which they already were, being white panels.
- The lights move by `transform` alone, so nothing re-lays-out. The loop stops
  when the backdrop scrolls out of view or the tab is hidden, and never starts
  at all under `prefers-reduced-motion`, where the lights are simply dimmer and
  still.
