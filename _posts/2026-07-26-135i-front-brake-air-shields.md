---
title: "E82 135i Front Hub Brake Air Shields"
date: 2026-07-26 00:00:00 -0400
categories: car tech
tags: [bmw, 135i, e82, e88, brake-cooling, air-shields, deflectors, aluminum, sendcutsend, cults3d, track]
cover: /assets/images/135i-front-brake-air-shields/deflector-raw.jpg
lightbox: true
excerpt: "Aluminum front hub air shields that replace the E82 factory dust shields — cut flat, bend yourself, CAD on Cults3D for $25"
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/135i-front-brake-air-shields/deflector-raw.jpg
---

<!--more-->

## What they are

These are **front hub backing-plate air deflectors** for the **E82 / E88 135i**. They replace the factory dust shields and keep ducted (or wheel-well) air on the rotor instead of letting it wash out into the wheel.

They're the metal half of the front cooling path I wrote up in the [brake cooling ducts post](/car/tech/2026/07/14/135i-brake-cooling-ducts.html) — same parts, now sold as their own CAD pack.

![Bent aluminum brake air deflector](/assets/images/135i-front-brake-air-shields/deflector-raw.jpg){:.img-md}
*Finished aluminum shield — flat blank from SendCutSend, bent by hand.*

## Design

Modeled against a **3D scan of the hub and upright**, so the shield wraps the inboard rotor face and clears the caliper without fouling suspension travel.

![Deflector modeled at the hub against the 3D scan](/assets/images/135i-front-brake-air-shields/cad-deflector-hub.jpg){:.img-lg}
*Shield (gray) over the scanned hub (tan) — boxed around the back of the rotor with the caliper cleared to the side.*

![Deflector positioned inside the wheel](/assets/images/135i-front-brake-air-shields/cad-deflector-inwheel.jpg){:.img-lg}
*From inside the wheel — shrouds the inboard face so air stays on the rotor.*

## Material and fab

I run them in **5052 H32 aluminum, 0.080"**:

1. Cut **flat** at [SendCutSend](https://sendcutsend.com/)
2. **Bend by hand** to the finished shape
3. Trim as needed for your wheel package

`.080"` is stiff enough to hold shape next to a hot rotor, and still flexes a bit at full lock where clearance is tight.

Before bending metal, I **3D-printed a red mockup** — fit check on the car, and a former to bend the flat blank around.

![3D-printed red deflector used for fitment and as a bend helper](/assets/images/135i-front-brake-air-shields/deflector-red.jpg){:.img-md}
*Printed red part — mockup and bend helper, not the final car part.*

## Fitment notes

- Replaces the **factory front dust shields** on the E82 / E88 135i hub
- Some trimming is expected — these are fab parts, not bolt-on stamped OEM
- **Wheel clearance:** tight under **17×10**; better under **18×10**. On the 17s I ended up removing the **top and bottom front edge flaps**
- Full lock is a tight fit; the `.080"` aluminum has enough give

## What's in the download

Five files — STEP for fab shops, STL for print mockups / bend helpers:

| File | Use |
|------|-----|
| `pass-brake-shield.step` / `brake-backing-sheild.step` | Driver / passenger STEP for laser / waterjet |
| `passdeflector.stl` / `driverdeflector.stl` | Printed mockups (~147 × 325 × 342 mm) |
| `unbent-sheild.stl` | Flat blank reference (~2 × 325 × 374 mm) |

## Where to buy

**[e82 135i front brake air shields on Cults3D — $25](https://cults3d.com/en/3d-model/tool/e82-135i-front-brake-air-shields)**

Related printed ducts (same cooling system):

| Part | Cults3D |
|------|---------|
| Front control-arm duct | [View](https://cults3d.com/en/3d-model/various/e82-bmw-front-control-arm-brake-cooling-duct) |
| Rear brake ducts | [View](https://cults3d.com/en/3d-model/various/e82-rear-brake-ducts) |

## Fine print

- **Track use only**
- **No warranty** — sheet metal next to hot brakes; inspect after every event
