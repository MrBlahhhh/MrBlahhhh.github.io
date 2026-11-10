# F56 intake CFD post: running order

Not a post. The running order for `2026-11-10-f56-cold-air-intake-cfd.md`, whose
page is `_includes/f56-intake-feature.html`. Add to this structure instead of
appending to the end.

1. **Hero**: what it is (printed snorkel and airbox, modelled port to coupler
   before printing), stats (355 CFM logged peak, coupler vacuum, loss change
   against the first design, cells per run).
2. **What it is** (`#parts`): the three parts and the mounts, two CAD renders
   side by side, then the top view, then **The filter** (`#filter`): spec table
   for the KYOSTAR KD2045RD the housing is sized to, with a link to the listing
   Matt ordered from (11-10).
3. **How it was modelled** (`#model`): one model, real flows, mesh and solver,
   the filter assumption.
4. **The flow** (`#flow`): final top video, final side video, first design video.
5. **First design against final** (`#results`): totals table, loss by stretch,
   the housing section, the two housing slices, the snorkel slice and the MAF
   plane.
6. **Mounting it** (`#mounts`): front stud bosses, rear bracket and TPU sleeves.
7. **Next** (`#next`).

## Standing rules

- The filter's resistance is assumed (no published rating). Say so wherever a
  filter number appears.
- Flows: 355 CFM is the logged peak from my own car; 563 CFM is the 350 whp
  estimate. Don't present 563 as measured.
- "Coupler vacuum" includes the factory neck's velocity head, which the stock
  box pays too.

## How the pictures were made

- `cad-*.jpg`: Fusion 360 viewport captures of the final parts, scans hidden.
- `housing-section.jpg`: section through the drum axis of the exported first
  and final housings, filter outlines drawn from its catalogue sizes.
- `*-slice.jpg`, `final-maf-plane.jpg`: velocity on cutting planes from the CFD
  runs at 355 CFM (first design and final).
- `flow-*.mp4` and their posters: arrows traced through the solved velocity
  field from both ports, every arrow moving at the local air speed (slowed
  down), parts drawn see-through. Poster = a frame from each video.
- `stud-bosses.jpg`: section through each front boss over the snorkel-off bay
  scan of the studs and support.
- `rear-bracket.jpg`: Fusion 360 capture of the rear bracket under the drum.
