---
title: "Touge: offline back-road navigation, group tracking over LoRa, and a dashcam that keeps the overlay off the video"
date: 2026-09-16 00:00:00 -0400
last_modified_at: 2026-09-17 15:00:00 -0400
categories: car tech
tags: [touge, android, navigation, offline, maplibre, pmtiles, valhalla, meshtastic, lora, gmrs, tpms, radar, valentine-one, android-auto, dashcam, telemetry, kotlin, compose, openstreetmap, motorcycle, bronco, back-roads]
cover: /assets/images/touge/v2/route-picker.png
lightbox: true
excerpt: "One Android app for the roads worth driving. Offline maps that download themselves, four route choices per leg ranked on measured curvature, turn by turn with a Japanese touge voice, a group card that tracks five cars over cell or LoRa, tyre pressure alarms, a 100 mile radar disc, and a V1 radar detector that quiets itself in town."
article_header:
  type: overlay
  theme: dark
  background_color: "#14181E"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/touge/v2/route-picker.png
---

<!--more-->

Touge is an Android app for a tablet on the dash. It plans rides on back
roads, follows them with voice, keeps a group of cars together with or without
cell signal, and records the drive with the telemetry around the video rather
than on it. One APK, no account, no subscription. The phone can be in airplane
mode for the whole ride.

Every screenshot below is the app running, with the Blue Ridge pack installed.

## Four route choices, ranked on how twisty the road is

![Four routes from Sparta NC to Marion VA, each with time, distance and twist score](/assets/images/touge/v2/route-picker.png)

Sparta, NC to Marion, VA. Four real routes from Valhalla, each drawn on the
map at once, north up, with a bubble carrying time, distance and a TWIST
score.

The score is measured from the returned geometry: heading change per
kilometre, share of distance inside real corners, median corner radius,
junctions per kilometre. No model guesses which road sounds scenic. The number
comes from the line.

Four choices come from asking twice. Valhalla returns only alternates it
considers distinct, so the same pair is asked at four highway tolerances
(`use_highways` 0, 0.35, 0.7, 1.0) and the replies are merged and de-duplicated
by shape. The fastest route is always one of the four.

Each bubble sits where its route is furthest from every other candidate, which
is where they visibly diverge. The detour is shown in plain numbers against
the fastest option.

## A trip is legs, and every leg gets its own four

![Trip screen with two stops, a style per leg, and four timed options for each leg](/assets/images/touge/v2/trip.png)

Rides are slab out, twisty through the middle, quick way home. Each stop
carries a style for the leg that arrives at it: Highway, Mixed, Back roads,
Twistiest. Every leg gets its own four options, spanning how much highway it
uses, and you pick per leg.

Valhalla takes one costing per request, so a trip is one request per leg,
stitched. The start is the car. A setting sends the first leg on the highway
and everything after it the long way.

## Turn by turn, snapped to the road, with a touge voice

![Driving screen with the turn card counting down to a left onto US 21](/assets/images/touge/v2/driving.png)

The turn card counts down the next maneuver, with the one after it below.
Position snaps to the route. The heading tolerance widens with the bend, so
mid hairpin, where the road tangent sits 90 degrees off the car's heading, the
match holds instead of calling you off route. Leaving the route for three
seconds, or pointing the wrong way, triggers one reroute.

Voice has four modes:

- **Silent.**
- **Calm.** Valhalla's own wording.
- **Brief.** Direction and road, nothing else.
- **Touge.** Japanese, shouted, about the corner. `ヘアピン左！落とせ！` for a
  hairpin left. `次、右！行け！` for the next right. It needs a Japanese voice
  installed and says so if there is none.

Calls land about eight seconds out, so they scale with speed. Music ducks
rather than pausing.

![Voice settings with the four modes](/assets/images/touge/v2/settings-voice.png)

## Arriving without a dialog

![Arrival card naming the stop with distance and a single Next stop button](/assets/images/touge/v2/arrival.png)

Within 300 m of a stop the turn card becomes an arrival card: the stop's name,
the distance, one button. Arriving under 5 mph advances the trip on its own.
Nothing is modal and nothing has to be dismissed. The button is for skipping
ahead early.

## The group: five cars, who is where, how old the fix is

![Driving screen with five group cars on the map and the Group card listing gap and age for each](/assets/images/touge/v2/group.png)

Each car in the group is drawn on the map in its own colour with its name. The
Group card sorts the group front to back and gives three columns: name, gap,
age.

Gap is measured **along the route**. On a switchback the car two hairpins back
is 300 m away and four minutes behind, and the card says four minutes. A car
more than 120 m off the shared route falls back to straight line distance and
gets a `~` so a different kind of number looks different.

Age is how old the position is, not how long the packet took. A car that stops
reporting greys out and stays in the table with its last-seen age. A row that
vanished would read as "fine, off screen", which is the one thing it must not
mean. In the screenshot Tom is two minutes stale and still listed.

The worst gap and any silent car are spoken.

### How the positions travel

Two links, both on at once, freshest fix per car wins:

**Cell, to your own server.** `server/convoy.py` runs beside Valhalla on the
same VPS. A dict of positions, no database, no accounts, pruned after thirty
minutes. Eight second interval, about 200 bytes a position. The group key
never leaves the tablet: it signs each body with HMAC-SHA256, the signature
travels, and the group id is a hash of the key. Plain http is refused.

**LoRa, through a Meshtastic radio.** For the parts of the ride with no
signal at all. Touge talks to the radio directly over BLE using the Meshtastic
protobufs, sends the tablet's own fix as the node position, and reads the
others' positions and names off the mesh. The Meshnology N30 used here is a
Heltec V3 with no GPS, so the tablet supplying the fix is the design. Range is
ridge to ridge. The channel runs in the clear.

GMRS position bursts are legal under 47 CFR 95.1787 but every radio that does
them keeps the data inside its own ecosystem, so voice stays on GMRS and data
goes over cell and LoRa. Phone-to-phone Wi-Fi or Bluetooth reaches 50 to 200 m
from a dash, which is a "who is right behind me" link and not a group link.

## Tyre pressure, with a leak alarm

![Tyres screen showing four wheel tiles with psi, temperature, battery and age](/assets/images/touge/v2/tyres.png)

Bluetooth TPMS sensors (Zeepin/TPMSII, DJTPMS, Tesla) bind to wheel positions
by tapping a wheel then a sensor. The screen shows psi, temperature, battery,
the leak rate, and how old the reading is.

Alarms, in order of severity:

- **Under 20 psi.**
- **Losing 2 psi a minute or more.** A least-squares slope over two minutes,
  so quantisation noise does not trip it and a puncture does, minutes before
  the total drop would.
- **10 psi below the tyre's peak.** The peak resets after four hours of
  silence, so a cold morning is not read as a leak.
- **Over 158°F.** A dragging brake or a bearing.

A red strip appears on the driving screen while any wheel is alarming and the
worst one is spoken once, naming the corner.

## Weather radar, 100 miles around the car

![Radar disc on the driving screen, car centred, range rings at 25, 50 and 75 miles](/assets/images/touge/v2/radar.png)

A NEXRAD composite from the IEM tile cache, the same source RyanWeather uses,
clipped to a 100 mile disc with the car at the centre and north up. Range
rings at 25, 50 and 75 miles. It refreshes every five minutes or after 8 km of
travel. Off by default; it needs a connection.

## Radar detector: Valentine One, quiet in town

![V1 alert card with direction ring, band, frequency and strength bars](/assets/images/touge/v2/v1.png)

Touge connects to a Valentine One Gen 2 over Bluetooth on the same protocol
JBV1 uses. Alerts show on screen with a ring around the car and an arrow on the
side the signal comes from (front, side, rear), the band, the frequency, and
eight strength bars. Ka and laser are red; K and X are orange.

The mode is automatic and shown on the card:

- **City.** X and K are muted. The display stays on.
- **Highway** and **Back road.** Everything is loud.
- **Under 10 over the posted limit.** Muted regardless.

The mute command is written to the V1 only when the decision changes. A Gen 2
pairs with one app at a time, so JBV1 is closed while Touge owns the radio.

## Off-road mode for the Bronco

![Off-road mode with forest tracks drawn bold orange](/assets/images/touge/v2/offroad.png)

A fourth vehicle mode. Tracks and forest roads are routed on, drawn bold
orange on the map, and leaving the planned line does not trigger a reroute.
The breadcrumb trail records where the wheels actually went.

## Android Auto

The head unit shows the next turn, its distance, and the ETA from the same
guidance the tablet runs, through the Android Auto navigation template.

## The overlay recorder

![The recorded frame: camera picture untouched, telemetry cards in the margin](/assets/images/touge/frame-composite.png)

The output canvas is bigger than the camera picture and the cards live in the
margin. A 1920x1080 camera plus a third again of margin lands on 2560x1440 and
the video is untouched, pixel for pixel. The margin is static, so it encodes
to almost nothing; the cost is encoder throughput, and 2560x1440 at 29 fps
sustained is a measured number on this hardware.

Every card has a switch. Cards are laid out by weight, so switching one off
gives its height to the rest. Cards that need a surveyed circuit (apex
verdicts, braking zones, lap delta) are off and stay off on a public road.

The recorder arms on the road ahead, not on the corner you are already in.
The map pack carries the geometry under the car, so turning onto something
good is knowable at the junction, and ten seconds of pre-roll puts the turn
itself in the clip. Corner radius comes from `v² / a_lat`: 70 mph at 0.35 g is
a 285 m interstate sweeper, 35 mph at 0.35 g is a 71 m corner worth filming.
Elevation swing off SRTM tiles arms it on a fast ridge road that is not
twisty. Freeways and divided four-lanes never arm it.

## The driving screen

![Driving screen at night, map left, media pane right](/assets/images/touge/real-night-map.png)

Map left, media pane right, 260 dp, derived from three 64 dp controls and
their spacing. The pane reads the Android media session, so Pandora, Spotify
and a podcast app all work with play, skip, and thumbs up or down.

Your own car is a top-down Mini in the vehicle's accent colour. The zoom
setting is a distance of road ahead, not a zoom level, corrected for the car
sitting 72% down the frame and for the tilt. The theme follows the sun at the
car's own position.

## Maps that download themselves

![Map pack downloader listing six regions with measured sizes](/assets/images/touge/v2/packs.png)

Six regions with measured sizes. PMTiles is read in place, so the app walks
the planet file's directory over HTTP range requests and adds up exactly the
bytes it needs before fetching any. Blue Ridge is 120 MB in 273 range
requests. Four states are 1.03 GB. The search index is built on the tablet
from the same tiles: 207,879 places, towns and roads for Blue Ridge, in about
forty seconds.

## Search that finds towns

![Search results for Sparta with the town ranked first](/assets/images/touge/v2/search.png)

Search runs over the on-device index, not the tiles on screen. Results rank by
name match, then by kind (a town beats a road of the same name, a road beats a
shop named after it), then by distance. Fuel, bathroom and food are one tap
each, ordered by distance ahead along the road. Food hides chains by their
brand tag and checks opening hours, overnight spans included. Fuel ranks the
brands first.

## Breadcrumbs

Every ride is recorded as GPX with no button. Recording starts on the first
fix above 5 mph. Points closer than 15 m are dropped unless the heading moved
more than 12 degrees, so the corners are kept and the straights are thinned.
A recorded ride can be the basis of a new route.

## Police reports

Alerts arrive over [SABRE](https://sabre.app/tech), an open Android protocol
where a proxy app owns the network side and hands alerts to a host over
broadcast intents. Every alert carries its age and fades as it gets older;
police expire at 25 minutes, a closed road at six hours. The map works with
the feed returning nothing.

## Your own routing server

![Routing server setting with the Valhalla URL field](/assets/images/touge/v2/settings-group.png)

Routing defaults to the public FOSSGIS Valhalla, which is free, shared and
rate limited. Settings take the URL of your own instance. A small VPS running
the Appalachian extract answers in well under a second, which is the
difference between a reroute that lands before the turn and one that lands
after it. Hosting on mine is about $3 a month per user.

## Numbers

288 unit tests, all green, covering the PMTiles reader, the MVT parser, the
curvature and relief scoring, the record trigger, the search index, GPX
import, the opening-hours parser, guidance, voice phrasing, the convoy
ranking, the Meshtastic protobufs, the TPMS rules and the V1 packet format.
