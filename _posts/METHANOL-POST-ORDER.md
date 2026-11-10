# Methanol controller post: running order

Not a post. The running order for `2026-10-23-methanol-injection-controller.md`,
whose page is `_includes/methanol-feature.html`. Add to this structure instead
of appending to the end.

Since 9 November 2026 this is a sales page for the F56 kit (controller board,
round screen in its housing, app). Sales copy only: capability claims, no
"honest limits" or "where it's at" sections. Ordering is by email or
Instagram, no price on the page.

1. **Hero**: what it is, the kit line, stats (0 extra sensors, 2 → 8 psi
   default curve, 10 ms pump current checks, 50 Hz log stream), buttons to
   `#order`, `#can`, `#app`.
2. **What you get** (`#kit`): board, screen, app, F56 Code logging.
3. **How the CAN control works** (`#can`): the bus diagram (inline SVG), the
   frame table (0x0A5, 0x2C7, 0x0D9), the lift gate, the shift-cut gate and
   freshness, when it talks and when it doesn't, F56 Code through the board,
   the CAN capture.
4. **Spray control** (`#spray`): six tiles, then "Before it sprays".
5. **Pump monitoring and faults** (`#safeties`): learned curves beside the
   pump health shot, the warning/fault table, then the warning, fault and
   Dash examples.
6. **The screen** (`#screen`): LCD gallery, TF card logs, night brightness,
   the boot clip and storyboard, the housing renders.
7. **The app** (`#app`): eight screenshots, "And the rest" beside the boot
   lettering shot, the screenshot note.
8. **Setting it up** (`#setup`): the wizard's steps.
9. **The board** (`#board`): parts list beside the bench photo, three PCB
   renders.
10. **See it run** (`#bench`): the 2026-10-23 ramp-test short
    (`s17SzJuKtCg`).
11. **Order one** (`#order`).

## Standing rules

- Present tense, current build only. No comparisons with other meth kits:
  the design is a simple boost-referenced PWM with pump current as the
  safety, and the page says that on its own terms.
- Numbers come from controller firmware 0.5.34 and screen firmware 0.1.13.
  Recheck them against the firmware when either changes.
- Dry tank, open pump, clogged nozzle and tank low are warnings: the pump
  keeps running. Only overload, driver short, battery, sensor, current
  reading and settings memory stop it. Don't show a dry-pump fault banner or
  a blue dry-tank strip; the firmware no longer does either.
- Don't claim what isn't built: night dimming from the car's dimmer, the
  washer-fluid level as the tank sender, the fail-safe wire (not on this
  board), all-module diagnostics.
- The controller repo is private. Don't link it or name its scripts.
- No boot clip with a cartoon character on the page.

## Images

All in `assets/images/methanol-controller/`.

- `cover.jpg`, `board-rev-e.jpg`: frames from the 2026-10-23 bench short.
  Replace with photos of the kit and of it installed when Matt takes them.
- `app-*-0633.jpg`: app 0.6.33 on the `Pixel_10_Pro` emulator with the
  debug-only demo board (`--es cmd demo`, `--ei screen N`, 10+N for the
  engineering tabs), which plays back a third-gear pull: cruise at 2,400 rpm,
  then 2,400 to 6,500 rpm with boost to 19 psi. Status and gesture bars
  cropped, 720 wide. Captioned as emulator and demo board. Swap for Matt's
  phone shots at the car when he takes them, and drop the note.
- `lcd-0113-*.png`: screen firmware 0.1.13's page code rendered on a PC by
  the controller repo's LCD preview in gallery mode, fed a simulated drive of
  seven third-gear pulls. Captioned as rendered. The dry-pump shot shows the
  amber warning line; the banner shot is an overload.
- `boot-charge-pipe-tunnel.mp4`: the built-in boot clip's branded preview
  (F56, WATER / METHANOL), remuxed with faststart. Poster is its 6.5 s
  frame; the storyboard is the clip's own stills sheet.
- `housing-*.png`: mesh renders of the current housing and bezel print
  files. CAD views.
- `pcb-*.png`: the board's design renders.

## Wanted from Matt

- Photos: the kit laid out, the board in the glovebox, the screen mounted
  in the car with the Live page up.
- A short of the screen during a pull on the car, to replace or sit beside
  the bench ramp test.
- Phone screenshots at the car: Dash spraying, Curve, Pump test, Pump
  health, Shift light.
