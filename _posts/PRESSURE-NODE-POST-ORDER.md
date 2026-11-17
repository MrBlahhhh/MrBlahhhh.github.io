# Pressure CAN node post: running order

Not a post. The running order for `2026-11-16-pressure-can-node.md`, whose page is
`_includes/pressure-node-feature.html`. Add to this structure instead of appending to the end.

1. **Hero**: what it is and what it does, four stats (6 pressure channels, 8 analog inputs,
   100 Hz on CAN, 12 V or USB), the angled render.
2. **What it measures** (`#measure`): each sensor's one barb read against the air in the node's
   box, the 10 kPa and 40 kPa parts, then four tiles: aero, airbox and turbo inlet, cowl and
   radiator, the eight analog inputs.
3. **The board** (`#board`): the labelled top view (render with leader lines to a label for each
   section, nine on the left, ten on the right), link to the full-size render, the sensor close-up.
4. **How it works** (`#how`): power, pressure, analog inputs, sensor supply, then the CAN frame
   table (0x520 to 0x525).
5. **Fitting it to a car** (`#install`): the car diagram (six channels numbered by sensor: pitot,
   front floor, diffuser, cowl, and the airbox either side of the filter; hoses to the node in the
   cabin; the pitot's static line onto the node's sealed box), four tap close-ups (drilled, tape-on
   pad, pitot-static, hose end), the plumbing table (sensor, range, port, reads), then tiles for the
   reference (sealed box on the static line, open box in the cabin, two channels for a drop, bottle),
   hoses and mounting, the five test steps, what the numbers look like (the intake tile links the
   F56 CFD post), what race teams do, and the Sources list. "What it measures" links here, and so
   do the F56 and R53 intake CFD posts.
6. **Schematic** (`#schematic`): legend, the overview block diagram (blocks link to their sheets),
   sheet links, sheets 1 to 6 with 4a, the ESP32 pin map.
7. **Last line**: these are renders, the fitting section is the plan, and rev E hasn't been built.

## Standing rules

- Rev E as of 2026-11-17: 79 × 70 mm, four layers, every part on top. Six WF100DPZ single-port
  gauge sensors in a 3 × 2 grid: DP1–4 0.1BG (10 kPa), DP5–6 0.4BG (40 kPa). Eight analog inputs,
  AIN5–8 on a second ADS1115 in the top right. LM66100 OR in TI's continuous-output wiring, logo on
  the back only, UART pads, no UART header. Keep the schematic, the tour and the text in step with
  the board files.
- The page describes the current board only. No revision history or reasons for changes.
- Nothing has been built or run on a car. Don't claim measured accuracy, rates or results until
  Matt reports a real board. He has bench-checked that the WF100DPZ reads below zero as well as
  above (2026-11-17); the counts-to-Pa scale still needs a manometer check.
- The board's repository is private. Don't link it or name its scripts.
- The fitting section is published practice in our own words, and it ends with a linked Sources
  list (`#install-sources`): every article, paper and thread it draws on, what each supports, PDFs
  marked and linked directly. Matt, 2026-11-17: "If we use sources like that, I want a link to the
  PDF on the blog." Link text is the real title where we have it, otherwise a plain description.
  No third-party photos; the diagrams are drawn in the include. When a board is fitted to a car,
  swap in photos and real readings.
- Numbers on the page: 10 kPa × 4 and 40 kPa × 2, 8 × 0–5 V with ÷5 dividers (500 kΩ load),
  five frames every 10 ms and a status frame every 100 ms at 500 kbit/s from 0x520, 1 A fuse, 33 V
  TVS, 500 mA sensor supply shared by all eight connectors.

## Images

All in `assets/images/pressure-node/`, rendered on 2026-11-17 from the rev E board files with
KiCad 10's raytracer: red mask, white silk, HAL finish. KiCad's library has no model for the
WF100DPZ, the XKB TS-1187A switch or the HRO USB-C, so those are simple stand-ins: a 7 × 7 mm black
body with six leads and one grey barb in the middle (9.5 mm tall), a 5.1 mm switch, and a plain
USB-C shell.

- `hero.jpg`: perspective, rotated −58° about X and 20° about Z.
- `sensors.jpg`: perspective close-up of the 3 × 2 sensor grid from the front edge, rotated −55°
  about X, cropped above the sensor silk line.
- `top.jpg`: straight down, orthographic. The labelled tour places its leader lines from part
  centres in mm, using the board outline measured in this image (17.8 px/mm at 1600 px wide).
  Labels on each side are ordered so no two leader lines cross.
- `cover.jpg`: the hero render cropped to 16:9.

Replace the renders with photos of a built board when Matt has one.
