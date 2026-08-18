---
title: "R53 Logger - Flasher — the Garage, the CAN bridge, and closing the tuning loop"
date: 2026-08-18 00:00:00 -0400
categories: car tech
tags: [mini, r53, android, datalogger, flasher, wideband, aem, esp32, can, tuning, ecu-flash, afr]
cover: /assets/images/r53-logger-august/header-afr-surface.png
lightbox: true
excerpt: "A month of near-daily builds: per-car Garage, an ESP32 CAN bridge with AEM wideband, per-block poll rates, AFR-vs-target analysis in the app, and the Python tooling that turns a log into the next flash."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-logger-august/header-afr-surface.png
---

<!--more-->

When [R53 Logger - Flasher went to the Play Store](/car/tech/2026/07/24/r53-logger-play-store.html) in July it was a logger that could also flash. A month and roughly 370 commits later it's become the whole tuning loop: log a pull, see exactly where fueling missed the target, change one thing, flash it, log again — all from the phone, with every file filed against the car it came from.

![Main screen at build 299](/assets/images/r53-logger-august/main-screen.png){:.img-md}
*Build 299. Same one-thumb layout, but the Garage is now front and center — and the status line tells you which car everything you're about to do belongs to.*

## The Garage — because one phone, two Minis

The moment a second R53 showed up, the flat file list stopped working. Whose backup is `ecu_backup_163021.bin`? Which car's log am I about to graph? Get that wrong with *logs* and you waste an evening. Get it wrong with a *flash* and you write one car's tune into another car's ECU.

So everything is per-car now. The app reads the VIN straight from the instrument cluster and identifies the car by its last seven, and every bin, log, and fault read files under it. When a file arrives that the app can't attribute — say a backup read before the cluster answered — it asks, once, instead of guessing:

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

There's also a **fast-link beta**: connect with the engine off and the app renegotiates the K-line from its ancient default up to 62,500 baud — several times the sample rate through the same cable. It survives everything a session throws at it now, including stopping and restarting logs.

## Autolog — never miss a pull

You don't fumble for a record button at wide-open throttle. Arm autolog and the app captures pulls by itself: the moment throttle crosses your arm threshold it starts a pull — and a **pre-trigger buffer** prepends the seconds *before*, so the launch is in the log even though the trigger came after it. A brief lift between gears doesn't end the recording; the pull carries through the shift as one run, then trails a few seconds after the final lift so you get the whole story. Each pull saves as its own file, with an optional continuous log alongside.

![Autolog settings screen](/assets/images/r53-android-logger/autolog.jpg){:.img-sm}
*Arm threshold, keep threshold, pre-trigger and trailing capture — set once, then just drive.*

## Closing the loop — how we actually tune with it

This is the part that used to live on a laptop. The methodology is simple to state: **decide what AFR the engine should run, measure what it actually ran, and let the difference tell you what to change.** The app now does the measuring, the comparing, and the flashing — the full loop, phone in hand.

You set a target AFR curve — two points, linear between them:

![Target AFR curve](/assets/images/r53-logger-august/target-afr-curve.png){:.img-md}
*Richer as revs climb. The wideband lag setting shifts the AFR trace forward in time so a reading lands on the cells that actually fueled it — exhaust transit plus sensor response is real, and ignoring it smears every pull.*

Then you make a pull and look at what happened:

![A pull in the live graph](/assets/images/r53-logger-august/live-graph-pull.png){:.img-md}
*Third gear to 7,200. RPM, spark advance, knock, trims and AFR on one time axis — tap anywhere to read the exact values at that instant.*

The graph is a full playback deck, not just a picture: replay a session at 1× to 4×, pinch to zoom time, drop marks, window in on one pull, and toggle any channel on or off. When you want the data elsewhere, it exports **CSV** for anything, or straight to **Datazap** for sharing a run online.

<div>{%- include extensions/youtube.html id='NA8mJ3ce8yA' -%}</div>

The AFR 3D surface is where it comes together — measured AFR versus target, over RPM and load, with the target curve you set driving the colouring. Green is on target. The hot ridge is where it ran lean of the curve; that's where the next fueling change goes:

![AFR error surface over RPM and load](/assets/images/r53-logger-august/afr-3d-surface.png){:.img-md}
*Open loop shows AFR against the target curve; closed loop shows total trim. One glance says which cells need fuel — no exporting, no spreadsheet.*

The 3D surface is great for spotting a lean *peak*; sometimes you want to read the numbers cell by cell. The same log plays back as flat, colour-coded maps across RPM (columns) and load (rows), one tap apart.

**Fuel map.** Each cell shows how far AFR ran off target in open loop (`+` lean, `–` rich), and switches to closed-loop total trim (italic) where the ECU was correcting. Dim numbers are the tune's own value for reference, so a cell that's fighting the table jumps right out.

![Fuel map heatmap — AFR error and trims per cell](/assets/images/r53-android-logger/fuel-map.jpg){:.img-lg}
*Fuel map: green is on-target, blue is pulling fuel out, red is adding. The thin line traces the pull scrubbed to on the timeline.*

**Trims map.** Total fuel trim (STFT + LTFT) per cell, full colour at ±20%. Blue means the ECU is yanking fuel back out, red means it's piling it in, green is happy. A whole low-rpm column glowing blue is the map telling you where the tune runs rich before you've touched a single table.

![Trims map heatmap — STFT plus LTFT per cell](/assets/images/r53-android-logger/trims-map.jpg){:.img-lg}
*Total trim per cell — the blue band down low is where it's commanding fuel back out.*

**Knock, pinned to the cell that pinged.** Every one of these views — 3D AFR, fuel, trims, and the spark map — drops a yellow dot on the exact cell where a knock event fired, and the dots stay put for the whole session. Instead of "I think it rattled somewhere in third," you see the precise RPM-and-load region that pings, laid right over the fuel and timing there. A running per-cylinder tally sits in the corner all session — `cyl4 ×67  cyl2 ×22  cyl3 ×14  cyl1 ×8` — so a single cylinder doing all the complaining is obvious at a glance.

![3D AFR surface with knock events marked](/assets/images/r53-android-logger/afr-3d-knock.jpg){:.img-lg}
*The AFR surface with knock events dotted on. The lean cluster up top and the knock dots landing in the same neighbourhood is exactly the story you want to catch.*

The dedicated **spark map** shows timing where it matters: each visited cell reads degrees off the tune's own spark table, and cells where the ECU *pulled* timing go red. Red cells stacked with yellow dots means knock and timing-pull are agreeing with each other — that's the corner of the map that needs the work.

![Spark / knock map — timing pulled off the tune, per cell](/assets/images/r53-android-logger/knock-map.jpg){:.img-lg}
*Spark map: red cells are where timing got pulled off the tune, yellow dots are knock events. They cluster in the same place for a reason.*

The loop is: log the pull, read the surface and the maps, make **one** change, flash it from the same phone, and log the next pull. When the surface goes green, the trims settle, and the knock dots stop landing, you're done. The discipline of one-change-at-a-time is the entire method — the tooling just makes each iteration take minutes instead of an evening.

## The auto-tune — how a log becomes numbers you can type

The **Fuel Rec** button turns the graph window you're looking at into a per-cell table of proposed changes. It's worth explaining exactly what it does, because the details are where most DIY fueling analysis goes wrong.

**It never mixes its two signals.** The engine lives in two regimes, and each one has exactly one trustworthy measurement:

- **Open loop — WOT.** The ECU switches its own correction off under full-load enrichment, so the wideband is the only authority. The app compares measured AFR against your target curve, cell by cell, and computes how far fuel delivery actually missed.
- **Closed loop — idle and cruise.** Here the wideband is nearly useless for analysis, because the ECU has *already fixed the error* — measured AFR sits near stoich by construction, no matter how wrong the table is. But the ECU tells you exactly how hard it's working to fix it: **short-term plus long-term trim, stacked, is the answer.** A cell where total trim sits at −8% is a cell running 8% rich, full stop.

Mixing these is the classic mistake. Read AFR at cruise and everything looks perfect over a table that's badly off; read trims at WOT and you're applying corrections the ECU froze back at cruise, under completely different conditions. The app attributes every cell to one regime and labels which — and since this ECU doesn't report loop status on any channel, the app infers it from the short-term trim's behaviour, holding a sample back until the evidence has settled.

**The wideband is treated as a physical sensor, not a number.** Every AFR reading is shifted back in time by the transport delay you set — exhaust travel plus sensor response — so a reading lands on the cells that actually *fueled* it, not the cells the engine had moved on to. Readings that are stale or pinned at the sensor's rails get rejected before they can vote.

**It refuses to overstate its confidence.** A cell needs multiple agreeing samples before it gets a recommendation at all — a blank cell is honest, a guess isn't. Every cell shows its own sample count so you judge the thin ones yourself. Wild corrections are clamped and *flagged* — beyond a certain size the right response is to distrust the data, not to rewrite the table. And neighbouring cells are smoothed against each other, weighted by how much data each one has.

**The numbers are geared to the ECU's actual math.** This is the subtle one. On this ECU, a percent typed into the full-load fuel table is **not** a percent of fuel — the table participates in the injection calculation in a way that gears cell changes down substantially, and by a different amount depending on the cell's own value. Read "8% lean" off the wideband, type "+8%" into the cell, and you've under-corrected several-fold. The app knows the ECU's arithmetic and converts the measured fuel error into the *cell* change that actually delivers it, using your own tune's values — which is why it wants the car's BIN loaded, and tells you when it's approximating without one. Closed-loop corrections belong to a different table that does scale one-to-one, and the app routes them there instead.

**Tap to add or pull fuel.** The Fuel table simulator puts the proposed table under your fingers — and on a tablet in landscape it becomes a proper two-pane workbench, table on the left, AFR trace on the right. Tap a cell or drag a block, then move it with the **−5% / −1% / +1% / +5%** buttons; **Undo cell** and **Reset all** walk anything back. **Fill from log** seeds every cell from what the session recommends, and you adjust from there.

![Fuel table simulator — selecting cells on a tablet](/assets/images/r53-logger-august/simulator-select.jpg){:.img-xl}
*The workbench on a tablet: the real fuel table on the left, the log's AFR trace on the right. A drag has two cells selected, ready for the nudge buttons — the scrubber up top picks which slice of the session you're judging against.*

And it doesn't just record your edits — it **redraws the log's AFR trace as if the table had carried them**: solid is what was recorded, dashed is the prediction, and the header keeps score for the window you've scrubbed to — how many samples land on edited cells, how many actually move, how many are held by the ECU, and the session's AFR before → after against your target. "Held" matters: adding fuel to a cell the ECU is trimming doesn't change the mixture — the trims just absorb it — and a simulation that pretended otherwise would be lying to you. Nothing here is written to the car.

![Simulator predicting a pull with 27 cells edited](/assets/images/r53-logger-august/simulator-pull-predicted.jpg){:.img-xl}
*A WOT pull with 27 cells edited: the white line traces the pull through the exact cells it visited, and the dashed AFR curve is what this table would have delivered. The header scores it — 64 samples land on edited cells, 61 move, AFR 12.72 → 12.64 against a 12.02 target.*

Every edit is capped at ±15% per flash — deliberately. A bigger correction means the measurement no longer describes the engine you're about to create; big moves are repeated flashes with a log in between. The app is blunt about it, too:

![Fill from log with the per-flash cap](/assets/images/r53-logger-august/simulator-fill-from-log.jpg){:.img-xl}
*Fill from log seeded 80 cells across the whole 18-minute session — and says so plainly: 70 were capped at the 15% per-flash limit, the biggest wanted +100%. Flash, log, and seed again. Note "5057 held by the ECU": most of an 18-minute drive is closed-loop cruise, and the simulator refuses to pretend those samples would move.*

**Getting it to your tuner.** When the table reads the way you want, one button exports the whole proposal as a CSV — every edited cell with its axes and its percentage — and hands it to the Android share sheet: email it, message it, drop it in Drive, whatever your tuner uses. The numbers are in table-percent, ready to be typed into a tuning tool as-is; the gearing is already done. And deliberately, **the auto-tune never writes the BIN itself** — proposed numbers stay proposed until a human puts them in a tune, and the app's flash path is a separate, checksummed affair. If you're your own tuner, that's the loop: export, apply, flash from the same phone, log the next pull.

## The Python side

Not everything belongs on a phone. Off the car, a set of Python tools does the heavier analysis: chewing through a session's CSV and turning it into a short list of recommendations, auditing a bin's rev-limit configuration, and diffing two tunes to confirm a change did only what it claimed. The app's CSV format is the interchange — every log shares straight into the pipeline. The scripts encode a lot of hard-won specifics about this ECU, so they're staying private, but the shape of the workflow is the point: the phone measures, the scripts deliberate, the phone flashes.

## ECU backup and flash — with an option deck

The flash side is where this stops being "a logger app." On facelift silver-cover ECUs it reads a **full backup** first — always — then writes either a bin you supply, or one of the built-in **factory images: US Cooper S, JCW, and GP1**. Every write is checksummed, and the app verifies the tune before it touches the car — and can auto-fix what it finds.

![ECU flash screen with factory images](/assets/images/r53-logger-play-store/flash-screen-factory.jpg){:.img-md}
*Backup, factory software, or your own bin. The summary line always says exactly what's armed.*

![Tune check before write](/assets/images/r53-logger-play-store/flash-tune-check.jpg){:.img-md}
*The tune check runs before every write — nothing goes on the car unverified.*

Then there's the option deck: tweaks applied **in memory** to whatever tune is loaded, before the write — no hex editor, no separate tool:

- **Pops** — decel crackle on or off
- **Injector scaling** — 330 (stock S), 380 (JCW/GP), 440, 550, 630 cc
- **Redline** — raise the gear-dependent hard limit; the soft cut stays stock, like every real factory performance tune
- **Throttle pedal remap** — stock, straight-linear, or a normalized Track curve with the same feel at every RPM
- **Overboost** — raised throttle opening
- **Idle RPM** — manual-transmission idle, cold and warm
- **Cooling-fan kick-on temperature** — the fan comes on sooner, before heat soak
- **Closed-loop trim learning range** — learn fuel trims further up the revs
- **Cruise timing** for economy, and an experimental **lean cruise bias**

![Flash options](/assets/images/r53-logger-play-store/flash-options.jpg){:.img-md}
*The option deck. Each toggle states what it does and what it leaves alone.*

![Pedal and fan presets](/assets/images/r53-logger-play-store/flash-options-pedal-fan.jpg){:.img-md}
*Track pedal and fan presets — and options that can't verify their target in your bin refuse to apply rather than guess.*

That last caption is the philosophy of the whole flash path: if a bin doesn't say which car it came off, the injector and redline options **decline** rather than write a wrong-variant value. The app would rather do nothing than do something silently wrong.

![Flash complete](/assets/images/r53-logger-play-store/ecu-flash-done.jpg){:.img-md}
*Backup taken, tune checked, written, verified.*

## Diagnostics — codes, and adaptation resets

The same cable reads **trouble codes** — the EMS2000 engine codes most people are after, plus other modules from a picker — and clears them where the clear command has been captured from a known-good tool. Where it hasn't, the app says so instead of sending a guess at your ECU. Codes can be shared out as a report, and every fault read files into the car's Garage history, so "has it thrown this before?" is a scroll, not a memory test.

It also does **adaptation resets**: pick which engine adaptations to clear, reset them, and let the engine idle to relearn — the usual ritual after hardware changes, without the dealer tool.

## Flash safety, since we're flashing more

Iterating faster means flashing more, and the flash path got hardened to match: nothing can take the cable away from a write in progress — logging, VIN reads, everything else waits. A backup read tells you which step failed and retries a security-locked ECU instead of giving up. And the app asks before a freshly flashed bin becomes the analysis baseline, so your graphs are always judged against the tune that's actually in the car.

The small stuff is covered too: light and dark themes, °C or °F, and a force-quit that cleanly releases the USB cable to other apps.

## Get it

**Mini R53 Logger - Flasher** is on the [Play Store](https://play.google.com/store/apps/details?id=com.geekopolis.r53logger). Logging works on any R53; flashing supports the facelift silver-cover ECU. All you need is the same cheap K+DCAN cable every BMW owner has, and an Android phone or tablet — the ESP32 bridge is optional, for the CAN channels and wideband. Everything else above works with just the cable.
