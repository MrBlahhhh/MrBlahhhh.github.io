---
title: "Mini R53 Logger — Heading to the Play Store"
date: 2026-07-24 00:00:00 -0400
categories: car tech
tags: [mini, r53, android, datalogger, play-store, boost, tuning, ecu-flash, immobilizer, for-sale]
cover: /assets/images/r53-logger-play-store/main-screen.jpg
lightbox: true
excerpt: "R53 logger heading to Play Store — flash options now cover pedal, idle (cold+warm), and fan 200/220 °F. Facelift silver-cover write only; prepared ECUs $275."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-logger-play-store/main-screen.jpg
---

<!--more-->

## It's finished

**Mini R53 Logger** — the Android app I've been building so I can actually *see* what the supercharged W11 is doing — is feature-complete, and I'm loading it onto the Google Play Store.

If you want to try it when the listing goes live (or before, if I can get you an early install), email me and I'll sort you out.

![Main screen — live telemetry and Mini chili-red theme](/assets/images/r53-logger-play-store/main-screen.jpg){:.img-md}
*Plug in, hit connect, and the car streams live: RPM, boost, temps, spark, knock, fuel trims, injection — the works.*

## What you get

Same idea as the [earlier writeup](/car/tech/2026/07/22/r53-android-datalogger.html), now finished:

- **Live engine data** fast enough for a real pull — boost, throttle, temps, per-cylinder spark, knock, MAF, injector duty, short/long fuel trim
- **Wideband AFR** over Bluetooth, merged into the same time-aligned CSV
- **Autolog** — arms on WOT, keeps the pull through gear changes, saves each run as its own file
- **Live graph** with channel toggles, fixed ranges, and a redline marker on the RPM trace
- **Diagnostics** — read and clear fault codes with R53-specific notes (chassis modules stay on BMW hex titles — no fake SAE crosswalk)
- **Channels & poll rate** — turn off blocks you don't care about so the ones you do watch update faster
- **ECU backup / flash** — read a full 512 KB backup, send it to your tuner, and (carefully) write a tuner BIN back — **facelift silver-cover ECUs only**
- **Flash options** — before a quick write, optionally layer on pops, injector size, redline, throttle-pedal map, idle RPM (manual, cold + warm), and cooling-fan kick-on including an even-earlier 200 / 220 °F preset (same silver-cover limit)

There's a separate companion app for **module coding** (BC1, EMS, airbag, …) — see [R53 Coding](/car/tech/2026/07/26/r53-coding.html).

Everything records to plain CSV and shares straight into datazap.me. Numbers were cross-checked against professional BMW diagnostic tools on a running engine.

![Live graph with a pull in the middle](/assets/images/r53-logger-play-store/live-graph.jpg){:.img-md}
*Live graph — idle, then a pull. Toggle the channels you care about; export CSV or open datazap from the bottom.*

![Channels and poll-rate screen](/assets/images/r53-logger-play-store/channels.jpg){:.img-md}
*Fast / Slow / Off per ECU block — drop what you don't tune with and the rest gets snappier.*

## ECU backup and flash

**Important:** flash and the at-flash tune options only work on **facelift R53 ECUs with the silver back cover**. Pre-facelift / black-cover boxes are out of scope for write — logging, graphing, and diagnostics still do what they do on the car you're connected to; the write path is the silver-cover family only.

Backup is read-only and safe. Write is dangerous — key on / engine off, keep the car on a battery charger, and keep a desktop OBD recovery path ready. The app checks the tune before writing (and can auto-fix checksum / layout issues). Pick a 512 KB BIN, or load factory **US S / JCW / GP1** as a base, then either a **quick write** (calibration region, ~60 KB) or a **full write** (512 KB).

![ECU flash — backup, factory software, flash options](/assets/images/r53-logger-play-store/flash-screen-factory.jpg){:.img-md}
*Flash screen — Read backup, pick a BIN, load US S / JCW / GP1, then Flash options before Quick or Full write.*

![Tune check dialog before write](/assets/images/r53-logger-play-store/flash-tune-check.jpg){:.img-md}
*If the BIN wouldn't be safe to write, you get a clear choice: Keep as-is or Fix tune and continue.*

![ECU flash screen with Flash options](/assets/images/r53-logger-play-store/flash-with-options.jpg){:.img-md}
*Earlier flash layout — same path: backup, options, write. Silver-cover facelift ECUs only.*

## Facelift ECU for sale — $275

Don't have a silver-cover facelift box? I sell them ready to drop in:

- **VIN swap** to your car
- **Immobilizer delete** so it runs without wrestling the factory EWS pairing
- **$275** for the prepared ECU

Shipping billed at actual cost. Email me your year/model and VIN and I'll confirm fitment and quote shipping.

**→ [matt@geekopolis.com](mailto:matt@geekopolis.com)** — subject something like "R53 facelift ECU".

Track / off-road use only; no warranty against engine or ECU damage — you're buying a prepared control unit for a 20-year-old supercharged car.

## Flash options (at write time)

Before a quick write, you can tweak the loaded tune **in memory** — no separate desktop editor required for the common bits. Toggle what you want; Save turns chili-red when something changed.

![Flash options — Track pedal and fan presets](/assets/images/r53-logger-play-store/flash-options-pedal-fan.jpg){:.img-md}
*Pedal map (Stock / Straight / Track) and cooling fan — Stock, Earlier 216/230 °F, or Even earlier 200/220 °F.*

![Flash options — pops, injectors, redline, pedal](/assets/images/r53-logger-play-store/flash-options.jpg){:.img-md}
*Same options list — pops, injectors, redline, pedal, idle, and fan all live here.*

- **Enable pops (decel crackle)** — with an aggressiveness slider from stock to max
- **Set injector size** — for when you've upsized injectors (stock S / JCW sizes and common bigger ones)
- **Set redline** — raises the hard rev limit; soft cut stays alone
- **Remap throttle pedal** — stock, straight (linear), or track feel
- **Set idle RPM (manual transmission)** — manuals only. Writes the same target across **cold/startup and warm** idle on normal idle, A/C on, and coasting back to idle so it stays consistent. Stock restores factory cold (1250) plus the warm schedules; raised presets for mild / street / race-cam / big-cam. Automatic cars: leave this off.
- **Cooling-fan kick-on temperature** — low and medium speeds only (high stays factory). Shown in **°F**. **Stock** 221 / 233 °F; **Earlier** 216 / 230 °F; **Even earlier** 200 / 220 °F for heat-soak / track use.

The summary line on the flash screen shows what's armed (`Pops off · Injectors unchanged · Idle unchanged · Fan unchanged · …`). The app auto-checks (and can auto-fix) the tune whenever options change the image, and again before write.

![ECU flash after a completed backup](/assets/images/r53-logger-play-store/ecu-flash-done.jpg){:.img-md}
*Backup complete — file saved on the phone, ready to send to your tuner or keep as your recovery copy.*

## Want to try it?

I'm putting it on the Play Store. If you want a crack at it — early install, feedback, or once the listing is up — **email me**:

**→ [matt@geekopolis.com](mailto:matt@geekopolis.com)**

Tell me you're an R53 owner (or tuner) and what you want — the app, a facelift ECU, or both. I'll reply with how to get the app and/or quote the ECU.

Instagram works too if you prefer: [**@mattryan6729**](https://www.instagram.com/mattryan6729/).

If you give it a weekend on the car, tell me what breaks and what's missing — that's how this thing got finished.
