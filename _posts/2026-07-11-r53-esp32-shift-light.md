---
title: "ESP32 CAN Bus Shift Light for the R53"
date: 2026-07-11 08:00:00 -0400
categories: car tech
tags: [esp32, can-bus, ws2812b, fastled, mini, r53, shift-light, bluetooth, android, 3d-printing, cults3d]
cover: /assets/images/r53-shift-light/shift-light-installed.jpg
lightbox: true
excerpt: "CAN bus RPM to an 8-LED WS2812B bar — a shift light for the price of a track-day lunch"
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-shift-light/shift-light-installed.jpg
---

<!--more-->

The R53 has a lot of charm, but a track-friendly tach is not part of it. The column pod is small, and mid-corner with your eyes up where they belong, it might as well not exist. Aftermarket shift lights either cost real money or want a dedicated tach signal wired across the dash.

So I built my own: an **8-LED WS2812B bar** in a 3D-printed housing on the column shroud, driven by an ESP32 reading RPM straight off the **CAN bus**. Source is on GitHub: [esp32-shift-light-R53-mini](https://github.com/MrBlahhhh/esp32-shift-light-R53-mini).

> ### Update: it configures from your phone now
>
> The shift point used to be a `#define`. It isn't any more. The board runs a
> Bluetooth LE service, and an Android app sets the RPM thresholds, the colours,
> the brightness and the LED count live, with no cable and no reflash. It also
> shows you the CAN bus. Details below, under
> [Setting it up from your phone](#setting-it-up-from-your-phone).

<div style="max-width:340px;margin:1.5rem auto;">
  <div style="position:relative;padding-bottom:177.78%;height:0;overflow:hidden;border-radius:8px;box-shadow:0 2px 14px rgba(0,0,0,.45);">
    <iframe src="https://www.youtube.com/embed/FrWZCIEsT3Q"
            title="R53 ESP32 shift light"
            loading="lazy" allowfullscreen
            allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
            style="position:absolute;top:0;left:0;width:100%;height:100%;border:0;"></iframe>
  </div>
</div>

*The bar sweeping through the whole range: dark, green fill, the fade to red, then the flash.*


## Reading RPM from the CAN bus

The car already broadcasts RPM constantly — it's on the CAN bus. An **SN65HVD230** transceiver (a $3 breakout) sits between the bus and the ESP32, which picks the engine-speed frames off the bus, extracts RPM, and translates it into light.

![Shift light electronics — ESP32 mini and 8-LED WS2812B stick](/assets/images/r53-shift-light/shift-light-bench.jpg){:.img-md}
*The whole system: one tiny USB-C ESP32, a screw terminal, three wires, and the 8-LED WS2812B stick.*

Parts total is a dev board, the CAN transceiver breakout, and a $5 LED stick — a shift light for the price of a track-day lunch.

## Reading the bar without reading it

A shift light you have to *interpret* is a shift light you'll ignore. The display encodes RPM two ways at once — how many LEDs are lit, and what color they are — and you process both peripherally:

- **Below 3,000 RPM** — dark. Cruising the paddock shouldn't look like a police chase.
- **3,000–6,000** — solid green, LED pairs filling in as revs climb.
- **6,000–7,100** — the color fades continuously from green through amber to red. The "get ready" zone.
- **Past 7,100** — the whole bar flashes red at 5 Hz. Unmissable. Shift. Now.

The color math is a linear crossfade through the warning zone:

{% highlight cpp %}
} else if (rpm <= 7100) {
  // Fade from green to red (6000 to 7100 RPM)
  uint8_t t = map(rpm, 6000, 7100, 0, 255);
  color = CRGB(t, 255 - t, 0);
}
{% endhighlight %}

One detail I'm fond of: the bar doesn't fill left-to-right like a progress bar. The eight LEDs light **in symmetric pairs from the ends toward the center** — outer pair first, then inward, until the middle pair completes the bar right at the shift point:

{% highlight cpp %}
if (numPairs >= 1 && (i == 0 || i == 7)) leds[i] = color; // Pair 1: Ends
if (numPairs >= 2 && (i == 1 || i == 6)) leds[i] = color; // Pair 2
if (numPairs >= 3 && (i == 2 || i == 5)) leds[i] = color; // Pair 3
if (numPairs >= 4 && (i == 3 || i == 4)) leds[i] = color; // Pair 4: Center
{% endhighlight %}

A bar growing from one side reads as "a number changing." A bar collapsing toward its own center reads as *convergence* — the visual finishes exactly when the engine does. Much easier to catch in peripheral vision, which is the only vision you're sparing it.

Brightness is capped at 75/255 — WS2812Bs at full tilt in a dark car will cook your night vision.

## Bench testing without an engine

You don't want your first test of a shift light to be at 7,000 RPM on a back straight. The firmware (PlatformIO, Arduino framework, FastLED) has a compile-time simulation mode:

{% highlight cpp %}
//#define SIMULATE_RPM   // Uncomment to test without the car
{% endhighlight %}

With it defined, the code ignores the CAN input and sweeps RPM from 1,000 to 8,000 and back on a ten-second cycle — the dark zone, the green fill, the fade, the flash, all on the bench. Comment it back out, reflash, and it's reading the car again.

That flag is still there, and it's still the right way to bring up a bench board with nothing else connected. But it's no longer the only way: the app toggles the same sweep at runtime, and while it runs the board synthesises the RPM CAN frame too, so the bus view and the bar agree instead of one showing a dead bus. Simulated readings are flagged as simulated everywhere they appear. Nothing that comes out of that mode can be mistaken for the car.

## Setting it up from your phone

Every number in the section above used to be baked into the firmware. Changing a shift point meant a laptop, a USB cable, and a reflash, which is a miserable thing to want in a paddock.

Now the board advertises a Bluetooth LE service, and an Android app talks to it.

![The shift light app, thresholds and live strip preview](/assets/images/r53-shift-light/app-thresholds.jpg){:.img-sm}
*Live RPM off the car, a mimic of the bar, and every threshold on a slider.*

The strip along the top of that screen is the real one, mirrored. It updates from live RPM at 10 Hz, so you set the thresholds while watching the thing you're actually setting them for. Drag the redline down to 6,800 and the bar fills sooner, right there, before you've moved the car.

What you can change:

- **Four RPM thresholds.** Where the LEDs start, where the colour begins shifting, where the bar fills, and where it starts flashing.
- **Four colours,** from a palette picked to stay distinguishable through a windscreen in daylight.
- **Brightness and LED count.** The count matters if you build a longer bar than eight.
- **Fill direction.** Pairs from both ends inward, or a plain left-to-right bar.
- **Blink rate.**

### Apply and Save do different things

**Apply** pushes the settings to the board and they take effect instantly, but they're gone at the next key-off. That's the mode for dialling a shift point in with the engine running, because dragging a slider shouldn't burn a flash write on every frame.

**Save** commits them to the board's flash, where they survive power-off.

![Strip settings, colour palette, and the Apply and Save buttons](/assets/images/r53-shift-light/app-strip.jpg){:.img-sm}
*Colours, brightness, LED count, fill direction, and the simulator switch. Applied is greyed out because nothing has changed since the last push.*

The board keeps track of which state it's in and the app says so. There's no guessing whether that change you made an hour ago is going to still be there tomorrow.

### It also shows you the CAN bus

The second tab lists every CAN ID on the bus with its frame rate and latest payload. It's there because RPM is only the first thing worth reading off an R53, and because when a CAN node goes quiet you want to see it rather than infer it.

Streaming is off until you switch it on. A phone that isn't looking shouldn't cost the board airtime it needs for the shift light.

### The phone is optional

Worth being clear about this, because it's the part that matters in the car: **the shift light does not need the app.** It boots, reads CAN, and lights up with no phone in the car and no app installed. Settings live in the board's own flash. The app is a screwdriver, not a component.

Nothing on the Bluetooth side sits between the CAN bus and the LEDs.

### Getting the app

It's free, and it isn't on the Play Store. You install the APK directly:

> **Download:** [r53-shift-light.apk](https://mrblahhhh.github.io/assets/apk/r53-shift-light.apk) · version 1.1 · Android 8.0 or newer · about 1 MB

<div style="max-width:180px;margin:1.25rem auto;">
  <img src="/assets/images/r53-shift-light/app-qr.png"
       alt="QR code linking to the R53 shift light APK download"
       style="width:100%;height:auto;display:block;border-radius:6px;">
</div>

*Scan it from the car and install without touching a computer.*

Android will warn you about installing an app from outside the Play Store. That's expected for anything sideloaded, and you'll need to let your browser install unknown apps when it asks. The app asks for one permission: Nearby devices on Android 12 and up, or Location on 11 and older, which is how those versions gate Bluetooth scanning. It holds no internet permission at all, so it cannot send anything anywhere even if you wanted it to.

## Mounting

![Shift light installed on the column shroud, under the tach pod](/assets/images/r53-shift-light/shift-light-installed.jpg){:.img-md}
*3D-printed housing clipped to the column shroud, directly below the tach pod.*

That spot earns its keep: dead-center in the forward sightline, moves with the column so the wheel rim never blocks it, and the housing shrouds the LEDs enough to kill windshield reflections. The flashing red bar and the tach needle end up in the same glance — not that I read the needle anymore.

The housing is up on Cults3D if you want to print your own:

> **Download:** [R53 shift light housing on Cults3D](https://cults3d.com/en/3d-model/various/r53-shift-light-housing)

## Powering it

The board takes 5 V over USB-C, and the car has 12 V. Two cheap parts bridge that, and both are worth buying rather than improvising.

**An add-a-fuse tap** in the fuse box. It piggybacks a new fused circuit onto an existing one without cutting a single factory wire, and it puts your circuit on its own fuse instead of borrowing someone else's headroom.

On the R53 the interior fuse box is in the **driver's side lower kick panel, behind a cover**, which is convenient: it is directly below where the wire comes down the A-pillar, so the whole run stays on one side of the car and out of sight.

Pick an **ignition-switched** fuse, not a permanent live. A shift light that stays awake in the car park will flatten the battery over a weekend, and it is a genuinely annoying fault to diagnose because everything works perfectly right up until the morning it doesn't.

Test before you commit, because the box carries both kinds. Probe the fuse with a meter or a test light: key out should read 0 V, key to position 1 should read 12 V. Some circuits also drop out while cranking, which only means the light reboots when the engine starts. Stay off anything with a **yellow connector**, which is airbag.

![Running the power wire down behind the A-pillar trim](/assets/images/r53-shift-light/install-wire-run.jpg){:.img-md}
*Feeding the 12 V supply down behind the A-pillar trim in the R53. It runs behind the trim rather than across the dash, so nothing is visible and nothing crosses the airbag path.*

**A 12 V to 5 V USB buck converter**, the sort sold for golf carts and motorcycles. The dual-USB Aideepen modules are about sixteen dollars for a two-pack and rated 5 V at 3 A, which is generous: the board and eight WS2812Bs together draw well under half an amp even with the bar full and the brightness up. Headroom costs nothing here and a supply running at its limit gets hot.

Buck, not a resistor divider and not a linear regulator. A linear part dropping 12 V to 5 V burns the difference as heat, and at half an amp that is three and a half watts out of a plastic box tucked behind a dash.

![Quick-splice taps on the switched feed, with ring terminals on a ground stud](/assets/images/r53-shift-light/install-power-tap.jpg){:.img-md}
*Add-a-fuse feed tapped in behind the R53's dash, with ground on a proper stud rather than a random screw into painted metal. A shift light with a poor ground flickers, and it looks exactly like a firmware bug.*

## One board instead of five things

Everything above works, and it is still a dev board, a CAN breakout, a USB buck module and a knot of jumper wires. Every one of those joints is a thing that vibrates loose in a car. So there is now a PCB that is all of it at once.

![The carrier board, rendered](/assets/images/r53-shift-light/carrier-board.jpg){:.img-lg}
*54 x 48 mm, four layer. Three screw terminals along the bottom edge with every pin labelled, the ESP32 soldered down dead centre with its USB-C shell overhanging the top edge, the CAN transceiver on the board rather than hanging off it. M3 holes in all four corners, because a board that lives behind a dash needs to be bolted to something.*

What changed against the loose-parts build:

- **The CAN transceiver is on the board.** No breakout module, and the bus gets its own screw terminal with clamps on both lines.
- **The 12 V input is protected properly.** A resettable fuse, a bidirectional TVS clamping at 26 V, then a Schottky blocking reverse polarity, then the same switching regulator down to 5 V.
- **The strip data line goes through a level shifter.** A WS2812B on a 5 V rail wants 3.5 V to read a logic high and the ESP32 puts out 3.3 V. It usually works. "Usually" is the problem, and a 74AHCT1G125 costs eight cents.
- **Nothing plugs in.** The ESP32-C3 SuperMini is soldered down, not socketed. No socket to walk out of on a rough surface.
- **It bolts down.** M3 clearance holes in all four corners, sized around a washer rather than the screw, so the row of terminals still has room to breathe. Cable-tying a bare PCB to a loom behind the dash is how you find out what a stray track can short against.
- **Every terminal pin is labelled on the board.** `+12V GND`, `CANH CANL`, `+5V DATA GND`, printed above the pin it belongs to. You wire one of these upside down under a dash with a torch in your teeth, and a designator like "J2" tells you nothing at that moment.

The module sits on plated through-holes in oblong pads that run outward past its body. Mine has plain through-holes set in from the edge rather than castellated half-holes, and a flat SMD land would have put the copper underneath it with no way to reach it. The oblong pads work either way, and take a module with header pins already fitted too.

Every dimension on that footprint came off the actual part with calipers: 17.94 mm across the body, 22.89 mm of PCB, 24.17 mm overall — so the USB-C shell stands 1.28 mm proud of the board it is on. That last number matters more than it sounds. The connector has to end up at the edge of the carrier, or you cannot plug a cable in.

The first routed rev got exactly that wrong. The module went down at 270
degrees on the strength of a comment claiming 270 faced the top edge, but the
comment's numbers had been worked out for the footprint at 0 degrees, and
KiCad rotates the pads with the part. The port spent a week aimed at the bulk
capacitor from 5 mm away, through a clean ERC, a clean DRC, a netlist compare
and every audit in the pipeline. None of those checks knows what a connector
is.

The board you see above is the fix: module at 0 degrees, centred, riding high
enough that the shell overhangs the top edge by 1.2 mm, so a plug's overmold
never meets the carrier at all. And the pipeline got a new check out of it.
`audit_usb.py` takes the port's position and direction from pads 1 and 16 of
the placed board and fails if anything sits in a plug-sized corridor. It reads
the copper, not the comment. Pointed at the broken rev it says `C7 sits in the
plug corridor`, which is the sentence nobody wrote for a week.

![The carrier board from an angle](/assets/images/r53-shift-light/carrier-board-iso.jpg){:.img-lg}
*The same board from an angle. The module's oblong pads run outward past its
body, so every joint is reachable with an iron once the module is sitting on
them.*

The bus termination jumper ships **open**, and should stay that way. A car's CAN bus is already terminated at both ends; a third 120 Ω across the pair drops it to about 40 Ω and can stop it working. That jumper is there for a bench bus, not for the car.

### Parts

Sixteen lines, and everything except the module and the terminals is a common
jellybean part. LCSC numbers are the ones the design carries; blanks are parts
where any equivalent of the right rating will do.

| Ref | Part | Qty | Package | LCSC | Notes |
|---|---|---:|---|---|---|
| U2 | ESP32-C3 SuperMini | 1 | 2×8, 2.54 mm | — | buy separately, soldered through-hole |
| U1 | Recom R-78E5.0-1.0 | 1 | SIP-3 | — | 12 V→5 V, 1 A switching |
| U3 | SN65HVD230DR | 1 | SOIC-8 | C12084 | CAN transceiver |
| U4 | 74HCT1G125GV,125 | 1 | SOT-23-5 | C12502 | 3.3 V→5 V buffer |
| D1 | SMBJ26CA | 1 | SMB | — | bidirectional input clamp |
| D2 | SS34 | 1 | SMA | — | reverse-polarity block |
| D3, D4 | SMAJ26CA | 2 | SMA | C134976 | CAN bus clamps |
| F1 | Littelfuse 1812L110/33MR | 1 | 1812 | C142747 | 1.1 A PTC, **33 V** — see below |
| C1 | 10 µF 50 V | 1 | 1206 | — | regulator input |
| C2 | 22 µF 16 V | 1 | 1206 | C12891 | regulator output |
| C3–C6 | 100 nF 16 V | 4 | 0805 | C49678 | decoupling |
| C7 | 220 µF 10 V | 1 | SMD, 6.3 × 7.7 mm | — | LED inrush reservoir |
| R1 | 120 Ω | 1 | 0805 | — | termination, unused unless JP1 is bridged |
| R2 | 330 Ω | 1 | 0805 | — | series damping on the data line |
| J1, J2 | 2-way screw terminal | 2 | 5.08 mm | — | 12 V in, CAN |
| J3 | 3-way screw terminal | 1 | 5.08 mm | — | shift light |
| H1–H4 | M3 × 8 screw, washer, nut | 4 | — | — | corner fixings, hardware-store parts |

JP1 is not on the list because it is not a part: it is a solder jumper, two
pads on the board, and it stays open. The M3 hardware is on it for the sake of
ordering, not because JLC ships it.

Two of those rows are worth a sentence, because both were wrong in an earlier
version of this page and both would have cost money. A third thing is worth
knowing before you upload anything.

**U4 was listed as `C129276`.** That number is an STM32F767BIT6 — a 216 MHz
microcontroller, about thirty dollars each. JLC's matcher offered it without
complaint, because the number was exactly what it had been given. The part is a
nine-cent buffer. The package was wrong independently too: Nexperia's `GW`
suffix is SOT-353, not the SOT-23-5 land on the board. `GV` is SOT-753, which
*is* JEDEC SOT-23-5.

**F1 is rated 33 V and that is the whole point of it.** The obvious 1812 PTC on
LCSC is rated 8 V, and this fuse sits *ahead* of the TVS, where it sees the raw
harness including whatever the alternator is doing. An 8 V part on a 12 V line
is not protection.

**Check the placement preview, and expect two parts to be turned.** KiCad and
LCSC do not agree on which way a part points inside its own package, and for
two of these — the SOIC-8 transceiver and the SOT-23-5 buffer — the disagreement
is a right angle. JLC's preview shows both 90 degrees off. Nothing is wrong with
the board; the placement file is what needs correcting, so the design applies a
−90 to those two footprints on the way out to `positions.csv` and leaves the
copper alone. It is worth a look at the preview regardless. It is the only
stage of the whole order where a human sees the parts on the board before a
machine puts them there.

Off the board you still need the eight-LED WS2812B strip, an add-a-fuse, and
wire.

Gerbers, the machine-readable BOM and the placement file are in the repo under
[`hardware/pcb/fab`](https://github.com/MrBlahhhh/esp32-shift-light-R53-mini/tree/main/hardware/pcb/fab).

### Building one

The through-hole parts cannot go through a hot plate, and neither can the
module: the SuperMini is a populated board with its own reflowed components, and
taking it to paste temperature makes them move. So the order is reflow the SMD
parts, hand-solder the three terminals and the regulator, then solder the module
last. Its pads run outward past the module edge, so you work down the outside
with an iron and watch each joint form.

Only four parts are hand-soldered: the three terminals and the regulator.
Everything else, the bulk capacitor included, is SMD and goes through the plate
in one pass.

Gerbers, BOM and placement files are in the repo under `hardware/pcb/fab`.
Nothing has been manufactured yet, so treat rev A as exactly that.

## Reading the whole bus, not just RPM

The board was already listening to every frame on the bus and throwing away all
but one of them. The app now decodes eleven channels off the car, plus the AEM
wideband that shares the same pair, and plots each one.

**No firmware change was needed.** The frames were always arriving; the protocol
already had a filter command. Switching the graph on narrows the stream to the
ten IDs those channels need, because a 500 kbit bus in full flow is far more
than Bluetooth will carry.

![The graph tab, three channels overlaid](/assets/images/r53-shift-light/app-graph.jpg){:.img-sm}
*Engine speed, throttle and air/fuel on one plot. That trace is a real pull: throttle spikes, revs climb behind it, and the mixture goes rich and stays busy while the wideband works.*

### One plot, not one axis

Engine speed runs to 7500. AFR lives between 10 and 19. Put both on one *scale*
and AFR is a flat line along the bottom that tells you nothing.

So they share a plot without sharing a scale: each channel is drawn against its
own fixed range, and the two most important get a labelled axis in their own
colour — engine speed on the left, whatever else you are watching on the right.
Colouring each axis to match its trace is what stops two scales being
ambiguous. A number on the left edge is red, and so is the line it belongs to.

That is the whole point of putting them together. "Throttle came off and the
mixture went lean right there" is one glance instead of two.

### What the R53 puts on the bus

![The CAN bus tab listing every id on the bus](/assets/images/r53-shift-light/app-canbus.jpg){:.img-sm}
*Every id the car is sending, its rate, and its latest bytes. Fifteen of them, and just over a thousand frames a second between them — which is why the graph filters down to only the ids it needs rather than streaming the lot over Bluetooth.*


Every one of these came from a DBC-generated decoder rather than guesswork. The
start bit is into the frame read as a single little-endian word, which is how a
DBC describes an Intel signal.

| Channel | ID | Start bit | Length | Scale | Offset |
|---|---|---:|---:|---:|---:|
| Engine speed | `0x316` | 16 | 16 | 0.15625 | 0 |
| Coolant | `0x329` | 8 | 8 | 0.75 | −48.373 |
| Throttle | `0x329` | 40 | 8 | 1 | 0 |
| Oil temp | `0x545` | 32 | 8 | 1 | −48.373 |
| Oil pressure | `0x565` | 48 | 8 | 2 | 0 |
| Road speed | `0x153` | 11 | 13 | 0.0625 | −0.625 |
| Wheel speeds | `0x1F0` | 0/16/32/48 | 12 | 0.0625 | 0 |
| Steering angle | `0x1F5` | 0 | 15 | 0.045 | 0 |
| Fuel level | `0x613` | 16 | 7 | 1 | 0 |
| Outside temp | `0x615` | 24 | 7 | 1 | 0 |

You can check those against the screenshot above. `0x329` reads `C0 B6 ...`, and
byte 1 of 182 gives 182 × 0.75 − 48.373 = 88.1 °C — a warm engine, which it was.
`0x316` reads `01 00 06 23`, and bytes 2 and 3 little-endian give 8966 × 0.15625
= 1400 rpm, which is what the graph was showing at the time. The AEM's byte 4
reads `89`, which is 13.7 V: a running car's charging voltage, and about the
strongest confirmation of a byte layout you can get without a reference.

Two of those have a catch. **Steering angle keeps its sign in a separate bit**
above the magnitude — read it as one 16-bit field and it comes out half a turn
wrong. **Outside temp does the same.** And the RPM scale is worth a note: the
0.15625 above is 1/6.4, which is exactly what the shift light has been running
on since the first version. The struct comment in that DBC header says 0.2. The
unpack function says 0.156, and the car agrees with the unpack function.

### The wideband

The AEM is the odd one out. It is **big-endian where the car is little-endian**,
and it uses 29-bit extended IDs:

| Field | Bytes | Scale |
|---|---|---|
| Lambda | 0–1, big-endian | ×0.0001 |
| Oxygen % | 2–3, signed | ×0.001 |
| Volts | 4 | ×0.1 |
| Status | 6 | `0x02` sensor · `0x20` free-air cal · `0x80` valid |
| Fault | 7 | `0x40` sensor fault |

That validity bit matters more than it looks. A wideband that is still warming
up publishes a lambda anyway, and it is wrong. The app holds those readings back
rather than drawing a confident line through them, and says why: *warming up,
readings held back*.

## What's next

- **Per-gear shift points.** The thresholds are adjustable now, but they're one set for the whole car. An R53 doesn't want the same shift point in second as it does in fifth.
- **Ambient dimming.** A light sensor to knock the brightness down for night sessions. There are spare pins on the carrier for it.
- **Channels on the bar itself.** The app graphs eleven of them now; putting coolant on the LEDs during a cool-down lap is the obvious next step.

For eight LEDs, one board, and an afternoon of firmware, it already does the one thing it needs to do: when the bar goes red and starts flashing, I shift. My eyes never leave the track. The difference now is that when I want it flashing 300 RPM earlier, I change it from the driver's seat.
