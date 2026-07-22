---
title: "Trackday Pyrometer Helper Is Live on Google Play"
date: 2026-07-22 00:00:00 -0400
categories: car tech
tags: [android, app, play-store, pyrometer, tpms, corner-balance, ble, track-day]
cover: /assets/images/pyrometer-helper-manual/hero.png
lightbox: true
excerpt: "My Android track-day app — pyrometer, TPMS, and corner-balance scales in one place — is out on the Play Store. On-device, no accounts, and I'm giving the TPMS and ProForm scale BLE code away for free."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/pyrometer-helper-manual/hero.png
---

<!--more-->

It's live. **Trackday Pyrometer Helper** is out on the Google Play Store — the one-screen-per-job Android app I've been building and dogfooding at every track weekend for months.

> ### ▶ [Get it on Google Play](https://play.google.com/store/apps/details?id=com.trackdaypyrometerhelper&hl=en_US)

## What it does

One app for three jobs I used to juggle on paper and three separate gadgets:

- **Tire temps** — OUT / MID / IN across all four corners from my custom [PyroTC BLE pyrometer](/car/tech/2026/06/18/trackday-pyrometer-helper.html), turned into camber and pressure advice from the spread.
- **Tire pressures** — cold and hot PSI from cheap BLE TPMS sensors (Zeepin and DJTPMS), plus an **all-day background monitor** that logs pressure and temperature and auto-counts heat cycles.
- **Corner balance** — cross-weight from ProForm wireless scale pads streamed live over Bluetooth, with "move this much onto the diagonal" recommendations.
- Tire inventory with heat-cycle tracking, resettable packing checklists, CSV export, and full JSON backup. **Everything is on-device — no accounts, no cloud.**

## Read the manual

Full setup and workflow, start to finish — Bluetooth, the color-coded TPMS system, temp sessions, the background monitor, and the scale-pad setup, all with screenshots:

**→ [Trackday Pyrometer Helper — User Manual](/car/tech/2026/07/12/trackday-pyrometer-helper-manual.html)**

## I want your feedback

If you run track days on Android, please try it and tell me what's confusing, what breaks, or what's missing. Bug reports and "this workflow is dumb" notes are exactly what I'm after — this thing got good by getting used and complained about.

**UI and feature suggestions → [matt@geekopolis.com](mailto:matt@geekopolis.com).**

## Free code: TPMS + ProForm scale BLE integration

Two of the hardest parts to build were the **Bluetooth integrations**, and I'm happy to give them away:

- **TPMS integration** — decoders for **Zeepin / TPMSII** and **DJTPMS** BLE sensors: pressure, temperature, battery, protocol auto-detected per sensor.
- **ProForm wireless scale integration** — the reverse-engineered BLE wake / map / calibrate / live-stream protocol for the corner-weight pads.

If you're building your own app or just tinkering and want either one, reach out — **I'll share the code for free and help you get it installed.** The pyrometer hardware is open too: the [PyroTC firmware](https://github.com/MrBlahhhh/PyroTC) is already public.

Easiest ways to reach me: [matt@geekopolis.com](mailto:matt@geekopolis.com), or DM [**@mattryan6729**](https://www.instagram.com/mattryan6729/) on Instagram.

If you give it a run this weekend, let me know how it goes. 🏁
