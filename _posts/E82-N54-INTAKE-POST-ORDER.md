# E82 N54 intake CFD post: running order

Not a post. The running order for `2026-11-17-e82-n54-intake-cfd.md`, whose page is
`_includes/e82-n54-intake-feature.html`. Add to this structure instead of appending to the end.

Since 2026-11-19 the newest work comes first and the duct on the car today is at the bottom (Matt: most people don't read
far enough). Keep it that way: new versions, videos and numbers go near the top, superseded material further down.

1. **Hero**: what the intake is; stats = total loss with the new duct (9.2 kPa, 12.8 today), 28% less, 57 against 93 m/s
   into the box, the airflow. Then the airbox photo.
2. **The new duct** (`#redesign`): why (the jet into the box), what changed (round end matching the 100/94 mm socket, stopping
   12 mm short of the rim with a soft TPU coupler over the gap because the parts never line up perfectly on the car; the back
   wide out to the passenger side; the driver side widened), the four pictures (`duct-final-*`), today vs new duct table
   (coarse mesh for both, `v2-coarse-480gs` against `v7-coarse-480gs`), the new duct's flow video. "New duct" everywhere means
   this one.
3. **Where the pressure goes** (`#budget`): the loss chart and table, today's duct against the new one, then why today's jet
   loses so much. Then that the new duct isn't printed or on Cults3D, and the buy panel (`#buy`): "Print it yourself" with
   the three STL buttons. No print-to-order offer: Matt hasn't priced these.
4. **The lid and the hood** (`#lid`): the lid dome lowered 2 mm (it touched the hood); `lid-lowered.png`.
5. **With the lid off** (`#lidoff`): the unrestricted-filter baseline (2.4 kPa; 95% of the air over the open box); the hood gap
   makes the real open-lid case worse.
6. **Hot air** (`#heat`): heat soak from the CFD, the held-boost turbo model and its assumptions, the power table, the
   break-even bay temperature. Numbers from `cad/n54-airbox/results/iat-power.json`.
7. **Wastegate duty** (`#duty`): extra compressor work for the restriction, the heat it puts back, no duty-cycle percent without
   the tune's tables.
8. **The parts on the car today** (`#parts`): grille snorkels, relocated inlet duct, sealed airbox, each linked to its Cults3D
   listing; photos of the snorkels, the duct on the N54 and the snorkels on the red car.
9. **What the CFD says about today's duct** (`#results`): per-pipe loss and vacuum, vacuum against loss, the compressor inlet
   pressure, the snorkel split (71% passenger, 29% driver), today's flow video.
10. **How I got to the new duct** (`#steps`): the first step (round end and wide back, `v6-coarse-480gs`) against today, the
    narrowing crossing that made it worse; h3 "Then the driver side" (Fusion capture from behind, top-view cut against the first
    step, first step vs wider driver side table).
11. **How it was modelled** (`#model`).
12. **What this does not show** (`#limits`): links the pressure CAN node post, which isn't built.

## Standing rules

- Two cars: the **silver N54 135i** has the airbox, duct and snorkels; the **red LS-swapped 135i**
  runs the grille snorkels into its own intake. Matt calls the red one LS3; the build post says L92,
  so the page says "LS-swapped".
- The three Cults3D listings: E82 air intake duct (the grille snorkels), E82 135i relocated inlet air
  duct (under `/3d-model/tool/`), E82 N54 relocated inlet airbox. Cults3D refuses automated reads;
  listing text and photos come from Matt.
- 0.48 kg/s (63.5 lb/min) is the airflow for about 550 whp, a target. Don't present it as measured.
- The filters' 500 Pa each is assumed. Say so wherever it appears.
- "Vacuum in the pipe" is static pressure and includes about 4 kPa of air speed; "lost" is total
  pressure and doesn't. Keep the two apart.
- These are pressures in a simulated intake, not horsepower.
- Temperatures in °F only, across the whole blog (Matt, 2026-11-18). Rises and differences are x1.8, not +32.

## Images

All in `assets/images/e82-n54-intake/`.

- `airbox-n54-installed.jpg`: Matt's photo of the airbox on the N54, lid on, duct and blue ring (1130 x 1500, sent
  2026-11-19). The lead photo: the header background, the post's cover, and the first figure (uncropped, `figure.portrait`,
  640 px wide at most). On screens 760 px and wider the header shows the whole photo at the right (scaled to the header's
  height) with the excerpt on the left, so it isn't cropped; on phones it fills the header. Replaces the 773 px crop of
  his listing screenshot (`airbox-lid-n54.jpg`, removed).
- `grille-snorkels.jpg`, `grille-snorkel-in-hand.jpg`, `snorkels-and-duct.jpg`: the red LS car.
- `duct-radiator-support-n54.jpg`, `duct-outlet-n54.jpg`: the silver N54, before the airbox.
- Photos resized to 1600 px, JPEG quality 84, no metadata.
- `cfd-flow-duct-today.mp4` / `cfd-flow-duct-today.jpg`: arrows traced through the solved velocity field from the snorkel
  mouths, each moving at the local air speed, slowed 250 times, colour 0 to 120 m/s, 1280 x 720,
  10 s. From the medium-mesh case at 0.48 kg/s. Both videos re-rendered 2026-11-18 with side-labelled snorkel boxes, the duct
  label clear of them, walls at 13% opacity so the duct shape reads, and zoom 1.5.
- `cfd-flow-duct-final.mp4` / `cfd-flow-duct-final.jpg`: the same video for the final duct (case `v7-coarse-480gs`, coarse mesh),
  same camera, colour scale and slow-motion, so the two can be compared side by side.
- `cfd-loss-budget-compare.png`: total pressure lost at stations along the rear-turbo path, today's duct (`v2-coarse-480gs`) against the
  final duct (`v7-coarse-480gs`), same coarse mesh (`cad/n54-airbox/cfd/plot_loss_budget_compare.py`, `results/loss-budget-compare.png`).
  The budget table uses the same two cases.
- `lid-lowered.png`: cuts of the lid exported from Fusion before and after lowering, with the rear filter's scanned top edge.
- `duct-final-overview.jpg`, `duct-final-behind.jpg` (2026-11-19): renders of the final duct (v7 with the end cut back,
  `cad/n54-airbox/source/airduct-intakeduct-v7b-joint-gap.step`) and the TPU coupler (`source/airbox-coupler-tpu.step`),
  and of today's duct from the same view, made offline from the STEP files (pyvista) because Fusion was busy.
  `duct-final-plan.png`: an exact cut of the STEP files at mid height (y = 172 mm, duct frame). `duct-final-joint.png`: the
  joint section from `cad/n54-airbox/coupler/section.py` in the post's colours. These replaced the first redesign's
  pictures (`duct-redesign-overview.jpg`, `-end.jpg`, `-plan.png`), which showed the old driver side and the rigid sleeve.
- The redesign numbers are from `cad/n54-airbox/results/v6-coarse-480gs.json`, compared with `v2-coarse-480gs` (same mesh).
- `duct-driver-wide.jpg`: Fusion 360 viewport capture of the duct's driver side from behind (version 7 of
  `135-airduct-finalversion-cults3d`). `duct-driver-plan.png`: a mid-height cut of the exported STEPs, redesign (v6) against v7
  (`cad/n54-airbox/views/plan-compare-driver.png`). The driver-side numbers are from `results/v7-coarse-480gs.json`.
- Hot air, wastegate duty and the on-track table use the final duct (v7): `results/iat-power.json`, `iat-track-sweep-F.json`.
- The CFD numbers and the two CFD pictures are from the corrected run of 2026-11-18 (12.7 kPa). An earlier
  run let part of the air into the duct downstream of the passenger snorkel and came out at 11 kPa; never
  use that figure.
