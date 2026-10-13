# PumpWitness post: canonical section order

Not a post. The running order for `2026-10-12-pumpwitness.md`, whose page is
`_includes/pumpwitness-feature.html`. Add to this structure instead of appending
to the end.

**The rule: sales pitch order. Features first, setup last.**

## Running order

0. **Get PumpWitness**, directly under the hero, the same two steps as Touge:
   join the Google Group (`groups.google.com/g/pumpwitness`), then the Play
   testing opt-in (`play.google.com/apps/testing/com.geekopolis.pumpwitness`).
   The store listing 404s for anyone not in the test, so the button goes to the
   opt-in; switch it to the listing when the app is in production. The note
   says Google takes about 10 minutes to give a new member access. Then two lines: it's Jackie's idea and her first app, and the
   screenshots are our family's real data.
1. **Three photos at the pump**: the review screen, the pump photo (the
   evidence copies go with it), then reading with or without a Gemini key.
2. **Every car in the garage**: the dashboard, a car's page (with CSV import),
   adding a car.
3. **MPG you can trust**: MPG against the EPA, then the missed fill-up kept
   out of the average.
4. **Bad gas**: tiles, no screenshot until a station is really flagged.
5. **Your log, on your phone**: privacy tiles and the policy link.
6. The app icon, and the closing link back to Get.

## Layout rules

- Sections alternate `row` and `row flip`; check the alternation after any
  insert.
- One figure per row. `grid3` tiles for three or more short things.
- Lead each section with a `<p class="lead">` one-liner.

## Screenshot rules

- From Matt's Pixel (`192.168.1.201:5555`) with the family's real data, status
  bar in SystemUI demo mode (12:00, full battery), portrait.
- **No street addresses or towns.** The Stations list and a fill-up's station
  field show the station's street and North Carolina town; leave those screens
  out or crop above them. Settings is cropped below the Family garage row,
  which only our family can use.
- Nothing staged: open screens, never save or edit while shooting.
