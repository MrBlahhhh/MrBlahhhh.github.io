---
title: "Trackday Pyrometer Helper — User Manual"
date: 2026-07-12 00:00:00 -0400
categories: car tech
tags: [android, pyrometer, tpms, ble, track-day, manual]
cover: /assets/images/pyrometer-helper-manual/hero.png
lightbox: true
excerpt: "How to set up and use my track-day app: TPMS colors, tire inventory, temp sessions, the background monitor, and corner balance."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/pyrometer-helper-manual/hero.png
---

<!--more-->

## What this app does

**Trackday Pyrometer Helper** is my one-screen-per-job track app: tire temps from my [PyroTC BLE pyrometer](/car/tech/2026/06/18/trackday-pyrometer-helper.html), hot/cold pressures from cheap BLE TPMS sensors (Zeepin and DJTPMS), an all-day background pressure monitor, per-tire heat-cycle tracking, corner balance with ProForm wireless scales, and packing checklists. Everything is on-device — no accounts, no cloud.

This is the manual I wish I'd written earlier. It's ordered the way you'd actually set the app up.

## The main screen

![Tires tab home](/assets/images/pyrometer-helper-manual/tires-home.png){:.img-md}
*The TIRES tab: session log grouped by track and day, static camber at the bottom.*

The header has four buttons: **TPMS** (sensor setup), **MON** (background monitor), **⇣** (CSV export of everything), and **⚙** (settings). The **CAR** dropdown switches between car profiles — each car keeps its own weights, camber, targets, and sessions. The **TIRES / BALANCE** tabs switch between pyrometer work and corner-weighting.

## First-time setup: TPMS colors

Everything pressure-related hangs off one idea: **paint each valve stem a color** (I use Posca PC-3M paint markers — the app's color list matches my 14-marker kit) and teach the app two things:

1. **SENSORS** — which color is which sensor. Tap SCAN, roll the car a few feet so the sensors wake, then tap a color chip and tap its sensor in the DISCOVERED list. Do this once per sensor, ever.
2. **WHEELS** — which color is on each corner right now. Tap a corner, pick a color.

![TPMS setup screen](/assets/images/pyrometer-helper-manual/tpms-setup.png){:.img-md}
*Wheel map on top, tagged sensor library below. Zeepin and DJTPMS sensors can be mixed — the type is auto-detected when you bind.*

Two-tone stems (my spare is half red, half white) get the **+ SPLIT** button: pick two colors and the app builds a "Red/White" label with a split swatch.

**When you rotate tires, this is the only screen you touch** — re-point the colors to their new corners (or save layouts as named SETS and load one with a tap). Every other feature resolves corner → color → sensor at the moment it's needed, so rotations propagate everywhere automatically.

## Tire inventory and heat cycles

Settings → **Tire inventory**. Each physical tire is its own record with a heat-cycle count and corner history.

![Tire inventory](/assets/images/pyrometer-helper-manual/tire-inventory.png){:.img-md}
*ADD SET creates a whole numbered set in one tap and binds your current corner sensors to it.*

- **ADD SET** — type a base name, pick a compound and count, done. "sm7.5 #1…#4" are created and each gets the sensor currently on its corner, so cycles start counting immediately.
- **Heat cycles log themselves** three ways: the background monitor detects a heat-up/cool-down on track, SYNC HOT after a session, or the manual **+CYCLE** button (tap it N times to back-fill N known cycles — it's never deduped; the automatic paths dedupe to one cycle per 10 minutes so a single session isn't double-counted).
- The corner is recorded per cycle, so a tire's history reads like `LF×4 RF×2` — rotations captured for free.
- **RETIRE** keeps history and frees the sensor; **UNRETIRE** brings a tire back; long-press a tire card to delete it entirely. Everything destructive asks first.

If a tire shows an amber "no sensor" warning, automatic cycles can't reach it — assign its sensor.

## Recording a tire-temp session

Tap **＋ NEW** on the TIRES tab. The new session inherits everything from the last one (tire, unit, location, toggles), auto-numbers itself within the day, and — if the tires in your inventory are sensor-bound — names the tire set automatically.

![Expanded session card](/assets/images/pyrometer-helper-manual/session-card.png){:.img-md}
*Each corner block shows which physical tire is on it (sm7.5 #1, #2…), resolved live through the sensor chain.*

My session flow:

1. **AMBIENT ⇣ GET** — pulls local temperature (Open-Meteo) so the app can verify the tires are actually cold.
2. **⌖ NEAREST** — auto-fills the track from a bundled list of ~1,900 circuits (works offline; paddocks have no signal).
3. **SYNC COLD PSI** — one tap grabs every corner's TPMS reading.
4. Run the session.
5. **SYNC FROM PYROMETER** — record OUT/MID/IN on the PyroTC gun, hit sync, all twelve cells fill over BLE. Take temps immediately — they fade fast.
6. **SYNC HOT PSI** — hot pressures land per corner, and a heat cycle is logged on each tire that reported.

Set a **HOT TARGET** per tire model once and the app derives the rest: the bleed amount ("bleed 1.5 → 32 psi"), and a predicted **COLD START** pressure computed from your own measured cold→hot rise — ambient-aware, so a cold morning's +6 psi doesn't pollute a hot afternoon's prediction. Until there's history it assumes ~3 psi and refines as you log.

The collapsible **RECOMMENDATIONS** section reads the temp spread per tire: inside-vs-outside drives camber advice, center-vs-edges drives pressure advice.

## Background pressure monitor (MON)

![TPMS monitor](/assets/images/pyrometer-helper-manual/mon-live.png){:.img-md}
*Live corner cards with the wheel-map color tags; logged runs below.*

**MON** runs as a foreground service and logs every corner's pressure and temperature all day, even with the app closed. I switch it on in the paddock while the tires are cold and off at the end of the day.

- Tap a **live wheel** for that corner's full pressure + temperature chart; tap a **logged run** for all four corners. Charts have a real time axis — points are placed by elapsed time, min/max are marked.
- Every run can be exported to CSV from the LOGGED RUNS list.
- It's also the heat-cycle auto-detector: a corner that climbs 20°F and falls back 15°F counts one cycle on whatever tire is there.

The notification shows per-corner psi/°F and has a Stop action. Off by default — it costs battery.

## Corner balance

![Balance tab](/assets/images/pyrometer-helper-manual/balance.png){:.img-md}
*Cross weight with the RF+LR diagonal highlighted. MANUAL entry or LIVE SCALES.*

The **BALANCE** tab computes cross weight (RF+LR as a percentage of total) from four corner weights — typed in, or streamed live from ProForm wireless scale pads over BLE.

- **SCALES ›** opens setup: map pads to corners (AUTO-STEP walks you around the car), calibrate each pad once with a known weight, and ZERO before rolling the car on.
- In LIVE mode, **CAPTURE** freezes the current weights into the car profile and drops back to MANUAL so the numbers hold while you turn perches.
- The recommendation card tells you exactly how much weight to move onto or off the RF+LR diagonal, which corners to raise or lower, and — once you calibrate lb-per-turn — how many perch turns that is.

Scale in race trim: driver weight in the seat, fuel set, ride heights where you want them.

## Settings, checklists, backup

![Settings](/assets/images/pyrometer-helper-manual/settings.png){:.img-md}
*App-wide items on top; per-car tire interpretation targets below.*

- **Theme** — Daylight (default, built for direct sun), Dark, or follow the system.
- **Packing checklists** — reusable lists (Day Drive / Tow / Camp presets); check items off as you pack, UNCHECK ALL resets for the next event, SHARE sends plain text to Keep.
- **Backup** — one JSON file with every car, sensor, tire, and checklist. Export before phone swaps; restore replaces everything. Monitor logs aren't included — export those runs to CSV if you want to keep them.
- **Tire targets** — the camber-gradient and pressure-band thresholds the recommendations use, per car. Defaults follow the SuperMiata radial guide.
