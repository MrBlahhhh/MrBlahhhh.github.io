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

## What's next

- **Per-gear shift points.** The thresholds are adjustable now, but they're one set for the whole car. An R53 doesn't want the same shift point in second as it does in fifth.
- **Ambient dimming.** A light sensor to knock the brightness down for night sessions.
- **More than RPM.** The board already sees the whole bus. Coolant temp on the bar during a cool-down lap is close to free.

For eight LEDs, one board, and an afternoon of firmware, it already does the one thing it needs to do: when the bar goes red and starts flashing, I shift. My eyes never leave the track. The difference now is that when I want it flashing 300 RPM earlier, I change it from the driver's seat.
