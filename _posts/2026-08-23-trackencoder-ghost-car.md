---
title: "TrackEncoder — the ghost car: a video-game racing line on a $150 phone, and a coach that phones a friend"
date: 2026-08-23 12:00:00 -0400
categories: car tech
tags: [trackencoder, android, telemetry, coaching, garmin-catalyst, gps, llm, track]
cover: /assets/images/trackencoder-metrics/hud-full.jpg
lightbox: true
excerpt: "The overlay now draws the corner ahead like a racing game: my best lap painted on the road in brake/transition/throttle colours, my actual line drawn live against it, and a ghost running my best lap's clock. And after the session, one button hands the numbers to an LLM that names the three things worth the most time."
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
       poster="/assets/images/trackencoder-metrics/ghost-car-poster.jpg"
       style="width:100%;height:auto;display:block;border-radius:8px;box-shadow:0 2px 14px rgba(0,0,0,.45);">
  <source src="/assets/images/trackencoder-metrics/ghost-car.mp4" type="video/mp4">
</video>

*The left column of the overlay, magnified. Top: the whole circuit. Below it:
the next 100 metres of road, drawn like a racing game draws it. The wide faded
line is my best lap painted onto the tarmac — red where it braked, amber
between brake release and apex, green on throttle. The crisp line is me, live,
wearing the same colours from my own pedals. The dot is my car, lit by what my
feet are doing right now; the hollow ring is the ghost — my best lap replayed
on this lap's clock, ahead of me or behind me exactly like a video game. B and
A mark its brake point and apex on the road.*

Racing games solved coaching display decades ago: paint the ideal line on the
road, colour it by what the pedals should be doing, and run a ghost car so
"ahead" and "behind" are something you *see* rather than compute. That's now
burned into every frame my glovebox recorder writes.

## What the panel is actually showing

Everything on it is measured, and each element has its own source of truth:

- **The road** — centreline and width from my Garmin Catalyst's surveyed track
  database, one point per metre. All my track maps now come from that survey,
  which is also what cuts the sectors, so every distance in the system —
  brake points, apexes, sector times — lives on the same ruler the Catalyst
  uses.
- **The painted line** — where my fastest lap of the session actually drove,
  recorded metre by metre, coloured by *its* pedals: red from its measured
  brake onset for its measured braking zone, amber to its apex, green
  everywhere else. The colour boundary where the paint turns red **is** the
  target brake point, on the road, arriving before the corner does.
- **My line and my dot** — the same measurement, this lap, coloured by my own
  pedals live. Where my red starts against where the paint turns red is the
  brake-point comparison, readable at speed. About five seconds of history
  stays on screen, so the braking story is still visible mid-corner.
- **The ghost** — the reference lap replayed against this lap's clock through
  the delta grid's metre-by-metre timing, which is the same machinery that
  produces the live delta number.

Two honesty rules keep it from lying. The road draws at twice its true width —
at real scale the lines nearly fill the corridor — but every lateral offset
is scaled by the same factor, so *where you are between the edges* stays true;
a car at the real edge draws at the drawn edge. And the display physically
cannot draw an impossible move: the drawn line's sideways rate is capped at
what a tyre can actually do at your current speed, so GPS scatter shows up as
nothing instead of as a 100-mph sideways hop.

## How the coaching underneath works

The panel is the newest face on a pipeline the
[overlay](/car/tech/2026/08/21/trackencoder-metrics-explained.html) and
[voice coach](/car/tech/2026/08/23/trackencoder-voice-coach.html) posts
describe in full. The short version: the app detects each corner as it
happens, scores it against the best pass it has ever stored for that corner —
entry speed, brake point, apex speed and position, turn-in, track-out — and
writes every finding down with the measured seconds it cost, taken from a
millisecond clock the delta grid keeps in every metre of the lap. Laps that
are really out-laps or cool-downs are fenced off statistically and never
coached from. None of this needs the cloud; it runs in a hot glovebox with
the radio off.

## The handoff: one button, three findings

What the car can't do is notice that five findings are one habit. "T7 apex
speed", "T9 apex speed" and "T12 apex speed" are three lines in a list; *"you
turn in early on every long-radius right, and it's worth eight tenths"* is a
level of reasoning above what a threshold can reach.

So there's a **COACH** button on the
[Telegram remote](/car/tech/2026/08/22/trackencoder-remote-control.html).
Press it and the phone builds a plain-text summary and sends it to a language
model, which reads the whole session at once and answers with a ranked three —
straight back into the same chat, next to the temperature warnings.

What goes out is deliberately narrow — **no video, no GPS trace, no raw
telemetry**. It's the analysis the app already wrote, a few tens of
kilobytes of derived numbers:

- lap times, the optimal-splice lap, and per-sector consistency
- the session's pace trend (improving / stable / degrading, seconds per lap)
- every corner's findings aggregated with counts, worst cases, and measured
  seconds lost — for today and for every session on record at that track
- which tyre-and-surface bucket the numbers came from, so the model can't
  compare a wet lap to a dry one
- the circuit's kerb map, so "use more kerb at T6" is only ever said where a
  kerb exists

The provider is one text file on the card — key, URL, model. The request
speaks the standard OpenAI-compatible chat format, so OpenRouter, Nous,
Together, a local llama.cpp on the bench, or Anthropic's API (its shape is
auto-detected from the URL) all work by editing one line. Mine currently
points at DeepSeek. The model is capped at a few hundred tokens of reply:
three findings and their evidence, not an essay.

---

The ghost car rides along to the next event in the passenger seat of the
recording pipeline that hasn't changed all summer: same glovebox, same SD
card in the SIM tray, same Telegram remote running the session from my
pocket. The difference is that when I glance at the replay now, the corner
tells me its story the way a game would — and on Monday, the model tells me
which three corners to spend my next twenty minutes on.
