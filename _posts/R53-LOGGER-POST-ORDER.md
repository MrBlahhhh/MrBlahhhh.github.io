# R53 Logger - Flasher page: running order

Not a post. The page body lives in `_includes/r53-logger-feature.html` and is
rendered by two posts:

- `2026-08-18-r53-logger-august-update.md`, the feature page.
- `2026-07-22-r53-android-datalogger.md`, a mirror so old links land on the
  same text.

Both posts are front matter plus one include line. Edit the include, never the
posts, or the two drift apart. Keep the two front matters identical apart from
`date`.

The look copies the Touge radio flasher page (`touge-mesh-firmware-pages`):
dark velvet background with an accent glow, Atkinson Hyperlegible, translucent
rounded cards, numbered step circles. The accent is the app's chili red. All
styles are scoped under `.r53`.

## Running order

Sales order: the hook first, setup and prices last.

1. **Hero**: what it is, the "this week" line, stats strip.
2. **Stage 1 flash**: MINI's JCW / GP1 as the factory Stage 1. What the car
   needs (pulley, 380 cc injectors, exhaust or header; no head needed), early
   and late ECUs, the Send path for unmapped versions, honest limits against
   a custom tune.
3. **Stage 0 flash**: BMW's early 200 hp JCW kit software for a 740G00 Cooper
   S with a pulley on stock injectors, in plain words: what it is, what it
   changes, what it leaves alone, Stage 0 or Stage 1.
4. **Log it**: fast aligned mode, autolog, the graph, the video.
5. **See the tune**: AFR 3D, fuel and trims maps, spark map with knock,
   Ign 3D.
6. **Close the loop**: fuel recommendation and the simulator.
7. **Flash options**: JCW and GP1 (Stage 1), Stage 0, Stock S, the twelve
   options and a Dashboards tile, as tiles.
8. **Why it's safe to flash**: backup, tune check, wrong-family refusal.
9. **Garage**.
10. **Diagnostics**.
11. **What you need**: the K+DCAN cable and a USB-C OTG adapter as the best
    setup (tiles with Amazon links), the pin 7/8 switch, the OBDLink and the
    bridge as the other wires, supported ECUs as tiles.
12. **What it costs**.
13. **Get it**: one Google Play button. It's on Play for everyone since
    2026-10-11; no test group.

## Layout rules

- Phone shots go in `row` / `row flip`, alternating down the page. Tablet
  shots go in `row full` (figure full width, text in two columns below).
- One figure per row. `pair` is the exception for two phone maps side by side.
- `grid3` tiles for sets of three or more short things.
- Every section opens with a `<p class="lead">` that states the argument.
- New screenshots go in `assets/images/r53-logger/v2/`, dark theme, status and
  nav bars cropped off.

## Standing content rules

- Present tense, current build only. No bug history, no "coming soon".
- Every screenshot is the real app on real data.
- No customer names, VINs or file names on screen.
- Prices come from Play Console, not memory.

## Still to shoot (needs the car)

- Main screen live, engine running with the wideband: the hero and cover.
- Diagnostics with codes read in plain English.
- Garage with this car's own bins and logs (the current image is the August
  one).
