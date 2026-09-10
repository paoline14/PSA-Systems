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

- No webfont, matching both systems, so it renders on an office machine with
  no internet.
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
- Four systems is too many for a row of equal cards; at that width they stop
  being panels and become thumbnails. The gallery puts one forward at full size
  and keeps the rest legible either side, so choosing stays a decision between
  neighbours rather than a scan across four.
- Every card position is recomputed from one number — which slide is in front.
  The arrows, the dots, the keyboard, a click on a neighbour and a swipe all
  set that number and nothing else, so none of them can leave the row
  half-arranged.
- Cards behind the front one are `aria-hidden` and out of the tab order. A
  half-turned card is a target you cannot see properly, and clicking one by
  accident is how somebody ends up in the wrong system. Clicking a side card
  brings it forward instead of following its link.
- Below 780px the 3D is dropped and the cards stack. A rotated card on a phone
  is a card you cannot read, and swiping past two systems to reach the third is
  worse than scrolling.
- HRIS and Inventory carry an "In development" badge and no link. Inventory has
  its supplied artwork; HRIS has a drawn placeholder, because there is no HRIS
  artwork yet and putting another system's banner there would label the wrong
  picture.
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
