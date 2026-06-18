---
title: "My First Android App, TrackDayPyrometerHelper"
date: 2026-06-18 00:00:00 -0400
categories: car tech
tags: [android, esp32, pyrometer, proform, tpms, ble, track-day]
cover: /assets/images/trackday-pyrometer-helper/tiretemp-home.jpg
lightbox: true
article_header:
  type: overlay
  align: center
  theme: dark
  background_image:
    src: /assets/images/trackday-pyrometer-helper/tiretemp-home.jpg
---

What started as a project to read my ProForm corner scales over Bluetooth turned into a full track-day setup tool — a custom ESP32 pyrometer to replace my broken gun, TPMS sensors for hot and cold pressures, and my first Android app to tie it all together.

<!--more-->

## How it started

I wanted live corner weights from my **ProForm wireless scale pads** on my phone instead of juggling the factory display. Reverse-engineering the BLE protocol got the four pads waking, mapping to FL/FR/RL/RR, and streaming live weights. That was useful on its own, but every other piece of data I cared about at the track still lived on paper, in separate apps, or on hardware that was dying.

So the scope grew: corner balance with cross-weight math and perch recommendations, tire temp sessions with inside/middle/outside per corner, and BLE TPMS for cold/hot pressures — all in one app built around the LS3 135i.

The Android source is private for now. I'm aiming for the Play Store next month. The ESP32 pyrometer firmware is public: [MrBlahhhh/PyroTC](https://github.com/MrBlahhhh/PyroTC).

## TrackDayPyrometerHelper

The app has two main tabs — **TIRES** and **BALANCE** — under per-car profiles (tire targets, camber, sessions, scale calibrations). Everything stays on the device. No accounts, no analytics, no backend.

### Corner balance (ProForm scales)

![ProForm scales wake and map screen](/assets/images/trackday-pyrometer-helper/livescales-setup.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Wake each pad over BLE, map to a corner, calibrate, then zero with the car off the pads.*

![Live corner weights from four scale pads](/assets/images/trackday-pyrometer-helper/livescales.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Live mode streams all four corners. Manual mode works when you only have corner numbers from somewhere else.*

The scales screen handles the painful parts: waking sleepy pads by MAC, assigning each pad to a corner, per-corner calibration, and **ZERO ALL** with the car off the scales. In live mode the main balance view shows FL/FR/RL/RR weights, total, front/rear and left/right split, and cross weight against a target (I run **50.00%**).

![Cross weight with manual corner entry](/assets/images/trackday-pyrometer-helper/crossweight.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Manual entry still gives you cross weight, diagonal totals, and whether to adjust perches.*

When cross is off target, the app tells you how much weight to move onto the RF+LR diagonal and whether to raise RF/LR or lower LF/RR. Optional perch-turn calibration converts pounds into turns for your car.

![Cross weight recommendation with perch guidance](/assets/images/trackday-pyrometer-helper/crossweight-bottom.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Example: 44.83% cross on 2,904 lb — move 150 lb onto the RF+LR diagonal to hit 50%.*

### Tire temps

![Tire temp sessions list](/assets/images/trackday-pyrometer-helper/tiretemp-home.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Sessions are timestamped and tagged by compound — slick, R-Comp, RE-71RS, etc.*

![Tire temp entry with pyrometer and PSI sync](/assets/images/trackday-pyrometer-helper/tiretemp-reading.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*OUT / MID / IN per corner, plus sync from the pyrometer and from TPMS for cold and hot PSI.*

Each session records **OUT**, **MID**, and **IN** for all four corners. Static camber per corner feeds setup notes. The built-in guidance is the stuff I actually use: aim for the lowest hot pressure that doesn't roll the outer shoulder, watch outer-edge heat as the rollover signal, and take temps right after coming off track.

**SYNC FROM PYROMETER** pulls live readings from the PyroTC over BLE. **SYNC COLD PSI** and **SYNC HOT PSI** fill pressures from mapped TPMS sensors. Ambient temperature comes from a one-shot Open-Meteo lookup using last-known location so the app can flag cold tires.

### TPMS sensors

![TPMS sensor mapping by color tag](/assets/images/trackday-pyrometer-helper/tpms-sensors.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Tag each sensor by valve-stem color, map colors to corners. Rotate tires — re-point colors, not MAC addresses.*

TPMS decoding lives in the app, not the pyrometer firmware. Scan finds nearby sensors (they only broadcast while rolling or just stopped), you tag each one with a color on the valve stem, then assign colors to LF/RF/LR/RR. Saved **sets** cover different wheel/tire combos. After a rotation you remap colors to corners — the sensors stay tagged.

## PyroTC — custom BLE pyrometer

My old tire pyrometer died. Rather than buy another standalone gun, I built **PyroTC**: a handheld K-type reader on the Waveshare round ESP32-S3 touch LCD, in a 3D-printed enclosure.

![PyroTC enclosure CAD exploded view](/assets/images/trackday-pyrometer-helper/pyro-cad.png){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Head, handle, faceplate, and screw-on battery cap — designed around the round display module and an 18650 in the grip.*

The device owns all twelve readings (four corners × OUT/MID/IN). On the **SELECT** screen you pick LF/RF/LR/RR; on **RECORD** you see live probe temp, the three slots, and big **RECORD** / **CLEAR** / **BACK** buttons. A short beep confirms a capture. No hardwired trigger — tap record when you're on the patch.

Firmware is a single `main.cpp` (~450 lines), PlatformIO, **NimBLE-Arduino 2.x**, and a **MAX6675** on software SPI. The round panel, touch, and battery ADC are fixed by the Waveshare board; you only wire the thermocouple breakout, buzzer, and power switch into the printed shell.

### Using the pyrometer

<div>{%- include extensions/youtube.html id='VCvQOdNqaJc' -%}</div>

Tap a corner, record OUT → MID → IN, hit **SYNC FROM PYROMETER** in the app — cells fill live over BLE.

### BLE contract

The app (`PyroSync.kt`) and firmware share a fixed GATT layout — change either side without updating the other and things break silently.

| Item | Value |
|------|-------|
| Device name | `PyroTC` |
| Service | `a1b20001-7a9c-4b1e-9d3a-2f6c8e5d4c30` |
| **TEMP** `…0002` | READ + NOTIFY ~4 Hz — `float32` LE °C + `uint8` fault (live probe) |
| **STATE** `…0003` | READ + NOTIFY on change — 24 bytes, 12 × `int16` LE deci-°C in order LF[O,M,I], RF[O,M,I], LR[O,M,I], RR[O,M,I]; `-32768` = empty |

Build and flash:

{% highlight bash %}
pio run -t upload
pio device monitor -b 115200
{% endhighlight %}

Full pinout, touch-orientation flags, and source: [github.com/MrBlahhhh/PyroTC](https://github.com/MrBlahhhh/PyroTC).

> **Download the enclosure:** Three STL files in [`/cad/pyrotc/`](/cad/pyrotc/):
> - [`tiretester.stl`](/cad/pyrotc/tiretester.stl) — main body (head + handle)
> - [`cover.stl`](/cad/pyrotc/cover.stl) — faceplate over the display bay
> - [`cap.stl`](/cad/pyrotc/cap.stl) — screw-on 18650 battery cap with contact spring

## Parts list

### ProForm corner scales (existing hardware)

- **ProForm wireless corner weight scale pads** (4) — BLE, wake-on-connect

### PyroTC pyrometer

| Part | Notes |
|------|-------|
| Waveshare **ESP32-S3-Touch-LCD-1.28** | GC9A01 round LCD + CST816S touch; display, touch, IMU, and battery ADC are onboard |
| **MAX6675** breakout | K-type, software SPI — SCK GPIO15, SDO GPIO17, CS GPIO18 |
| K-type **tire pyrometer probe** | Yellow +, red − on the terminal block |
| **Active buzzer** | GPIO33 → (+), (−) → GND (`BUZZER_ACTIVE 1` in firmware) |
| **18650** Li-ion cell | Fits the printed handle; screw-on cap with contact spring at the base |
| **Rocker power switch** | Panel mount in the head |
| **USB** charge/data port | CH343P USB-UART on the Waveshare board |
| **3D-printed enclosure** | [`tiretester.stl`](/cad/pyrotc/tiretester.stl) (body), [`cover.stl`](/cad/pyrotc/cover.stl) (faceplate), [`cap.stl`](/cad/pyrotc/cap.stl) (battery cap) |

Optional: 3.7 V LiPo on the board's MX1.25 connector instead of the 18650 grip pack.

### TPMS

- **BLE TPMS sensors** (Tesla-style 401 MHz valve-stem units) — one per corner, color-tagged on the stem

### Phone

- Android device with BLE — app targets Play Store release soon

## What's next

Play Store submission is the immediate goal. The pyrometer hardware and firmware are done enough for track weekends; the app already replaced my paper log for cross, temps, and pressures on the 135i.

If you build a PyroTC, start with [nRF Connect](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp): confirm **TEMP** tracks the probe and **STATE** updates when you tap RECORD. Flip `TOUCH_FLIP_X` / `TOUCH_FLIP_Y` in firmware if corner buttons land wrong on your panel.
