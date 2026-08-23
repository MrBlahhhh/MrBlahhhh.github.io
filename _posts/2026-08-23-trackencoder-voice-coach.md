---
title: "TrackEncoder — it talks now: brake calls in your helmet, and coaching ranked by measured seconds"
date: 2026-08-23 00:00:00 -0400
categories: car tech
tags: [trackencoder, android, telemetry, coaching, tts, bluetooth, video, track, garmin-catalyst]
cover: /assets/images/trackencoder-metrics/hud-full.jpg
lightbox: true
excerpt: "The recorder in my glovebox now calls my brake points into my helmet — in board feet, anchored to my own best lap — and every piece of coaching it writes carries the measured seconds it would save."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/trackencoder-metrics/hud-full.jpg
---

<!--more-->

<video controls playsinline preload="metadata"
       poster="/assets/images/trackencoder-metrics/voice-lap-poster.jpg"
       style="width:100%;height:auto;display:block;border-radius:8px;box-shadow:0 2px 14px rgba(0,0,0,.45);">
  <source src="/assets/images/trackencoder-metrics/voice-lap.mp4" type="video/mp4">
</video>

*The aggressive lap again — same half of the circuit as the last showcase,
slides and all — but turn the sound on this time. Three brake calls land
before their zones ("turn 8, brake at 475"), coaching notes follow the corners
that earned them, and the 26% slides light the slip car up in between. What
you hear is what my helmet hears over Bluetooth: the audio is reconstructed
from the session's own sidecar file, each phrase at the moment the phone
actually spoke it. The brake numbers are measured against the track's real
marker boards, read off my own footage frame by frame.*

The phone records from the glovebox. I [can't see it, and I never touch
it](/car/tech/2026/08/22/trackencoder-remote-control.html) — which meant every
number the overlay computes arrived after the session, burned into video. The
[overlay itself](/car/tech/2026/08/21/trackencoder-metrics-explained.html)
hasn't changed. What's new is that the recorder now has a voice, and the
coaching behind that voice got a lot sharper.

## "Turn five, brake at 150"

I brake against the marker boards — the 300 / 200 / 100 boards that sit before
every real corner. So the voice doesn't say *brake now*, which would make
Bluetooth latency part of my braking zone. It says a **number**, well in
advance — about seven seconds before the braking zone at whatever speed I'm
carrying — and I execute it against the boards with my own eyes. Nothing about
the call is timing-critical, which is what makes a $150 Android phone able to
do the Catalyst's headline trick.

The number is my own best lap's brake point for that corner, measured back
from turn-in and rounded to the 25 ft a board lets you place. That makes it a
ratchet: brake later than the call, and the best updates, so next lap the call
moves deeper. Compressing a braking zone stops being something I reconstruct
from data on Monday and becomes something the car asks me to do, corner by
corner, while I'm there.

Corners where my best trail-brakes past turn-in get no call — "brake at zero"
isn't a thing you can do with a board, and the system would rather stay quiet
than say something unusable.

## Coaching that knows what a corner costs

Every coaching line the overlay writes now ends with measured time:

```
T7: braked 42 ft earlier than your best [0.4 s]
T2: 4.1 mph less at the apex than your best [0.2 s]
```

That `[0.4 s]` is not an estimate. The delta grid keeps a millisecond clock in
every metre of the lap for both the current lap and the reference, so the time
lost through a corner is the subtraction of four numbers. The session coach
ranks its findings by those measured seconds — so when it says *three things
that would cut the most lap time*, the ranking is by time, not by how often a
flag happened to trip.

Two new things it can say, too: **turn-in** — early or late, in feet, from
where lateral load actually rises rather than where the map thinks the corner
starts — and **track-out**, which only speaks on circuits where the GPS is
honest enough to support it, and stays silent everywhere else rather than
coaching noise.

## The spoken advice, if you want it

Brake calls are what I'm running at my next event. But the voice can also
speak the coaching itself — the line above compressed to something that
survives being heard at 100 mph: *"brake later into turn seven."* One phrase
every six seconds at most, and a brake call interrupts advice mid-sentence,
because the corner that's arriving outranks the one that's gone.

The whole thing is one empty file to turn on, and the file's contents pick the
mode: empty means brake calls only, the word `all` adds the advice. No model
runs in the car — the voice is a deterministic rule reading numbers the
overlay already computed, which means it works with the radio off, the cloud
unreachable, and the phone in a glovebox at 140°F.

## The session knows its own shape

The coach's summary now opens with a pace trend — lap time fitted across the
session's real laps: `IMPROVING`, `STABLE`, or `DEGRADING`, with a predicted
next lap. And the moment a session turns DEGRADING, the phone sends a Telegram
message to my pocket, right next to where its temperature warnings already
arrive — because when the pace falls off half a second a lap, the answer is
tyres, driver, or heat, and I'd like to be asking that question in the paddock
rather than discovering it in the data on Monday.

Out-laps and cool-down laps stopped polluting all of this, too. The coach now
fences laps statistically — a lap far slower than the session's own spread is
recorded but never coached from — so "4 mph slower at every apex" on a
cool-down lap no longer buries the three findings that matter.

## Kerbs, from the data I already have

One more thing the Garmin can't do: TrackEncoder now carries a **kerb map**.
Built offline from my Catalyst's own logged laps, it marks the stretches of
each circuit where running wide genuinely changes the ride — 18 zones at CMP,
each tagged with the turn it belongs to and which side of the road it's on.
The session coach uses it to say where there's kerb available to take, and
because the map knows where kerbs *aren't*, it will never invent kerb advice
at a track that has none.

---

The voice goes to its first real event next weekend — brake calls only, into
my helmet, with the advice held back until I know how a voice in the car
feels at speed. The recording pipeline underneath is unchanged: same
glovebox, same SD card in the SIM tray, same
[Telegram remote](/car/tech/2026/08/22/trackencoder-remote-control.html)
running the session from my pocket.
