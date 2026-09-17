---
title: "Touge"
date: 2026-09-16 00:00:00 -0400
last_modified_at: 2026-09-17 20:00:00 -0400
categories: car tech
tags: [touge, android, navigation, offline, maplibre, pmtiles, valhalla, meshtastic, lora, gmrs, tpms, radar, valentine-one, android-auto, dashcam, telemetry, kotlin, compose, openstreetmap, motorcycle, bronco, back-roads]
cover: /assets/images/touge/v2/group.png
lightbox: true
excerpt: "An Android app for the roads worth driving. Four routes per leg ranked on measured curvature, turn by turn with a Japanese touge voice, five cars kept together over cell or LoRa with no signal at all, tyre alarms, a 100 mile radar disc, a V1 that quiets itself in town, and a dashcam that keeps the telemetry off the video."
article_header:
  type: overlay
  theme: dark
  background_color: "#0B0D10"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .25), rgba(0, 0, 0, .75))"
    src: /assets/images/touge/v2/group.png
---

<style>
.tg{--ink:#e9edf2;--mute:#9aa3ad;--acc:#ff3366;--acc2:#ff8a00;--bg:#0b0d10;--card:#14181e;--line:#232a33}
.tg *{box-sizing:border-box}
.tg .hero{margin:0 0 28px;padding:28px 0 8px;border-bottom:1px solid var(--line)}
.tg .hero h1{font-size:clamp(34px,6vw,64px);line-height:1;margin:0 0 10px;letter-spacing:-.02em}
.tg .hero p{font-size:clamp(17px,2.2vw,21px);color:var(--mute);max-width:52ch;margin:0}
.tg .stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:12px;margin:22px 0 0}
.tg .stat{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:14px 16px}
.tg .stat b{display:block;font-size:28px;line-height:1;font-variant-numeric:tabular-nums}
.tg .stat span{color:var(--mute);font-size:13px;letter-spacing:.04em;text-transform:uppercase}
.tg .k{display:inline-block;font-size:12px;letter-spacing:.12em;text-transform:uppercase;color:var(--acc);margin:0 0 6px}
.tg h2{font-size:clamp(26px,3.6vw,40px);letter-spacing:-.01em;margin:44px 0 6px}
.tg .lead{font-size:18px;color:var(--mute);margin:0 0 18px;max-width:64ch}
.tg .row{display:grid;grid-template-columns:1.25fr 1fr;gap:22px;align-items:center;margin:18px 0 26px}
.tg .row.flip{grid-template-columns:1fr 1.25fr}
.tg .row.flip figure{order:2}
.tg figure{margin:0;background:var(--card);border:1px solid var(--line);border-radius:16px;overflow:hidden}
.tg figure img{display:block;width:100%;height:auto}
.tg figcaption{padding:10px 14px;font-size:13px;color:var(--mute);border-top:1px solid var(--line)}
.tg ul.tight{margin:0;padding-left:18px}
.tg ul.tight li{margin:4px 0}
.tg .grid3{display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:14px;margin:14px 0 22px}
.tg .tile{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:16px}
.tg .tile b{display:block;font-size:17px;margin-bottom:4px}
.tg .tile p{margin:0;color:var(--mute);font-size:15px}
.tg .quote{border-left:4px solid var(--acc);padding:6px 16px;margin:18px 0;font-size:19px;color:var(--ink)}
.tg .wire{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:16px;margin:14px 0 22px}
.tg .wire svg{width:100%;height:auto;display:block}
.tg .last{margin-top:56px;padding-top:24px;border-top:1px solid var(--line)}
@media (max-width:820px){.tg .row,.tg .row.flip{grid-template-columns:1fr}.tg .row.flip figure{order:0}}
</style>

<div class="tg" markdown="1">

<div class="hero">
<span class="k">Android · offline · for the pass</span>
<h1>Touge</h1>
<p>Plans the twisty way, calls the corners, keeps five cars on one map with no signal, and records the drive with the numbers around the video instead of on it.</p>
<div class="stats">
<div class="stat"><b>4</b><span>routes per leg</span></div>
<div class="stat"><b>0</b><span>accounts, subscriptions</span></div>
<div class="stat"><b>30 s</b><span>group pings, mesh or cell</span></div>
<div class="stat"><b>100 mi</b><span>radar disc</span></div>
<div class="stat"><b>60</b><span>car icons</span></div>
<div class="stat"><b>317</b><span>unit tests</span></div>
</div>
</div>

Everything below is the app running with the Blue Ridge map pack installed. Nothing is a mockup.

## Plan the road, not the arrival time

<p class="lead">Every other nav app offers alternatives as minutes saved. Touge offers them as corners.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/route-picker.png" alt="Four routes from Sparta NC to Marion VA, each in its own colour with time, distance and twist score"><figcaption>Sparta, NC to Marion, VA. Four routes, four colours, one card per route in the same colour.</figcaption></figure>
<div>
<span class="k">Route picker</span>
<ul class="tight">
<li>The TWIST score is measured off the geometry: heading change per mile, share of distance in real corners, median corner radius, junctions per mile. No model guesses which road sounds scenic.</li>
<li>Four routes come from asking Valhalla four times at different highway tolerances and merging by shape. The fastest is always one of them.</li>
<li>Each card sits where its route is furthest from the others and wears the route's colour. The selected one is outlined.</li>
<li>The detour is a number against the fastest option, not a feeling.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/trip.png" alt="Trip screen with two stops, a style per leg, and four timed options for each leg"><figcaption>Every leg gets its own style and its own four options.</figcaption></figure>
<div>
<span class="k">Trips</span>
<ul class="tight">
<li>Rides are slab out, twisty through the middle, quick way home. Each stop carries the style of the leg that arrives at it: Highway, Mixed, Back roads, Twistiest.</li>
<li>One request per leg, stitched, so the engine does what you asked instead of averaging two wishes into a compromise.</li>
<li>The start is always the car.</li>
</ul>
</div>
</div>

## Drive it

<div class="row">
<figure><img src="/assets/images/touge/v2/driving.png" alt="Driving screen with the turn card, the Mini icon, the group card and the V1 alert"><figcaption>The turn card: maneuver, distance, seconds, the road in title size, lanes when the map has them.</figcaption></figure>
<div>
<span class="k">Turn by turn</span>
<ul class="tight">
<li>Position snaps to the route. The heading tolerance widens with the bend, so a hairpin, where the road sits 90 degrees off the car, still matches instead of calling you off route.</li>
<li>Off the route for three seconds, or pointing the wrong way, gets one reroute.</li>
<li>Lane guidance draws the lanes to be in when the map data carries them.</li>
<li>Calls land about eight seconds out, so they scale with speed. Music ducks, never pauses.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/stop-ahead.png" alt="Stop ahead card 1.3 miles out with Skip, Later and Go on"><figcaption>Two miles before a stop: Skip, Later, or Go on. Nothing to dismiss.</figcaption></figure>
<div>
<span class="k">Stops</span>
<div class="quote">The choice comes before the route drags you into a town centre you were only passing through.</div>
<ul class="tight">
<li><b>Skip</b> drops the stop. <b>Later</b> moves it to the end of the trip. <b>Go on</b> stops as planned.</li>
<li>Arriving under 5 mph advances the trip on its own. The card is never modal.</li>
</ul>
</div>
</div>

<div class="grid3">
<div class="tile"><b>Silent</b><p>The turn card still counts down.</p></div>
<div class="tile"><b>Calm</b><p>Valhalla's own wording.</p></div>
<div class="tile"><b>Brief</b><p>Direction and road, nothing else.</p></div>
<div class="tile"><b>Touge</b><p>Japanese, shouted, about the corner. <code>ヘアピン左！落とせ！</code> for a hairpin left. Needs a Japanese voice installed and says so if there is none.</p></div>
</div>

## Ride together

<p class="lead">A group spread over eight miles of ridge road is not a question of pins on a map. It is who, how far, and how old that fix is.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/group.png" alt="Five group cars drawn on the route in their own colours and icons, with the group card listing gap and age"><figcaption>Five cars, each its own model and colour. The group card sorts front to back.</figcaption></figure>
<div>
<span class="k">Group card</span>
<ul class="tight">
<li><b>Gap is measured along the road.</b> On a switchback the car two hairpins back is 300 m away and four minutes behind, and the card says four minutes.</li>
<li><b>Age is the age of the fix</b>, not of the packet. A car that stops reporting greys out and stays in the table with its last-seen time. Tom, two minutes stale, is still there.</li>
<li>More than 120 m off the shared route falls back to straight line and gets a <code>~</code>.</li>
<li>The worst gap and any silent car are spoken.</li>
</ul>
</div>
</div>

<span class="k">How positions travel</span>
<div class="wire">
<svg viewBox="0 0 900 250" role="img" aria-label="Mesh and server paths for group positions">
<defs><marker id="a" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#ff8a00"/></marker></defs>
<rect x="0" y="0" width="900" height="250" fill="#14181e"/>
<g font-family="system-ui,Segoe UI,Roboto,sans-serif" font-size="14" fill="#e9edf2">
<rect x="30" y="40" width="150" height="60" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="105" y="66" text-anchor="middle">Tablet</text><text x="105" y="86" text-anchor="middle" fill="#9aa3ad">GPS fix, 30 s</text>
<rect x="240" y="40" width="150" height="60" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="315" y="66" text-anchor="middle">Meshtastic N30</text><text x="315" y="86" text-anchor="middle" fill="#9aa3ad">BLE in, LoRa out</text>
<rect x="510" y="40" width="150" height="60" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="585" y="66" text-anchor="middle">Their radio</text><text x="585" y="86" text-anchor="middle" fill="#9aa3ad">915 MHz, miles</text>
<rect x="720" y="40" width="150" height="60" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="795" y="66" text-anchor="middle">Their tablet</text><text x="795" y="86" text-anchor="middle" fill="#9aa3ad">group card</text>
<line x1="180" y1="70" x2="238" y2="70" stroke="#ff8a00" stroke-width="3" marker-end="url(#a)"/>
<line x1="390" y1="70" x2="508" y2="70" stroke="#ff8a00" stroke-width="3" stroke-dasharray="8 6" marker-end="url(#a)"/>
<line x1="660" y1="70" x2="718" y2="70" stroke="#ff8a00" stroke-width="3" marker-end="url(#a)"/>
<text x="449" y="55" text-anchor="middle" fill="#ff8a00" font-size="12">no cell needed</text>
<rect x="360" y="160" width="180" height="60" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="450" y="186" text-anchor="middle">Your server</text><text x="450" y="206" text-anchor="middle" fill="#9aa3ad">signed, no accounts</text>
<path d="M105,100 C105,190 200,190 358,190" fill="none" stroke="#3da5ff" stroke-width="3" marker-end="url(#a)"/>
<path d="M542,190 C700,190 795,190 795,100" fill="none" stroke="#3da5ff" stroke-width="3" marker-end="url(#a)"/>
<text x="230" y="235" fill="#3da5ff" font-size="12">cell, when the radio is not connected</text>
<text x="620" y="235" fill="#9aa3ad" font-size="12">newest fix per rider wins, by timestamp</text>
</g>
</svg>
</div>

<ul class="tight">
<li><b>Mesh first.</b> With a radio connected, positions go out over LoRa every 30 seconds. Touge talks to the radio directly over BLE using the Meshtastic protobufs; the N30 has no GPS, so the tablet supplies the fix. The channel runs in the clear.</li>
<li><b>Cell when there is no radio.</b> The same fix, with its timestamp, signed with HMAC-SHA256 under the ride key, to <code>server/convoy.py</code> beside Valhalla. No database, no accounts, positions pruned after thirty minutes. Plain http is refused.</li>
<li><b>Timestamps decide.</b> Every fix carries the time it was measured. A late cell packet never overwrites a newer mesh one.</li>
<li><b>Name, colour and car icon travel with the fix</b> on both links, so what you set is what the others see.</li>
</ul>

<div class="row">
<figure><img src="/assets/images/touge/v2/invite.png" alt="Ride screen with a QR code and touge://join link"><figcaption>One key per ride. QR code, link, or a broadcast over the mesh.</figcaption></figure>
<div>
<span class="k">Start or join a ride</span>
<ul class="tight">
<li>Start a ride and the app makes a key. Share it as a QR code for the phone in the next car, as a <code>touge://join</code> link, or over the mesh, which pops a join prompt on every Touge radio in range.</li>
<li>Join by scanning, by opening the link, or by answering the prompt.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/profile.png" alt="Profile screen: name, colour swatches, and a searchable grid of car icons"><figcaption>You: a name, a colour, and your car from a catalogue of sixty.</figcaption></figure>
<div>
<span class="k">Your car</span>
<ul class="tight">
<li>Sixty plan-view icons drawn from a few parameters each: R53 and R56 Mini, E82, GT86 and GR86, Bronco, Miata, S2000, Type R, RX-7, 911, Cayman, Elise, Mustang, WRX and STI, Evo, GTI, Wrangler, 4Runner, Tacoma, Raptor, two bikes, and more.</li>
<li>Pick a swatch and every icon repaints in your colour. The preview is what the group sees.</li>
</ul>
</div>
</div>

<div class="row">
<figure><img src="/assets/images/touge/v2/pair.png" alt="Meshtastic radio screen listing paired and nearby radios"><figcaption>Pick the radio this tablet talks through. Bonded first, then nearby.</figcaption></figure>
<div>
<span class="k">Pairing a radio</span>
<ul class="tight">
<li>Bonded Meshtastic radios are listed; a scan finds ones not yet paired and a tap starts the bond.</li>
<li>One radio per car. The chosen address is the one the app connects to, so two radios in range are not a coin toss.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/phone.png" alt="Phone layout: map, next turn, group card and three buttons in portrait"><figcaption>Phone layout: the basics, in portrait.</figcaption></figure>
<div>
<span class="k">Riding along on a phone</span>
<ul class="tight">
<li>Under 600 dp of width the app switches to one column: map, next turn, the group, three buttons.</li>
<li><b>Background presence</b> keeps sending your position every 30 seconds with the app closed, under one notification, for the rider who only wants to be on the map.</li>
</ul>
</div>
</div>

## The car

<div class="row">
<figure><img src="/assets/images/touge/v2/tyres.png" alt="Tyres screen with four wheel tiles; rear left is leaking"><figcaption>Four tiles, one leaking. The rate is a fitted slope, not two samples.</figcaption></figure>
<div>
<span class="k">Tyre pressure</span>
<ul class="tight">
<li>Bluetooth TPMS sensors (Zeepin/TPMSII, DJTPMS, Tesla) bind to a wheel by tapping the wheel, then the sensor.</li>
<li>Alarms in order: under 20 psi; losing 2 psi a minute (a least-squares slope over two minutes, so a puncture trips it and quantisation does not); 10 psi below the tyre's peak; over 158°F.</li>
<li>A red strip on the driving screen and one spoken line naming the corner.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/v1.png" alt="V1 alert card: Ka ahead, 34.7 GHz, six bars, Highway mode"><figcaption>Ka, 34.7, ahead, six bars. Highway, loud.</figcaption></figure>
<div>
<span class="k">Valentine One</span>
<ul class="tight">
<li>A V1 Gen 2 over Bluetooth on the same protocol JBV1 uses. A ring around the car points where the signal comes from; band, frequency and eight bars beside it. Ka and laser are red.</li>
<li><b>City</b> mutes X and K. <b>Highway</b> and <b>Back road</b> are loud. <b>Under 10 over</b> the limit mutes regardless. The mode is automatic and shown on the card.</li>
<li>A Gen 2 pairs with one app at a time, so JBV1 is closed while Touge owns the radio.</li>
</ul>
</div>
</div>

<div class="row">
<figure><img src="/assets/images/touge/v2/radar.png" alt="Driving screen with the 100 mile radar disc top right"><figcaption>NEXRAD, 100 miles around the car, north up, rings at 25, 50 and 75.</figcaption></figure>
<div>
<span class="k">Weather radar</span>
<ul class="tight">
<li>Same IEM tile source RyanWeather uses, refreshed every five minutes or after 8 km of travel.</li>
<li>Off by default; it needs a connection.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/offroad.png" alt="Off-road mode with the Bronco icon and tracks drawn bold"><figcaption>Off-road: tracks routed on and drawn bold, no reroute when you leave the line.</figcaption></figure>
<div>
<span class="k">Off-road mode</span>
<ul class="tight">
<li>A fourth vehicle mode for the Bronco. Forest roads and trails are routed on and drawn orange.</li>
<li>Leaving the planned line is the plan, so there is no reroute nag. The breadcrumb trail records where the wheels went.</li>
</ul>
</div>
</div>

## Maps, search, and your own server

<div class="row">
<figure><img src="/assets/images/touge/v2/packs.png" alt="Map packs screen listing six regions with measured sizes"><figcaption>Six regions with measured sizes. Blue Ridge is 120 MB in 273 range requests.</figcaption></figure>
<div>
<span class="k">Map packs</span>
<ul class="tight">
<li>PMTiles is read in place, so the app walks the planet file's directory and adds up exactly the bytes it needs before fetching any.</li>
<li>The search index is built on the device from the same tiles: 207,879 places, towns and roads for Blue Ridge in about forty seconds.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/search.png" alt="Search results for Sparta, the town first"><figcaption>Search ranks by name, then kind, then distance. A town beats a road of the same name.</figcaption></figure>
<div>
<span class="k">Search</span>
<ul class="tight">
<li>Runs over the on-device index, not the tiles on screen. A diner 200 km ahead is as findable as one in view.</li>
<li>Fuel, bathroom, food and coffee are one tap each, ordered by distance ahead. Food hides chains by brand tag and checks opening hours, overnight spans included.</li>
</ul>
</div>
</div>

<div class="grid3">
<div class="tile"><b>Breadcrumbs</b><p>Every ride is recorded as GPX with no button. Points closer than 15 m are dropped unless the heading moved 12 degrees, so the corners are kept.</p></div>
<div class="tile"><b>Police reports</b><p>Over SABRE, an open Android protocol. Every alert carries its age and fades; police expire at 25 minutes, a closed road at six hours.</p></div>
<div class="tile"><b>Your own Valhalla</b><p>Routing defaults to the public FOSSGIS instance. Settings take your own URL. Hosting on mine is about $3 a month per user and answers in well under a second.</p></div>
<div class="tile"><b>Android Auto</b><p>The head unit shows the next turn, its distance and the ETA from the same guidance the tablet runs.</p></div>
</div>

<div class="last"></div>

## The recorder

<div class="row">
<figure><img src="/assets/images/touge/frame-composite.png" alt="The recorded frame: camera picture untouched, telemetry cards in the margin"><figcaption>The video is untouched, pixel for pixel. The cards live in the margin.</figcaption></figure>
<div>
<span class="k">Overlay around the video, not on it</span>
<ul class="tight">
<li>A 1920x1080 camera plus a third again of margin lands on 2560x1440. The margin is static, so it encodes to almost nothing; the cost is encoder throughput, and 2560x1440 at 29 fps sustained is a measured number on this hardware.</li>
<li>Every card has a switch. Cards are laid out by weight, so switching one off gives its height to the rest. Cards that need a surveyed circuit stay off on a public road.</li>
<li>The recorder arms on the road ahead, not the corner you are in. Corner radius comes from <code>v² / a_lat</code>: 70 mph at 0.35 g is a 285 m interstate sweeper, 35 mph at 0.35 g is a 71 m corner worth filming. Ten seconds of pre-roll puts the turn in the clip. Freeways never arm it.</li>
</ul>
</div>
</div>

</div>
