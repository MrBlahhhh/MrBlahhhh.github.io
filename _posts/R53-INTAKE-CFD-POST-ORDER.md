# R53 intake CFD post: running order

Not a post. The running order for `2026-11-15-r53-intake-factory-vs-printed-cfd.md`, whose
page is `_includes/r53-intake-cfd-feature.html`. Add to this structure instead of
appending to the end.

1. **Hero**: what it is (printed spout against the factory front piece, same simulated
   airbox), stats (0.269 kg/s, 9.75 kPa printed, 15.99 kPa factory, 1.94 kPa with a 3-inch rear hole).
2. **What was compared** (`#parts`): the two front pieces and the shared tract, two car photos
   reused from the July airbox post, links to both Cults3D listings.
3. **How it was modelled** (`#model`): one piece of air, airflow, filter assumption, how the
   factory piece was rebuilt from the scan, rear partition intact.
4. **The flow** (`#flow`): whole-tract videos side by side (printed, factory), then the two inlet close-ups.
5. **The numbers** (`#results`): whole tract and bench table, the 6 kPa gap, stock-airflow note, bench cut picture.
6. **Cutting the rear partition** (`#rear`): 3-inch hole video and the split.
7. **What I tried that did not beat it** (`#tried`).
8. **What this does not show** (`#limits`).

## Standing rules

- The car is supercharged. The outlet is the throttle body, never a turbo.
- The rear opening is the buyer's cut in the factory partition, not a fixed feature. Say
  "with a 3-inch hole", not "the 3-inch version".
- 0.269 kg/s is the airflow for about 240 whp, a target. Don't present it as measured.
- The filter's resistance (500 Pa at this flow) is assumed. Say so wherever it appears.
- The factory piece is a reconstruction from a scan: 64% scanned inside wall, the rest
  outside less 2.8 mm. That is stated once, in "How it was modelled". Matt removed the
  reconstruction bullet from the limits list and the "Next" section on 2026-11-15; do not add them back.
- These are pressure losses, not horsepower.

## How the pictures were made

- `flow-*.mp4` and their posters: arrows traced through the solved velocity field from the
  inlet patches, every arrow moving at the local air speed (slowed 250 times), colour 0 to
  120 m/s. Made with `cad/r53-solidcraft/cfd/flow_video.py` in the f56-airbox repo
  (`--vmax 120 --seed-density 4000 --min-speed 4`), then scaled to 1280 x 720.
  - `flow-printed-sealed`, `flow-printed-inlet`: case `current-rear-closed-medium-500pa-269gs`.
  - `flow-factory-sealed`, `flow-factory-inlet`: case `oem-rear-closed-medium-500pa-269gs`.
  - `flow-printed-rear-3in`: case `current-rear-3in-medium-500pa-269gs`.
- `bench-speed-cuts.png`: air speed on cut planes from the two bench runs
  (`cfd/bench-inlet-results/` in the f56-airbox repo).
- Car photos are the July post's, in `assets/images/r53-airbox`.

## Where the numbers come from

`cad/r53-solidcraft/cfd/bench-inlet-results/README.md` and
`cad/r53-solidcraft/cfd/plenum-inlet-results/README.md` in the f56-airbox repo.
