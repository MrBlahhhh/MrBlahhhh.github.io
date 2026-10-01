---
title: "No 7/8 switch on your K+DCAN cable? One wire in the R53's OBD socket fixes it"
date: 2026-10-01 08:00:00 -0400
categories: car tech
tags: [mini, r53, obd2, k-line, k-dcan, inpa, ncs, coding, wiring, how-to]
cover: /assets/images/r53-obd-jumper/jumper-fitted.jpg
lightbox: true
excerpt: "The R53 brings the engine out on OBD pin 7 and every other module on pin 8. A K+DCAN cable with the 7/8 switch, or with the two pins linked inside its plug, reaches both. One without it reaches the engine and nothing else. A short piece of wire across the back of the car's socket fixes it for every cable."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/r53-obd-jumper/jumper-fitted.jpg
---

<!--more-->

The R53 puts the engine on one K-line and the body and chassis modules on a second, and they come out of the OBD socket as two separate pins: 7 for the engine, 8 for everything else. A K+DCAN cable reaches both by linking 7 and 8, either inside its plug or with a small switch on the side.

Get a cable without that link, or with the switch off, and only the engine answers. The cluster, body computer, DSC and airbag all look dead. In [R53 Coding](/car/tech/2026/07/26/r53-coding.html) every module but the engine fails to read, and INPA and NCS Expert do the same.

You can buy a cable with the switch. Or link the two pins at the car once, and every cable works from then on.

## The fix

**1. Get to the back of the socket.** Unclip the OBD socket from its mount under the dash and pull it out far enough to see where the wires go in.

![The OBD socket's mount under the dash with the socket unclipped](/assets/images/r53-obd-jumper/socket-mount.jpg){: style="width: 100%; max-width: 420px; display: block; margin: 1.25rem auto;"}

**2. Make the jumper.** An inch or so of thin wire, both ends stripped and the bare copper bent over into a small hook so it's thick enough to stay put.

![The jumper: a short blue wire with both stripped ends bent into hooks](/assets/images/r53-obd-jumper/jumper.jpg){: style="width: 100%; max-width: 420px; display: block; margin: 1.25rem auto;"}

**3. Fit it across pins 7 and 8.** Push one end into the back of pin 7 and the other into pin 8, alongside the wires already there. Pin 16 is the red wire, permanent battery. Pin 8 is directly across from it in the other row, and pin 7 is next to pin 8. Keep bare wire well away from pin 16: it's live all the time and not fused.

![The back of the OBD socket with the blue jumper looped across pins 7 and 8](/assets/images/r53-obd-jumper/jumper-fitted.jpg){: style="width: 100%; max-width: 420px; display: block; margin: 1.25rem auto;"}

**4. Check it.** Clip the socket back in, plug in the cable, key on, and read something that isn't the engine. The instrument cluster is a quick one.

## Leaving it in

Linking the two lines is exactly what the switch on a K+DCAN cable does, and what the [K-line bridge](/car/tech/2026/08/27/r53-kline-canbus-bridge-hardware.html) does at its input terminal, so the car sees nothing it hasn't seen before. To undo it, pull the wire back out.
