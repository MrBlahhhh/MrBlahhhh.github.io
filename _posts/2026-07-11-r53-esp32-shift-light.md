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

> ### Update: iPhone works now too
>
> The app is a web page as well, so an iPhone no longer needs an app I cannot
> ship. One catch: Safari has never had Web Bluetooth, so it has to be opened in
> **Bluefy**. See
> [The R53 shift light app now runs on iPhone](/car/tech/2026/09/02/r53-shift-light-web-app-for-iphone.html).

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

## Wiring it in

Four connections to the car and one converter. Every wire you need is behind
the access panel on the left end of the dash, which pops off by hand, and none
of it needs a factory wire cut.

Here is the whole job before the detail:

| Connection | Colour | WDS | Terminal | Cluster pin |
|---|---|---|---|---|
| Switched 12 V | **green / blue** | GN/BL 0.5 mm² | 15, fuse F40 (5 A) | X11177 pin 16 |
| Ground | **brown** | BR | 31 | body stud preferred |
| CAN High | **yellow / black** | GE/SW 0.5 mm² | CAN_H | X11177 pin 11 |
| CAN Low | **yellow / brown** | GE/BR 0.5 mm² | CAN_L | X11177 pin 24 |

Those colours and pins come off the factory WDS sheets for the instrument
cluster, not off a forum post. One of them is wrong in the reference most
people start from, which is covered in step 4.

### What you need

Electronics:

- **ESP32-C3 SuperMini** or S3-Zero, whichever you have
- **SN65HVD230** CAN transceiver breakout, the blue screw-terminal type
- **8-LED WS2812B** stick
- **12 V to 5 V USB buck converter**, the golf-cart or motorcycle sort. The
  Aideepen dual-USB modules are about sixteen dollars for a two-pack, rated
  5 V at 3 A. The board and a full strip together draw well under half an amp,
  so that is generous, and a supply running at its limit gets hot.

Buck, not a resistor divider and not a linear regulator. A linear part dropping
12 V to 5 V burns the difference as heat, and at half an amp that is three and a
half watts inside a plastic box behind a dash.

For the install:

- **Multimeter** and a **12 V test lamp**. Both, not either. Step 2 explains why.
- **Four taps** sized to the wire, Posi-Tap or T-tap
- **Ring terminal** and an 8 mm spanner, if you ground to a stud
- **Trim tools** for the dash end panel and the column shroud
- Cable ties

If you would rather not build one, I have made a couple for other people. The
first prototypes went out at **$65**.

### Step 1 — Pop the dash end panel

Open the driver's door and look at the end of the dash. There is an access panel
there that comes off by hand, or with gentle help from a trim tool. Behind it,
in easy reach, is the main harness carrying both the switched feed and the CAN
pair.

That is the only panel the wiring needs. **The fuse box is somewhere else**,
lower down in the left footwell side trim, and you only need it if you take the
add-a-fuse route in the last section or go hunting a blown fuse later.

The panel also sits right at the base of the A-pillar, so the strip wire comes
straight down the pillar and lands next to the board with nothing crossing the
dash.

### Step 2 — Find and verify switched 12 V

![Quick-splice taps on the switched feed, with ring terminals on a ground stud](/assets/images/r53-shift-light/install-power-tap.jpg){:.img-md}
*The switched feed tapped in behind the R53's dash, with ground on a proper stud rather than a random screw into painted metal.*

You are looking for the **green wire with a blue stripe**. WDS calls it
`GN/BL`, 0.5 mm², terminal 15, fed from fuse **F40 at 5 A**, landing on pin 16
of connector X11177 at the instrument cluster.

Colours can move by build date and market, so verify before you commit. Four
checks, meter on the wire and the black lead on a known brown ground:

1. **Key out** — should read 0 V
2. **Key on** — should read about 12 V
3. **Cranking** — watch whether it drops out. Plenty of switched circuits do,
   which only means the light reboots when the engine starts.
4. **Under load** — this is the one people skip. A signal wire reads a happy
   12 V on a meter and then collapses the moment you draw anything from it.
   Put the test lamp on it. If the lamp lights properly and the voltage holds,
   the wire can carry the light.

The cluster's other rails are on the same connector, which is how you tell them
apart if you are probing around:

| Terminal | What it is | Fuse | Colour | X11177 pin |
|---|---|---|---|---|
| 15 | ignition on | F40, 5 A | GN/BL | 16 |
| R | accessory | F9, 5 A | VI/SW | 2 |
| 30 | permanent | F21, 10 A | RT/GE | 15 |
| 50 | starter signal | F5, 5 A | SW/VI | 3 |
| 58G | dash illumination | via S241 | GR/RT | 14 |

**Stay off anything in a yellow connector housing.** That is airbag, and the
clock spring is right there under the shroud.

Once the wire checks out, the tap goes over it. Match the tap to the wire
gauge, seat the metal blade all the way down with pliers rather than thumb
pressure, and close the cover until it clicks. Then pull on the wire. A tap that
lets go on the bench will let go on a kerb.

### Step 3 — Ground

**Brown is ground** on a BMW or MINI. Not black. If you have come off a Japanese
or domestic car that one will catch you out, because black here is usually
switched power.

Two options, and one is better:

- **A body stud with a ring terminal.** Do this if there is one in reach. Scrape
  to bare metal if the stud is painted.
- **Tap the brown in the same harness.** Works, but it is a shared return.

A shift light with a poor ground flickers, and it looks exactly like a firmware
bug. I have chased it as one. Ground it properly the first time.

### Step 4 — Tap the CAN pair

![The CAN pair and the switched feed tapped into the main harness behind the dash end panel](/assets/images/r53-shift-light/install-can-tap.jpg){:.img-md}
*The twisted yellow pair in the middle is CAN. The green/blue switched feed is tapped above it. Both taps sit in the same loom behind the dash end panel.*

The pair is **twisted**, which is the easiest way to pick it out of the loom.
Follow the twist and you have found it.

| Signal | Colour | WDS | Cluster pin | Board terminal |
|---|---|---|---|---|
| CAN High | yellow / **black** | GE/SW | X11177 pin 11 | `CANH` |
| CAN Low | yellow / **brown** | GE/BR | X11177 pin 24 | `CANL` |

Bus speed is **500 kb/s**.

Write those down, because the reference most people start from is wrong on one
of them. The [Autosport Labs RaceCapture notes for the
R50/R52/R53](https://wiki.autosportlabs.com/Mini_Cooper_R50_R52_R5) list CAN
High as yellow/**red**, and flag the whole entry "VERIFY" themselves. Yellow/red
on this car is **BC-TRIP**, the on-board computer trip button, which BMW's own
OBC retrofit instructions put on X11175 pin 3. An easy wire to grab by mistake,
and it would sit there doing nothing.

If you would rather not trust colours at all, the connector settles it. X11177
is the black 26-pin plug on the back of the **centre speedo**, CAN on pins 11
and 24.

One trap if you go looking in the factory documents: BMW call that centre pod
the *Tachometer*, which is German for speedometer, not the rev counter. Read it
as the English word and you end up behind the wrong dial.

**Getting the pair backwards costs you nothing.** The transceiver is
listen-only and never drives the bus, so a swapped pair means the board hears
nothing and blinks red. Swap them and try again.

Do not fit a termination resistor. A car's CAN bus is already terminated at both
ends, and a third 120 Ω across the pair drops it to about 40 Ω and can stop the
bus working.

### Step 5 — Run the strip to the column

![Running the power wire down behind the A-pillar trim](/assets/images/r53-shift-light/install-wire-run.jpg){:.img-md}
*Feeding the supply down behind the A-pillar trim. It runs behind the trim rather than across the dash, so nothing is visible and nothing crosses the airbag path.*

Three wires to the strip: `+5V`, `DATA`, `GND`. Run them behind the A-pillar
trim, not across the dash. Nothing visible, and nothing crossing the airbag
path.

Cable-tie the board and the converter to an existing loom rather than letting
them hang. Anything loose behind a dash eventually finds something to rattle
against or short on.

### Step 6 — Optional: an AEM wideband on the same pair

If you run a wideband, its CAN output goes onto the **same pair**, into the same
two taps you made in step 4. AEM CAN High joins the yellow/black tap, AEM CAN Low
joins the yellow/brown. A Posi-Tap will take the second wire alongside the first.
Nothing else changes, and the board picks the gauge up with no configuration.

That works because of how AEM addresses its frames:

| | Car | AEM X-Series |
|---|---|---|
| ID format | 11-bit standard | **29-bit extended** |
| Bit rate | 500 kb/s | 500 kb/s |

Same bit rate, different ID space. The car's `0x316`, `0x329`, `0x1F0` and the
rest cannot collide with anything the AEM sends, so neither device has to know
the other is there.

**This is specific to the AEM.** A gauge or controller that puts standard 11-bit
IDs on the bus can land on an address the car is already using, and then you are
troubleshooting the car instead of the gauge. Check what any other device
transmits before you put it on the factory pair.

One difference worth knowing. The shift light is listen-only and never drives the
bus. The AEM **transmits**: it adds frames and it acknowledges. That is fine for
the reasons above, but it makes the wideband a real node on your car's bus rather
than a passenger, which the shift light is not.

No termination for it either, same reason as step 4.

Byte layout and the warm-up handling are further down, under
[The wideband](#the-wideband).

### Powering up

Key on. The onboard LED tells you where you are:

| Status LED | Meaning | What to check |
|---|---|---|
| Nothing at all | no power | fuse, the 12 V tap, the buck converter output |
| **Blinking red** | CAN down | the pair is swapped, or a tap is not biting |
| **Green** | CAN up, reading the bus | you are done |
| **Blue** | CAN up and a phone connected | done, and paired |

Green with the strip dead is usually the data line or the strip ground, not CAN.
Blinking red with good 12 V is nearly always the CAN pair: reverse it first, and
only then start doubting the taps.

### If you popped a fuse

Blow **F40** and the cluster loses terminal 15. It stays alive on terminal 30,
so the odometer, the clock and the stored faults all survive, but the ignition
side of the cluster stops waking with the key.

That presents as a **dead or half-dead instrument cluster**, which is exactly
the symptom that sends people shopping for a replacement cluster. Check the 5 A
fuse first. The fuse box is not behind the dash end panel: it is lower down, in
the left footwell side trim.

The cluster's four rails, so you can work out which one went:

| Fuse | Amps | Feeds the cluster's |
|---|---|---|
| 40 | 5 A | terminal 15, ignition |
| 9 | 5 A | terminal R, accessory |
| 21 | 10 A | terminal 30, permanent |
| 5 | 5 A | terminal 50, starter and EWS |

One honest caveat on that first row. WDS is unambiguous that terminal 15 at the
cluster comes off F40, and fuses 9, 21 and 5 line up exactly with the public
fuse charts. Fuse 40 is the row where those charts disagree, listing it as the
steering control unit fan relay and PDC instead. They also print 40 and 41
identically and leave 42 blank, so I would trust the label inside your own fuse
box cover over any chart on the internet, this post included.

F40 is a 5 A fuse and the cluster is already on it. The board and a full strip
stay under half an amp so it fits, but you are sharing the instrument cluster's
ignition feed. If that bothers you, use the add-a-fuse instead.

### Alternative: an add-a-fuse instead of tapping the harness

**An add-a-fuse** in the interior fuse box does the same job without touching
the harness at all. It piggybacks a new fused circuit onto an existing one
without cutting a single factory wire, and it puts the light on its own fuse
instead of borrowing the cluster's headroom.

The box is in the left footwell side trim, lower down and a different panel from
the dash end cap, so this route means opening two panels instead of one. Nothing
about the wire run changes.

Pick an **ignition-switched** position, not a permanent live. A shift light that
stays awake in the car park will flatten the battery over a weekend, and it is a
genuinely annoying fault to diagnose because everything works perfectly right up
until the morning it does not. Same probe test as step 2: key out 0 V, key on
12 V, then load it.

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

For eight LEDs, one board, and an afternoon of firmware, it already does the one thing it needs to do: when the bar goes red and starts flashing, I shift. My eyes never leave the track. The difference now is that when I want it flashing 300 RPM earlier, I change it from the driver's seat.
