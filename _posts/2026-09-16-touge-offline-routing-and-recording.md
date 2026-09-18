---
title: "Touge"
date: 2026-09-16 00:00:00 -0400
last_modified_at: 2026-09-18 09:00:00 -0400
categories: car tech
tags: [touge, android, navigation, offline, maplibre, pmtiles, valhalla, meshtastic, lora, gmrs, tpms, radar, valentine-one, android-auto, dashcam, telemetry, kotlin, compose, openstreetmap, motorcycle, bronco, back-roads]
cover: /assets/images/touge/v2/group.png
lightbox: true
excerpt: "Offline back-road navigation, five cars kept together over LoRa with no signal, and a dashcam that keeps the telemetry off the video."
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
<p>Plans the twisty way, calls the corners, keeps five cars on one map with no signal, and records the drive with the numbers around the video instead of on it. A paid app, with an optional small monthly subscription for routing on my server.</p>
<div class="stats">
<div class="stat"><b>4</b><span>routes per leg</span></div>
<div class="stat"><b>0</b><span>accounts</span></div>
<div class="stat"><b>30 s</b><span>group pings, mesh or cell</span></div>
<div class="stat"><b>100 mi</b><span>radar disc</span></div>
<div class="stat"><b>60</b><span>car icons</span></div>
<div class="stat"><b>4</b><span>states offline</span></div>
<div class="stat"><b>398</b><span>unit tests</span></div>
</div>
</div>

Everything below is the app running against the North Carolina map pack, routing on my own Valhalla box. Nothing is a mockup and nothing is a render.

## Plan the road, not the arrival time

<p class="lead">Every other nav app offers alternatives as minutes saved. Touge offers them as corners.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/route-picker.png" alt="Four routes from Sparta to Boone, North Carolina, each in its own color with time, distance and twist score"><figcaption>Sparta to Boone, 53 miles. Four routes, four colors, one card per route in the same color.</figcaption></figure>
<div>
<span class="k">Route picker</span>
<ul class="tight">
<li>The TWIST score is measured off the geometry: heading change per mile, share of distance in real corners, median corner radius, junctions per mile. No model guesses which road sounds scenic.</li>
<li>Four routes come from asking Valhalla four times at different highway tolerances and merging by shape. The fastest is always one of them.</li>
<li>Old NC 16 scores 28 and costs half an hour. NC 88 scores 24 and costs eight minutes. That is the whole decision, in two numbers.</li>
<li>Each card sits where its route is furthest from the others and wears the route's color. The selected one is outlined.</li>
<li><b>Tap the name to change it.</b> Picking the wrong town is a tap to fix, not a reason to plan the ride again.</li>
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
<figure><img src="/assets/images/touge/v2/driving.png" alt="Driving screen with the turn card, a Ka band radar alert, the group table, a tire pressure warning and the weather radar disc"><figcaption>One screen, everything at once: the next turn, a Ka alert with its bearing, four cars up the road, a rear-left tire going down, and 100 miles of weather.</figcaption></figure>
<div>
<span class="k">Turn by turn</span>
<ul class="tight">
<li>Position snaps to the route. The heading tolerance widens with the bend, so a hairpin, where the road sits 90 degrees off the car, still matches instead of calling you off route.</li>
<li>Off the route for three seconds, or pointing the wrong way, gets one reroute.</li>
<li>Lane guidance draws the lanes to be in when the map data carries them.</li>
<li>Calls land about eight seconds out, so they scale with speed. Music ducks, never pauses.</li>
<li>The strip counts down. Arrival, time left and distance left come from where you actually are, not from the plan the route was fetched with.</li>
<li>The voice is a button on the rail, not a trip into settings. Silent is one press from anything.</li>
</ul>
</div>
</div>

## Skipping a stop without stopping

<p class="lead">Google's version is a small dialog at the moment you are looking for a parking space. This one comes up two miles out, with three big buttons, and needs no answer.</p>

<figure><img src="/assets/images/touge/v2/stop-ahead.png" alt="Stop ahead card 1.3 miles out with Skip, Later and Go on"><figcaption>1.3 miles from Marion, an intermediate stop. Skip drops it, Later moves it to the end of the trip, Go on keeps it.</figcaption></figure>

<ul class="tight">
<li><b>Skip</b> drops the stop and the route goes straight on to the next one, so a town you only meant to pass does not pull you into its center and back out.</li>
<li><b>Later</b> moves it to the end of the trip.</li>
<li><b>Go on</b> keeps it as planned. Nothing is modal; ignore the card and the trip advances on its own when you arrive under 5 mph.</li>
<li>To try it: set two stops, start, and drive to within two miles of the first. The card appears on the driving screen and on the phone layout.</li>
</ul>

<div class="grid3">
<div class="tile"><b>Silent</b><p>The turn card still counts down.</p></div>
<div class="tile"><b>Calm</b><p>Valhalla's own wording.</p></div>
<div class="tile"><b>Brief</b><p>Direction and road, nothing else.</p></div>
<div class="tile"><b>Touge</b><p>Japanese, shouted, about the corner. <code>ヘアピン左！落とせ！</code> for a hairpin left. Needs a Japanese voice installed and says so if there is none.</p></div>
</div>

## Shape the line yourself

<p class="lead">Search finds roads by name — type "bledsoe" and Bledsoe Creek Road comes back, 1.5 miles out. What it cannot do is say <em>which way</em>: up this one, over the gap, down the other side, in that order. That is a shape, not a name, and the place to draw a shape is a map.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/editor.png" alt="Route editor with three numbered pins dropped on back roads near Sparta and the routed line running through them"><figcaption>Three pins, dragged onto the roads I meant. The engine joins them up; the line is the answer, not a sketch.</figcaption></figure>
<div>
<span class="k">Route editor</span>
<ul class="tight">
<li>Long-press to drop a pin. Drag it to move it. Tap it to change or remove it.</li>
<li>Dragging is the point. "Not that road, the one a ridge over" is a decision you make by looking, and you can see immediately whether the line went where you meant.</li>
<li>A pin does not need to be anywhere named — a pull-off, a gate, the car park everyone meets in.</li>
<li>Reverse rides it the other way, and the leg styles travel with the legs — highway out stays highway out, it does not become highway home.</li>
<li>Close loop brings you back to the first place you chose, not to wherever the car was parked.</li>
<li>Save GPX, or Start and drive it.</li>
<li>The map holds still while you work. Each pin handles its own touches, so panning and zooming behave exactly as they do everywhere else.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/library.png" alt="Rides and routes library listing recorded rides, each with Follow, Route it, share and delete"><figcaption>Every ride it recorded, and every GPX you brought in. One flat list.</figcaption></figure>
<div>
<span class="k">Rides and routes</span>
<ul class="tight">
<li><b>Follow</b> pins you to the line exactly as recorded, with no router involved. That is how a forest road the map has never heard of stays the route.</li>
<li><b>Route it</b> hands the same line to the engine as shaping points and gives back street names, lanes and rerouting.</li>
<li>A recorded ride defaults to Follow. Something shared out of Maps defaults to Route it. They are different questions.</li>
<li>Import a GPX, share one out. A plan is written as a route, a recording as a track — this will not pass off a computed line as something the wheels did.</li>
<li>Recorded rides prune to the newest thirty. Files you imported are never touched.</li>
</ul>
</div>
</div>

<div class="quote">A ride you liked is a file. Ride it again, hand it to somebody, or open it and move three pins.</div>

<p class="lead">And the list screen still beats the map for what it is good at: order, and the style of each leg. The two are for different questions, so both are there.</p>

## Ride together

<p class="lead">A group spread over eight miles of ridge road is not a question of pins on a map. It is who, how far, and how old that fix is.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/group.png" alt="Five group cars drawn on the route in their own colors and icons, with the group card listing gap and age"><figcaption>Five cars, each its own model and color. The group card sorts front to back.</figcaption></figure>
<div>
<span class="k">Group card</span>
<ul class="tight">
<li><b>Gap is measured along the road.</b> On a switchback the car two hairpins back is 1,000 feet away and four minutes behind, and the card says four minutes.</li>
<li><b>Age is the age of the fix</b>, not of the packet. A car that stops reporting greys out and stays in the table with its last-seen time. Tom, two minutes stale, is still there.</li>
<li>More than 400 feet off the shared route falls back to straight line and gets a <code>~</code>.</li>
<li>The worst gap and any silent car are spoken.</li>
<li><b>No teleporting.</b> Between pings each car is dead-reckoned along the route at its last speed (a 30 s ping at 45 mph is 660 feet), and when the real fix lands the icon eases onto it over a couple of seconds. Prediction stops after a minute, so a car that has gone quiet stays where it was last seen.</li>
</ul>
</div>
</div>

<div class="row">
<figure><img src="/assets/images/touge/v2/report.png" alt="Report tray open with six large targets: Police, Hazard, Crash, Traffic, Closed, Animal"><figcaption>One tap opens six targets. The second files it and closes.</figcaption></figure>
<div>
<span class="k">Tap to report</span>
<ul class="tight">
<li><b>Two taps, not five.</b> Waze asks for a category, then a subtype, then a confirmation, with small targets. Here every target is 96 dp and one word, there is no subtype, and the undo lives in the toast that follows rather than in a dialog before it.</li>
<li><b>It goes to the group.</b> A report rides the same authenticated exchange as the positions, so it reaches everyone on the ride over cell or LoRa. The report from the car 400 yards ahead is the one that matters on a back road.</li>
<li><b>Duplicates collapse.</b> The same kind within 150 feet is the same thing seen twice, so the car behind filing the same speed trap is one pin.</li>
<li>Each expires on its own clock: police at 25 minutes, a closed road at six hours. They survive a restart mid-ride.</li>
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
<li><b>Name, color and car icon travel with the fix</b> on both links, so what you set is what the others see.</li>
<li><b>The ping interval is a setting:</b> 10 s, 30 s, 1 min or 2 min, for both links. Thirty seconds is a few hundred milliseconds of LoRa airtime per car.</li>
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
<figure><img src="/assets/images/touge/v2/profile.png" alt="Profile screen: name, color swatches, and a searchable grid of car icons"><figcaption>You: a name, a color, and your car from a catalogue of sixty.</figcaption></figure>
<div>
<span class="k">Your car</span>
<ul class="tight">
<li><b>Sixty cars, drawn not photographed.</b> Each one is a description: nose shape, where the cabin sits, what the headlights look like, and its own details. The R53's contrast roof and bonnet stripes, the E82's quad lights, the GT86's pointed nose, the NA Miata's pop-up lights, the 911's rear grille, the Bronco's spare on the tailgate, wings, flares, roof racks, kidney grilles, quad exhausts.</li>
<li>Search by make or model: Mini, GT86, Bronco, 911, Wrangler, Raptor, two bikes.</li>
<li>Pick a swatch and every icon repaints in your color. The preview is exactly what the group sees on their map.</li>
</ul>
</div>
</div>

### Setting up the radio

<div class="row">
<figure><img src="/assets/images/touge/v2/mesh-setup.png" alt="Meshtastic radio screen: paired and nearby radios, and the radio setup for this ride"><figcaption>Pair, then one button configures the radio for the ride.</figcaption></figure>
<div>
<span class="k">Standard Meshtastic, nothing custom on the air</span>
<ul class="tight">
<li><b>Pairing:</b> bonded radios are listed, a scan finds ones not yet paired, a tap starts the bond. PIN is the Meshtastic default, 123456.</li>
<li><b>Configure this radio for the ride</b> sends four admin messages over BLE: region US (915 MHz), preset LONG_FAST, hop limit 3, and a primary channel named for the ride (<code>tg-</code> plus six characters of the key's hash) with encryption off. The radio applies it and restarts. Every tablet on the ride derives the same channel name, so nothing is typed.</li>
<li><b>Speed:</b> LONG_FAST is about 1 kbit/s. A position packet is under 40 bytes, so a ping is a few hundred milliseconds of airtime and range is measured in miles of ridge line.</li>
<li><b>Discovery</b> is Meshtastic's own: every radio on the channel rebroadcasts up to three hops and keeps a node list. Touge reads that list and the standard Position and NodeInfo packets. Only the car icon and color go on a private port (256) that other apps ignore.</li>
<li><b>The official Meshtastic app on an iPhone or Android</b>, with its own radio on the same channel name and encryption off, sees every car as a node on its map, and its position shows on ours, as a generic car.</li>
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

## Alerts you control

<div class="row">
<figure><img src="/assets/images/touge/v2/alerts.png" alt="Alerts screen: one row per kind with a map toggle and a voice toggle"><figcaption>One row per kind, both switches on the row.</figcaption></figure>
<div>
<span class="k">Map and voice, separately</span>
<ul class="tight">
<li>Waze puts every report type behind its own page with the same two switches on each. Here it is one screen: every kind is a row, and the pin toggle and the speaker toggle sit on it.</li>
<li>They answer different questions. A pin is <em>what is out there</em>, worth a glance in traffic; a spoken warning is <em>act now</em>, and far fewer things earn one. Traffic drawn and silent is the setting most drivers land on, and one switch cannot say that.</li>
<li>Defaults: police, crashes, hazards and closed roads speak; traffic and animals draw and stay quiet.</li>
<li>Warning distance is ½, 1, 1.5 or 2 miles, and a chime can lead the words so the first syllable is not the warning.</li>
<li>Voice follows the voice mode, so Silent stays silent.</li>
</ul>
</div>
</div>

## The car

<div class="row">
<figure><img src="/assets/images/touge/v2/tires.png" alt="Tires screen with four wheel tiles; rear left is leaking"><figcaption>Four tiles, one leaking. The rate is a fitted slope, not two samples.</figcaption></figure>
<div>
<span class="k">Tire pressure</span>
<ul class="tight">
<li>Bluetooth TPMS sensors (Zeepin/TPMSII, DJTPMS, Tesla) bind to a wheel by tapping the wheel, then the sensor.</li>
<li>Alarms in order: under 20 psi; losing 2 psi a minute (a least-squares slope over two minutes, so a puncture trips it and quantization does not); 10 psi below the tire's peak; over 158°F.</li>
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
<li>No key, no account. The default feed is the Iowa Environmental Mesonet's national composite, served as public map tiles, the same feed RyanWeather reads. Refreshed every five minutes or after five miles of travel.</li>
<li>Any XYZ tile template with <code>{z}/{x}/{y}</code> can be pasted into the setting instead, RainViewer included.</li>
<li>Off by default; it needs a connection.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/settings-traffic.png" alt="Settings: live traffic switch and TomTom key field"><figcaption>Traffic is one switch and a free TomTom key.</figcaption></figure>
<div>
<span class="k">Live traffic</span>
<ul class="tight">
<li>Flow is drawn over the roads, green through red, from TomTom's traffic tiles. Incidents are pins with the delay and a description; a closed road is dark red.</li>
<li>Refreshed every two minutes around the car, not per pan, so a full day is well inside the free tier (2,500 requests a day). The key is yours, from developer.tomtom.com, pasted into settings.</li>
<li>Off by default; it needs a connection. Routing does not use it, so the twisty way stays the twisty way.</li>
</ul>
</div>
</div>

<div class="row">
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
<figure><img src="/assets/images/touge/v2/packs.png" alt="Map packs screen listing six regions with measured sizes and installed state"><figcaption>Measured, not estimated. North Carolina is 88,675 tiles and 392 MB; the three states I actually ride are 1.14 GB in one tap.</figcaption></figure>
<div>
<span class="k">Map packs</span>
<ul class="tight">
<li>PMTiles is read in place, so the app walks the planet file's directory and adds up exactly the bytes it needs before fetching any.</li>
<li><b>Keep as many as you have room for.</b> Every pack is drawn as its own layer, so North Carolina, Virginia and West Virginia are one map with no seam and no switching.</li>
<li>The search index is built on the device from the same tiles and spans every pack, so a Virginia town is findable from a North Carolina car park.</li>
<li>A pack you have downloaded never expires and needs no signal. Each row shows how old its roads are, and only suggests a refresh once that actually means something.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/search.png" alt="Search results for Sparta, the town first"><figcaption>Search ranks by name, then kind, then distance. A town beats a road of the same name.</figcaption></figure>
<div>
<span class="k">Search</span>
<ul class="tight">
<li>Runs over the on-device index, not the tiles on screen. A diner 120 miles ahead is as findable as one in view.</li>
<li>Fuel, bathroom, food and coffee are one tap each. On a route they are ordered by how soon you reach them, not by how near they are — the closest pump is often twenty minutes behind you. Food hides chains by brand tag and checks opening hours, overnight spans included.</li>
<li>The last places you picked, and any you starred, come up on an empty box. Typing a town name in a moving car is the most expensive thing this app asks for.</li>
</ul>
</div>
</div>

<div class="grid3">
<div class="tile"><b>Breadcrumbs</b><p>Every ride is recorded as GPX with no button. Points closer than 50 feet are dropped unless the heading moved 12 degrees, so the corners are kept. They land in Rides and routes, ready to Follow.</p></div>
<div class="tile"><b>Police reports</b><p>Over SABRE, an open Android protocol. Every alert carries its age and fades; police expire at 25 minutes, a closed road at six hours.</p></div>
<div class="tile"><b>Routing server</b><p>Routing goes to my Valhalla box first — well under a second, four states of tiles — with the public FOSSGIS instance behind it as a fallback for anywhere outside them. A list, not one address, because the day the public instance stopped answering it took every device with it. A small monthly subscription (about $3) covers the server. Settings take any Valhalla URL of your own.</p></div>
<div class="tile"><b>Android Auto</b><p>The tablet's map on the head unit: same style and pack, the route, the group's cars with their icons, the radar disc as an inset, heading up. The turn card with lanes and ETA; Skip, Later and Go on when a stop is ahead; Group, Routes, Search and Tires as car screens; tire and radar alerts as car toasts. The phone app does the work and the head unit shows it.</p></div>
</div>

## The settings screen

<p class="lead">One long screen, written back on change. Every toggle stays flipped.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/settings-group.png" alt="Settings: group ride section with You, Start or join, Meshtastic radio buttons, sharing switch, ping interval chips, LoRa relay"><figcaption>Group ride: identity, ride, radio, sharing, ping interval, LoRa relay, demo group.</figcaption></figure>
<div>
<span class="k">Sections</span>
<ul class="tight">
<li>Display and map view, vehicle, speed readout, recorded and track overlays.</li>
<li>Layout: tablet or phone; background presence.</li>
<li>Route line color, voice, routing server, group ride, alerts, traffic, weather radar feed, radar detector, tires, places, sensors, map packs.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/settings-routeline.png" alt="Settings: route line color swatches, blue selected"><figcaption>The route line is your color. Blue by default, the one drivers already read as "the way you are going".</figcaption></figure>
<div>
<span class="k">Route line</span>
<ul class="tight">
<li>Seven swatches, blue by default. Separate from the vehicle accent, which marks this car on the map and in the group — a route in the same color was one more thing that looked like you.</li>
<li>Android Auto draws the same line in the same color.</li>
</ul>
</div>
</div>

<div class="row">
<figure><img src="/assets/images/touge/v2/settings-radar.png" alt="Settings: weather radar switch and tile feed field, radar detector switch"><figcaption>The radar feed field and the V1 switch.</figcaption></figure>
<div>
<span class="k">What needs setting up</span>
<ul class="tight">
<li>Nothing, to drive: the public routing instance, the IEM radar feed and the demo group need no keys.</li>
<li>A ride needs a key, made on the Ride screen and shared by QR, link or mesh.</li>
<li>A radio needs pairing once and one tap of configure per ride.</li>
</ul>
</div>
</div>

<div class="last"></div>

## The recorder

<video controls loop muted playsinline preload="metadata"
       poster="/assets/images/touge/v2/recorder-nccar-poster.jpg"
       style="width:100%;height:auto;display:block;border-radius:16px;box-shadow:0 2px 14px rgba(0,0,0,.45);">
  <source src="/assets/images/touge/v2/recorder-nccar.mp4" type="video/mp4">
</video>
<p style="color:#9aa3ad;font-size:13px;margin:8px 0 22px">NCCAR, 13 September, recorded with TrackEncoder, the recorder Touge's is lifted from. Same cards, same trigger; Touge draws them in a margin around the picture instead of over it.</p>

<div class="row">
<figure><img src="/assets/images/touge/frame-composite.png" alt="The recorded frame: camera picture untouched, telemetry cards in the margin"><figcaption>The video is untouched, pixel for pixel. The cards live in the margin.</figcaption></figure>
<div>
<span class="k">Overlay around the video, not on it</span>
<ul class="tight">
<li>A 1920x1080 camera plus a third again of margin lands on 2560x1440. The margin is static, so it encodes to almost nothing; the cost is encoder throughput, and 2560x1440 at 29 fps sustained is a measured number on this hardware.</li>
<li>Every card has a switch. Cards are laid out by weight, so switching one off gives its height to the rest. Cards that need a surveyed circuit stay off on a public road.</li>
<li>The recorder arms on the road ahead, not the corner you are in. Corner radius comes from <code>v² / a_lat</code>: 70 mph at 0.35 g is a 935 ft interstate sweeper, 35 mph at 0.35 g is a 230 ft corner worth filming. Ten seconds of pre-roll puts the turn in the clip. Freeways never arm it.</li>
</ul>
</div>
</div>

</div>
