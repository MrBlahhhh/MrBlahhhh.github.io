---
title: "Building a Predictive Maintenance Logger for My Haltech Rebel LS"
date: 2026-07-30 00:00:00 -0400
categories: car tech
tags: [haltech, rebel-ls, android, datalogger, telemetry, predictive-maintenance, ls, v8]
cover: /assets/images/haltech-rebel-logger/sessions-channel-map.jpg
lightbox: true
excerpt: "Reverse-engineering the Haltech Rebel LS Wi-Fi telemetry protocol and building an Android app that logs 1,000+ ECU channels for predictive engine health monitoring — per-cylinder anomaly detection, drift trending, and what I've learned so far."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/haltech-rebel-logger/sessions-channel-map.jpg
---

<!--more-->

## Why I built this

My Haltech Rebel LS is a fully programmable ECU on a supercharged LS V8. It monitors everything — injector current per cylinder, ignition coil voltage, knock sensors, lambda, fuel pressure, coolant temp, manifold pressure, RPM — and it has a built-in Wi-Fi access point that broadcasts this data over UDP.

The obvious thing: log it all and watch for trouble. The problem is **volume**: at 5-10 Hz across 1,000 channels, a 20-minute track session generates 35-60 MB of raw data. Tens of millions of data points per session. How do you find the signal in that noise?

This is the story of how I reverse-engineered the protocol, built an Android app to pull it all down, and wrote a Python analysis pipeline that looks for two different kinds of failures — without drowning in data.

![Channel map browser — each slot's identity is tagged with the correlation source](/assets/images/haltech-rebel-logger/sessions-channel-map.jpg){:.img-md}
*The channel map browser. Each slot's identity is tagged with its correlation status — CONFIRMED, TENTATIVE, or PENDING. "Not identified yet" channels are ones we haven't matched to a known signal. The app logs everything, but detectors only run on CONFIRMED channels.*

## Two kinds of failures

I realized early on that engine failures show up in two completely different ways, and they need different tools:

| Failure mode | Timescale | Example | What it looks like |
|---|---|---|---|
| **Sudden** | Seconds to minutes within one session | An injector starts clogging mid-session, a coil breaks down | One cylinder's injector current drops 8% below the other seven |
| **Slow drift** | Weeks to months across many sessions | Rod bearing clearance grows, fuel pump wears | Oil pressure at 4,000 RPM is 5% lower than it was 10 sessions ago |

One detector cannot cover both. So the system has two separate analysis paths — and completely different storage strategies to support them.

## How it works

The whole pipeline is just four layers:

**Phone → Binary files → Python analysis → One-page summary**

### The app

The phone joins the ECU's Wi-Fi (it broadcasts its own access point), polls it over a reverse-engineered UDP telemetry protocol, and writes every sample to a custom binary format. The format is self-describing — it carries the channel map inside it, so a session recorded today will still be readable years from now regardless of what app version wrote it.

![Connect screen — ECU SSID, IP, and port settings](/assets/images/haltech-rebel-logger/connect-screen.jpg){:.img-md}
*Connect screen. The ECU runs its own Wi-Fi access point — just tell the app the SSID and IP, and it handles the rest. The red error is a keepalive handshake failure, which happens when the phone's Wi-Fi radio decides to roam mid-session. Still working through that.*

At the end of a session, the app computes per-channel statistics: min, max, mean, p05, p50, p95, standard deviation, and sample count. That's ~300 channels × 12 numbers = a few kilobytes per session. **This is the only data that ever gets queried across sessions.** The raw 35 MB file stays on-device.

### Sudden-failure detection

The core technique is beautifully simple: **compare each cylinder against the other seven.**

If cylinder 3's injector current is 8% below the average of cylinders 1, 2, 4, 5, 6, 7, and 8, that's a signal — regardless of how hard you're driving or what the fuel temperature is. No history needed, no calibration, no absolute thresholds.

This is the same principle used in aero-engine diagnostics, where they compare symmetric halves of a jet engine to each other. I just apply it to cylinders instead of compressor stages.

The math uses a median-based z-score (robust to one bad cylinder skewing the reference), and a cylinder is only flagged if it stays anomalous for several consecutive samples — not just a single noise spike.

Dual-element sensors (throttle pedal A/B, throttle position A/B) get their own cross-check: the two channels should track each other linearly. A drop in correlation means the sensor is failing right now, before any DTC sets.

### Slow-drift detection

Raw session averages are a trap. A session with more idling reads lower oil pressure; a session with more high-RPM pulls reads higher. Comparing averages across sessions measures *driving style*, not *wear*.

The fix is **operating-point binning**: every sample gets classified into a cell by (RPM range × engine load × temperature), and statistics are computed within each cell. Then the same cell is compared across sessions:

> "Oil pressure p50 in the cell 4,000-4,500 RPM × 70-80% load was 52 psi for sessions 1-8, 50 psi for sessions 9-14, and 47 psi for sessions 15-20."

That series is comparable. A raw session-mean would hide it entirely.

There's also a **hot-idle fingerprint**: at the end of every session (fully warm, stable idle), I record oil pressure, manifold vacuum, fuel trims, and knock background level. One small, deliberately-revisited operating point is worth more for month-over-month comparison than the entire rest of the session. Rod bearing clearance growth shows up at hot idle first.

### The LLM summary

The LLM never sees raw data. Deterministic Python does all the arithmetic, and hands the model a compact, unit-labeled report. The model's job is to correlate across subsystems — "cylinder 3 injector current is low *and* bank 1 fuel trim is positive *and* knock trim is retarding cylinder 3" — and produce a ranked list of what to inspect.

Every number carries units and a sample count: "Injector 3 current p50 = 0.71 A (n=3100)" — never a bare number. Findings are ranked, cite their source sessions and channels, and the output is always a **"what to check"** list, not a verdict. The human makes the final call.

## The hardest part: the channel map

The ECU broadcasts data in a fixed set of memory-mapped slots. Only a subset are actually mapped to ECU channels, and the slot a channel lives in can shift between firmware versions. The mapping from slot to channel identity was worked out by correlating raw bytes against Haltech's own PC software (NSP) CSV exports during known state changes.

Every mapping carries a confidence level:

| Confidence | What it means | What we do with it |
|---|---|---|
| **CONFIRMED** | Verified against NSP CSV or known state change | Include in detectors |
| **TENTATIVE** | Plausible but not yet verified | Include but flag it |
| **PENDING** | In the CSV but not yet located in the window | **Exclude entirely** — never guess |
| **COLLISION** | Two channels map to the same slot | **Exclude** — ambiguous |

We never act on a channel we're not sure about. The injector and ignition channels are fully confirmed. The big ones still pending: RPM, manifold pressure, coolant temperature, wideband O2 sensors, fuel pressure, and per-cylinder knock and ignition trim. These are all blocked on getting an engine-running capture with the NSP CSV correlation running simultaneously. That's the next milestone.

## What I'd do differently

**Verify bytes off the wire, not in a simulation.** I spent a week debugging a protocol issue that didn't exist in my Python model. The actual app was sending a wrong-sized request while my "proven" reference was actually a different size. The fix: capture the phone's actual UDP traffic with tshark and compare against that — never against a filtered CSV extract.

**Never compare session averages.** Operating-point binning is the only way to get comparable numbers across sessions.

**LLMs are synthesis tools, not data analysts.** They're terrible at reading raw numbers — the tokenizer splits `1024` into either `10` + `24` or `102` + `4` depending on training data. Give them features, not samples.

**Every channel needs a confidence level.** The first time I "confirmed" RPM, MAP, and TPS from a single engine-off capture, all three were wrong — they were raw analogue input millivolts, not the actual sensor readings. You need correlation against a known-good reference.

## What's next

The engine-running capture unlocks everything: operating-point binning, the hot-idle fingerprint, bank-differential O2 sensors, and per-cylinder knock trim monitoring. After that it's just collecting sessions and tuning thresholds against real flagged events.

Deliberately deferred: cloud sync, multi-vehicle support, live on-track alerting, and machine learning models. Each has to earn its place by proving the deterministic layer isn't enough.

The full project covers the Android app in Kotlin, Python analysis scripts, and the protocol documentation. If you're reverse-engineering an aftermarket ECU's telemetry, the protocol-analysis work covers the full approach.