---
title: "TrackEncoder — what every number on my overlay actually means"
date: 2026-08-21 00:00:00 -0400
categories: car tech
tags: [trackencoder, android, telemetry, racecapture, can, datalogger, vehicle-dynamics, track, cmp, vir]
cover: /assets/images/trackencoder-metrics/hud-full.jpg
lightbox: true
excerpt: "My phone burns a live coaching overlay into track video. Here's every metric in plain language — what it means, the formula behind it, and where the idea came from."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/trackencoder-metrics/hud-full.jpg
---

<!--more-->

<video controls loop muted playsinline preload="metadata"
       poster="/assets/images/trackencoder-metrics/catching-a-slide-poster.jpg"
       style="width:100%;height:auto;display:block;border-radius:8px;box-shadow:0 2px 14px rgba(0,0,0,.45);">
  <source src="/assets/images/trackencoder-metrics/catching-a-slide.mp4" type="video/mp4">
</video>

*Seven seconds of catching the rear on a corner exit, and everything below
working at once: the slip chips filling and the rears colouring up, red rings
pinging over whichever tyre let go, the steering bar swinging amber as the rate
climbs past 200 °/s, POWER OVERSTEER appearing, and the amber and red ribbons
stacking along the top of the input trace behind it. The corner card on the
right is scoring the corner while it happens.*

## What this thing is

TrackEncoder is an Android app that takes a USB camera feed and my RaceCapture
MK4's CAN telemetry over Bluetooth, burns a coaching overlay into the video
live, and writes it to an SD card in the car. No post-processing, no syncing
data to footage afterwards — the analysis is already in the frame when I get
home.

![The full overlay](/assets/images/trackencoder-metrics/hud-full.jpg){:.img-lg}
*The whole thing on the in-car Moto G, replaying my 1:43.60 at Carolina Motorsports Park. Everything sits on the A-pillar, the headliner and the dashboard — the parts of the frame the car body blocks anyway. Only about 20% of a bolted-in camera's frame ever carries road, and the overlay is laid out around that. Camera is unplugged here so the widgets are legible.*

This is the "what do these numbers mean" writeup.

## Two rules everything follows

**Compare a measurement to a reference the car taught you, not to a number you
assumed.** All four of my wheels free-roll about 4.7% slower than GPS — rolling
circumference versus whatever the logger is configured for, which moves every
time slicks go on. My throttle channel reads 12 at rest, not 0. So every
threshold that matters is a percentage of something the app learns during the
session, and nothing here compares a wheel speed to GPS.

**Never trust one sensor for something two can settle** — and know which sensor
fails when:

| Sensor | Trust | Fails when |
|---|---|---|
| Wheel speeds | Highest — hardwired into the car | Below ~5 mph |
| Steering angle | High — hardwired | Zero can drift |
| Accelerometers | Good for magnitude | **Goes quiet during a slide** |
| GPS | Position only | Drops, drifts, lags under braking |
| Yaw gyro | Lowest — glitchy on this unit | Anywhere |

That third row shapes a lot of the design. It gets its own section.

## Coasting — the biggest time-loser

If you're not speeding up, not slowing down, and not turning, the car is doing
nothing with its tyres and you could have gone faster. That's the whole idea,
and it's the first thing Ross Bentley goes after in a novice's data in *Speed
Secrets*.

![Coasting regions on the input trace](/assets/images/trackencoder-metrics/trace-coast.jpg){:.img-lg}
*Two coasts in one eight-second window. Throttle green, brake red, steering white — the MoTeC/AiM colour convention. Each yellow region has a bright line at its start and end, and it's recorded per column, so it scrolls off with the data that produced it instead of vanishing the moment the car settles.*

```
coasting  =  speed        > 20 mph
         AND throttle     < 90 % of learned travel
         AND brake        < 5 % of learned travel
         AND sqrt(ax² + ay²) < 0.25 g
         AND |steering|   < 30°
         AND rear slip    < 4 %
         sustained for    > 250 ms
```

Every term earns its place. The throttle test is a **ceiling**, not a floor — a
partial lift held down a straight is dead time too. The g test does the real
work, because a car earning its lap time is always using grip for something. The
throttle ceiling covers the one case g can't see: flat out at terminal velocity,
where every g is zero and nothing is being wasted. Steering and slip cover the
case where g gets it *wrong* — see the slide section below. And the 250 ms is so
a gear shift isn't counted as a coast.

![The out-lap](/assets/images/trackencoder-metrics/coasting.jpg){:.img-lg}
*Same frame in context — `COAST 6%` in the metrics block, on the 2:09 out-lap. On my 1:43 it reads under 1%.*

Across three real laps it reads 9.3% on the out-lap, 0.50% on my fast lap, and
0.41% on my most aggressive one.

## Wheel spin versus wheel lock

A wheel turning faster than the car is spinning. A wheel turning slower is
locking. Opposite problems, opposite fixes, so they're measured separately and
drawn differently.

![Wheel spin with radar pings](/assets/images/trackencoder-metrics/slip-car.jpg){:.img-md}
*25% rear slip on the throttle. Red rings expand and fade over the tyres that broke traction, the rears colour up the green-amber-red ramp, and the chip names it: POWER OVERSTEER, because the throttle was open when the wheels let go.*

### Road speed without GPS

You can't measure slip without knowing how fast the car is actually going, and
GPS lags hardest under braking — exactly when a lock-up test needs it. So road
speed comes from the wheels:

```
fastest = max(FL, FR)                       undriven, so it can only read low
roadRef = max(fastest, roadRef − 1.5 g · dt)
```

The rate limit is the whole trick. If both fronts lock together the estimate
would collapse with them and hide the lock inside itself. Capping how fast the
reference may fall at slightly more than the tyres could ever slow the car keeps
it honest. This is what production ABS does with its reference velocity, for
exactly the same reason.

My measured braking is p999 1.30 g across 213 minutes of logs, so 1.5 g is clear
of anything real. The same figure taken from GPS speed differentiated at 40 ms
claims 4.2 g, which is a good illustration of why the reference doesn't use GPS.

### The two questions

```
spin (rear)  = (rear − frontAvg × axleRatio) / (frontAvg × axleRatio) × 100
lock (any)   = (roadRef − wheel) / roadRef × 100
```

`axleRatio` is learned free-rolling each session — straight, off the brakes,
under 30% throttle, under 0.35 g. On my car the two axles agree to **0.32%**,
which is what same-size tyres front and rear should look like, but it's measured
rather than assumed so a tyre change or a staggered setup can't move it
silently.

### Which one it is

The rule is mine, from the driver's seat, and it's just torque:

> Off the gas it cannot be wheelspin. Hard on the brakes it's probably lock-up.

```
SPIN  if  slip > +4 %  AND throttle > 15 %
LOCK  if  lock > +8 %  AND brake    > 15 %
none  otherwise → the number prints, with no colour
```

That last line matters more than it looks. A reading past the threshold with
both feet off is tyres and road, not something I did, and colouring it is how an
instrument teaches you to ignore it.

![Front lock-up ping](/assets/images/trackencoder-metrics/wheel-lock.jpg){:.img-lg}
*57% brake, and both fronts have just pinged — the pale blue rings. Lock draws blue deliberately, outside the green-amber-red ramp entirely, because it's a different fault rather than a worse one. Note the rears reading **−1%** and **−3%**: negative, meaning slower than the front axle. The sign is the answer, so it's kept.*

Worth noting what *isn't* in that frame: the tyres themselves are green. That
lock lasted a couple of hundred milliseconds and was over before the screenshot
landed — the ring outlives it by 620 ms, which is the entire reason it's a ring
and not just a colour change. Across 157 frames sampled over three laps I
couldn't catch a single blue-filled tyre, which is what the calibration
predicts: at an 8% threshold a clean lap produces **zero** front-lock events. If
it were easy to screenshot, the threshold would be wrong.

## The radar pings

A ring flashes over whichever tyre misbehaved and fades as it expands, so you
see *when* it happened rather than just *that* it's happening.

```
fires when  classified event AND |slip| > 6 %  AND  260 ms since the last one
age    = (now − startMs) / 620 ms          0 → 1   ring lifetime
radius = wheelHeight × (0.5 + age × 1.9)
alpha  = (1 − age) × 220
colour = red for SPIN, blue for LOCK
```

A ring reads as an *event*. A filled dot reads as a *state*. The re-arm timer is
why a long slide pulses instead of drawing one solid disc.

## The colour ramp

One ramp, used by everything that has a colour:

```
t = value / limit                                   0 → 1
w = t^0.7                                           gamma weighting
w < 0.5 :  lerp(#3FB950 → #E8A020, w × 2)           green → amber
w ≥ 0.5 :  lerp(#E8A020 → #E5484D, (w−0.5) × 2)     amber → red
```

The `^0.7` is the important part. Linear interpolation keeps a gauge stubbornly
green until it's nearly at the limit; gamma weighting starts the colour moving
early, so a climbing temperature is visible well before it's a problem. Same
reason a temperature needle sweeps across its face instead of sitting pinned
until it redlines.

Used for oil and water temperature, rear slip, steering rate and grip
percentage. Lock-up sits deliberately outside it, in blue.

## The steering bar, coloured by how fast I turn

How *far* I turned is the length of the bar. How *fast* I turned is its colour —
green smooth, red catching something.

I didn't want to guess that scale, so I processed every log I have. The script
splits each steering-rate sample by whether the rear axle was away at the time,
which is exactly the question the colour needs answered:

```
                     p50    p95    p99   p999      n
  rear settled       5.0   75.0  152.5  312.5   307k
  rear away >5%     27.5  177.5  385.0  792.5    10k
```

Ordinary driving lives under about 150 °/s. Catching a slide runs 175–400. So
full scale is **400 °/s**, and it's capped at 800 — one sample in the logs
glitched to 1307 °/s, and an uncapped session maximum would leave every real
save reading green for the rest of the day.

```
rate    = |Δsteer| / Δt          at 25 Hz, smoothed 0.35
scale   = clamp(session max, 400, 800)
colour  = ramp(rate / scale)
```

![Steering bar at two rates](/assets/images/trackencoder-metrics/steering-bar.jpg){:.img-lg}
*Same widget, four minutes apart in the same session. 246 °/s catching the rear on a corner exit, and 9 °/s tracking down a straight. The rate is printed next to the angle — a colour with no number behind it is unreviewable.*

## AT LIMIT — asking for more than the car had

Two independent ways the car tells me I asked too much, and neither needs a
measured car constant:

```
SAWING       3 steering reversals within 1.6 s,
             each carrying ≥ 12° of hand wheel

SATURATION   steering rose ≥ 8° across 600 ms
             while lateral g rose ≤ 0.02 g,
             held 400 ms
```

Saturation is understeer stated directly: more steering bought no more
cornering. Sawing is me running out of ideas.

Which end let go is decided by the wheels, never the steering:

```
rear slip ≥ 5 % held 120 ms  AND throttle > 25 %  →  POWER OVERSTEER
rear slip ≥ 5 % held 120 ms                       →  OVERSTEER
otherwise                                         →  UNDERSTEER
```

The verdict can be **upgraded mid-event**, which is the interesting case. A
ribbon along the top of the input trace shades amber while the front pushes and
turns red the moment the rear lets go, so a corner that understeers into a
power-on slide reads as one event with a story. It's counted once, as the
oversteer it ended as. The ribbon is backfilled to where the evidence started —
a saw only qualifies on its third reversal, so without that it would mark the
recovery instead of the moment that caused it.

## Why slip decides when the car is working, not lateral g

This is the physics behind that third row in the sensor table, and it shapes
four different widgets.

**A car with the rear away isn't generating lateral force.** The accelerometer
goes quiet at exactly the moment I'm working hardest. Here's a second and a half
of a real opposite-lock catch:

```
 t      mph  steer   latG   |g|  rearSlip%
 6.00  49.2  +46.4   0.18  0.25      5.7
 6.24  50.4  +43.8   0.17  0.24     12.3
 6.72  54.5  −10.6   0.17  0.24     16.7
 6.96  56.1  −53.4   0.17  0.24     −0.4
```

46 degrees of lock reversing to −53 in one second, 17% rear slip, 40% throttle —
and combined g pinned at 0.24 the whole way through. Any test written as
`|latG| > threshold` is blind through that, so four of them take confirmed rear
wheel slip as evidence in its own right instead:

| Gate | Would be blind at | Also accepts |
|---|---|---|
| AT LIMIT load gate | 0.55 g | confirmed rear slip |
| `CORRECT` counter | 0.30 g | confirmed rear slip |
| Corner segmentation | 0.35 g | confirmed rear slip |
| Coasting | < 0.25 g | vetoed by steering **or** slip |

Slip is loudest exactly where lateral g goes quiet, which is what makes it the
right corroborator rather than just another signal.

## The grip circle

A dot showing what the tyres are doing now, inside a ring showing the most
they've ever done.

![Grip circle](/assets/images/trackencoder-metrics/grip-circle.jpg){:.img-md}
*The friction circle from Milliken's Race Car Vehicle Dynamics, with the trail coloured by phase — brake red, lateral amber, drive green. Egg-shaped on purpose: this car brakes and corners far harder than it accelerates, and a perfect circle makes every power-limited exit look like timidity.*

```
GRIP % = current combined g / envelope radius in this direction
```

The envelope is **banded by speed** (<40, 40–70, >70 mph), because the achievable
g-g set changes with speed and one session-wide envelope flatters slow corners
while libelling fast ones. A band with under 200 live samples falls back to its
neighbour rather than drawing a confident ring around twelve points.

## Lap metrics

![Lap metrics](/assets/images/trackencoder-metrics/metrics.jpg){:.img-md}
*Live lap on the left, last completed on the right. Green beats the session best, red is worse. `AT LIMIT 6U/20` means six understeer events and twenty oversteer ones so far this lap — this car does not lack for oversteer.*

| Row | Definition | Better is |
|---|---|---|
| `WOT` | % of samples at ≥ 95% of learned throttle travel | higher |
| `COAST` | % meeting the coasting rule above | lower |
| `GRIP` | mean of combined g / envelope, above 0.25 g | higher |
| `PK BRAKE` | max brake as % of learned travel | higher |
| `STEER °/s` | mean \|Δsteer/Δt\| while more than 5° of lock is on | lower |
| `CORRECT` | reversals ≥ 12° and ≥ 25 °/s, while loaded **or sliding** | lower |
| `AT LIMIT` | understeer / oversteer events, as `nU/nO` | lower |

`STEER °/s` and `CORRECT` are review statistics, not live scolds — a correction
that catches a slide is not a fault. They're reported, never flashed.

Everything accumulates on a fixed 25 Hz tick driven by the device's own clock,
not per frame. The GL loop runs at display refresh and would triple-count.

## The balance bar

This compares how fast the car *is* rotating against how fast the steering
*asked* it to. The gap is understeer or oversteer.

```
δ      = SteerAngle / steering_ratio         road-wheel angle
r_ref  = v · δ / (L · (1 + K · v²))          the rate asked for
error  = r_ref − yaw_measured                + understeer, − oversteer
```

That's the bicycle model from Milliken, and it sits inside every production
stability-control system. `K` is the understeer gradient, and with `K = 0` the
model over-predicts yaw badly at speed. The bar ships dark until the car
constants are measured, because an instrument that's *confidently wrong* is
worse than one that's absent.

I did get an estimate out of the logs. Regressing steering against lateral g
directly doesn't work, because a slow hairpin and a fast sweeper at the same g
need completely different lock — the corner radius contaminates the slope. An
FSAE skidpad removes that by holding radius constant, and radius is recoverable
arithmetically, which lets every corner in every log contribute:

```
R      = v² / (a_y · g)
δ_ack  = L / R                    the angle the geometry alone needs
δ_meas − δ_ack = K_us · a_y
```

Over 28,736 steady-state samples — hands not moving, load not changing, speed
held — `K_us` comes out at **1.56 °/g** at the road wheel with an intercept of
−0.55°, near enough zero that the fit holds together.

It's in the car file, still flagged unmeasured, because it assumes steady state
at every sample and scales with a wheelbase I haven't measured. Nice property
though: it uses **no gyro at all**, only steering, speed and lateral g.

## Where the ideas came from

- **Ross Bentley, *Speed Secrets*** — coasting as the primary time-loser;
  brake/throttle/steering traces as the first thing to read in any log.
- **Milliken & Milliken, *Race Car Vehicle Dynamics*** — the friction circle,
  the bicycle model, the understeer gradient and the reference-yaw-rate relation.
- **The FSAE skidpad procedure** — the standard way to measure an understeer
  gradient. I did it arithmetically instead of physically.
- **MoTeC i2 and AiM RaceStudio conventions** — throttle green, brake red, short
  trace windows so one corner fills the panel, steering smoothness as a primary
  channel.
- **Production ABS reference velocity** — the fastest-wheel road-speed estimate
  with a bounded deceleration.

Source is at [github.com/MrBlahhhh/TrackEncoder](https://github.com/MrBlahhhh/TrackEncoder)
— `docs/metrics-explained.md` has the same content as a reference, and
`tools/calibrate.py` regenerates the numbers from a log directory.
