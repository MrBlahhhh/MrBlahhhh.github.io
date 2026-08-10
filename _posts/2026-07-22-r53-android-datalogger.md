---
title: "R53 Logger - Flasher — Android app for the Mini"
date: 2026-07-22 00:00:00 -0400
categories: car tech
tags: [mini, r53, android, datalogger, flasher, obd, wideband, boost, tuning, ecu-flash, knock, esp32, shift-light]
cover: /assets/images/r53-android-logger/main-screen.jpg
lightbox: true
excerpt: "R53 Logger - Flasher: live engine logging, wideband AFR, autolog WOT pulls, 3D AFR tuning graph, per-cell fuel/trim/knock maps with knock events pinned to the map, wideband calibration, fault codes — and ECU backup / flash on facelift silver-cover boxes"
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-android-logger/main-screen.jpg
---

<!--more-->

The R53's factory diagnostics are twenty years old, and the generic OBD apps poll so slowly they're useless for anything beyond checking why the CEL is on. The good logging tools cost real money or live on a laptop. I wanted something I could plug in at a red light and actually *see* what the supercharged W11 was doing — and, on the same cable, back up and carefully flash a facelift ECU. So I built **R53 Logger - Flasher**.

![Main screen with live engine data](/assets/images/r53-android-logger/main-screen.jpg){:.img-md}
*Live readout at idle: RPM, boost, throttle, temps, per-cylinder spark, knock, injector duty, and more.*

## What it does

Plug the car into the phone, hit connect, and it streams live engine data fast enough to catch what happens mid-pull: RPM, boost, throttle position, coolant and intake temps, spark advance per cylinder, knock level, fuel trims, injector pulse width and duty cycle.

A wideband joins over Bluetooth and its air-fuel ratio gets merged into the same log, time-aligned with everything else — so I can see exactly what AFR the car was running at 6,000 RPM in third, instead of lining up two separate logs by hand afterwards.

Everything records to plain CSV files, shareable straight from the phone or opened in datazap.me for real analysis. And every value has been cross-checked against the professional BMW diagnostic tools on a running engine, so the numbers can actually be trusted.

## Autolog — it records my pulls for me

The feature I'm happiest with. Street pulls never line up with remembering to hit record, and even when they do, you're left scrubbing forty minutes of cruising for eight seconds of action.

Autolog fixes it: the app notices when I go wide-open-throttle and records the pull automatically, start to finish.

![Autolog settings screen](/assets/images/r53-android-logger/autolog.jpg){:.img-md}
*Set the thresholds once — the app captures every pull on its own from then on.*

- It grabs the moments **just before** I mat it, so the launch is never clipped.
- A quick lift between gears doesn't end the recording — the pull carries through the shift as one continuous run.
- It hangs on a few seconds after the final lift, so the full run-out is captured.

Every pull saves as its own file. No scrubbing, no clipping, no forgetting. Drive the car, and every WOT run is waiting when I stop.

## A live graph that actually helps

The built-in graph plots the last 30 seconds of everything at once, each channel colour-coded against fixed, meaningful ranges — the RPM trace reads to 7,500 with a dashed redline marker at 7,400, drawn thick on top where your eye needs it. Toggles let me focus on what matters: spark and knock during a pull, boost and throttle when chasing a leak.

![Live graph with headline values and channel toggles](/assets/images/r53-logger-play-store/live-graph-30s.jpg){:.img-md}
*The last 30 seconds of every channel, with the headline numbers up top. Export CSV or open in datazap.me from the bottom.*

## 3D AFR tuning graph — see where it runs lean

Tuning fuel means knowing which cells the engine visited and how far off target each one ran. A live graph shows *when* something happened; the 3D surface shows *where* in the fuel map the problem lives.

The tuning graph plots **actual AFR vs target** as a 3D surface across RPM and load. It ignores DFCO and tip-out leans so you're reading real combustion, not decel noise. Green means on-target; red/orange is lean. A target ramp (e.g. 12.8 at 2,000 rpm → 11.5 at 7,800 rpm, ±1.5) sits at the bottom for reference.

Play back any log, scrub with two-finger pan, swap axes with the channel selector — and when you find the cell that's off, you know exactly where to add fuel.

![3D AFR surface — visited cells, AFR error](/assets/images/r53-logger-play-store/3d-afr-graph-1.jpg){:.img-md}
*Surface plot of AFR error. Red peaks are lean cells — that's exactly where the fuel table needs work.*

The wideband calibration screen got its own upgrade too — set the ESP32 bridge hardware path (resistor divider or ADS1115) and controller curve (Innovate, AEM, or custom) from the phone, no serial terminal needed. Full details in the [Play Store writeup](/car/tech/2026/07/24/r53-logger-play-store.html).

## Fuel, trims, and spark — the whole map, colour-coded

The 3D surface is great for spotting a lean *peak*; sometimes I just want to read the numbers cell by cell. So the same log plays back as flat, colour-coded maps across RPM (columns) and load (rows), and one tap swaps between them.

**Fuel map.** Each cell shows how far AFR ran off target in open loop (`+` lean, `–` rich), and switches to closed-loop total trim (italic) where the ECU was correcting. Dim numbers are the tune's own value for reference, so a cell that's fighting the table jumps right out.

![Fuel map heatmap — AFR error and trims per cell](/assets/images/r53-android-logger/fuel-map.jpg){:.img-xl}
*Fuel map: green is on-target, blue is pulling fuel out, red is adding. The thin line traces the pull I've scrubbed to on the timeline.*

**Trims map.** Total fuel trim (STFT + LTFT) per cell, full colour at ±20%. Blue means the ECU is yanking fuel back out, red means it's piling it in, green is happy. A whole low-rpm column glowing blue is the map telling me where the tune runs rich before I've touched a single table.

![Trims map heatmap — STFT plus LTFT per cell](/assets/images/r53-android-logger/trims-map.jpg){:.img-xl}
*Total trim per cell — the blue band down low is where it's commanding fuel back out.*

## Knock, pinned to the cell that pinged

This is the part I built the maps *around*. Every one of these views — 3D AFR, fuel, trims, and the spark map — drops a **yellow dot on the exact cell where a knock event fired**, and the dots stay put for the whole session. Instead of "I think it rattled somewhere in third," I can see the precise RPM-and-load region that pings, laid right over the fuel and timing there.

![3D AFR surface with knock events marked](/assets/images/r53-android-logger/afr-3d-knock.jpg){:.img-lg}
*The AFR surface with knock events dotted on. The lean cluster up top and the knock dots landing in the same neighbourhood is exactly the story you want to catch.*

A running per-cylinder tally sits in the corner all session — `cyl4 ×67  cyl2 ×22  cyl3 ×14  cyl1 ×8` — so a single cylinder doing all the complaining (number 4, here) is obvious at a glance instead of buried in the log.

The dedicated **spark map** shows timing where it matters: each visited cell reads degrees off the tune's own spark table, and cells where the ECU *pulled* timing go red. Dim numbers are the map target. Red cells stacked with yellow dots means knock and timing-pull are agreeing with each other — that's the corner of the map that needs the work.

![Spark / knock map — timing pulled off the tune, per cell](/assets/images/r53-android-logger/knock-map.jpg){:.img-xl}
*Spark map: red cells are where timing got pulled off the tune, yellow dots are knock events. They cluster in the same place for a reason.*

The live readout backs it all up in real time — knock level and energy, retard in degrees, and the detector's noise-floor voltage, so I know it's calibrated to the engine's own mechanical racket instead of chasing ghosts.

## It reads fault codes too

Since it's already talking to the car, it doubles as a diagnostics tool: reads stored and pending fault codes, translates them to plain English with R53-specific notes where it counts (lean code? check boost and vacuum leaks first; that P0116? classic R53 thermostat), and clears them with a tap once the fix is in. If I try to clear codes with the engine running, it tells me why the car refused in plain English instead of just failing.

## ECU backup, flash, and flash-time options

Same K+DCAN cable, same app: read a full 512 KB ECU backup and (carefully) write a tune back — **facelift silver-cover ECUs only**. Before a quick write I can layer common changes onto the loaded BIN on the phone: **pops**, **injector size**, **redline**, **throttle pedal map**, idle, and fan kick-on. Details, screenshots, and how to get the app are in the [Play Store writeup for R53 Logger - Flasher](/car/tech/2026/07/24/r53-logger-play-store.html).

## One ESP32, two jobs: wideband bridge meets shift light

The wideband side has quietly grown up. A small **ESP32 bridge** reads the analog wideband, streams AFR to the app over Bluetooth LE, and the app merges it into the log time-aligned with everything else. When the bridge isn't around, the app just says so and keeps retrying in the background instead of hanging — you can see that state called out at the top of the main screen.

![Main screen — build 236, wideband status and full live readout](/assets/images/r53-android-logger/main-screen-wideband.jpg){:.img-lg}
*Live readout with the wideband merged in (AFR carries its own freshness age), and the K-Line / wideband connection state spelled out plainly up top.*

Two small quality-of-life wins from the latest builds: the active controller curve gets stamped into each log's filename (`…_aem.csv` for the AEM curve), so I never have to guess which wideband a log came from, and CSV export is back to a plain, datazap-friendly format.

Here's the part that isn't public *yet*: that same ESP32 is growing into the [CAN-bus shift light](/car/tech/2026/07/11/r53-esp32-shift-light.html) I built earlier — **one board reading the wideband and driving the WS2812B bar at once**, AFR to the phone and RPM to the LEDs off the same chip. It's on the bench and in testing right now; once I've shaken the bugs out I'll push the firmware and wiring to the existing repo: [esp32-shift-light-R53-mini](https://github.com/MrBlahhhh/esp32-shift-light-R53-mini).

## Why build it myself

Nothing out there did the whole job. The generic apps are too slow to tune with, the Mini-specific tools are desktop relics, and none of them merge wideband AFR with engine data, auto-record pulls, show a 3D AFR surface, pin every knock event to the exact map cell, *and* flash a facelift box from the phone. Now **R53 Logger - Flasher** does all of it — and every drive is a chance to learn something new about what the old supercharged four is up to.
