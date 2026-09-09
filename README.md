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

Both have problems worth a look before this is public:

- **The training room clip carries a "Pika" watermark**, visible at the top
  left over the PSA globe. That is the generator's mark on an official
  provincial office page. Re-exporting without it is a paid feature of that
  tool — worth doing, or worth using different media, rather than editing the
  watermark out.
- **Text inside both clips is garbled.** The PSA tagline reads as nonsense
  rather than "Solid · Responsive · World-class", and on the vehicle clip
  "Efficient. Safe. Reliable." is mangled. It is legible enough to notice and
  wrong enough to look careless, which is the worst combination on a
  government site.

Neither problem is fixable from my side without either removing another tool's
watermark or regenerating the media. Hence the second version.

A third option: use real photographs. You already have a good one of the
training room — it is in the reservation app at
`public/psa-training-room.jpg`. An equivalent photo of the office vehicles
would let both cards carry real imagery, which reads better on a government
page than any illustration.

## Files

    index.html              clips version
    index-no-video.html     drawn-panel version
    assets/
      rooms.mp4, rooms-poster.jpg
      vehicle.mp4, vehicle-poster.jpg
      psa-logo.png, bagong-pilipinas.png

Posters were pulled from frame 20 of each clip, so a card is never blank while
its clip loads.

## Notes

- No webfont, matching both systems, so it renders on an office machine with
  no internet.
- The whole card is the link. A small button inside a large clickable panel
  gives two targets for one action.
- Keyboard: cards are focusable, focus ring is visible, and focus starts the
  preview the same way hover does.
- `prefers-reduced-motion` suppresses the lift and the clips.
