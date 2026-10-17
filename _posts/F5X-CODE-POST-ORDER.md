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
8. **Coding and more**: oil level row with its screenshot, the coding tiles,
   then the eleven modules and the option-size note.
9. **Get it**: closed testing, three steps (group, opt-in, install). Switch to
   one Play button when it goes to production.
10. Screenshot note, privacy line and the independent-app note.

## Standing rules

- Present tense, current build only. No "coming soon".
- Presets and modules must match the app: `LogPresets.kt` (7 presets) and
  `ModuleSigningProfiles.kt` (11 modules) in the F56coding repo.
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
