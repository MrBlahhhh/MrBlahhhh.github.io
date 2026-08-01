---
title: "MXR4000GT4 Control — A Clean App for My Inverter Generator"
date: 2026-08-01 00:00:00 -0400
categories: tech
tags: [android, app, generator, ble, mxr4000gt4, maxpeedingrods, garage]
cover: /assets/images/mxr4000gt4-generator-app/dashboard-power.jpg
lightbox: true
excerpt: "A no-ads Android client for the MaXpeedingRods MXR4000GT4 — live watts, fuel estimate, and remote START/ECO. Play Store release is on the someday list."
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

It starts from a simple full-tank baseline (~3 hours at ~1800 W) and learns a burn rate on the phone as you run. There's a rolling chart for the last twenty minutes, and a background monitor that can ping you when fuel gets low. Export dumps the usage log if you want to keep a record.

## Controls and themes

START and ECO are toggles on the same screen. Theme is System / Light / Dark — dark is what I leave it on in the garage.

![START/ECO toggles, theme chips, export and disconnect](/assets/images/mxr4000gt4-generator-app/controls-theme.jpg){:.img-md}
*Remote START and ECO, theme preference, log export, and disconnect — serial at the bottom for confirmation.*

## What's next

Still polishing against real runs — remaining-time accuracy under different loads, reconnect behavior, and a cleaner first-connect flow. When it's ready for other MXR4000GT4 owners, I'll put it on Play closed testing the same way I did the track-day apps.

Questions or want an early install when testing opens:

**→ [matt@geekopolis.com](mailto:matt@geekopolis.com)** or [**@mattryan6729**](https://www.instagram.com/mattryan6729/) on Instagram.
