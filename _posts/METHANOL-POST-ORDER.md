# Methanol controller post: running order

Not a post. The running order for `2026-10-23-methanol-injection-controller.md`,
whose page is `_includes/methanol-feature.html`. Add to this structure instead
of appending to the end.

1. **Hero**: what it is (boost-referenced PWM, boost from the DME, pump
   current as the safety, shift light), stats strip (2 → 8 psi, 25 % floor,
   300 ms soft start, 11 A trip), three buttons.
2. **On the bench** (`#bench`): the 2026-10-23 ramp-test short
   (`s17SzJuKtCg`) beside what's in the frame.
3. **How it works**: six tiles (DME boost, curve, PWM, pump current, soft
   start, shift light), then the line about cars without UDS.
4. **The board**: Rev E parts list beside the photo, then Rev F in one
   paragraph. Follow with the PCB render gallery and full assembly sheet.
5. **The screen**: what it is and how it links, Boot (all four clips, current
   water/meth splash first), the F56 HUD storyboard, current navigation,
   the current housing and screw-on bezel. No old LCD captures.
6. **The app**: four main screens, then shift light and Advanced beside the
   shift light shot, then the screenshot note and F5X diagnostic link.
7. **Setting it up** (`#setup`): eight steps, then the qualification note.
8. **Safeties** (`#safeties`): fault table, latching, "Before it sprays at
   all", where you see it, firmware guards.
9. **Where it's at**: installed on 26 October, with spray and on-car logging
   still to qualify. Bench footage and CAD renders remain labeled as such. Do not republish the
   rejected nine-panel screenshot gallery or the obsolete ball-arm mount.

## Standing rules

- Present tense, current build only. No comparisons with other meth kits:
  the design is a simple boost-referenced PWM with pump current as the
  safety, and the page says that on its own terms.
- Numbers come from the controller firmware (`pwm_controller`, branch
  `firmware-rev-e`): `defaults()` and `validate()` in
  `firmware/src/logic/control.cpp`, the fault list in `firmware/src/proto.h`.
  The 11 A trip is Matt's setting, not a default (the default is 0, which
  blocks the pump).
- The repo is private. Don't link it or name its scripts as if readers can
  run them.
- Matt confirmed installation on 26 October 2026. That supersedes the old
  "before it goes in the car" status, but does not prove spraying is enabled
  or qualified. Keep the qualification note until the firmware's Arm gate
  (`MP_INTAKE_QUALIFIED`) and installed operation have been verified.

## Images

All in `assets/images/methanol-controller/`.

- `cover.jpg`, `board-rev-e.jpg`: frames from the 2026-10-23
  short (the 12 s frame, 100 % duty, 0.6 A).
- `screen-*.png`: 480 x 480 frame-buffer captures off the 2.1" LCD with
  `tools/screen_shot.py` on the screen's demo loop, 2026-10-17 (after the
  sweep gauges, before the 25 % floor, so the demo curve starts at 10 %).
  `screen-fault.png` is the right half of a two-up capture from the same
  session. Retake them with `screen_shot.py COM5 out.png --demo --all` once
  the screen is on the PC's USB again, and drop the capture note.
- `app-dash.jpg`, `app-pump-test.jpg`, `app-settings.jpg`: Matt's phone on
  the bench board, 2026-10-22 (0.5.0), status and gesture bars cropped, 720
  wide. The pump-test shot has the cut-off footer line removed above the nav
  bar.
- `app-curve.jpg`, `app-shift-light.jpg`: 0.5.1 on the `Pixel_10_Pro` AVD,
  from a throwaway local build (never committed) that loaded the app's
  `default-config.bin` into the board slot with an 11 A limit and set the
  shift-light capability, so the screens render without a board. The
  shift-light shot is cropped below the Advanced header and its
  telemetry-unknown banner.
- `boot-charge-pipe.mp4`, `boot-turbo.mp4`: the repo's
  `screen/boot/alternates/charge-pipe/charge-pipe.mp4` (built into the
  screen) and `screen/boot/boot-preview.mp4`, remuxed with `+faststart`.
  Posters are frames at 4.5 s and 6.5 s.
- Added 26 October: `boot-jcw-splash.mp4` and `boot-f56-hud.mp4` from
  `pwm_controller-fw-rev-e/screen/boot/alternates/`, remuxed with faststart.
  The splash poster is its supplied preview; the HUD poster is its 3.1 s
  frame. `boot-f56-hud-storyboard.jpg` is the supplied HUD preview sheet.
  The current built-in clip is the water/meth splash, as confirmed by
  `screen/README.md` and `screen/bootclip.py`; the HUD README's older
  built-in claim is superseded. All four videos have controls and no autoplay.
- Added 26 October: `pcb-iso.png`, `pcb-back.png`, `pcb-routed.png` and
  `pcb-assembly.png` from Rev E's `pcb/plots/`. These are design renders,
  not photographs or proof of physical testing. The assembly sheet includes
  an older screen note, called out in its caption.
- Added 26 October: `screen-mount.png` from
  `screen/mechanical/mount/preview.png`, labeled as a CAD preview. The mount
  joins a 1-inch RAM ball and a 17 mm screen ball with one M4 knob.
- Old app/panel captures remain for their historical detail. Do not present
  them as screenshots of the current Live/Graph/Menu interface. The 13 October
  Android UI-smoke captures on the rev-f checkout are older than the phone
  photos already in the article, and are not replacements.

## Correction after Matt's review, 26 October

- Remove all nine 17 October LCD demo images from the article. Matt rejected
  that entire gallery as the wrong interface. Dating them is not a substitute
  for showing the current screen. No current panel captures are available in
  the project assets; keep the source-verified Live/Graph/Menu description.
- Remove the obsolete two-ball mount and its description. The current CAD is
  `screen/mechanical/enclosure/screen-housing.stl` and `screen-bezel.stl`, with
  the rear quarter-inch 20 TPI RAM mounting thread and screw-on bezel described
  in that directory's README. Do not use `mechanical/mount/preview.png` again.
- `housing-front.png`, `housing-rear.png` and `housing-bezel.png` are direct
  mesh renders of those current STL files. The two meshes use separate print
  coordinates, so show the parts individually instead of guessing an assembled
  alignment. These are CAD views, not installation photographs.
