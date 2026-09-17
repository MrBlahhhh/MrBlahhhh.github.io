---
title: "Touge, running: real routes, offline maps that download themselves, and four ways to ride every leg"
date: 2026-09-17 00:00:00 -0400
categories: car tech
tags: [touge, android, navigation, offline, maplibre, pmtiles, valhalla, gpx, dashcam, telemetry, kotlin, compose, openstreetmap, motorcycle, back-roads]
cover: /assets/images/touge/real-driving-route.png
lightbox: true
excerpt: "Yesterday's post was mockups. This one is screenshots off the tablet: 120 MB of map downloaded by the app itself, 207,879 searchable places built on the device, and real routes that refuse the interstate and tell you what refusing it costs."
article_header:
  type: overlay
  theme: dark
  background_color: "#14181E"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/touge/real-driving-route.png
---

<!--more-->

The [previous post](/car/tech/2026/09/16/touge-offline-routing-and-recording.html)
was renders. Every picture below is a screenshot off the tablet on the bench.

## The map downloads itself

The old plan had me copying a `.pmtiles` file into `Android/data` over USB.
That is not an instruction, it is a description of a dead end, and it was the
first thing anyone saw on opening the app.

![Map pack downloader listing six regions with measured sizes](/assets/images/touge/real-map-packs.png)

Those sizes are counts, not estimates. PMTiles is designed to be read in place,
so the app walks the planet file's directories over HTTP range requests and
adds up exactly the bytes it will need before downloading any of them. Working
out the figure for all six regions costs about 2 MB.

| Region | Tiles | Size |
|---|---|---|
| Blue Ridge | 21,521 | 120 MB |
| Southern Appalachia | 108,094 | 502 MB |
| North Carolina | 88,675 | 392 MB |
| NC + TN + VA + WV | 217,148 | 1.03 GB |

Blue Ridge came down in **273 range requests** against a 137 GB planet file.
The four states as one big box would be 1.52 GB — half again as much, for the
Atlantic and slices of five states nobody asked for — so they stay separate and
get unioned.

Four things went wrong getting there, every one of them producing an archive
that opened without complaint and drew nothing:

- Gathering tile data in memory died trying to allocate 143 MB with 58 MB free.
  It streams to disk now, and no request holds more than 8 MB.
- The metadata was written as plain JSON under a header declaring gzip.
- The metadata also has to be clamped to the zooms actually copied, or the
  layers still claim to reach the planet's z15 and the renderer asks for tiles
  that were never downloaded.
- The root directory has a 16 KB ceiling; past it you need leaf directories.

## Search that finds towns

Searching for Sparta returned three churches and a housing estate, and not the
town. The index only read the `pois` layer — shops and amenities — so the two
most common things anyone types into a nav app, a town and a street, were the
two it could not find. That is the exact complaint I have about DMD2.

![Search results for Sparta, the town ranked first at 122 miles](/assets/images/touge/real-search-sparta.png)

It now indexes places and roads as well: **207,879 entries** for the Blue Ridge
pack, up from 26,115, built on the tablet in about forty seconds. Ranking was
distance alone, which is the wrong first question — "Sparta Heights" won
because it happened to be nearer than the town hall. Results now sort by how
well the name matches, then by what kind of thing it is (a town beats a road of
the same name, a road beats a shop named after it), then by distance.

## Real routes, and what the twisty one costs

![Four route choices to Sparta with times and twist scores](/assets/images/touge/real-four-choices.png)

Four choices, and the one that matters is the spread: **2 h 49 at TWIST 5**
against **4 h 34 at TWIST 8**. An hour and three quarters for three points of
twist, laid out so it is my call rather than the engine's.

Getting four took two requests rather than a bigger number. Valhalla only
returns alternates it considers genuinely distinct, and for that pair it
returns three whether you ask for three, five or eight — I measured all three.
The lever is the question, not the count, so the same pair is asked at two
highway tolerances and the results merged and de-duplicated by shape.

`use_highways = 0` is the whole argument of the app. Ask for a normal route and
re-sort the replies by curviness and you get a worse set to sort: the engine
has already decided the interstate is the answer and offered three variations
on it. Sparta comes back 170 miles on back roads rather than 120 on the
highway.

## A trip is legs, not a destination

These rides are never one destination. They are: slab out to where the good
roads start, twisty through the middle, quick way home. Each of those is a
decision about one leg.

![Trip screen showing leg styles and four timed options for the leg](/assets/images/touge/real-trip-legs.png)

Every leg carries its own style — Highway, Mixed, Back roads, Twistiest — and
gets four options of its own, spanning how much highway they use. Valhalla
takes one costing per request, so a trip whose legs differ is one request per
leg, stitched. That is the only way to say "fast to the mountains, then the
long way round everything after" and have the engine do it rather than average
the two into a compromise neither half wanted.

The style belongs to the stop it arrives at, so dragging a stop up the list
takes its leg style with it. The start is always the car: a nav app that asks
you to set an origin is asking a question it can already answer.

## On the road

![Driving screen with the route drawn, chevron marker, trip strip showing 4 h 34 and 170 miles](/assets/images/touge/real-driving-route.png)

Trip strip is the real route now — arrival, time left, 170 miles, TWIST 8.

Three bugs had to die for that screen to be honest. The turn card was showing
the demo turn regardless of the actual route, which is the same lie the route
screen used to tell and worse, because this is the screen you read at speed.
Pressing Start drew nothing. And when the line finally drew, it vanished:
loading a style discards every layer in it, and my guard was on the route id
rather than on the style.

The subtlest one: the route reaching the draw call was always null.
`activeRoute` was a value computed during composition, and the telemetry loop
is a `LaunchedEffect(Unit)` — created once, never restarted — so it closed over
the value from the first composition, when no route existed, and would have
held that null for the life of the app. The trip strip showed real numbers the
whole time, which made it look like a drawing problem.

## The stutter was the dot

Dragging the map felt glitchy. I assumed the map was slow and spent a while
throttling work — which made it worse.

`dumpsys gfxinfo` reported 48% janky frames, and I nearly reported that as the
answer. It measures the Compose overlay window; MapLibre draws into its own
SurfaceView. The giveaway was an experiment with the telemetry loop turned down
to 0.5 Hz: it rendered **8 frames** across eight full-screen drags, because the
overlay had simply stopped changing while the map underneath carried on.

`MapLibreMap.setOnFpsChangedListener` is the number that describes the thing
under your finger, and it said **60 fps mean** through continuous panning. The
map was never slow. The orange dot was a Compose marker positioned in screen
pixels, so it only moved when something recomposed — and I had just throttled
recomposition to 4 Hz. The map slid at sixty frames a second under a marker
hopping four times a second.

It is a symbol layer in the map's own style now, so it moves with the tiles.
Same ten drags, before and after:

| | before | after |
|---|---|---|
| overlay frames drawn | 80 | 2 |
| slow UI-thread frames | 35 | 1 |
| high input latency | 46 | 1 |
| map surface fps | 60 | 60–64 |

![Night map showing labelled back roads around Chatham Church Road](/assets/images/touge/real-night-map.png)

The night palette also had minor roads four shades off the background, which on
a rural screen reads as an empty map. A back road at night is the thing this
app exists to show.

## Breadcrumbs

Every ride is now recorded as GPX, because the planned route is what an engine
thought was a good idea and the track is where the wheels actually went — the
wrong turn that turned out better, the detour round the closed bridge, the bit
nobody would have routed. That is the half worth keeping and the half every nav
app throws away.

Points closer than 15 m are dropped, but any point where the heading moved more
than 12 degrees is kept however short the move: on these roads the corner *is*
the information, and thinning by distance alone keeps the straights and loses
the bends. There is no record button — it starts on the first fix above 5 mph,
because a ride you forgot to record is the one you wanted.

## Police alerts

Waze's live-map endpoint now answers 403 to anything that is not a real
browser: reCAPTCHA cookies generated in JavaScript and bound to the session
that made them. I verified it rather than assuming — full browser headers, a
Referer, a prior page load for cookies, all refused — and the older
`rtserver` paths are gone.

So the scrape is deleted. Alerts come over [SABRE](https://sabre.app/tech)
instead, an open Android protocol where a proxy app owns the network side and
hands alerts to a host over broadcast intents. Same reports JBV1 shows, from
the same proxy, by a supported route.

## What it still cannot do

It cannot tell you to turn. There is no snapping, no maneuver tracking, no
rerouting and no voice — so it plans a ride and draws it and then goes quiet.
The maneuvers are already in every routing reply and currently thrown away
after their street names are taken, so that parse is the next thing.

There is also no camera yet. The auto-record trigger — corner radius from
`v²/a_lat`, road class, elevation change — is the most finished code in the
repo and has nothing to record with.

Routing currently runs through FOSSGIS's public Valhalla, which is free, rate
limited and somebody else's machine. My own instance is building on a VPS as I
write this: four states, bound to localhost, behind its own vhost with a bearer
token and a memory cap so a tile build cannot starve the shop that shares the
box.
