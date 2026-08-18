---
title: "R53 Logger - Flasher — the Garage, the CAN bridge, and closing the tuning loop"
date: 2026-08-18 00:00:00 -0400
categories: car tech
tags: [mini, r53, android, datalogger, flasher, wideband, aem, esp32, can, tuning, ecu-flash, afr]
cover: /assets/images/r53-logger-august/afr-3d-surface.png
lightbox: true
excerpt: "A month of near-daily builds: per-car Garage, an ESP32 CAN bridge with AEM wideband, per-block poll rates, AFR-vs-target analysis in the app, and the Python tooling that turns a log into the next flash."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-logger-august/afr-3d-surface.png
---

<!--more-->

When [R53 Logger - Flasher went to the Play Store](/car/tech/2026/07/24/r53-logger-play-store.html) in July it was a logger that could also flash. A month and roughly 370 commits later it's become the whole tuning loop: log a pull, see exactly where fueling missed the target, change one thing, flash it, log again — all from the phone, with every file filed against the car it came from.

![Main screen at build 299](/assets/images/r53-logger-august/main-screen.png){:.img-md}
*Build 299. Same one-thumb layout, but the Garage is now front and center — and the status line tells you which car everything you're about to do belongs to.*

## The Garage — because one phone, two Minis

The moment a second R53 showed up, the flat file list stopped working. Whose backup is `ecu_backup_163021.bin`? Which car's log am I about to graph? Get that wrong with *logs* and you waste an evening. Get it wrong with a *flash* and you write one car's tune into another car's ECU.

So everything is per-car now. The app identifies the car by the last seven of the VIN, and every bin, log, and fault read files under it. When a file arrives that the app can't attribute — say a backup read before the cluster answered — it asks, once, instead of guessing:

![Which car is this?](/assets/images/r53-logger-august/garage-which-car.png){:.img-md}
*Attribution is explicit. A file never silently lands on the wrong car.*

Each car's Garage page shows what's actually **in force** on the ECU right now — the last thing flashed or read — plus its backups, manual picks, fault history, and logs, each one shareable from the phone:

![Garage — bins and logs for one car](/assets/images/r53-logger-august/garage-bins-logs.png){:.img-md}
*READ, FLASH and MANUAL bins tracked separately, logs listed with dates and sizes — and the 173 orphan files from before the Garage existed, waiting to be attributed or deleted rather than quietly mixed in.*

The whole Garage exports to a zip and imports on another phone. Import never overwrites: an existing file lands beside it with a `~1` suffix, and importing the same archive twice changes nothing.

![Logs filtered to one car](/assets/images/r53-logger-august/logs-per-car.png){:.img-md}
*The Logs screen shows this car's sessions. The 12 logs from the other car are one tap away, not mixed in.*

## The ESP32 bridge — CAN and a real wideband

The R53's K-line is the bottleneck: every channel you poll costs time on a 20-year-old serial bus. The new ESP32 bridge takes some of that load off — it reads the car's CAN side and streams an AEM wideband, and the app merges it all into one time-aligned log over BLE.

The wideband path deserved its own screen. The bridge streams the controller's analog output, and the app converts volts to AFR with the controller's actual curve — Innovate, AEM UEGO/X-Series, 14point7, or custom endpoints:

![Wideband calibration](/assets/images/r53-logger-august/wideband-calibration.png){:.img-md}
*Pick how the board reads the analog out, pick the controller curve, and trim out the divider's few percent of error by typing in what a meter reads at the gauge — one tap matches the app to the meter.*

Somewhere in here I also fixed the "wideband dropouts" I'd been blaming on the hardware for weeks. The sensor was fine. The bridge was fine. The app was throwing away samples whenever they arrived faster than it drained them. The fix was in my own buffer handling — a good reminder that when data goes missing, suspect your own code before the sensor.

There's a bridge debug console in the app now too — pull the ESP's log, watch raw CAN frames, reboot it, all over BLE — because debugging a box that lives in the engine bay from a laptop got old.

## Channels & poll rate — spend the K-line where you're tuning

Every block the app polls has a cost, so now you choose: **Fast** (every cycle), **Slow** (round-robin), or **Off**. Turning off what you don't need speeds up what you keep — and the changes apply live, mid-log.

![Channels and poll rate](/assets/images/r53-logger-august/channels-poll-rate.png){:.img-md}
*Per-block rates, and each block says where it's coming from. With the bridge connected, the app tells you what CAN does and doesn't carry.*

The CAN presets push this further: hand RPM, throttle and engine temp to the bridge, and spend the freed K-line time entirely on spark, knock, MAF, injection and trims. The app states the trade before it applies — this mode costs you boost logging, and it refuses to record a log with no RPM source at all:

![CAN presets](/assets/images/r53-logger-august/channels-can-presets.png){:.img-md}
*Presets state their trade-offs instead of surprising you. And for the curious: raw block capture saves every ECU block verbatim, for working out where an unknown value lives.*

## Closing the loop — how we actually tune with it

This is the part that used to live on a laptop. The [July post](/car/tech/2026/07/22/r53-android-datalogger.html) covered the analysis views themselves — the per-cell fuel, trim and spark maps, and knock events pinned to the exact cell that pinged. What's new is that they're wired into a complete workflow. The methodology is simple to state: **decide what AFR the engine should run, measure what it actually ran, and let the difference tell you what to change.**

You set a target AFR curve — two points, linear between them:

![Target AFR curve](/assets/images/r53-logger-august/target-afr-curve.png){:.img-md}
*Richer as revs climb. The wideband lag setting shifts the AFR trace forward in time so a reading lands on the cells that actually fueled it — exhaust transit plus sensor response is real, and ignoring it smears every pull.*

Then you make a pull and look at what happened:

![A pull in the live graph](/assets/images/r53-logger-august/live-graph-pull.png){:.img-md}
*Third gear to 7,200. RPM, spark advance, knock, trims and AFR on one time axis — tap anywhere to read the exact values at that instant.*

<div>{%- include extensions/youtube.html id='NA8mJ3ce8yA' -%}</div>

The AFR 3D surface is where it comes together — measured AFR versus target, over RPM and load, with the target curve you set driving the colouring. Green is on target. The hot ridge is where it ran lean of the curve; that's where the next fueling change goes:

![AFR error surface over RPM and load](/assets/images/r53-logger-august/afr-3d-surface.png){:.img-md}
*Open loop shows AFR against the target curve; closed loop shows total trim. One glance says which cells need fuel — no exporting, no spreadsheet.*

The loop is: log the pull, read the surface, make **one** change, flash it from the same phone, and log the next pull. When the surface goes green and the knock trace stays quiet, you're done. The discipline of one-change-at-a-time is the entire method — the tooling just makes each iteration take minutes instead of an evening.

## The Python side

Not everything belongs on a phone. Off the car, a set of Python tools does the heavier analysis: chewing through a session's CSV and turning it into a short list of recommendations, auditing a bin's rev-limit configuration, and diffing two tunes to confirm a change did only what it claimed. The app's CSV format is the interchange — every log shares straight into the pipeline. The scripts encode a lot of hard-won specifics about this ECU, so they're staying private, but the shape of the workflow is the point: the phone measures, the scripts deliberate, the phone flashes.

## Flash safety, since we're flashing more

Iterating faster means flashing more, and the flash path got hardened to match: nothing can take the cable away from a write in progress — logging, VIN reads, everything else waits. A backup read tells you which step failed and retries a security-locked ECU instead of giving up. The app checks the tune before every write and asks before a freshly flashed bin becomes the analysis baseline. And the beta 62.5k logging link now survives things that used to drop it, like stopping a log.

## Get it

**Mini R53 Logger - Flasher** is on the [Play Store](https://play.google.com/store/apps/details?id=com.geekopolis.r53logger). It logs on any R53; flashing supports the facelift silver-cover ECU. Same K+DCAN cable as before, and the ESP32 bridge is optional — everything above except the CAN channels and wideband works with just the cable.
