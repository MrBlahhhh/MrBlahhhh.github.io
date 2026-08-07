---
title: "MXR4000GT4 Control — A Clean App for My Inverter Generator"
date: 2026-08-01 00:00:00 -0400
categories: tech
tags: [android, app, generator, ble, mxr4000gt4, maxpeedingrods, garage]
cover: /assets/images/mxr4000gt4-generator-app/dashboard-power.jpg
lightbox: true
excerpt: "A no-ads Android client for the MaXpeedingRods MXR4000GT4 — live watts, a load-aware fuel estimate, surge capture, and a maintenance log. Built it to chase six unexplained shutdowns, and it found the culprit."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/mxr4000gt4-generator-app/dashboard-power.jpg
---

<!--more-->

## Why I built it

I run a **MaXpeedingRods MXR4000GT4** inverter generator around the garage and for weekend power. The factory app works, but it's the usual package — ads, cloud account, and more UI than I want when I'm just checking load and fuel.

**MXR4000GT4 Control** is my own Android client for that unit. Bluetooth to the generator, live gauges on the phone, no vendor account. Source stays private for now; a Play Store release is on the someday list.

Personal project for hardware I own — not affiliated with MaXpeedingRods.

## Live dashboard

Connect and you get the basics at a glance: power, fuel, voltage, and frequency.

![Dashboard — watts dial, fuel ring, voltage and frequency](/assets/images/mxr4000gt4-generator-app/dashboard-power.jpg){:.img-md}
*~1.4 kW on the dial, peak marked on the rim. Fuel at 15% goes red so you don't miss it.*

The big dial is **watts**, with a sticky peak arrow on the outer ring — long-press clears it after a pull or a heavy tool run. Fuel is a ring gauge (green → yellow → red). Voltage and frequency sit beside it so you can sanity-check the AC output without walking over to the panel.

## Fuel estimate

The part I actually wanted: **how long until empty**, not just a percentage.

![Fuel estimate and last-20-minute chart](/assets/images/mxr4000gt4-generator-app/fuel-estimate.jpg){:.img-md}
*Remaining time up top, fuel and time-left over the last 20 minutes below. Low-fuel warning when you're under 10%.*

The first version used one flat burn rate, and it was badly wrong at light load — 627 W on the dial and the app told me two and a half hours when the real answer was closer to five. A generator idling at a few hundred watts is not burning fuel at the same rate as one pushing 1800 W, and a single number can't say otherwise.

Burn is modelled against load now: a fixed idle share plus a slope that scales with watts, anchored so 1800 W still lands on the ~3-hour full tank the manual implies. Learning happens on top of that as a calibration multiplier — when the tank gauge finally steps down, the app compares what actually burned against what the model predicted at that step's average load, and nudges the whole curve. A step observed at any load improves the estimate at every load, which the flat version couldn't do.

The caption on the card tells you which one you're looking at: *modeled*, or *learned* with the multiplier once there's enough data. The chart now carries watts as a third line, so you can see load and fuel drop against each other rather than guessing at the correlation.

The idle share is my assumption, not a published figure — it's the first constant I'll re-derive once I have enough logged tank cycles to fit it properly.

## Chasing six shutdowns

Somewhere in here the generator started stopping on its own. Six times, no pattern I could see, always under a load it should have handled.

The app could tell me *when* it died and never *why*, so I fixed that first: the log now records fault state, the controller's own status, voltage and frequency alongside the watts, and the dashboard shows a red banner when the unit reports a protection trip. Anything that changes gets written the moment it changes rather than waiting for the next sample.

That turned out to matter more than I expected, because the thing that was killing it is invisible at a 30-second sampling interval.

A motor load kicking on pulled **4117 W for about a second**, with line voltage sagging to 111 V, then settled back to ~1600 W like nothing happened. It appeared in exactly one Bluetooth frame out of 2236. The sampled log topped out at 1805 W — the spike simply wasn't in it. That peak is at or past what this unit is rated to surge, and a compressor cycling on and off would explain every one of the six stops: the controller logs no fault, and the shutdown plus auto-restart both fit inside a single sampling gap.

So there's a surge log now. Anything over 3000 W or under 115 V gets recorded as one event — peak watts, minimum volts, how long it lasted, and the load either side of it — and it shows up as **Recent surges** on the dashboard, red past 3500 W. It's pinned to the same timeline as the usage samples so surges and stops can be lined up against each other. I replayed the captured frames through it to confirm it catches the real event and doesn't invent extras.

Not a solved problem yet, but it's the first time I've had evidence instead of a theory.

## Maintenance log

The other thing a generator needs and no app gives you: a service record that counts **running hours**, not calendar days.

It reads the machine's own hour meter, so an entry is stamped with real engine time. Log an oil change from the Maintenance card and you get total hours, hours since the last change, and a delete for mis-taps. The last meter reading is cached, so you can still log a service correctly while you're nowhere near the generator.

The oil reminder escalates — 25, 35, then 45 hours — instead of firing once at a single threshold and being ignored. Each step notifies once, and the 45-hour one won't dismiss. This engine holds about 0.45 L, which is not much margin for the continuous loads I actually run it under, so I'd rather be nagged. With nothing ever logged it falls back to the machine's lifetime hours, on the theory that an engine with no records isn't a fresh one.

That interval is general small-engine practice, not a MaXpeedingRods spec. Use your own judgement.

## Controls and themes

START and ECO are toggles on the same screen. Theme is System / Light / Dark — dark is what I leave it on in the garage.

![START/ECO toggles, theme chips, export and disconnect](/assets/images/mxr4000gt4-generator-app/controls-theme.jpg){:.img-md}
*Remote START and ECO, theme preference, log export, and disconnect — serial at the bottom for confirmation.*

Export is a zip now, and it carries the surge and maintenance logs along with the usage samples.

It also has a proper icon — a stylised MXR4000GT4, white side panel with the red livery slashes, tubular handle and base rail. All vector, generated from a script so the geometry has one source rather than being redrawn by hand at five sizes.

## What's next

Most of the last week was a bug-fix pass rather than features, and a few of them mattered: a surge that was still in progress when the engine stopped — precisely the event the log exists to catch — was being thrown away instead of written. A refused Bluetooth write could wedge the command queue silently, so START and ECO would go nowhere while the UI still claimed it was connected and ready. And if the generator gets re-paired to another phone, the app now tells you that's what happened instead of quietly showing you garbage.

Still chasing the shutdowns — that's the open question, and now I have surge data to chase it with. After that: remaining-time accuracy across more tank cycles, reconnect behavior, and a cleaner first-connect flow. When it's ready for other MXR4000GT4 owners, I'll put it on Play closed testing the same way I did the track-day apps.

Questions or want an early install when testing opens:

**→ [matt@geekopolis.com](mailto:matt@geekopolis.com)** or [**@mattryan6729**](https://www.instagram.com/mattryan6729/) on Instagram.
