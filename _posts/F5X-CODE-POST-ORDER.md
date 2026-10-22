# F5X Code post: running order

Not a post. The running order for `2026-10-15-f5x-code.md`, whose page is
`_includes/f5xcode-feature.html`. Add to this structure instead of appending
to the end.

**The rule: what it is, what it costs and what hardware it needs come first,
then how to hook it up and what it can't do, then the feature detail.**
(Reordered 2026-10-17 at Matt's request, the way the R53 Logger page was; the
old order put the seven logging presets ahead of the price and hardware.)

1. **Hero**: what it is (the four cars, the three jobs, the connection), stats
   strip (Free engine fault codes, $45 once, 11 modules, 7 presets), Join the
   testers / $45 · what you need / Hook up the cable.
2. **What it does**: three tiles (faults, logging, coding) and three
   screenshots under them.
3. **What it costs**: the engine's fault codes are free; everything else is
   the one-time $45 unlock (`codingunlock`).
4. **What you need**: ENET cable plus USB-C Ethernet adapter, or the red MHD
   Universal adapter (both recommended, both ENET); Android 7+, and 11+ for
   the cable because it needs Ethernet tethering; OBDLink MX+ or CX optional.
5. **Hook up the ENET cable** (`#connect`): five steps, tethering called out,
   the chooser and ENET dialog screenshots, then the MHD adapter's three
   steps.
6. **What it can and can't do**. Keep the line that a listed option isn't
   always accepted: MINI changed these modules many times, and as with NCS
   Expert, seeing an option doesn't mean the car takes it.
7. **What it logs**: the seven presets from `LogPresets.kt` as tiles, the
   graph notes beside the preset picker screenshot.
8. **Coding and more**: oil level row with its screenshot, Quick codings (by
   module, from `CodingPresets.kt`) with the list and True speed review
   screenshots side by side, Track settings (`#track`: the e90post nannies
   guide mapped onto the F56's stability control, two settings-list shots and
   the Brake standby and EDLC reviews), the coding tiles, then the eleven
   modules and the option-size note.
9. **Get it**: closed testing, three steps (group, opt-in, install). Switch to
   one Play button when it goes to production.
10. Screenshot note, privacy line and the independent-app note.

## Standing rules

- Present tense, current build only. No "coming soon".
- Presets and modules must match the app: `LogPresets.kt` (7 presets),
  `ModuleSigningProfiles.kt` (11 modules) and `CodingPresets.kt` (the quick
  codings: 4 cluster, 4 body, 2 head unit) in the F56coding repo.
- The price is set in Play Console; the page says $45 (Matt, 2026-10-17).
- Connection labels match the app's chooser: ENET cable / USB-C, MHD Wi-Fi /
  ENET, OBDLink CX / Bluetooth LE, Paired Bluetooth adapter.

## Screenshots

In `assets/images/f5xcode/v2/`, 720 px wide, dark mode, status and nav bars
cropped. Every one is the real app on Matt's F56's own data: the 10-16 Check
all fault codes run and the 10-11 oil level reads, played back with the debug
`ReplayTransport` (`--es cmd connect --es adapter replay --es file <log>`),
which swaps the VIN for a placeholder; the 10-11 Tuning drive log; and the BDC
backup with its VIN replaced. Shot on the Pixel_10_Pro AVD. No VIN on screen.

`quick-codings.jpg` is the 10-09 cluster, head unit and BDC reads from
`F56coding-backups`, VIN swapped for `WMW00000000000000` everywhere in the file
(not just the VIN line: the Sources line has it too), pushed into the app's
`files/backups` with `run-as` and dated 10-09, then `--es cmd quick`;
`true-speed.jpg` is `--es cmd quick --es review true_speed`. Wake the
AVD first (`input keyevent KEYCODE_WAKEUP`): a sleeping display screencaps as
solid black. `faults.jpg` is the replayed Check all run, scrolled so the
Engine card is on top, cropped to 720 x 911 to match the other two.

The four `track-*.jpg` shots (2026-10-21, build 122) are the stability
control (29) backup from the 10-10 MHD capture, VIN swapped the same way and
pushed into `files/backups`. `track-brakes.jpg` is `--es cmd codingview
--es addr 29 --es scroll 3000/0042` and `track-ediff.jpg` the same with
`--es scroll 3000/0048`; `track-brake-standby.jpg` and `track-edlc.jpg` are
`--es review 3000/0046 --es value Off` and `--es review 3000/0065 --es value
Off`. All cropped from the toolbar down, the reviews at the bottom of the Your
change card. The AVD has a lock screen: `wm dismiss-keyguard` after waking it.
The guide's names map onto the F56's DSC as: e-diff 3000/0065, FLR 3000/0045,
fade compensation 3000/0048, HBA = Dynamic Brake Control 3000/0042, brake
standby 3000/0046, DPC = Performance Control 3000/0067; Maximum Brake Support
has no switch.
