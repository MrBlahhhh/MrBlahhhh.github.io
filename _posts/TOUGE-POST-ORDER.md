# Touge post: canonical section order

Not a post. This is the running order for
`2026-09-16-touge-offline-routing-and-recording.md`, kept beside it so an
update adds to the structure instead of appending to the end of it. The post
had drifted into the order things were *built*, which is not the order anybody
reads them in.

**The rule: sales pitch order. Features first, setup last.** The one exception
is the invite wizard, which belongs with the group because that is the feature
it is part of.

## Running order

0. **Get Touge** - directly under the hero, before the first section: join the
   Google Group (`groups.google.com/g/tougenav`), then the Play testing opt-in
   (`play.google.com/apps/testing/com.geekopolis.touge`). The store listing
   404s for anyone not yet in the test, so the button goes to the opt-in. Google
   takes about 10 minutes to give a new member access; the note says so. The
   closing line links back to it.
1. **Ride together** — the showcase, and the reason the app exists. One
   feature set, in this order inside the section:
   1. the group on one map and the table
   2. how positions travel (cell, then LoRa, the hand-off)
   3. the invite wizard and the Advertise button
2. **Routing and planning** — five ways by default, the TWIST score, and how
   the private Valhalla is asked with Touge's own preferences.
3. **Trip settings** — highway to the first stop, back roads after. The thing
   no other routing app will do, and it deserves its own section rather than
   a bullet under routing.
   Route editing and **Save, share and import your routes** follow it. Use
   the real Tazewell–Marion and Floyd–Woolwine plans for routing examples.
4. **Drive it** — turn by turn, the strip, the voice.
5. **Skipping a stop without stopping** — solved here, painful everywhere else.
6. **Weather and police radar** — the disc, police dots ageing on the map, the
   popup with thumbs-up count and age.
7. **Alerts and alert settings** — what expires and what does not, per-kind
   map/voice control, warning distance.
8. **Valentine One Gen 2** — how an alert is shown: the card, the table, the
   bearing on the map.
9. **TPMS** — BLE only, which means newer in-wheel Tesla sensors or the
   screw-on caps.
10. **Setup, maps, search, server** — every technical screen, last. Inside
    it: map packs, then **routing with no signal** (it is built out of the
    packs, so it follows them), then search, then the tiles, then traffic and
    off-road. **Search on Android Auto** follows the search tiles, with the
    real desktop head-unit town and address screenshots labelled as bench
    checks. Settings remain last.

## Layout rules

- Sections alternate `<div class="row">` and `<div class="row flip">` down the
  page, so images run left, right, left, right. Check this after any insert:
  adding one section shifts every alternation below it.
- Every `row` is one figure and one `<div>` of prose. Never two figures.
- `grid3` tiles are for sets of three or more short things, not for prose.
- Lead each section with a `<p class="lead">` one-liner that states the
  argument, then evidence.

## Standing content rules

- Present tense, current features only. No "coming soon", no bug history, no
  "this used to be broken".
- Nothing that is not real. Every screenshot is the app running; if a feature
  is untested on hardware, either leave it out or say so plainly in one line.
- No fabricated imagery. The recorder had a generated overlay mock-up and it
  came out: group rides are being tested before video, so the recorder is not
  a shipping feature to show.
- Units: miles and feet, tires not tyres.

## Screenshot rules

Matt, 2026-10-09: the shots had drifted out of date and out of area again.

- **2D only.** Set Display & layout › Perspective to 2D, flat and 3D terrain
  to Off before any map shot, then put them back. Terrain on also streaks the
  route chooser's hillshade.
- **Southwest Virginia.** Map shots are around Tazewell, Marion, Floyd and
  Woolwine: VA 16 for the drive, VA 8 for stops and the phone. No North
  Carolina towns in map shots, alt text or captions.
- Shoot group screens with Setup › App & data › Demo ride, following
  Tazewell-Marion-VA16 or routing Floyd-Woolwine-Loop from Floyd. A search shot
  is taken while the demo is driving, so distances are measured from the demo
  car and not from wherever the tablet really is.
- After a demo session, delete the `ride-*` files it recorded in Rides and
  routes: the recorder keeps the demo's invented track.
- Retake a shot when its screen changes. Setup was redrawn in bands in build
  164 and the route line went purple in 169, and both left stale shots here.
