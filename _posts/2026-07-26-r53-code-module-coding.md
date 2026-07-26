---
title: "R53 Code — Module Coding From the Phone"
date: 2026-07-26 00:00:00 -0400
categories: car tech
tags: [mini, r53, android, coding, bc1, ems2000, mrs5k, ncs]
cover: /assets/images/r53-code-module-coding/hub-write-ok.jpg
lightbox: true
excerpt: "Companion Android app for first-gen MINI module coding — read, edit in English, write with verify, backups, planning mode, and trouble codes."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-code-module-coding/hub-write-ok.jpg
---

<!--more-->

## A second app, same car

The [logger](/car/tech/2026/07/24/r53-logger-play-store.html) is for live data, pulls, flash, and diagnostics. **R53 Code** is the sibling APK for **module coding** — the stuff NCS Expert does on a laptop, but on the phone with English labels, a backup before every edit, and a verify re-read after write.

Same K+DCAN cable, key ON / engine OFF. Separate Play listing and unlock from the logger.

![Module coding hub after a verified write](/assets/images/r53-code-module-coding/hub-write-ok.jpg){:.img-md}
*Hub after Write to car — toast confirms the block was written and verified.*

## Live module coding

Pick a module, read from the car, edit options, write back. Definitions ship in the APK — no `.man` files to import.

Live transports today cover the modules I've captured end-to-end: **BC1** body, **EMS2000** engine, **MK60** DSC, **MRS5K** airbag, **KMB** cluster. Planning mode still lets me browse everything else offline.

![Hub ready to read — BC1 selected](/assets/images/r53-code-module-coding/hub-ready.jpg){:.img-md}
*Live hub — select module, Read, Edit, Write / Restore, Share, Planning, Trouble codes.*

![Hub with empty comms log](/assets/images/r53-code-module-coding/hub-empty-log.jpg){:.img-md}
*Before a session — TX/RX fills the log once you read or write.*

Flow:

1. **Select a module** (BC1, EMS, airbag, …)
2. **Read from car** — identity + coding block; a dated backup is saved automatically
3. **Edit options** — full-screen list with English titles and the raw FSW under each row
4. **Write to car** — only the changed bytes, then verify

## Edit options — English, not German soup

After a read, Edit opens the full list. Everyday settings sit up top; the rest is under Advanced. Each row shows a plain title, the NCS FSW name in green, a short hint where it helps, and the current value.

![EMS2000 Features — cruise, A/C, traction](/assets/images/r53-code-module-coding/edit-ems-features.jpg){:.img-md}
*Engine module Features group — cruise, A/C fitted, A/C cut at WOT, traction control.*

Browsing and staging changes is free. Writing to the car is the unlock.

## Backups — restore, edit-only, or share

Every successful read drops a timestamped `.bin` on the phone. Restore backup opens a card per file: restore to the car, load into the editor without writing, or share the file out.

![Choose a backup — BC1 cards](/assets/images/r53-code-module-coding/choose-backup.jpg){:.img-md}
*Backup picker — VIN, module, software index, and Restore / Edit only / Share on each card.*

I keep a known-good BC1 and EMS backup on the phone before I touch race-seat or lighting experiments.

## Planning mode — browse without the car

Planning mode is offline: pick any shipped definition, scroll every option, expand the choices. Nothing talks to the bus. Useful when I'm deciding what to change before I walk out to the garage.

![Planning mode — lighting options](/assets/images/r53-code-module-coding/planning-lights.jpg){:.img-md}
*BC1 planning — country variant, ECE daytime running lights, fog behaviour.*

![Planning mode — follow-me-home lights](/assets/images/r53-code-module-coding/planning-follow-me-home.jpg){:.img-md}
*Follow-me-home duration and automatic headlight coding — Off / 40 s / 90 s / …*

## Trouble codes and race seats

Same hub also opens **Trouble codes** — read / clear chassis and airbag faults with the same BMW-hex titles the logger uses (no fake SAE crosswalk on cluster / MRS / body).

On **MRS5K**, Edit options has **Race seats…**: one screen asks 1 vs 2 race seats, whether steering and passenger front bags stay On, and whether curtain / head bags stay On. A live preview lists every FSW that will go Off so driver / passenger / curtain pairings aren't mixed up. Apply only stages the editor; Write to car is still the gated step. Battery safety terminal coding is left alone on purpose.

## Want it?

I'm putting R53 Code on the Play Store next to the logger. If you want an early install or the listing ping when it goes live, email me:

**→ [matt@geekopolis.com](mailto:matt@geekopolis.com)**

Subject something like "R53 Code". Tell me which modules you care about (body, engine, airbag, cluster). Instagram: [**@mattryan6729**](https://www.instagram.com/mattryan6729/).

Track / off-road use for anything that disables restraints. Wrong coding can leave lamps on or change how the car behaves — read first, keep a backup, write only when you mean it.
