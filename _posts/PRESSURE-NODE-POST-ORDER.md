# Pressure CAN node post: running order

Not a post. The running order for `2026-11-16-pressure-can-node.md`, whose page is
`_includes/pressure-node-feature.html`. Add to this structure instead of appending to the end.

1. **Hero**: what it is and what it does, four stats (6 pressure channels, 8 analog inputs at
   1 MΩ, 100 Hz on CAN in a layout set from the phone, 12 V or USB), the angled render.
2. **Capabilities** (`#capabilities`): one table, a line each for pressure, barometer, analog
   inputs, sensor supply, conversions, CAN, phone app, screen, power, controls, processor,
   connectors and size.
3. **What it measures** (`#measure`): each sensor's one barb read against the air in the node's
   box, the 10 kPa and 40 kPa parts, the barometer reading that air, then six tiles: aero, airbox
   and turbo inlet, cowl and radiator, the eight analog inputs, barometer and box temperature,
   supply and battery.
4. **The board** (`#board`): the labelled top view (render with leader lines to a label for each
   section, nine on the left, ten on the right), link to the full-size render, the sensor close-up.
   The tour is marked `<!-- TOUR: parent redraws after routing -->` until it is redrawn on a rev F
   render; its labels and leader lines still sit on the old render.
5. **How it works** (`#how`): power, sensor supply, supply sense, pressure, barometer, analog
   inputs, thermistors and thermocouples, links.
6. **On the CAN bus** (`#can`): the default frames table (0x520 to 0x526), the ±32.7 kPa note on
   the default pressure fields, then "Your own layout": what the app can set (bus, frames, signals,
   values, conversions, zero command, checks, DBC), the four presets and the terminator.
7. **The phone app** (`#app`): four tiles: live, graph, zero and record, setup.
8. **An optional screen** (`#screen`): one paragraph on the round touch screen on USB-C.
9. **Fitting it to a car** (`#install`): the car diagram (six channels numbered by sensor: pitot,
   front floor, diffuser, cowl, and the airbox either side of the filter; hoses to the node in the
   cabin; the pitot's static line onto the node's sealed box), four tap close-ups (drilled, tape-on
   pad, pitot-static, hose end), the plumbing table (sensor, range, port, reads), then tiles for the
   reference (sealed box on the static line, open box in the cabin, two channels for a drop, bottle,
   each saying what the barometer adds), hoses and mounting, the five test steps, what the numbers
   look like (the intake tile links the F56 CFD post), what race teams do, and the Sources list.
   "What it measures" links here, and so do the F56 and R53 intake CFD posts.
10. **Schematic** (`#schematic`): legend, the overview block diagram (blocks link to their sheets),
    sheet links, then sheets 1 power input and buck, 2 5 V OR and 3.3 V, 3 sensor 5 V supply and
    supply sense, 4 ESP32-S3 with USB and buttons, 5 sensor bus and barometer, 5a one sensor branch,
    6 one analog input with the AIN1–8 parts table under it, 7 CAN, and 8 the ESP32 pin map with the
    test pads.
11. **Last line**: these are renders, the fitting section is the plan, and the board hasn't been
    built.

## Standing rules

- Rev F as of 2026-11-24: 89 × 70 mm, four layers, every part on top. Six WF100DPZ single-port
  gauge sensors in a 3 × 2 grid: DP1–4 0.1BG (10 kPa), DP5–6 0.4BG (40 kPa). BMP388 barometer on
  the main I²C bus at 0x76. Eight analog inputs, AIN5–8 on a second ADS1115. Sensor 5 V from a
  TLE4252D tracker on the protected 12 V, following an LM4040 5.000 V reference; GPIO1/GPIO2 read
  it and the 12 V, GPIO18 is free. LM66100 OR in TI's continuous-output wiring, D6 feeding a screen
  on USB-C, UART pads, no UART header. Keep the schematic, the tour and the text in step with the
  board files.
- The page describes the current board only. No revision history or reasons for changes.
- Nothing has been built or run on a car. Don't claim measured accuracy, rates or results until
  Matt reports a real board. He has bench-checked that the WF100DPZ reads below zero as well as
  above (2026-11-17); the counts-to-Pa scale still needs a manometer check. Datasheet figures are
  fine as datasheet figures.
- The board's repository is private. Don't link it or name its scripts.
- The fitting section is published practice in our own words, and it ends with a linked Sources
  list (`#install-sources`): every article, paper and thread it draws on, what each supports, PDFs
  marked and linked directly. Matt, 2026-11-17: "If we use sources like that, I want a link to the
  PDF on the blog." Link text is the real title where we have it, otherwise a plain description.
  No third-party photos; the diagrams are drawn in the include. When a board is fitted to a car,
  swap in photos and real readings.
- Numbers on the page: 10 kPa × 4 and 40 kPa × 2, 8 × 0–5 V with ÷5 dividers (1 MΩ, 4.7 nF,
  about 210 Hz; a 16 V fault reaches the switch as 3.2 V), barometer 30–125 kPa. Default CAN: seven
  frames from 0x520 at 500 kbit/s, 0x520–0x524 every 10 ms, 0x525 and 0x526 every 100 ms; up to 16
  frames of the user's own from the app. 1 A fuse, 33 V TVS. Sensor supply 5.000 V, about 120 mA
  continuous shared by all eight connectors (117 mA at 14.4 V in a 140 °F box), survives shorts to
  ground and to battery. Buck budget 0.49 A, 0.74 A with a screen.
- Temperatures in °F first, °C in brackets where it helps (Matt uses Fahrenheit). CAN and protocol
  units stay as sent (°C, Pa, mV).
- Schematic sheets are inline SVG in the include, using the legend's classes: red rails, green
  signals, a dot on every junction, a hop where wires cross, tags for nets that leave the sheet,
  each part with its reference and value and each IC pin with its number. On 2026-11-24 every drawn
  connection, value and pin number was checked against the netlist exported from the rev F
  schematic; check again whenever the circuit changes.

## Images

All in `assets/images/pressure-node/`. They are to be re-rendered from the rev F board files once
its routing is finished; until then the files in place are the 2026-11-17 renders of the earlier
79 × 70 mm board, so their captions and alt text describe rev F in general terms. Rendered with
KiCad 10's raytracer: red mask, white silk, HAL finish. KiCad's library has no model for the
WF100DPZ, the XKB TS-1187A switch or the HRO USB-C, so those are simple stand-ins: a 7 × 7 mm black
body with six leads and one grey barb in the middle (9.5 mm tall), a 5.1 mm switch, and a plain
USB-C shell.

- `hero.jpg`: perspective, rotated −58° about X and 20° about Z.
- `sensors.jpg`: perspective close-up of the 3 × 2 sensor grid from the front edge, rotated −55°
  about X, cropped above the sensor silk line.
- `top.jpg`: straight down, orthographic. The labelled tour places its leader lines from part
  centres in mm, using the board outline measured in this image (17.8 px/mm at 1600 px wide on
  the old board). Labels on each side are ordered so no two leader lines cross.
- `cover.jpg`: the hero render cropped to 16:9.

Replace the renders with photos of a built board when Matt has one.
