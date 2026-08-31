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

> **Download:** [r53-shift-light.apk](https://mrblahhhh.github.io/assets/apk/r53-shift-light.apk) · Android 8.0 or newer · about 1 MB

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
*52 x 44 mm, four layer. Three screw terminals along the bottom edge, the ESP32 soldered down in the middle, the CAN transceiver on the board rather than hanging off it.*

What changed against the loose-parts build:

- **The CAN transceiver is on the board.** No breakout module, and the bus gets its own screw terminal with clamps on both lines.
- **The 12 V input is protected properly.** A resettable fuse, a bidirectional TVS clamping at 26 V, then a Schottky blocking reverse polarity, then the same switching regulator down to 5 V.
- **The strip data line goes through a level shifter.** A WS2812B on a 5 V rail wants 3.5 V to read a logic high and the ESP32 puts out 3.3 V. It usually works. "Usually" is the problem, and a 74AHCT1G125 costs eight cents.
- **Nothing plugs in.** The ESP32-C3 SuperMini is soldered down, not socketed. No socket to walk out of on a rough surface.

The module sits on plated through-holes in oblong pads that run outward past its body. That covers every way these boards ship: castellated half-holes, plain through-holes, or header pins already fitted. A flat SMD land only works for the first kind, and on the other two the copper ends up underneath the module with no way to reach it.

The bus termination jumper ships **open**, and should stay that way. A car's CAN bus is already terminated at both ends; a third 120 Ω across the pair drops it to about 40 Ω and can stop it working. That jumper is there for a bench bus, not for the car.

### Parts

Everything except the module and the terminals is a common jellybean part. The
LCSC numbers are the ones the design carries; the blanks are parts where any
equivalent will do.

| Ref | Part | Qty | Package | LCSC |
|---|---|---:|---|---|
| U2 | ESP32-C3 SuperMini | 1 | module, soldered flat | buy separately |
| U1 | Recom R-78E5.0-1.0 | 1 | SIP-3 | 12 V to 5 V, 1 A switching |
| U3 | SN65HVD230DR | 1 | SOIC-8 | C12084 |
| U4 | 74AHCT1G125GW | 1 | SOT-23-5 | C129276 |
| D1 | SMBJ26CA | 1 | SMB | bidirectional, input clamp |
| D2 | SS34 | 1 | SMA | reverse-polarity block |
| D3, D4 | SMAJ26CA | 2 | SMA | C134976 |
| F1 | 1.1 A PTC resettable | 1 | 1812 | |
| C1 | 10 µF 50 V | 1 | 1206 | |
| C2 | 22 µF 16 V | 1 | 1206 | C12891 |
| C3–C6 | 100 nF 16 V | 4 | 0805 | C49678 |
| C7 | 220 µF 10 V | 1 | SMD, 6.3 × 7.7 mm | |
| R1 | 120 Ω | 1 | 0805 | termination, unused unless JP1 is bridged |
| R2 | 330 Ω | 1 | 0805 | |
| J1, J2 | 2-way screw terminal | 2 | 5.08 mm | |
| J3 | 3-way screw terminal | 1 | 5.08 mm | |

Off the board you still need the eight-LED WS2812B strip, an add-a-fuse, and
wire.

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

## What's next

- **Per-gear shift points.** The thresholds are adjustable now, but they're one set for the whole car. An R53 doesn't want the same shift point in second as it does in fifth.
- **Ambient dimming.** A light sensor to knock the brightness down for night sessions. There are spare pins on the carrier for it.
- **More than RPM.** The board already sees the whole bus. Coolant temp on the bar during a cool-down lap is close to free.

For eight LEDs, one board, and an afternoon of firmware, it already does the one thing it needs to do: when the bar goes red and starts flashing, I shift. My eyes never leave the track. The difference now is that when I want it flashing 300 RPM earlier, I change it from the driver's seat.
