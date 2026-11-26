# R53 bridge dash screen post: running order

Not a post. The running order for `2026-11-17-r53-bridge-dash-screen.md`, whose page is
`_includes/r53-dash-feature.html`. Add to this structure instead of appending to the end.
Styles are scoped under `.rd`.

Matt asked for it on 2026-11-17 ("push this new setup to our blog, with the screens and sample
photos"): the dash screen (bridge build 381) and the always-on SD log (build 380).

1. **Hero**: what it is, stats (5 pages, 20 Hz, 2 Waveshare screens, always logging).
2. **Render note**: every screen image is a firmware render with simulated data, not a photo. Keep
   this until real photos replace them, and say which are which when they do.
3. **The pages** (`#pages`): the five pages and the no-bridge state, on the 2.8.
4. **Two screens, one firmware** (`#screens`): the gauge page on the 2.1 (the 2.8 render shown
   smaller; same 480 x 480 panel) and the 2.8, then the table (Waveshare part number, panel, what
   it fits, Amazon link) and a line to buy the exact part number.
5. **It logs everything** (`#logging`): raw CAN and K-line, the decoded log, delete-oldest.
6. **Presets** (`#presets`): the four presets and their DME blocks.
7. **Hooking it up** (`#wiring`): one USB-C, the bridge is host, console moves to D4/D5; then
   flashing both from the R53 flasher (build 11): the "drives a dash screen" box, BOOT to reflash
   the bridge, and the screen picker (2.1 or 2.8).
8. **Where it's at** (`#status`): builds and renders, on the flasher as build 11, not yet run on a
   screen in the car; keep build 10 on a bridge you rely on.

## Standing rules

- The bridge firmware repo is private: no GitHub links. The flasher
  (mrblahhhh.github.io/esp32-shift-light-R53-mini) is public and is the way to install it.
  Its build numbers (11) are the flasher's, not the firmware's (381); say which when quoting.
- Numbers come from the firmware: presets and channels from `shared/screenlink/r53screen.h`, the
  SD reserve (256 / 512 MB, 64 MB files) from `sdlog.cpp`, RPM arc colours (5,800 amber, 6,800 red)
  from `screen/src/ui/gauges.cpp`. Change them here when they change there.
- Matt supports two screens only (2026-11-26): Waveshare ESP32-S3-Touch-LCD-2.1
  (amazon.com/dp/B0DDPQSKJD) and ESP32-S3-Touch-LCD-2.8C (amazon.com/dp/B0DMJZPH2R). Don't list the
  AMOLED boards, even though the firmware still has builds for them.
- Brightness for the 2.1 and 2.8 isn't published by Waveshare; don't invent a figure.
- No ordering section: Matt hasn't said the screen is for sale. If it becomes so, orders go to
  ryan3ddesigns@gmail.com like the other kits.

## How the images were made

`assets/images/r53-dash-screen/`: `screen/tools/preview` in the R53 bridge repo renders the real
`screen/src/ui` pages headless against LVGL 9.4 (`r53_preview <dir>`, then `png.py`), with a
simulated third-gear pull held at 11.2 s (5,940 rpm, AFR 12.3, MAP 176 kPa, knock retard -1.5).
Round panels were masked to their circle with transparent corners. `cover.jpg` is the 2.8 gauge
page twice on #0B0D10 at 1200 x 630, at 390 and 520 px for the 2.1 and 2.8 (their size ratio).
