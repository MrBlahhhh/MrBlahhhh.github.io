---
title: "The R53 K-line + CAN bridge — one board that reads the ECU, watches the bus and runs the shift light"
date: 2026-08-27 08:00:00 -0400
categories: car tech
tags: [mini, r53, esp32, esp32-s3, xiao, can-bus, k-line, obd2, l9637d, sn65hvd230, ems2000, datalogger, shift-light, wiring]
cover: /assets/images/r53-kline-bridge/board-pictorial.png
lightbox: true
excerpt: "The shift light grew a second job, then a third. Here is the whole board — transceiver choice, OBD2 pin colours, the two parts that destroy something if you fit them backwards, and every wire drawn in colour."
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

Repo, with the full interactive wiring reference and every diagram above:
**[github.com/MrBlahhhh/R53_Mini_Kline_Canbus_Logger_Shiftlight](https://github.com/MrBlahhhh/R53_Mini_Kline_Canbus_Logger_Shiftlight)**
