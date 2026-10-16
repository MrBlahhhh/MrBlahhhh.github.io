# F5X Code post: running order

Not a post. The running order for `2026-10-15-f5x-code.md`, whose page is
`_includes/f5xcode-feature.html`. Add to this structure instead of appending
to the end.

**The rule: what it logs and does first, then price, hardware and can't, then
how to get it.**

1. **Hero**: what it is, stats strip (presets, codable modules, fault titles),
   Join the testers / What it logs / Price.
2. **What it logs**: the seven presets from `LogPresets.kt` as tiles, then the
   graph features.
3. **What else it does**: faults, oil level, coding read, change, compare and
   history, body module recovery; then the eleven modules in one line.
4. **What it costs**: free vs the one-time unlock (`codingunlock`). Add the
   price once it's set in Play Console.
5. **What you need**: ENET cable, MHD Wi-Fi, Bluetooth/Wi-Fi OBD.
6. **What it can and can't do**.
7. **Get it**: closed testing, three steps (group, opt-in, install). Switch to
   one Play button when it goes to production.
8. Privacy line and the independent-app note.

## Standing rules

- Present tense, current build only. No "coming soon".
- Presets and modules must match the app: `LogPresets.kt` (7 presets) and
  `ModuleSigningProfiles.kt` (11 modules) in the F56coding repo.
- Screenshots still to add: real app, no VIN on screen, status bar cropped.
  Put them in `assets/images/f5xcode/`.
