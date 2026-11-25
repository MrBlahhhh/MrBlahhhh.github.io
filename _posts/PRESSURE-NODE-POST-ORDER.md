# Pressure CAN node post: running order

Not a post. The running order for `2026-11-16-pressure-can-node.md`, whose page is
`_includes/pressure-node-feature.html`. Add to this structure instead of appending to the end.

1. **Hero**: what it is and what it does (the signal outputs in its last sentence), five stats
   (6 pressure channels, 8 analog inputs at 1 MΩ, 8 signal outputs, 100 Hz on CAN in a layout set
   from the phone, 12 V or USB), the angled render.
2. **Capabilities** (`#capabilities`): one table, a line each for pressure, barometer, analog
   inputs, sensor supply, conversions, CAN, phone app, outputs, screen, power, controls,
   processor, connectors and size.
3. **What it measures** (`#measure`): each sensor's one barb read against the air in the node's
   box, the 10 kPa and 40 kPa parts, the barometer reading that air, then six tiles: aero, airbox
   and turbo inlet, cowl and radiator, the eight analog inputs, barometer and box temperature,
   supply and battery.
4. **The board** (`#board`): the labelled top view (top.jpg with leader lines to a label for each
   section, twelve on the left, ten on the right), link to the full-size render, the sensor
   close-up. The tour is generated from the board's placement table (part centres in mm) with the
   top.jpg mapping below; labels on each side are ordered so no two leader lines cross and no line
   passes over another label's dot.
5. **How it works** (`#how`): power, sensor supply, supply sense, pressure, barometer, analog
   inputs, thermistors and thermocouples, signal outputs, outputs off by default (with the intended
   control: CAN command, app rule, by hand; firmware not written yet), links.
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
    6 one analog input with the AIN1–8 parts table under it, 7 CAN, 8 signal outputs with the
    per-output table under it, and 9 the ESP32 pin map with the test pads.
11. **Last line**: these are renders, the fitting section is the plan, and the board hasn't been
    built.

## Latest revision

Rev H keeps the 89 × 80 mm layout and four nominal 12 V PWM controller
signals on J13 and four 12 V relay-coil outputs on J14. Both banks use the
separate fused J15 supply. The planned coil load is 200 mA per output and
800 mA for the group. Pump power is external. The new schematic image
shows the two driver banks, and the GPIO table gives their pin mapping.
J1 now matches the right-edge JSTs; U23 is the BMP384.
The LM74700-Q1 and Q1 now protect the J15 feed. The bidirectional TVS
clamps the fused input, and the new output power schematic shows the stage.

The Rev F review notes below describe the previous design.

## Standing rules

- Rev F as of 2026-11-24: 89 × 80 mm, four layers, every part on top. Six WF100DPZ single-port
  gauge sensors in a 3 × 2 grid: DP1–4 0.1BG (10 kPa), DP5–6 0.4BG (40 kPa). BMP388 barometer on
  the main I²C bus at 0x76. Eight analog inputs, AIN5–8 on a second ADS1115. Sensor 5 V from a
  TLE4252D tracker on the protected 12 V, following an LM4040 5.000 V reference; GPIO1/GPIO2 read
  it and the 12 V, GPIO18 is free. Eight 0/5 V signal outputs: GPIO38–35 (PWM1–4, J13) and
  GPIO42–39 (OUT1–4, J14) through an SN74HCT541 (U24) on 5V SYS, 1k series, 10k and 100k
  pull-downs, a PESD1CAN per pair. LM66100 OR in TI's continuous-output wiring, D6 feeding a screen
  on USB-C, UART pads (TP19/TP20, now on the left edge), no UART header. Keep the schematic, the tour and the text in step with the
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
  ground and to battery. Buck budget 0.49 A, 0.74 A with a screen. Outputs: 0/5 V push-pull
  signals, not load drivers (Matt: no high-current outputs planned), receiving input above about
  2 kΩ, about 4.95 V high unloaded, a 12 V short pushes about 6.5 mA back, off by default.
- The output firmware isn't written. Describe the intended control (CAN command, a rule set in the
  app, by hand from the app) without claiming it runs, and don't give a CAN ID for it.
- Temperatures in °F first, °C in brackets where it helps (Matt uses Fahrenheit). CAN and protocol
  units stay as sent (°C, Pa, mV).
- Schematic sheets are inline SVG in the include, using the legend's classes: red rails, green
  signals, a dot on every junction, a hop where wires cross, tags for nets that leave the sheet,
  each part with its reference and value and each IC pin with its number. On 2026-11-24 every drawn
  connection, value and pin number was checked against the netlist exported from the rev F
  schematic; check again whenever the circuit changes.

## Images

The Rev H board renders are in `assets/images/pressure-node/`. KiCad 10
rendered the routed 89 × 80 mm board with red mask, white silkscreen and
HAL finish. Stand-in models represent the WF100DPZ sensors, switches, HRO
USB-C, BMP384, LM5164 buck converter and JST PH output power connector.

- `hero.jpg` (1568 × 872): perspective from the front left.
- `sensors.jpg` (1600 × 477): close-up of the 3 × 2 sensor grid, with the
  output drivers and the PWM and relay connectors behind it.
- `top.jpg` (1568 × 1432): straight down, orthographic. Board mm (x right, y down, origin at the
  board's top-left corner) to image px: px = 89 + 15.596 x, py = 92 + 15.6 y, so 15.6 px/mm at
  1568 px wide. The labelled tour places its leader lines from part centres with this mapping.
- `cover.jpg` (1568 × 882): the angled render fitted to 16:9.

Replace the renders with photos of a built board when Matt has one.
