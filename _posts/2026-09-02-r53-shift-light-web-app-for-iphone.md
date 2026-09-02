---
title: "The R53 Shift Light and K-line Bridge Now Work With iPhone"
date: 2026-09-02 08:00:00 -0400
categories: car tech
tags: [mini, r53, esp32, shift-light, k-line, can-bus, bluetooth, web-bluetooth, bluefy, iphone, ios, android, datalogger]
cover: /assets/images/r53-shift-light/app-thresholds.jpg
lightbox: true
excerpt: "Both boards now configure from an iPhone, with no App Store install. One free browser is required, and here is exactly why Safari is not it."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-shift-light/app-thresholds.jpg
---

<!--more-->

If you have an iPhone, both boards now work for you.

The [CAN shift light](/car/tech/2026/07/11/r53-esp32-shift-light.html) and the [K-line + CAN bridge](/car/tech/2026/08/27/r53-kline-canbus-bridge-hardware.html) have always configured from an Android phone over Bluetooth. iPhone was the gap, because putting an app on the App Store means a Mac to build it and a paid Apple developer account to sign it.

So the app is a web page instead. Same board, same Bluetooth, same screens. Nothing to install and nothing to pay.

**[Open the app](https://logs.geekopolis.com/shiftlight/)**

## Setting it up on an iPhone

One thing to know first, because it will otherwise look broken: **Safari cannot do this, and neither can Chrome or Edge on your iPhone.**

Apple has never shipped Web Bluetooth in Safari, and every browser on iOS is required to use Safari's engine underneath. So they all inherit the same gap. It isn't something an iOS update fixes.

The fix is one free browser that bridges Bluetooth properly:

1. Install **[Bluefy](https://apps.apple.com/us/app/bluefy-web-ble-browser/id1492822055)** from the App Store. Free.
2. Open Bluefy and go to `logs.geekopolis.com/shiftlight/`
3. Tap **Connect** and pick your board.

One-time setup, then bookmark it in Bluefy. The app checks for this and tells you plainly if you've landed on it in the wrong browser, rather than showing a Connect button that does nothing.

**On Android, Windows, Mac or Linux** there's no special browser needed. Chrome or Edge, and it works.

## What each board gives you

The page works out which board you connected to on its own. You don't choose.

**Shift light.** Everything the Android app does. Shift points and colours on live sliders, the mirrored LED strip so you see the change as you drag it, the graph, and the raw CAN monitor.

**K-line + CAN bridge.** A live screen: engine speed, throttle, coolant, road speed and wideband lambda, with a minute of history plotted behind them.

If a channel isn't arriving it reads as a dash rather than zero. With the key off most of that screen is blank, which is correct. A gauge showing 0 °C coolant when nothing is being measured tells you something false.

## What stays on Android

ECU logging, the analysis and the flashing stay in [R53 Logger - Flasher](/car/tech/2026/07/24/r53-logger-play-store.html), on Android. That app writes to your ECU and it is not becoming a web page. **The bridge still needs an Android phone for datalogging and flashing.** The web app covers configuration and live data, not the ECU side.

The shift light has no such caveat. iPhone does everything there.

## Honest limits of the web version

- **You tap Connect each session.** Browsers can't scan in the background or remember a device, so it doesn't reconnect on its own when you start the car.
- **It won't run in the background.** Switch apps on iOS and the browser gets suspended and the link drops. The app notices and shows you it dropped.
- **Bluefy has no working add-to-home-screen.** Bookmark it inside Bluefy instead.

The screen stays awake while you're connected, so the phone can sit in a cradle without dimming.

## If the graph stays empty

If the config screen works but the graph stays empty, that's firmware, not your phone.

Older firmware sends its CAN data in a fixed-size batch that only fits because the Android app asks for a larger packet size. A browser can't ask, iOS settles on a smaller size, and anything over it gets dropped instead of split up. The result is a shift light that configures perfectly and a graph that never draws.

Flashing [`36d3b6c`](https://github.com/MrBlahhhh/esp32-shift-light-R53-mini) or later fixes it.

The K-line bridge never had this problem and works in a browser as it ships.

## Source

It's a Svelte app and the whole bundle is about 86 KB. The protocol decoding is ported from the Kotlin in the Android app and carries the same tests, because there are now three copies of each wire format in the world (firmware, Android, browser) and they have to agree.

The shift light firmware is [esp32-shift-light-R53-mini](https://github.com/MrBlahhhh/esp32-shift-light-R53-mini). The bridge is [R53_Mini_Kline_Canbus_Logger_Shiftlight](https://github.com/MrBlahhhh/R53_Mini_Kline_Canbus_Logger_Shiftlight).
