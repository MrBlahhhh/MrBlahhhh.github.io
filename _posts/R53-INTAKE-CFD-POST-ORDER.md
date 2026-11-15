# R53 intake CFD post: running order

Not a post. The running order for `2026-11-15-r53-intake-factory-vs-printed-cfd.md`, whose
page is `_includes/r53-intake-cfd-feature.html`. Add to this structure instead of
appending to the end.

The post is built around four cases, which Matt asked for by name: factory piece with the
rear port closed, my snout with the rear port closed, my snout with a 3-inch rear port, my
snout with a 6 x 4.2 inch rear port.

1. **Hero**: the four cases, then the buy panel (`#buy`, added 2026-11-15 at Matt's request, keep it
   at the top): "Print it yourself" with both Cults3D STL buttons, and "Get one printed" with email
   and Instagram buttons plus a link to the July printed-parts pricing post. Then the stats = vacuum
   ahead of the throttle body for each (20.7, 14.4, 6.6, 5.9 kPa).
2. **The four cases** (`#cases`): four-column table (vacuum, total loss, front and rear split), the
   three step-by-step differences, then the four videos in a 2 x 2 grid.
3. **What was compared** (`#parts`): the two front pieces, the rear port, the shared tract, two car
   photos reused from the July airbox post, links to both Cults3D listings.
4. **How it was modelled** (`#model`): one piece of air, airflow, filter assumption, how the factory
   piece was rebuilt from the scan, vacuum against loss.
5. **Factory piece against my snout** (`#front`): the two inlet close-up videos, bench table, the
   6 kPa gap, stock-airflow note, bench cut picture.
6. **What the rear port does** (`#rear`).
7. **What I tried that did not beat it** (`#tried`).
8. **What this does not show** (`#limits`).

## Standing rules

- Matt calls his part the **snout**. Not "spout".
- The car is supercharged. The outlet is the throttle body, never a turbo.
- The rear port is the buyer's cut in the factory partition, not a fixed feature. "Closed" means
  the partition is intact.
- 0.269 kg/s is the airflow for about 240 whp, a target. Don't present it as measured.
- The filter's resistance (500 Pa at this flow) is assumed. Say so wherever it appears.
- "Vacuum ahead of the throttle body" is static pressure at the outlet and includes about 4.6 kPa of
  velocity head in the 61 mm bore; "loss" is total pressure and does not. Keep the two apart.
- The factory piece is a reconstruction from a scan: 64% scanned inside wall, the rest
  outside less 2.8 mm. That is stated once, in "How it was modelled". Matt removed the
  reconstruction bullet from the limits list and the "Next" section on 2026-11-15; do not add them back.
- These are pressures in a simulated intake, not horsepower.

## How the pictures were made

- `case*.mp4`, `flow-*-inlet.mp4` and their posters: arrows traced through the solved velocity field
  from the inlet patches, every arrow moving at the local air speed (slowed 250 times), colour 0 to
  120 m/s. Made with `cad/r53-solidcraft/cfd/flow_video.py` in the f56-airbox repo
  (`--vmax 120 --min-speed 4`), then scaled to 1280 x 720.
  - `case1-factory-closed`, `flow-factory-inlet`: case `oem-rear-closed-medium-500pa-269gs`.
  - `case2-snout-closed`, `flow-printed-inlet`: case `current-rear-closed-medium-500pa-269gs`.
  - `case3-snout-rear-3in`: case `current-rear-3in-medium-500pa-269gs`.
  - `case4-snout-rear-6x4`: case `current-rear-6x4-medium-500pa-269gs` (like-for-like with the other
    three; it replaced an earlier coarse run on 2026-11-15).
- `bench-speed-cuts.png`: air speed on cut planes from the two bench runs
  (`cfd/bench-inlet-results/` in the f56-airbox repo).
- Car photos are the July post's, in `assets/images/r53-airbox`.

## Where the numbers come from

`cad/r53-solidcraft/cfd/plenum-inlet-results/README.md` (the four cases, vacuum and loss)
and `cad/r53-solidcraft/cfd/bench-inlet-results/README.md` (factory piece, bench) in the f56-airbox repo.
