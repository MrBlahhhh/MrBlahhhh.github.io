# E82 N54 intake CFD post: running order

Not a post. The running order for `2026-11-17-e82-n54-intake-cfd.md`, whose page is
`_includes/e82-n54-intake-feature.html`. Add to this structure instead of appending to the end.

1. **Hero**: what the intake is, stats = total loss before the turbos, vacuum in the turbo pipes,
   the duct-jet share, the airflow. Then the airbox photo.
2. **The parts** (`#parts`): grille snorkels, relocated inlet duct, sealed airbox, each linked to its
   Cults3D listing; photos of the snorkels, the duct on the N54 and the snorkels on the red car.
   Then the buy panel (`#buy`): "Print it yourself" with the three STL buttons. No print-to-order
   offer: Matt hasn't priced these.
3. **What the CFD says** (`#results`): per-pipe loss and vacuum, vacuum against loss, the compressor
   inlet pressure, the snorkel split (71% passenger, 29% driver), the flow video.
4. **Where the pressure goes** (`#budget`): the loss chart and table, the duct-end jet.
5. **The redesigned duct** (`#redesign`, added 2026-11-18): the round end matching the airbox socket (100/94 mm, butt joint,
   slide-over coupler) and the back kept wide out to the passenger side; Fusion renders, the top-view cut, today vs redesign
   table (coarse mesh for both), the redesign flow video, the narrowing crossing that made it worse, and that the redesign isn't printed or on Cults3D.
6. **The lid and the hood** (`#lid`): the lid dome lowered 2 mm (it touched the hood); `lid-lowered.png`.
7. **With the lid off** (`#lidoff`): the unrestricted-filter baseline (2.4 kPa; 95% of the air over the open box); the hood gap
   makes the real open-lid case worse.
8. **Hot air** (`#heat`): heat soak from the CFD, the held-boost turbo model and its assumptions, the power table, the
   break-even bay temperature. Numbers from `cad/n54-airbox/results/iat-power.json`.
9. **Wastegate duty** (`#duty`): extra compressor work for the restriction, the heat it puts back, no duty-cycle percent without
   the tune's tables.
10. **How it was modelled** (`#model`).
11. **What this does not show** (`#limits`): links the pressure CAN node post, which isn't built.

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

- `airbox-lid-n54.jpg`: the lead photo of the airbox listing, cropped from Matt's screenshot of it
  (773 px). Replace with his original when he sends it.
- `grille-snorkels.jpg`, `grille-snorkel-in-hand.jpg`, `snorkels-and-duct.jpg`: the red LS car.
- `duct-radiator-support-n54.jpg`, `duct-outlet-n54.jpg`: the silver N54, before the airbox.
- Photos resized to 1600 px, JPEG quality 84, no metadata.
- `cfd-flow.mp4` / `cfd-flow.jpg`: arrows traced through the solved velocity field from the snorkel
  mouths, each moving at the local air speed, slowed 250 times, colour 0 to 120 m/s, 1280 x 720,
  10 s. From the medium-mesh case at 0.48 kg/s.
- `cfd-flow-redesign.mp4` / `cfd-flow-redesign.jpg`: the same video for the redesigned duct (case `v6-coarse-480gs`, coarse
  mesh), same camera, colour scale and slow-motion, so the two can be compared side by side.
- `cfd-loss-budget.png`: total pressure lost at stations along the rear-turbo path, same case.
- `lid-lowered.png`: cuts of the lid exported from Fusion before and after lowering, with the rear filter's scanned top edge.
- `duct-redesign-overview.jpg`, `duct-redesign-end.jpg`: Fusion 360 viewport captures of the
  redesigned duct and coupler (the real model, `135-airduct-finalversion-cults3d`). `duct-redesign-plan.png`: a cut of the
  exported STEP at mid height, today's duct against the redesign (`cad/n54-airbox/views/plan-compare.png`).
- The redesign numbers are from `cad/n54-airbox/results/v6-coarse-480gs.json`, compared with `v2-coarse-480gs` (same mesh).
- The CFD numbers and the two CFD pictures are from the corrected run of 2026-11-18 (12.7 kPa). An earlier
  run let part of the air into the duct downstream of the passenger snorkel and came out at 11 kPa; never
  use that figure.
