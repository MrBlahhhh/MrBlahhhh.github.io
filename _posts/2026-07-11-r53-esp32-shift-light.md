---
title: "Wireless ESP32 Shift Light for the R53"
date: 2026-07-11 00:00:00 -0400
categories: car tech
tags: [esp32, esp-now, can-bus, ws2812b, fastled, mini, r53, shift-light, 3d-printing]
cover: /assets/images/r53-shift-light/shift-light-installed.jpg
lightbox: true
excerpt: "CAN bus RPM to an 8-LED bar over ESP-NOW — no wires between the light and the car"
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-shift-light/shift-light-installed.jpg
---

<!--more-->

The R53 has a lot of charm, but a track-friendly tach is not part of it. The column pod is small, and mid-corner with your eyes up where they belong, it might as well not exist. Aftermarket shift lights either cost real money or want a hardwired tach signal run across the dash.

So I built my own: an **8-LED WS2812B bar** in a 3D-printed housing on the column shroud, fed RPM **wirelessly** from the CAN bus. Source is on GitHub: [esp32-shift-light-R53-mini](https://github.com/MrBlahhhh/esp32-shift-light-R53-mini).

## Split it in two

The car already broadcasts RPM constantly — on the CAN bus. The trick is getting that number to a display up on the column without threading cable through the interior. Two cheap ESP32 boards handle it:

- **Sender** — lives at the CAN tap. An **SN65HVD230** transceiver sits between the bus and the ESP32, which picks the engine-speed frames off the bus and extracts RPM.
- **Receiver** — the shift light itself. A tiny USB-C ESP32 mini driving the WS2812B stick, mounted where my eyes actually look.

![Receiver electronics — ESP32 mini and 8-LED WS2812B stick](/assets/images/r53-shift-light/shift-light-bench.jpg){:.img-md}
*The entire display end of the system: one board, a screw terminal for power, three wires, eight LEDs.*

Between them: **ESP-NOW**, Espressif's connectionless 2.4 GHz protocol. No WiFi network, no pairing, no Bluetooth stack. The sender fires a two-byte packet — a single `uint16_t` of RPM — and the receiver catches it. Latency is low enough that the light feels wired.

Parts total is two dev boards, a $3 CAN transceiver breakout, and a $5 LED stick — a shift light for the price of a track-day lunch.

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

## The receiver firmware

PlatformIO, Arduino framework, FastLED. The whole receiver is about 127 lines: listen, translate RPM into light, repeat. ESP-NOW hands you a callback and the payload is just the raw integer:

{% highlight cpp %}
void OnDataRecv(const uint8_t *mac_addr, const uint8_t *incomingData, int len) {
  if (len == sizeof(uint16_t)) {
    memcpy(&rpm, incomingData, sizeof(rpm)); // Update global rpm
  }
}
{% endhighlight %}

## Bench testing without an engine

You don't want your first test of a shift light to be at 7,000 RPM on a back straight. The firmware has a compile-time simulation mode:

{% highlight cpp %}
//#define SIMULATE_RPM   // Uncomment to test without the car
{% endhighlight %}

With it defined, the receiver skips ESP-NOW and sweeps RPM from 1,000 to 8,000 and back on a ten-second cycle — the dark zone, the green fill, the fade, the flash, all on the bench. Comment it back out, reflash, and it's a receiver again.

## Mounting

![Shift light installed on the column shroud, under the tach pod](/assets/images/r53-shift-light/shift-light-installed.jpg){:.img-md}
*3D-printed housing clipped to the column shroud, directly below the tach pod.*

That spot earns its keep: dead-center in the forward sightline, moves with the column so the wheel rim never blocks it, and the housing shrouds the LEDs enough to kill windshield reflections. The flashing red bar and the tach needle end up in the same glance — not that I read the needle anymore.

## What's next

- **Configurable shift point** — the 7,100 RPM threshold is a `#define` today; per-gear shift points over ESP-NOW would be a fun upgrade.
- **Ambient dimming** — a light sensor to knock the brightness down for night sessions.

For eight LEDs, two boards, and an afternoon of firmware, it already does the one thing it needs to do: when the bar goes red and starts flashing, I shift. My eyes never leave the track.
