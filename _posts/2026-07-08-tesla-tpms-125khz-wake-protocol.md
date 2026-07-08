---
title: "Reverse-engineered Tesla / Continental TPMS 125 kHz wake-up protocol"
date: 2026-07-08 00:00:00 -0400
categories: car tech
tags: [tpms, esp32, pyrotc, tesla, continental, "125khz", ble, reverse-engineering]
cover: /assets/images/tpms-lf-wake-protocol/scope-125khz-burst.jpg
lightbox: true
excerpt: "Capturing and replaying the 125 kHz LF wake telegram on PyroTC"
article_header:
  type: overlay
  theme: dark
  background_image: false
---

<!--more-->

## Why bother with 125 kHz?

The cheap **Tesla-style BLE TPMS sensors** I run on the 135i (and decode in [TrackDayPyrometerHelper](/car/tech/2026/06/18/trackday-pyrometer-helper.html) and [TPMS Monitor](/car/tech/2026/06/21/tpms-monitor-app.html)) are great once they are awake — pressure, temperature, and battery over 401 MHz BLE. The problem is **getting them to wake up on demand**.

Motion and a few PSI of pressure change will eventually rouse a sleeping sensor, but that is slow and unreliable when you want a reading in the paddock with the car on stands. Factory tools like the **ATEQ VT30** (what Tesla service uses for Continental sensors) fire a **125 kHz low-frequency burst** at the valve stem. The sensor hears it, wakes, and starts broadcasting.

I wanted that same trick on my handheld **PyroTC** pyrometer — tap a button, wake the nearest sensor, read pressure in the app. That meant reverse-engineering the LF telegram and building a coil driver small enough to fit inside the enclosure I already designed.

## Capturing the wake telegram

The starting point was an **Autel TPMS tool** scope capture — the same kind of 125 kHz burst an EL-50448-style relearn wand sends, but from the Continental/Tesla side of the ecosystem. I ran the raw scope trace through `decode_scope_to_replay.py` to pull out the bit timings.

The result is a fixed **OOK replay table**: **184 uniform half-bit slots at 128 µs each** (~23.6 ms total telegram). Each slot is either on or off — no variable Manchester edges to chase at runtime. The table lives in auto-generated `cont_wake_data.h`; regenerate upstream and re-copy `lf_wake.h`, `lf_wake.cpp`, and `cont_wake_data.h` if the capture changes. Do not hand-edit the data header.

## What 125 kHz looks like on the bench

On a **Hantek DSO2C10** at 20 µs/div and 500 mV/div, the carrier is a clean **125 kHz** sine burst — about 2.5 cycles per division. Amplitude peaks around **2 Vpp** at the coil, gated on and off by the OOK envelope.

![Hantek scope — 125 kHz TPMS LF wake burst](/assets/images/tpms-lf-wake-protocol/scope-125khz-burst.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*~160 µs burst envelope on a 125 kHz carrier — this is what the replay table modulates.*

The scope shot is the sanity check that the decoded timings actually produce the right carrier frequency before you point it at a tire.

## LF coil driver hardware

The ESP32 cannot drive a 125 kHz tank coil directly, so the driver is a separate **9 V domain** (only GND shared with the MCU):

| Part | Role |
|------|------|
| **EL-50448** coil (or equivalent 125 kHz antenna) | Magnetic coupling to the valve-stem sensor |
| **10 nF** tank capacitor | Parallel resonance with the coil |
| **IRLZ44N** logic-level MOSFET | Low-side switch — GPIO → 100 Ω → gate |
| **9 V** supply | Enough headroom for the burst; isolated from the 3.3 V ESP32 rail |

GPIO4 (`LF_COIL_PIN`, set in `platformio.ini` build flags) drives the MOSFET through **LEDC PWM** at the bit-slot rate. The buzzer stays on plain `digitalWrite` — no pin clash.

## Firmware on PyroTC

The LF wake driver is **vendored into [PyroTC](https://github.com/MrBlahhhh/PyroTC)** (`lf_wake.h`, `lf_wake.cpp`, `cont_wake_data.h`). On the **RECORD** screen a cyan **WAKE** button sits centre of the bottom rim between **CLEAR** and **BACK**. Tap it and the gun fires a **10 second burst** (`startWakeBurst`) — long enough to tickle every sensor on the car if you walk it around.

Timing is **exclusive**: while `wakeActive`, `loop()` runs only `serviceLfWake()` and returns early. No MAX6675 read, no LCD repaint, no BLE — a thermocouple conversion takes ~220 ms and would jitter the 128 µs bit grid. The gap loop calls `delay(1)` to keep the task watchdog fed. The burst auto-ends after 10 s; there is no mid-burst cancel.

The wake path is **one-way** — no new BLE characteristics. The existing PRESS/SENS/MAP slots (`…0004` / `…0005` / `…0006`) stay free. Pressure still comes back through the phone app scanning 401 MHz BLE after the sensor wakes.

{% highlight cpp %}
// LF_COIL_PIN = 4  (platformio.ini build_flags)
// 184 × 128 µs OOK slots from cont_wake_data.h
// WAKE button → startWakeBurst() → 10 s exclusive serviceLfWake()
{% endhighlight %}

Full build and flash on your machine:

{% highlight bash %}
pio run -t upload
pio device monitor -b 115200
{% endhighlight %}

## Fitting it all in the housing

The pyrometer enclosure was already tight — round Waveshare head, 18650 grip, MAX6675 breakout, rocker switch. Adding a **125 kHz coil**, a **9 V boost module** (XL6009E1 step-up), and the MOSFET driver meant revisiting the Fusion 360 assembly from scratch.

![Fusion 360 — handheld tire tester with LF coil and 9 V boost](/assets/images/tpms-lf-wake-protocol/housing-cad-fusion.png){:.border.rounded style="max-width:640px;display:block;margin:1.25rem auto;"}
*`handheld-tire-tester-125khz` — ESP32-S3 round LCD up top, coil and DC-DC in the head, battery shields on the right, USB-C extension on the side.*

The coil sits forward in the head so you can hold the gun near the valve stem like a factory relearn tool. The XL6009E1 boost board feeds the 9 V tank; the ESP32 and MAX6675 stay on the existing Li-ion rail. Everything still has to clear the round faceplate cutout and the screw-on battery cap at the bottom of the grip.

Original STLs for the pre-LF enclosure are in [`/cad/pyrotc/`](/cad/pyrotc/) (`tiretester.stl`, `cover.stl`, `cap.stl`). The 125 kHz revision is still in CAD — print-test-fit cycle next.

## What is next

- Print the revised head and confirm coil placement wakes a sleeping Continental sensor on the first tap.
- Decide whether the 10 s burst duration is right for paddock use or if a shorter pulse train is enough.
- If TPMS **data** ever flows back through PyroTC over BLE, that needs new GATT characteristics coordinated with `PyroSync.kt` in the Android app — same rule as the temperature contract.

For now, the hard part is done: I have the telegram, the replay firmware, and a housing layout that might actually close. The rest is mechanical patience.
