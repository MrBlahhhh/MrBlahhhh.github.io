# F56 3D post: running order

Not a post. The running order for `2026-10-26-f56-front-brake-3d.md`, whose
page is `_includes/f56-3d-feature.html`. Add to this structure instead of
appending to the end.

1. **Hero**: what it is (the F56 as a hologram you can pull apart, one
   section at a time: front brake, then the battery), stats strip (totals
   across sections: 30 parts, 71 repair photos, 12 procedures, 4 wiring
   diagrams), Open the explorer / How to use it / Jump to a part, then the
   exploded poster linking to the explorer.
2. **How to use it** (`#use`): five steps, then six screenshots (caliper,
   wear sensor, wiring, procedures, battery, phone).
3. **Jump to a part** (`#parts`): ten tiles, each a deep link into the
   explorer (`#<section>`, `#<section>/part:<id>`, `#<section>/wiring`,
   `#<section>/procedures`).
4. **The 3D** (`#3d`): modelled parts, one hologram look, beside the
   exploded brake.
5. **Where it's at** (`#status`): what's open.
6. Contact line.

## Standing rules

- The explorer at `f56-3d/brake/` is a build copied in from the f56-3d repo.
  Don't edit it here; rebuild and copy. Its link back to this post is set in
  that build, so a new post date or slug means rebuilding it.
- Numbers on the page (torques, wear limits, part and photo counts) match the
  explorer's data file. Keep them in step when it changes.

## Screenshots

All in `assets/images/f56-3d/`, taken from the explorer build in headless
Chrome with `?instant`, glow on, then saved as JPEG:

- `cover.jpg`, `exploded.jpg`: `#exploded` at 1400 x 900.
- `caliper.jpg`: `#part:caliper` at 1400 x 900.
- `wear-sensor.jpg`: `#part:wear-sensor` at 1400 x 900.
- `wiring.jpg`: `#wiring:wear-sensor` at 1400 x 900.
- `procedures.jpg`: `#procedures` at 1400 x 900.
- `battery.jpg`: `#battery` at 1400 x 900.
- `phone.jpg`: `#part:caliper` at 390 x 844, 2x, phone emulation.
- Retaken 2026-10-26 after the sections, phone layout and wire colours went in.
