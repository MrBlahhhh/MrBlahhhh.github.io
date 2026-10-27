# F56 3D post: running order

Not a post. The running order for `2026-10-26-f56-front-brake-3d.md`, whose
page is `_includes/f56-3d-feature.html`. Add to this structure instead of
appending to the end.

1. **Hero**: what it is (the F56 as a hologram you can pull apart, one
   section at a time: front assemblies, rear corners, axle and mechanical parking brake), stats strip (part groups,
   part numbers, sections, wiring diagrams), Open the explorer / How to use
   it / Jump to a part, then the exploded poster linking to the explorer.
2. **How to use it** (`#use`): six steps, then visibility and part screenshots (caliper,
   wear sensor, wiring, battery, phone).
   **More of the front** (`#front`) follows with body, bumper, right corner, wipers, cooling, subframe and powertrain views.
   **Rear brakes and suspension** (`#rear`) shows the separate spring/damper, right wear sensor,
   shared axle and mechanical handbrake. Shapes and cable routes remain approximate.
   **Under the car** (`#underbody`) shows tank, exhaust, panels and rear liners. Routes and
   mounting seats remain schematic; unconfirmed pins and settings are disclosed.
3. **Jump to a part** (`#parts`): thirty-eight tiles, each a deep link into the
   explorer (`#<section>`, `#<section>/part:<id>`, `#<section>/wiring`).
4. **The 3D** (`#3d`): modelled parts, one hologram look, beside the
   exploded brake.
5. **Where it's at** (`#status`): what's open.
6. Contact line.

## Standing rules

- The explorer at `f56-3d/brake/` is a build copied in from the f56-3d repo.
  Don't edit it here; rebuild and copy. Its link back to this post is set in
  that build, so a new post date or slug means rebuilding it.
- Numbers on the page (torques, wear limits, part and part-number counts) match the
  explorer's data file. Keep them in step when it changes.

## Screenshots

All in `assets/images/f56-3d/`, taken from the explorer build in headless
Chrome with `?instant`, glow on, then saved as JPEG:

- `cover.jpg`: default car view at 1400 x 900, only the battery and driver's front wheel visible.
- `exploded.jpg`: `#brake` at 1400 x 900.
- `caliper.jpg`: `#part:caliper` at 1400 x 900.
- `wear-sensor.jpg`: `#part:wear-sensor` at 1400 x 900.
- `wiring.jpg`: `#wiring:wear-sensor` at 1400 x 900.
- `battery.jpg`: `#battery` at 1400 x 900.
- `phone.jpg`: `#part:caliper` at 390 x 844, 2x, phone emulation.
- Retaken 2026-10-27 from the current public explorer after the final front assemblies went in.

The first front expansion has six sections, 263 part groups and 283 distinct part numbers.
The assembly selector includes cooling pack, coolant plumbing, air conditioning and
subframe/steering. Keep the page's counts in step with later additions.

The external powertrain expansion brings the total to 14 sections, 486 part groups and
517 distinct part numbers. It adds engine exterior, covers/ignition, accessories, intake,
fuel, turbo, catalyst and gearbox/mounts. Neither engine nor gearbox internals are modeled.

- `front-cooling.jpg`: public explorer at `#cooling`, 1400 x 900, glow on.
- `front-subframe.jpg`: public explorer at `#subframe`, 1400 x 900, glow on.
- `front-phone.jpg`: public explorer at `#cooling`, 390 x 844, touch emulation, glow on.
  These are rendered model views; the new exterior geometry and placement are approximate.
- `powertrain-engine.jpg`: public explorer at `#engine-cover`, 1400 x 900, glow on.
- `powertrain-gearbox.jpg`: public explorer at `#gearbox/assembled`, 1400 x 900, glow on.

The confirmed front fitment has 23 sections, 796 part groups and 766 distinct part numbers.
The engine and gearbox remain external assemblies. Equipment alternatives are conditional;
the standard Cooper S bumper and halogen headlights are confirmed. Fog/indicator, parking and other independent equipment options remain open. All earlier screenshots are
retaken from this version so the selector and controls match the live explorer.

- `front-body.jpg`: `#bonnet/assembled`, 1400 x 900.
- `front-bumper.jpg`: `#front-trim/assembled`, 1400 x 900.
- `front-right.jpg`: `#front-right`, 1400 x 900.
- `front-wipers.jpg`: `#wipers/assembled`, 1400 x 900.
These are browser captures of the approximate models, with glow on.

The underbody expansion has 34 sections, 1,015 part groups, 923 distinct part numbers
and seven wiring diagrams. New sections default off. All captures are refreshed to
match the current controls and selector.

- `fuel-tank.jpg`: `#fuel-tank/assembled`, 1400 x 900.
- `rear-exhaust.jpg`: `#rear-exhaust/assembled`, 1400 x 900.
- `underfloor.jpg`: `#underfloor/assembled`, 1400 x 900.
- `rear-liners.jpg`: `#rear-liners/assembled`, 1400 x 900.
These are browser captures of the approximate models, with glow on.

The bonnet and bumper captures were refreshed after confirming the standard Cooper S
bumper and halogen headlights. LED-only headlight modules and JCW front ducts are excluded
from the current fitment; shared hardware stays in place.

Section visibility update: a fresh page starts with battery and driver's front wheel only.
The Sections panel toggles complete assemblies, with All, None and Defaults. Choosing an
assembly checks it. Existing captures were refreshed with these controls and the cleaner
context. `sections.jpg` and `sections-phone.jpg` show that panel open at 1400 x 900 and
390 x 844 respectively, from the public explorer with glow on.

The rear expansion has 27 sections, 938 part groups and 861 distinct part numbers.
All captures are refreshed from that public build. New assemblies default off.
Standard/sport/adaptive suspension remains conditional, spring numbers require weight
selection, and the cross-brace setting remains blank pending confirmation.

- `rear-left.jpg`: `#rear-left/assembled`, 1400 x 900.
- `rear-wear.jpg`: `#rear-right/part:wear-sensor-right`, 1400 x 900.
- `rear-axle.jpg`: `#rear-axle/assembled`, 1400 x 900.
- `parking-brake.jpg`: `#parking-brake/assembled`, 1400 x 900.
These are browser captures of the approximate models, with glow on.
