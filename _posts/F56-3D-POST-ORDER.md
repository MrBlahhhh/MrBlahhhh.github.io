# F56 3D post: running order

Not a post. The running order for `2026-10-26-f56-front-brake-3d.md`, whose
page is `_includes/f56-3d-feature.html`. Add to this structure instead of
appending to the end.

1. **Hero**: what it is (the front left brake as a hologram, BMW's own data
   from ISTA picked for this car, one corner as the test), stats strip (15
   parts, 56 ISTA photos, 7 procedures, 3 wiring diagrams), Open the
   explorer / How to use it / Jump to a part, then the exploded poster
   linking to the explorer.
2. **How to use it** (`#use`): five steps, then four screenshots (caliper,
   wear sensor, wiring, procedures).
3. **Jump to a part** (`#parts`): eight tiles, each a deep link into the
   explorer (`#part:<id>`, `#exploded`, `#wiring`, `#procedures`).
4. **Where it comes from** (`#data`): four tiles (picked for this car,
   torques, technical data, wiring), then the 3D paragraph beside the phone
   shot.
5. **Where it's at** (`#status`): what's open.
6. BMW credit line and contact.

## Standing rules

- The explorer at `f56-3d/brake/` is a build of the private f56-3d repo,
  copied in by `tools/publish-blog.sh` there. Don't edit it here; rebuild and
  copy. Its link back to this post comes from f56-3d's `.env.blog`, so a new
  post date or slug means changing that too.
- f56-3d is private: no repo links on the page.
- Every number on the page (torques, wear limits, part and photo counts)
  comes from the explorer's `front-brake-left.json`, which comes from ISTA.
  Keep them in step when the extract changes.
- `license: false` in the post: the photos, diagrams and repair text in the
  explorer are BMW's, not CC-BY.

## Screenshots

All in `assets/images/f56-3d/`, taken 2026-10-26 from the explorer build in
headless Chrome (SwiftShader) with `?instant`, glow on, then saved as JPEG:

- `cover.jpg`, `exploded.jpg`: `#exploded` at 1400 x 900.
- `caliper.jpg`: `#part:caliper` at 1400 x 900.
- `wear-sensor.jpg`: `#part:wear-sensor` at 1400 x 900.
- `wiring.jpg`: `#wiring:wear-sensor` at 1400 x 900.
- `procedures.jpg`: `#procedures` at 1400 x 900.
- `phone.jpg`: `#part:caliper` at 390 x 844, 2x, phone emulation.
