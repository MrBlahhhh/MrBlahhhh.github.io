---
title: "Pip-Boy Check Engine Display — Haltech CAN Warnings"
date: 2026-06-19 00:00:00 -0400
categories: car tech
tags: [esp32, can-bus, haltech, bmw, 135i, e82, pip-boy, telemetry]
cover: /assets/images/pipboy-check-engine-display/pipboy-installed.jpg
lightbox: true
excerpt: "Pip-Boy display for Haltech CAN warnings"
article_header:
  type: overlay
  theme: dark
  background_image: false
---

<!--more-->

The [135i track build](/car/2025/10/15/135i-l92-track-build.html) runs a **Haltech Rebel LS** with the factory BMW dash still in place — speed, temps, and the usual cluster stuff all work fine. What it does **not** show are Haltech-side warnings: high oil temp, low oil pressure, and other fault states the ECU knows about but never forwards to the BMW cluster.

This is a small **Pip-Boy-style display** mounted in the driver-side vent as a **second readout**, fed from the Haltech CAN bus over WiFi. Same car, two dashes — factory cluster for driving, Pip-Boy for the stuff Haltech won't put on the BMW screen.

## How it works

A **RaceCapture ESP32-CAN-X2** ([Autosport Labs](https://wiki.autosportlabs.com/ESP32-CAN-X2)) sits on the Haltech CAN network, reads the channels I care about, and sends updates wirelessly to the display module. The screen is a separate **Waveshare ESP32-S3-Touch-AMOLED-2.41** — 2.41″ square AMOLED at **600×450**, capacitive touch, WiFi/BLE — running a Pip-Boy boot sequence and alert UI.

On track, if oil pressure or oil temp crosses a limit, the display flashes the warning immediately.

<div>{%- include extensions/youtube.html id='4_iXQk7Ijtc' -%}</div>

Boot sequence and normal operation — Pip-Boy aesthetic, real Haltech data behind it.

## Display hardware

![Pip-Boy display mounted in the driver-side vent](/assets/images/pipboy-check-engine-display/pipboy-installed.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Waveshare 2.41″ AMOLED in the vent gauge pod — USB power routed through the mount.*

The dev board is a [Waveshare ESP32-S3-Touch-AMOLED-2.41](https://www.waveshare.com/esp32-s3-touch-amoled-2.41.htm): RM690B0 AMOLED over QSPI, FT6336 capacitive touch on I2C, QMI8658 IMU and RTC on board. USB-C for power and programming.

## Dash mount

![Fusion 360 — vent gauge pod bracket](/assets/images/pipboy-check-engine-display/pipboy-cad.png){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Bracket modeled around the 2.41″ AMOLED module — replaces the vent louver section and keeps airflow slots.*

![Gauge pod bracket — printed part](/assets/images/pipboy-check-engine-display/gauge-pod-bracket.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Square gauge pod bracket — snap-fit geometry for the E82 driver-side vent.*

![RAM mount adapter for the vent opening](/assets/images/pipboy-check-engine-display/ram-mount-adapter.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Separate RAM-ball adapter if you want an adjustable arm instead of the fixed pod.*

![Vent mount with gauge pod and RAM base installed](/assets/images/pipboy-check-engine-display/vent-gauge-installed.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Both mounts fit the E82/128i/135i driver-side top vent. RAM ball for phone or secondary gauge; pod bracket for the Pip-Boy screen.*

Two 3D-printed parts, both PETG or ASA minimum for in-car heat:

| Part | Link | Price |
|------|------|-------|
| **E82 vent gauge square gauge pod bracket** (LCD mount) | [Cults3D — free STL](https://cults3d.com/en/3d-model/tool/e82-vent-gauge-square-gauge-pod-bracket) | Free |
| **E82 driver left-side vent RAM mount** | [Cults3D — RAM ball adapter](https://cults3d.com/en/3d-model/various/e82-drivers-left-side-vent-ram-mount) | Paid |

The free bracket is sized for the Waveshare 2.41″ AMOLED module. The RAM mount is a vent insert with a standard RAM ball — useful on its own or combined with the pod above it.

## Parts list

| Item | Notes |
|------|-------|
| [Waveshare ESP32-S3-Touch-AMOLED-2.41](https://www.waveshare.com/esp32-s3-touch-amoled-2.41.htm) | 2.41″ AMOLED 600×450, touch, ESP32-S3 |
| **RaceCapture ESP32-CAN-X2** | [Autosport Labs ESP32-CAN-X2](https://wiki.autosportlabs.com/ESP32-CAN-X2) — dual CAN, automotive power, WiFi/BLE |
| **Haltech Rebel LS** | Already in the 135i — CAN source for oil pressure, oil temp, fault flags |
| **E82 vent gauge pod bracket** | [Free STL on Cults3D](https://cults3d.com/en/3d-model/tool/e82-vent-gauge-square-gauge-pod-bracket) — print PETG/ASA |
| **E82 vent RAM mount** (optional) | [Cults3D listing](https://cults3d.com/en/3d-model/various/e82-drivers-left-side-vent-ram-mount) |
| **RAM arm + phone holder** (optional) | Standard 1″ RAM components if using the RAM vent mount |
