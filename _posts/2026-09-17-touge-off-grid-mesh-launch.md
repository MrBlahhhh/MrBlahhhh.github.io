---
title: "Touge: Own The Mountain. Off The Grid."
date: 2026-09-17 09:00:00 -0400
categories: car tech
tags: [touge, lora, meshtastic, android, navigation, offline, mesh, convoy, mountain-driving, back-roads, gps, kotlin]
cover: /assets/images/touge/v2/group.png
lightbox: true
excerpt: "Navigation, crew tracking and hazard calls that run on a LoRa mesh instead of a cell tower. No bars. No servers. No trail."
article_header:
  type: overlay
  theme: dark
  background_color: "#0B0D10"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .35), rgba(0, 0, 0, .75))"
    src: /assets/images/touge/v2/group.png
---

# Own The Mountain. Off The Grid.

**Your crew's exact position, interval and hazard calls travel car to car on a LoRa mesh, so the run keeps working three valleys deep where every other nav app has already gone dark.**

<!--more-->

Midnight. The last bar of signal died at the bottom of the grade and nobody noticed, because nothing on the dash changed. The map still moved. The car two switchbacks back still showed 0.4 back, aged four seconds. The blind left at the quarry still called itself out before you could see it.

That is the whole pitch. Everything below is how.

![Five cars on the route, the group table, a tyre warning and the V1 card](/assets/images/touge/v2/group.png)

## No Tower. No Problem.

Cell coverage on a good road is a joke. The better the road, the worse the map. Touge stops pretending otherwise: the vector basemap, the routing graph and the search index all live on the tablet, and the crew link runs on 915 MHz LoRa between the cars.

- **Coverage:** Zero bars is a supported configuration.
- **Basemap:** Full vector tiles, on device, airplane mode.
- **Link:** Car to car radio, no carrier in the path.
- **Range:** Miles of ridge line, not metres of Bluetooth.

## Your Crew, In Real Time

Pins are not enough when the group has spread over eight miles. Touge gives you the table you actually read at speed: who, how far, how fresh.

- **Order:** Sorted front to back, the way you are driving.
- **Gap:** Measured along the road, not across the valley.
- **Age:** Every row carries how old its fix is.
- **Dropped:** A stale row greys out and stays put.

![The group card: gap along the road and the age of every fix, Tom two minutes stale](/assets/images/touge/v2/arrival.png)

## Lines, Not Directions

Route choice built for a pass, not a commute. Curvature is measured off the geometry, so a road either is twisty or it is not.

- **Ranking:** Sorted on measured corner density.
- **Legs:** Slab out, back roads home, per leg.
- **Choices:** Four real alternates, not three reskins.
- **Control:** Pick the highways you will and will not take.

![Four routes, Sparta NC to Marion VA, scored on measured curvature](/assets/images/touge/v2/route-picker.png)

## Turn By Turn That Keeps Up

Snapping tuned for switchbacks, where the road tangent legitimately swings past ninety degrees and every other engine calls you off route.

- **Snap:** Tolerance widens with the corner.
- **Calls:** Timed to speed, roughly eight seconds out.
- **Reroute:** One per wrong turn, not one per fix.
- **Voice:** Calm, brief, or touge mode in Japanese.

## Radar. Tyres. Weather.

The rest of the dash is the rest of the car.

- **Radar detector:** V1 Gen 2 over Bluetooth, direction on a ring, band and bars.
- **City mode:** X and K muted in town. Loud on the highway.
- **Tyres:** Under 20 psi, a fast leak, a hot wheel. Spoken, and on screen.
- **Weather:** 100 mile NEXRAD disc, car centred, north up.

![Tyre pressures with a leak on the rear left](/assets/images/touge/v2/tyres.png)

## No Servers, No Trail

The default configuration has no account, no upload and no third party. Run the mesh alone and nothing about the drive touches a server.

- **Storage:** Routes and breadcrumbs stay on device.
- **Mesh:** Peer to peer, radio to radio, no relay.
- **Optional:** A private server you own, signed requests only.
- **Retention:** Nothing to retain.

## Record Everything, Cover Nothing

The camera starts when a good road is coming, and the telemetry sits in a margin around the video instead of on top of the only frame you wanted.

- **Trigger:** Starts on the road ahead, not after.
- **Overlay:** Framed in a gutter, never across.
- **Data:** Speed, lean, g, line, elevation.
- **Export:** Straight to file, no cloud step.

![The recorded frame: video untouched, telemetry in the margin](/assets/images/touge/frame-composite.png)

## Built For The Pass

- **Vehicle:** Car, Bronco and motorcycle profiles.
- **Screen:** Dark, high contrast, glove sized targets.
- **Hardware:** Any Android tablet, any Meshtastic radio.
- **Cost:** One app, no subscription.

![Trip screen: a style and four options for every leg](/assets/images/touge/v2/trip.png)

## Get It

**[How every feature works, with screenshots](/car/tech/2026/09/16/touge-offline-routing-and-recording.html)**

Map packs for the Blue Ridge, Southern Appalachia, and NC, VA, TN and WV download inside the app.

The road does not care whether you have signal. Neither does this.
