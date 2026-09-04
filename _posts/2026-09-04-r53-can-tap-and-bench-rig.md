---
title: "Two yellow wires and a bench rig: tapping the R53's CAN bus, and testing the board before it goes in the car"
date: 2026-09-04 09:00:00 -0400
categories: car tech
tags: [mini, r53, esp32, esp32-c3, can-bus, wiring, wire-colours, instrument-cluster, shift-light, bench-testing, twai, kicad, pcb]
cover: /assets/images/r53-can-bench/shift-light-board.png
lightbox: true
excerpt: "The shift light needs exactly two wires off the car. Finding them means reading BMW's wire colour code, which is in German and not obvious. Then there is the part that wastes an evening: a listen-only board on a two-node bench bus hears nothing, and both boards look perfectly alive while it happens."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-can-bench/shift-light-board.png
---

<!--more-->

The [shift light](/car/tech/2026/07/11/r53-esp32-shift-light.html) needs two wires off the car and nothing else. CAN high, CAN low, and it reads engine speed off the bus the car is already talking on. No splicing into the ECU, no tapping a coil, no extra sensor.

Finding those two wires is the whole job, and the factory wiring diagrams tell you in a code that is not obvious if you have not seen it before.

## Reading BMW's wire colours

Every wire on a BMW or MINI diagram is labelled with an abbreviation of its colour in German, and multi-colour wires get the base colour first and the stripe after. Once you know the nine that matter, the diagrams read straight off.

| code | German | wire |
|---|---|---|
| SW | schwarz | black |
| BR | braun | brown |
| RT | rot | red |
| GE | gelb | yellow |
| GN | grün | green |
| BL | blau | blue |
| WS | weiß | white |
| GR | grau | grey |
| VI | violett | violet |

So `GE/SW` is a yellow wire with a black stripe. `WS/RT/GE` is white with red and yellow. The number under the colour on a diagram is the conductor size in square millimetres, which is how you tell a signal wire from something carrying current: 0.35 and 0.5 are signals, 0.75 and up are feeds.

## The two wires

On the instrument cluster, the CAN pair is:

| signal | colour | what it is |
|---|---|---|
| CAN_H | yellow / black | 0.5 mm², the dominant-high line |
| CAN_L | yellow / brown | 0.5 mm², the dominant-low line |

They arrive at the cluster as a pair and leave it as a pair, which is what makes the cluster a good place to tap. The wires are already bundled, already the right pair, and the connector is behind a panel you can get to without dropping the dash.

Two other lines on the same connector are worth knowing about because they look like candidates and are not:

- **K-BUS II**, white with red and yellow, 0.35. This is the body bus, not the powertrain bus. It carries door, light and climate traffic. Engine speed is not on it.
- **DIAG**, grey with white, 0.35. The diagnostic line to the OBD socket. Also not where RPM lives.

Buzz the pair before you cut anything. The function of a wire is fixed across the model and the colour of the wire is not always, and a swapped pair leaves the node deaf in a way that looks exactly like a dead board.

## Then you want to know it works before it goes in the car

Twenty boards went out this week. Every one of them got bench tested first, and the rig for that is two boards and a foot of wire.

Use a spare carrier as the transmitter. It already has the transceiver, the terminal and the right pin map, so it costs nothing but a flash.

```
   transmitter                 board under test
   CANH  ────────────────────  CANH
   CANL  ────────────────────  CANL
```

## The part that wastes an evening

The shift light runs its CAN controller in **listen-only** mode. That is deliberate: a device spliced onto a car's powertrain bus should not be able to talk on it, ever, no matter what the firmware does.

Listen-only also means it never acknowledges a frame.

On the car that is fine. Every other module on the bus acknowledges, and the transmitter is happy. On a two-node bench bus there is nobody to acknowledge, so a normal transmitter sends its frame, waits for an ACK that never comes, retries, retries again, climbs into error-passive, and stops. Meanwhile both boards are powered, both have healthy 5 V rails, and the LED strip sits dark.

Nothing about that looks like a bus problem. It looks like a firmware bug in the receiver, which is where you will spend the evening if you do not know.

The fix is one line. On an ESP32 the TWAI controller has a no-ACK mode that transmits without requiring the acknowledgement, and that is what a bench transmitter has to use:

```c
twai_general_config_t g = TWAI_GENERAL_CONFIG_DEFAULT(
    tx_pin, rx_pin, TWAI_MODE_NO_ACK);
```

## Termination, which is the other half

CAN wants 120 Ω at each end of the bus and nothing in the middle. In the car both ends are already terminated inside modules, which is why the shift light ships with its termination **not** fitted. Adding a third resistor to a terminated bus drags it down.

On the bench there are only two nodes and neither is terminated, so both ends need it. The carrier has a 120 Ω resistor on board behind a solder jumper for exactly this. Bridge it on both boards, and you are back to 60 Ω across the pair, which is what the transceivers expect to see.

An unterminated bench bus works right up until it does not, usually the moment you add a longer wire, and the symptom is intermittent frames. Same evening, different cause.

## What the sweep proves

The transmitter sends the engine speed frame at 50 Hz with a value that ramps from idle to past redline and back over twelve seconds. Watching that run tells you four things in one go:

1. The buck converter, the module and the 5 V rail work, because the board is awake.
2. The transceiver works, because frames are arriving.
3. The strip and its data line work, because the stages walk up in order.
4. The shift point is where the settings say it is, because the flash happens at the right moment in the ramp.

Unplug CAN high while it is running and the strip should go dark within a second rather than freezing at the last engine speed it saw. That behaviour is deliberate. A stale reading displayed as a live one is worse than no reading, because the one thing a shift light must never do is tell you 4000 rpm while the engine is at 7000.

## Test the strip on its own first

Build the firmware with the simulate flag and it drives the strip from an internal ramp, ignoring CAN entirely. That proves the power path, the module, the data pin and the strip with no bus involved at all.

Do that first and the CAN rig is only testing the transceiver and the wiring, which is a much smaller thing to debug when it goes wrong.
