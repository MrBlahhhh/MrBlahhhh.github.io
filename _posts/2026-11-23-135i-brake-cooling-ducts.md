---
title: "E82 135i Brake Cooling Ducts: Front and Rear"
date: 2026-11-23 00:00:00 -0500
categories: car tech
tags: [3d-printing, bmw, 135i, e82, e88, e90, brake-cooling, brake-ducts, track, cults3d, cfd]
cover: /assets/images/135i-brake-ducts/front-duct-installed.jpg
lightbox: true
license: true
redirect_from:
  - /car/tech/2026/07/14/135i-brake-cooling-ducts.html
excerpt: "Printed front and rear brake cooling ducts for the E82 135i, tested in CFD. Explore every part in 3D, then buy the STLs or a finished set printed and shipped."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/135i-brake-ducts/front-duct-installed.jpg
---

<style>
.bd-viewer { position: relative; width: 100%; height: min(78vh, 720px); min-height: 460px; border-radius: 10px; overflow: hidden; background: #06090d; margin: 0 0 .6rem; }
.bd-viewer iframe { position: absolute; inset: 0; width: 100%; height: 100%; border: 0; }
.bd-cta { display: flex; flex-wrap: wrap; gap: .6rem; margin: 1rem 0 1.6rem; }
.bd-cta a { display: inline-block; padding: .55rem 1rem; border-radius: 6px; background: #1f6feb; color: #fff !important; font-weight: 600; text-decoration: none; }
.bd-cta a.alt { background: transparent; color: inherit !important; border: 1px solid currentColor; }
.bd-cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1rem; margin: 1rem 0 1.6rem; }
.bd-card { border: 1px solid rgba(127,127,127,.35); border-radius: 8px; padding: .9rem 1rem; }
.bd-card h3 { margin: 0 0 .3rem; font-size: 1.05rem; }
.bd-card .big { font-size: 1.6rem; font-weight: 700; line-height: 1.2; }
.bd-card p { margin: .3rem 0 0; font-size: .92rem; }
</style>

<div class="bd-viewer"><iframe src="/135i-brake-ducts-3d/" title="135i brake ducts in 3D" loading="eager" allowfullscreen></iframe></div>
*Drag to orbit, scroll or pinch to zoom. The tabs switch between the whole front corner, the whole rear corner and the front arm duct on its own, with the air running through them. [Open the 3D view full screen](/135i-brake-ducts-3d/).*

<!--more-->

Printed brake cooling ducts for the E82 135i, front and rear, designed around a 3D scan of my own car and checked in CFD. Buy the STLs and print your own, or order a finished set and it'll be printed and shipped to you.

<div class="bd-cta">
  <a href="#get-a-set">Get a set</a>
  <a class="alt" href="https://cults3d.com/en/3d-model/various/e82-bmw-front-control-arm-brake-cooling-duct">Front duct STL</a>
  <a class="alt" href="https://cults3d.com/en/3d-model/various/e82-rear-brake-ducts">Rear duct STL</a>
</div>

<div class="bd-cards">
  <div class="bd-card"><h3>Front control-arm duct</h3><div class="big">209 CFM</div><p>through the duct at 100 mph in clean air. Rides on the arm, so the outlet stays on the caliper through travel.</p></div>
  <div class="bd-card"><h3>Rear duct</h3><div class="big">225 CFM</div><p>through the duct at 100 mph in clean air. The rear gets no factory cooling at all, and this is the brake I used to cook.</p></div>
  <div class="bd-card"><h3>Front air shields</h3><div class="big">Aluminum</div><p>flat blanks you bend yourself; they keep the ducted air on the rotor instead of washing into the wheel well.</p></div>
</div>

## Why I built them

The brakes never faded on me. The heat cooked the hardware instead: I overheated the rears plenty of times, and in the end the **ceramic piston tips in the calipers broke up from the heat, front and rear**.

The E82 has cooling passages molded into the front bumper, but they only let air into the wheel well. Nothing aims it at the caliper. The rear gets nothing at all, which is why the rears were the ones that kept cooking. Molded kits cost hundreds and end in a hose you still have to route, so I modeled my own: a front duct on the control arm and a rear duct off the subframe, both with the outlet pointed straight at the caliper.

## Designed from a 3D scan

Both ducts were modeled against a **3D scan of my car's hub, arms and floor** (the tan mesh below), so they clear the real hardware through suspension travel instead of fitting a drawing.

![Front duct modeled against the 3D-scanned suspension](/assets/images/135i-brake-ducts/cad-scan-front.jpg){:.img-lg}
*The front duct (gray) drawn in place against the scan (tan), clearing the real arm and upright.*

![Rear duct modeled against the 3D-scanned rear end](/assets/images/135i-brake-ducts/cad-scan-rear.jpg){:.img-lg}
*The rear duct routed through the scanned subframe and floor. Every bend was checked against the real geometry before anything was printed.*

## The front duct

The front duct clamps to the lower control arm and puts a scoop out in the air ahead of the wheel. The body necks down and the outlet dumps straight onto the caliper.

Because it's on the arm, not the strut or the backing plate, it **moves with the suspension**, so the outlet stays on the caliper through the whole range of travel. A strap and the modeled-in saddle hold it on: no drilling into anything structural.

![Front duct mounted to the control arm, scoop facing forward](/assets/images/135i-brake-ducts/front-duct-scoop.jpg){:.img-md}
*Clamped to the control arm, mouth out in clean air ahead of the wheel.*

![Front duct outlet at the caliper, wheel installed](/assets/images/135i-brake-ducts/front-duct-caliper.jpg){:.img-md}
*The outlet tucked in behind the wheel, aimed at the caliper.*

![Front duct viewed from under the car](/assets/images/135i-brake-ducts/front-duct-installed.jpg){:.img-md}
*From underneath: the neck-down from mouth to outlet speeds the air up onto the caliper.*

## The rear duct

The rear duct takes air through a wide finned mouth under the floor and sweeps it round to the caliper off the subframe, tucked in among the rear links. The mouth is a nod to the **Ford Mustang GTD**, which feeds its rear brakes the same way, scaled and reshaped to fit under the 135i.

![Rear duct finned inlet mouth under the floor](/assets/images/135i-brake-ducts/rear-duct-mouth.jpg){:.img-md}
*The finned mouth sits below the floor, ahead of the rear tire.*

![Rear duct mounted at the subframe, side profile](/assets/images/135i-brake-ducts/rear-duct-mounted.jpg){:.img-md}
*It picks up on the subframe hardware: no brackets to make.*

![Rear duct outlet aimed at the rear caliper](/assets/images/135i-brake-ducts/rear-duct-outlet.jpg){:.img-md}
*The outlet nozzle, pointed at the rear caliper.*

## Front air shields

A duct only helps if its air stays on the rotor. The **air shields** are aluminum plates that mount at the hub and keep the ducted air across the rotor face instead of letting it spill into the wheel well. They come as flat laser-cut blanks (I use [Send-Cut-Send](https://sendcutsend.com/)) that you bend to shape. I printed one first as a fit check and a bending former.

![Bent aluminum brake air shield](/assets/images/135i-brake-ducts/deflector-raw.jpg){:.img-md}
*The flat aluminum blank, bent by hand. The big radius clears the hub and the tab forms the air dam.*

![Air shield modeled at the hub against the 3D scan](/assets/images/135i-brake-ducts/cad-deflector-hub.jpg){:.img-lg}
*Drawn over the hub against the scan, boxed round the back of the rotor with the caliper cleared.*

More on them in the [air shields post](/car/tech/2026/07/26/135i-front-brake-air-shields.html).

## What the CFD shows

Each duct went through a wind tunnel simulation at 100 mph, built from the same scans the parts were designed against, with the suspension put back at ride height. The 3D view at the top is those runs: pick a duct tab and tick "Only air through the duct" to see just the air each one takes in.

| Duct | Air through it at 100 mph |
|------|---------------------------|
| **Front control-arm duct** | 209 CFM |
| **Rear duct** | 225 CFM |

The flow is measured on cross-sections inside each duct, and the sections agree within 1%. The front corner tab is the whole front corner of my car at 125 mph, with the air running through it: bumper duct, splitter with its end plates and ramps, fender flares, the arm duct, the air shield and the brake.

## Fitment

- The front duct fits the **E82 / E88 135i** front lower control arm, which much of the E9X family shares.
- The rear duct fits the **E82** rear subframe.
- Both mount with straps and modeled-in saddles. **No drilling into anything structural.**
- Clears **17×10** and **18×10** wheels.
- Takes standard **3 in brake hose** if you want to feed the front mouth from a bumper inlet, or run it as a scoop.

## How they've worked out

They work. The cooling has **noticeably extended pad and rotor life**, and the piston tips haven't failed since.

With this much air on them, the brakes can squeak on tracks without heavy braking zones, where the pads never get up to temperature. On a track with real braking zones that goes away.

![The 135i track car, #78, at a wet track weekend](/assets/images/135i-brake-ducts/track-78.jpg){:.img-lg}
*#78 out at a wet weekend.*

<div>{%- include extensions/youtube.html id='Nnp8NiAkXNw' -%}</div>
*#78 on track.*

## Get a set

### Printed and shipped

Printed to order on a **Bambu Lab H2S** and shipped to you, priced per pair (both sides of the axle):

| Set | ASA | PAHT-CF |
|-----|-----|---------|
| **Front pair** | **$100** | **$120** |
| **Rear pair** | **$150** | **$175** |
| **Full car** (front + rear) | **$250** | **$295** |

**ASA** is the standard UV-stable choice. **PAHT-CF** is the black, maximum heat resistance option. Shipping is billed at cost. Email [**ryan3ddesigns@gmail.com**](mailto:ryan3ddesigns@gmail.com?subject=135i%20brake%20ducts%20order) with the sets and material you want and your zip code, and you'll get a quote back.

### Print your own

| Part | Files |
|------|-------|
| **Front control-arm brake cooling duct** | [Cults3D](https://cults3d.com/en/3d-model/various/e82-bmw-front-control-arm-brake-cooling-duct) |
| **Rear brake ducts** | [Cults3D](https://cults3d.com/en/3d-model/various/e82-rear-brake-ducts) |
| **Front brake air shields** (flat aluminum blanks) | [Cults3D](https://cults3d.com/en/3d-model/tool/e82-135i-front-brake-air-shields) |

They live under the car in road grime, rotor heat and sun, so print them in a heat- and UV-tolerant material like ASA or PAHT-CF, not PLA. Dark colors hide brake dust.

![Both ducts fresh off the printer](/assets/images/135i-brake-ducts/printed-parts.jpg){:.img-lg}
*Off the plate: a finished front duct, and a rear duct with its supports still on.*

## The fine print

- **Track use only.**
- **No warranty.** It's a printed part under a moving car next to hot brakes. Check it and its straps at every event, like any other duct or heat shield.
