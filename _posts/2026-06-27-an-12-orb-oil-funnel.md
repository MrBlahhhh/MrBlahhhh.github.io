---
title: "AN-12 ORB Oil Funnel — Two-Part Swivel Fill"
date: 2026-06-27 00:00:00 -0400
categories: car tech
tags: [3d-printing, fusion-360, an-12, orb, ls3, oil-change, cults3d]
cover: /assets/images/an-12-orb-oil-funnel/installed.jpg
lightbox: true
excerpt: "No AN-12 ORB funnel for the LS BTR valve cover — quick Fusion 360 mock-up, two printed parts that snap together and spin independently so you can thread the base without twisting the whole funnel."
article_header:
  type: overlay
  theme: dark
  background_image: false
---

<!--more-->

The [135i track build](/car/2025/10/15/135i-l92-track-build.html) runs an **LS BTR valve cover** with an **AN-12 ORB** oil fill port. Off-the-shelf funnels for that fitting basically do not exist — and the port sits at an angle in a crowded bay, so a straight drop-in funnel is awkward to use anyway.

I mocked up a two-piece funnel in **Fusion 360**, printed it in ASA, and it works well enough that I put the STLs up on Cults3D.

## Two parts, one swivel joint

The funnel splits into a **basin** (`oilfillfunnel.stl`) and a **threaded base** (`oilfillbase.stl`). They lock together with a small interference lip — enough to stay assembled, not enough to spin on their own. You can hold the funnel still while you thread the base into the port, or spin the base independently once it is seated.

![Fusion 360 — AN-12 ORB oil funnel CAD](/assets/images/an-12-orb-oil-funnel/funnel-cad.png){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Wide basin, offset neck, and a separate AN-12 ORB base — modeled around the LS BTR valve cover fill location.*

## Snap-fit joint

The retention geometry is a simple groove-and-lip with chamfered lead-ins. Press the two halves together until they snap; the lip rides in the groove with a small clearance gap so the parts rotate freely but cannot pull apart vertically.

![Cross-section — swivel joint between funnel and base](/assets/images/an-12-orb-oil-funnel/joint-section.png){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Section view of the interference lip — captive joint with enough clearance for independent rotation.*

## In the engine bay

Printed in black ASA and installed on the red valve cover — the offset neck keeps the wide opening level even though the port is angled. No more balancing a cut bottle or a generic funnel that does not seal to ORB threads.

![AN-12 ORB oil funnel installed on the LS BTR valve cover](/assets/images/an-12-orb-oil-funnel/installed.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Two-piece funnel on the fill port — basin stays put while the base threads in.*

## Download the STLs

The files are **free or pay what you want** on Cults3D:

| Part | File | Approx. size |
|------|------|--------------|
| **Funnel basin** | `oilfillfunnel.stl` | 109 × 141 × 105 mm |
| **AN-12 ORB base** | `oilfillbase.stl` | 40 × 42 × 40 mm |

> **Download:** [AN-12 ORB oil funnel on Cults3D](https://cults3d.com/en/3d-model/tool/an-12-orb-oil-funnel) — free / open priced.

### Print settings

- **ASA** recommended — holds up to under-hood heat better than PLA or PETG
- Two separate prints, then snap the base into the funnel neck
- No supports needed on either part if you orient the base threads down and the funnel basin opening up

Designed for the **LS BTR valve cover** AN-12 ORB fill. If your port location or angle differs, measure before you commit filament.
