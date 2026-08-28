---
title: "The R53 K-line + CAN bridge — one board that reads the ECU, watches the bus and runs the shift light"
date: 2026-08-27 08:00:00 -0400
categories: car tech
tags: [mini, r53, esp32, esp32-s3, xiao, can-bus, k-line, obd2, l9637d, sn65hvd230, ems2000, datalogger, shift-light, wiring, pcb, kicad, jlcpcb]
cover: /assets/images/r53-kline-bridge/board-pictorial.png
lightbox: true
excerpt: "The shift light grew a second job, then a third. Here is the whole board — transceiver choice, OBD2 pin colours, the two parts that destroy something if you fit them backwards, every wire drawn in colour, and the PCB it all became."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-kline-bridge/board-pictorial.png
---

<!--more-->

The [CAN shift light](/car/tech/2026/07/11/r53-esp32-shift-light.html) was supposed to be a weekend job. Read RPM off the bus, light up eight LEDs, done. It has since grown a second job and then a third, and what is in the footwell now is a small bridge that does all of this at once:

- **Reads the ECU over K-line.** The R53's Siemens EMS2000 speaks DS2 at 9600 baud, 8E1, with B8/XOR framing. The board handles the protocol and ships raw block payloads over BLE, so the phone does the decoding.
- **Listens to the CAN bus.** RPM, throttle, coolant and road speed off the car's own bus, plus AEMnet lambda from the wideband gauge spliced onto the same pair.
- **Drives the shift light.** Same eight WS2812Bs as before, still working with no phone connected and no app running.

It feeds [R53 Logger - Flasher](/car/tech/2026/07/24/r53-logger-play-store.html), which does the logging, the analysis and the flashing. This post is the hardware.

Everything here now lives in its own repo: **[R53_Mini_Kline_Canbus_Logger_Shiftlight](https://github.com/MrBlahhhh/R53_Mini_Kline_Canbus_Logger_Shiftlight)**.

## The whole board in one picture

![Pictorial wiring diagram: CAN transceiver, XIAO ESP32-S3, L9637D front end and the OBD2 plug, every wire in its own colour](/assets/images/r53-kline-bridge/board-pictorial.png)

Every net gets a colour of its own, and the six car-side wires carry the actual colours of the OBD2 pigtail. Where two wires cross, the one on top is drawn with a bridge — a bridge is never a join.

The car sits on the *right* on purpose. The L9637D's car-side pins (6 K, 7 VS) are on its right-hand side and its micro-side pins (1 RX, 3 VCC, 4 TX) on its left, so this is the only arrangement where no wire has to cross the chip.

## The board is a XIAO, not the Waveshare

The shift light ran on a Waveshare ESP32-S3-Zero. The bridge runs on a Seeed **XIAO ESP32-S3**, and that swap is not free.

On the S3-Zero the silkscreen number *is* the GPIO number. On the XIAO it is not: `D3` is GPIO4 and `D8` is GPIO7. Flash the old firmware to a XIAO unchanged and the WS2812B data line comes out on GPIO4 — which is `D3`, the pin this wiring gives to the CAN transceiver's `TXD`. The LED strip ends up driving the transceiver's input, and the symptom is a CAN bus that appears dead.

The build config changes too. The Waveshare part is an ESP32-S3FH4R2 with 4 MB flash and 2 MB *quad* PSRAM, which is why `platformio.ini` forced a generic DevKitC with `qio_qspi`. The XIAO is an ESP32-S3R8: 8 MB flash, 8 MB *octal* PSRAM. Those overrides are actively wrong for it — and there is a real board definition, `board = seeed_xiao_esp32s3`, so the whole override block gets deleted rather than re-tuned.

## CAN comes from OBD2 pins 6 and 14

This surprises people: the R53 does not carry the powertrain bus to the OBD2 socket from the factory. So this is not a tap. The pair is being *run* to the connector and landed on pin 6 (CAN H) and pin 14 (CAN L), which makes the socket the single service point for CAN, 12 V and ground.

![Board-level block diagram: the car, the L9637D front end, the SN65HVD230 module and the XIAO](/assets/images/r53-kline-bridge/board-block-diagram.png)

Transceiver is the usual blue-terminal **SN65HVD230** breakout off the XIAO's `3V3` pad. Two things about it:

- **Take its 120 Ω terminator off.** With the AEM gauge on the same pair the bus already has the car's two ends terminated. Key off, CANH–CANL should read about 60 Ω; 40 Ω means a third terminator is still in circuit.
- **`TXD` and `RXD` go straight across, not crossed.** On the module `TXD` is an input the micro drives and `RXD` is an output. Swapping them is the classic reason a freshly built CAN node hears nothing.

The ESP32 stays in `TWAI_MODE_LISTEN_ONLY`, so it never ACKs and cannot disturb the bus whatever else is going on.

## The K-line front end

![K-line schematic: OBD2 pin 16 through the TVS and 1 k pull-up to the K net, into L9637D pin 6, with the divider back to the XIAO](/assets/images/r53-kline-bridge/kline-schematic.png)

K-line is a single bidirectional wire idling at battery voltage, so it needs a real transceiver rather than a transistor and hope. This uses an ST **L9637D** on a PA0002 SOIC-8 → DIP-8 adapter, which is convenient because DIP pin N is SOIC pin N.

Around it:

- **1 kΩ 1 W** from the 12 V rail to the K net — the pull-up that idles the bus high. One watt, not a quarter; it sits across 12 V whenever the line is pulled low.
- **1.5KE27A TVS** from the 12 V rail to ground, clamping whatever the car throws at it.
- **4.7 kΩ** from 5 V to the L9637D's TX pin, which is open-drain.
- **10 k / 20 k divider** on the way back, because the L9637D's RX output swings to 5 V and the ESP32 wants 3.3 V.
- **100 nF** sat right at pin 3.

The one requirement that drove the transceiver choice: **62500 baud**. That is the programming-session rate, and a bridge that cannot hold it is not worth building — it would log fine and then be useless for flashing. That figure is still on the bench-test list rather than proven.

## The OBD2 pigtail, and reading it properly

The harness is a J1962 male plug on a 16-way flying-lead pigtail. Six wires used, ten cut back.

![OBD2 connector face with all sixteen pins drawn in their wire colours](/assets/images/r53-kline-bridge/obd2-pin-colours.png)

| Pin | Colour | Signal |
|---:|---|---|
| 4 | Orange | chassis ground |
| 5 | Light blue | signal ground |
| 6 | Green | CAN H |
| 7 | Black | K-line |
| 14 | Green / White | CAN L |
| 16 | Red | +12 V permanent live |

Those are the colours on the card that came with this particular pigtail. **J1962 standardises the pin functions and never the wire colours**, so the card is the vendor's claim about their own product, and it cannot tell you whether *your* cable was actually assembled to it.

Two minutes with a meter is worth it on exactly two of them:

- **Pins 6 and 14.** Swapping CAN H and CAN L damages nothing, but it leaves the node deaf — which looks identical to a dead bus, so you would spend an hour debugging the wrong thing.
- **Pin 16.** For the opposite reason: it is permanent battery, not switched, and it is not fused.

Bond both grounds to the board. Pin 4 and pin 5 are joined in the car anyway, and it gives the 12 V side a lower-impedance return.

While you are at it: cut the ten unused leads back to *different* lengths and heat-shrink each individually. A bundle of bare ends all cut to the same length is a short waiting to find the chassis.

## Two orientations will destroy something

Everything else in the BOM fits either way round. These two do not.

**The 1.5KE27A is unidirectional.** Its silver band is the cathode, and that band goes to **+12 V**. Fitted the other way round it conducts on the first key-on and becomes a dead short across the battery. (The `CA` suffix is the bidirectional variant — this is not that part.)

**The L9637D has a pin 1.** It is marked by the dot in the corner of the package, with the notch at the same end. Get it backwards and 12 V lands on the wrong pins.

Resistors and the ceramic cap have no polarity at all, so those are free.

## Where it stands

The design is settled, the parts are in, and the firmware is ported — `board = seeed_xiao_esp32s3`, LED on GPIO2, CAN on GPIO4/GPIO7. It compiles clean. **It has not been flashed to a XIAO yet**, so the first power-up is a bench test, not an install.

One part of that port was not cosmetic, and is worth repeating for anyone doing the same swap. The optional ADS1115 sat on GPIO7 and GPIO8 — which on the XIAO become CAN RXD and the pins reserved for the K-line UART. The I²C bring-up is lazy, so nothing happens until the phone asks for ADS1115 mode; at that moment `Wire.begin(7, 8)` would seize the CAN receive line. The bus would go quiet after a settings change, with nothing in the wiring to blame. Moving it to `D4`/`D5` costs nothing and removes a fault that would have been miserable to find.

The K-line half is specified but not written — the board does CAN and the shift light today. The 62500-baud bench test on the L9637D is the next real milestone, and it is the one that decides whether this can carry a flashing session as well as a logging one. One L9637D is already mounted on an adapter for it, so it does not eat a board from the build stock.

Full write-up of the port, including the checklist that is still open: [PORTING.md](https://github.com/MrBlahhhh/R53_Mini_Kline_Canbus_Logger_Shiftlight/blob/main/docs/PORTING.md).

## It is a real board now

The wiring above describes a thing built on protoboard. It is also a PCB — 90 × 64 mm, four layers, routed clean.

![3D render of the carrier board](/assets/images/r53-kline-bridge/pcb-3d.png)

The XIAO drops into the two sockets in the middle. The car harness screws into the green block along the bottom, the 5 V module stands in its own three-way socket top left, and the eight yellow pads under the module are test points — every net you would want a scope on, brought somewhere you can reach it with the module fitted.

![Top view, routed](/assets/images/r53-kline-bridge/pcb-top.png)

Everything surface-mount is on the top side, in packages nothing exotic — SOIC-8, SOT-353, SOT-223, SOT-23, 0805, 1206, 1210, SMA, SMC. Nothing leadless, nothing on the bottom. That started as a constraint for reflowing it on a hotplate and turned out to be the right shape for machine assembly too.

The through-hole parts are deliberately *not* on the assembly BOM: the two XIAO sockets, the screw terminal, the two headers and the 5 V module's socket are five easy hand-solder joints on a board that arrives otherwise finished.

### The schematic

Four sheets. The netlist is written in Python and the KiCad files are build outputs — `python gen/build_board.py` places, routes, stitches the planes and tidies the silkscreen from nothing.

![Harness and rails](/assets/images/r53-kline-bridge/sch-power.png)

Fuse, then clamp, then module, in that order. Pin 16 is permanent live and the car does not fuse it, so the polyfuse is there to stop the OBD2 wire becoming the fusible link no matter what the TVS decides to do.

![K-line front end](/assets/images/r53-kline-bridge/sch-kline.png)

Note the two 1 kΩ pull-up positions in parallel. The second is not fitted. If 62500 baud will not frame cleanly the fix is a stiffer pull-up, and 500 Ω should cost a soldering iron rather than a board respin.

![CAN](/assets/images/r53-kline-bridge/sch-can.png)

![XIAO sockets and shift light](/assets/images/r53-kline-bridge/sch-mcu.png)

### It is 12 V automotive, which is not the same as 12 V

Two things were missing and both are the kind that only bite once.

**Reverse polarity.** The harness lands on a screw terminal. Red and black are one slip apart, in a footwell, probably in the dark — and the only thing between that and the switching module was a polyfuse, which is a thermal device that takes *seconds*. There is now an SS34 Schottky between the fuse and the clamp, so reverse voltage never reaches the TVS and a backwards harness is simply an open circuit rather than a short. It costs 0.4 V against four volts of headroom.

**The CAN pair had no clamp at all.** CANH and CANL leave the box and run the length of the car. One PESD1CAN — two TVS sharing a middle ground pin — covers both lines in a single SOT-23.

One thing I have left as a known limitation rather than pretend away: the module needs 8 V in, and cranking pulls a tired battery below that. The board will reset on every start. Not damage, but you lose the log across exactly the moment you might want it, and fixing it properly means a buck-boost rather than a buck.

### Six spare pins do not fit three ideas

The expansion header brings out 3.3 V, 5 V, ground and the six XIAO pins the board does not use. Worth doing the arithmetic before planning what goes on it:

| | bus | pins |
|---|---|---:|
| microSD | SPI | 4 |
| GPS | UART | 2 |
| 9-axis IMU | I²C | 2 |
| | **wanted** | **8** |
| | **available** | **6** |

An SD card and a UART GPS do not coexist. An **I²C** GPS shares the IMU's two pins and all three fit exactly.

There is also an AMS1117-3.3 footprint on the board, not fitted, with a solder jumper that chooses where the header's 3.3 V comes from — bridged to the XIAO as made. It is there because the XIAO's own regulator is already carrying the ESP32-S3 and the CAN transceiver. That is fine for an IMU. An SD card mid-write plus a GPS acquiring, on top of a WiFi burst, is asking too much of it.

And nine drilled 1 mm pads on the rails — three 3V3, four ground, two 5 V — because a flying lead soldered into a hole survives a car and a jumper wire pushed onto a header pin does not.

### Two things the tooling caught that I would have missed

**The TVS is drawn as a Zener.** KiCad has no unidirectional-TVS symbol, and `Device:D_TVS` is the bidirectional one — which reads identically either way round. This part does not: reversed, it is a forward diode across the battery and it conducts on the first key-on. Drawing it with a cathode means the polarity audit can check it, and it does.

**The 5 V module's footprint was wrong by the size of the component.** U1 is a socket, and a stock `PinSocket_1x03` footprint describes the socket — 3.6 × 8.7 mm. The thing that actually occupies that space is the module standing on it, 11.5 × 7.6. So DRC was happy to let five parts sit *underneath the module*, and said nothing, because it is a 3D clearance against a part that is not on the board. The footprint now carries the module's body as its courtyard, which turns "remember this" into a check the board makes every time it is rebuilt.

### Where the XIAO's dimensions came from

The socket row spacing is not in any spec table Seeed publish. It came out of **their own KiCad PCB file** for the part, opened with `pcbnew` and asked for its through-hole pads:

```
column x =  1.260   7 pads, 2.54 mm pitch   (pins 1-7)
column x = 16.500   7 pads, 2.54 mm pitch   (pins 8-14)

ROW SPACING = 15.240 mm, exactly 0.600"
```

The two further columns in that file, at x = −0.010 and x = 17.770, are the castellated half-holes on the board edges. A footprint built from those would be 2.5 mm too wide, and would look perfectly reasonable right up until the module would not go in.

The same file answered the antenna question. `ANT1` on a XIAO is a **U.FL connector**, not a PCB antenna — so the module radiates from a pigtail, copper underneath detunes nothing, and the ground plane stays whole under the CAN pair, which is where it is actually wanted.

### The assembly BOM was wrong in both directions

This one is worth the warning. The fab files were generated by a script forked from an earlier board, and that script carried a hardcoded list of parts to leave off — `{"C3", "C6", "C7"}`. Those are *reference designators*. On the earlier board they were aluminium electrolytics the assembler could not source. On this board C3 is the 5 V module's output bulk and C6 is the CAN transceiver's bypass.

Carried over unread, it silently dropped two capacitors that must be fitted. The boards would have arrived built, looking entirely correct.

It was also including three parts the design says to leave *off*: a second 1 kΩ that would have made the K-line pull-up 500 Ω, and two 62 Ω that would have put ~124 Ω across a CAN bus the car already terminates at both ends. That board would have come back measuring 40 Ω where the documentation says expect 60, and nothing would have looked wrong.

Both directions are now driven off the netlist note rather than a designator list, so it cannot rot the same way. The lesson generalises: **a hardcoded list of reference designators is a landmine in any file you fork.**

Board files, gerbers and a BOM where every line carries an in-stock LCSC part number: [hardware/pcb](https://github.com/MrBlahhhh/R53_Mini_Kline_Canbus_Logger_Shiftlight/tree/main/hardware/pcb). Not manufactured yet — boards are on order.

Repo, with the full interactive wiring reference and every diagram above:
**[github.com/MrBlahhhh/R53_Mini_Kline_Canbus_Logger_Shiftlight](https://github.com/MrBlahhhh/R53_Mini_Kline_Canbus_Logger_Shiftlight)**
