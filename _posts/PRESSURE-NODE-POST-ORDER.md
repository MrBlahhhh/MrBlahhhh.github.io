# Pressure CAN node post: running order

Not a post. The running order for `2026-11-16-pressure-can-node.md`, whose page is
`_includes/pressure-node-feature.html`. Add to this structure instead of appending to the end.

1. **Hero**: what it is and what it does, four stats (6 pressure channels, 4 analog inputs,
   100 Hz on CAN, 12 V or USB), the angled render.
2. **What it measures** (`#measure`): how a differential sensor is plumbed, then four tiles:
   aero, airbox and filter, cowl and radiator, the analog inputs.
3. **The board** (`#board`): the labelled top view (render with leader lines to a label for each
   section, eight on the left, nine on the right), link to the full-size render, the sensor close-up.
4. **How it works** (`#how`): power, pressure, analog inputs, sensor supply, then the CAN frame
   table (0x520 to 0x523).
5. **Schematic** (`#schematic`): legend, the overview block diagram (blocks link to their sheets),
   sheet links, sheets 1 to 6 with 4a, the ESP32 pin map.
6. **Last line**: these are renders and rev C hasn't been built.

## Standing rules

- Rev C as of 2026-11-16: buck resistors 47k/150k, KT-0805G status LED, J2 (DEBUG) not
  fitted, TCAN1042HGVDR. Keep the schematic, the tour and the text in step with the board files.
- Nothing has been built or run on a car. Don't claim measured accuracy, rates or results until
  Matt reports a real board; the sensor scale and port sign still need a bench check.
- The board's repository is private. Don't link it or name its scripts.
- Numbers on the page: ±10 kPa, 4 × 0–5 V with ÷5 dividers (500 kΩ load), four frames every
  10 ms at 500 kbit/s from 0x520, 1 A fuse, 33 V TVS, 500 mA sensor supply.

## Images

All in `assets/images/pressure-node/`, rendered on 2026-11-16 from the rev C board files with
KiCad 10's raytracer: red mask, white silk, HAL finish. KiCad's library has no model for the
WF200DPZ, the XKB TS-1187A switch or the HRO USB-C, so those are simple stand-ins: a SOIC-16W
body with two grey barbs at the P1/P2 positions (10.3 mm tall), a 5.1 mm switch, and a plain
USB-C shell. J2 is left bare because it isn't fitted.

- `hero.jpg`: perspective, rotated −58° about X and 20° about Z.
- `sensors.jpg`: perspective close-up of the sensor row.
- `top.jpg`: straight down, orthographic. The labelled tour places its leader lines from part
  centres in mm, using the board outline measured in this image (13.2 px/mm at 1600 px wide).
- `cover.jpg`: the hero render cropped to 16:9.

Replace the renders with photos of a built board when Matt has one.
