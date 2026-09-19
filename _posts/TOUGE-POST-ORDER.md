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
    off-road.

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
