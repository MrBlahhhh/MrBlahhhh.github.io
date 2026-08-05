---
title: "Mini R53 Logger - Flasher — Heading to the Play Store"
date: 2026-07-24 00:00:00 -0400
categories: car tech
tags: [mini, r53, android, datalogger, flasher, play-store, boost, tuning, ecu-flash, immobilizer, for-sale]
cover: /assets/images/r53-logger-play-store/main-screen.jpg
lightbox: true
excerpt: "R53 Logger - Flasher on Play — 3D AFR tuning graph, wideband calibration, live logging + ECU flash (pedal, idle, fan, pops…). Join r53-logger-testers; facelift silver-cover write only; prepared ECUs $275."
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

**Mini R53 Logger - Flasher** — the Android app I've been building so I can *log* what the supercharged W11 is doing and *flash* a facelift ECU from the same phone — is feature-complete, and I'm loading it onto the Google Play Store.

If you want to try it when the listing goes live (or before, if I can get you an early install), email me and I'll sort you out.

![Main screen — live telemetry and Mini chili-red theme](/assets/images/r53-logger-play-store/main-screen.jpg){:.img-md}
*Plug in, hit connect, and the car streams live: RPM, boost, temps, spark, knock, fuel trims, injection — the works.*

## What you get

Same idea as the [earlier Logger - Flasher writeup](/car/tech/2026/07/22/r53-android-datalogger.html), now finished:

- **Live engine data** fast enough for a real pull — boost, throttle, temps, per-cylinder spark, knock, MAF, injector duty, short/long fuel trim
- **Wideband AFR** over Bluetooth, merged into the same time-aligned CSV
- **Autolog** — arms on WOT, keeps the pull through gear changes, saves each run as its own file
- **Live graph** with channel toggles, fixed ranges, and a redline marker on the RPM trace — 30-second window, export straight to datazap.me
- **3D AFR tuning graph** — 3D surface plot of actual AFR vs target across RPM and load, ignoring DFCO and tip-out leans. Play back any log, scrub with 2-finger pan, export CSV or open in Datazap
- **Wideband calibration** — ESP32 bridge streams 0–5 V sensor output; pick resistor divider (easiest) or ADS1115 I2C (more accurate), then Innovate / AEM / custom curve. No laptop, no serial terminal — configured and saved from the phone
- **Diagnostics** — read and clear fault codes with R53-specific notes (chassis modules stay on BMW hex titles — no fake SAE crosswalk)
- **Channels & poll rate** — turn off blocks you don't care about so the ones you do watch update faster
- **ECU backup / flash** — read a full 512 KB backup, send it to your tuner, and (carefully) write a tuner BIN back — **facelift silver-cover ECUs only**
- **Flash options** — before a quick write, optionally layer on pops, injector size, redline, throttle-pedal map, idle RPM (manual, cold + warm), and cooling-fan kick-on including an even-earlier 200 / 220 °F preset (same silver-cover limit)

There's a separate companion app for **module coding** (BC1, EMS, airbag, …) — see [R53 Coding](/car/tech/2026/07/26/r53-coding.html).

Everything records to plain CSV and shares straight into datazap.me. Numbers were cross-checked against professional BMW diagnostic tools on a running engine.

![Live graph — 30-second window, channel toggles, datazap export](/assets/images/r53-logger-play-store/live-graph-30s.jpg){:.img-md}
*Live graph — 30s rolling window. Toggle the channels you care about; export CSV or open in datazap from the bottom.*

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

## 3D AFR Tuning Graph

The live graph shows what's happening *right now*, but tuning fuel means understanding what already happened — every cell the engine visited, and how far off target it ran. The 3D tuning graph plots **actual AFR minus target** as a surface across RPM and load, so a peak in the red means the car went lean right where you need the fuel.

![3D AFR tuning graph — surface plot of AFR vs target](/assets/images/r53-logger-play-store/3d-afr-graph-1.jpg){:.img-md}
*Surface plot: visited cells coloured by AFR error. Green is on-target; red/orange means lean — exactly where the fuel table needs work.*

The graph ignores DFCO and tip-out leans so you're looking at real combustion events, not decel artifacts. A target line sits at the bottom — e.g. 12.8 at 2,000 rpm tapering to 11.5 at 7,800 rpm (±1.5) — and the surface shows every cell's deviation from that ramp.

- **2-finger pan** to scrub through the log; **double-tap** resets the view
- **Play / 4× speed** with elapsed time — drop in anywhere and watch the surface build
- **Channel selector** — RPM, MAP, Boost, Throttle, MAF, Engine Temp, Intake Air Temp, Spark Adv (with per-cylinder), so you can swap axes or narrow to what matters
- **Window / Reset / CSV / Datazap** along the bottom — same export pipeline as the live graph

![Another angle of the 3D surface](/assets/images/r53-logger-play-store/3d-afr-graph-2.jpg){:.img-md}
*Rotate and zoom — the surface makes it obvious which RPM/load cells consistently run off-target.*

## Wideband calibration

The wideband doesn't just show up — you have to tell the app which sensor is wired to the bridge and what voltage curve it follows. The calibration screen handles all of it in one place, no laptop required.

![Wideband calibration — ESP32 bridge and controller curve](/assets/images/r53-logger-play-store/wideband-calibration.jpg){:.img-md}
*Pick the hardware path (resistor divider or ADS1115), then the controller curve — Innovate, AEM, or custom endpoints.*

**Hardware method** — how the ESP32 bridge reads the sensor's 0–5 V analog output:

- **Resistor divider (ESP32 ADC) — easiest:** two equal resistors (e.g. 10k+10k) form a divider — wideband out → 10k → mid → 10k → GND, mid to GPIO34. Common ground with the controller. Works with nothing but the ESP32.
- **ADS1115 I2C ADC — more accurate:** 16-bit external ADC over I²C for when you want the last tenth of an AFR point.

**Controller curve** — translate volts to AFR:

- **Innovate LC-1 / MTX-L** — 0.00 V → 7.35 AFR, 5.00 V → 22.39 AFR
- **AEM (0.5–4.5 V → 8.5–18 AFR)** — the most common aftermarket curve
- **Custom** — enter your own voltage/AFR endpoints for anything else

The bridge firmware is open source (ESP32, streams sensor volts over BLE). Reflash it once, then calibrate and save from the phone — the app remembers your settings across sessions.

The current build (172) defaults to the AEM curve. If you're running an Innovate or something custom, set it once and it sticks.

## Want to try it? (Play closed testing)

R53 Logger - Flasher uses **Google Play closed testing**, same pattern as R53 Coding.

### 1. Ask to join the tester group

**→ [r53-logger-testers](https://groups.google.com/g/r53-logger-testers)**  
(`r53-logger-testers@googlegroups.com`)

Join is **approval-required**. Include the Google account on your phone and whether you want the app, a facelift ECU, or both.

### 2. Opt in on Play (same Google account)

After you're approved, open the **closed testing join link** I send (also posted for members), tap **Become a tester**, then install from Play — don't sideload if you want purchases / unlock to work.

### 3. Still email / Instagram

Questions, ECU quotes, or stuck on signup: **[matt@geekopolis.com](mailto:matt@geekopolis.com)** or [**@mattryan6729**](https://www.instagram.com/mattryan6729/).

If you give it a weekend on the car, tell me what breaks and what's missing.
