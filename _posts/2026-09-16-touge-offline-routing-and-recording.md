---
title: "Touge"
date: 2026-09-16 00:00:00 -0400
last_modified_at: 2026-09-23 10:00:00 -0400
categories: car tech
tags: [touge, android, navigation, offline, maplibre, pmtiles, valhalla, meshtastic, lora, gmrs, tpms, radar, valentine-one, android-auto, dashcam, telemetry, kotlin, compose, openstreetmap, motorcycle, bronco, back-roads]
cover: /assets/images/touge/v2/group.png
lightbox: true
excerpt: "Five cars kept together on one map over LoRa with no signal, offline back-road navigation, and a dashcam that keeps the telemetry off the video."
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
<p>Keeps the whole group on one map with no signal — over LoRa radio when the bars run out — then plans the twisty way there and calls the corners. A paid app, with an optional small monthly subscription for routing on my server.</p>
<div class="stats">
<div class="stat"><b>5</b><span>ways per leg</span></div>
<div class="stat"><b>0</b><span>accounts</span></div>
<div class="stat"><b>1 s</b><span>car to car on 2.4 GHz</span></div>
<div class="stat"><b>100 mi</b><span>radar disc</span></div>
<div class="stat"><b>60</b><span>car icons</span></div>
<div class="stat"><b>4</b><span>states offline</span></div>
<div class="stat"><b>398</b><span>unit tests</span></div>
</div>
</div>

Everything below is the app running against the North Carolina map pack, routing on my own Valhalla box. Nothing is a mockup and nothing is a render.

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
<li><b>No teleporting.</b> Between fixes each car is carried along the route at its last speed, and when the real fix lands the icon eases onto it in about a third of a second. The guess stops after ten seconds on the route and three off it, which at 50 mph is about 200 m of invention at most. Past that the car sits where it was last heard and its age counts up, because that's the truth.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/report.png" alt="Report tray open with six large targets: Police, Hazard, Crash, Traffic, Closed, Animal"><figcaption>One tap opens six targets. The second files it and closes.</figcaption></figure>
<div>
<span class="k">Tap to report</span>
<ul class="tight">
<li><b>Two taps, not five.</b> Waze asks for a category, then a subtype, then a confirmation, with small targets. Here every target is 96 dp and one word, there is no subtype, and the undo lives in the toast that follows rather than in a dialog before it.</li>
<li><b>It goes to the group.</b> A report rides the same authenticated exchange as the positions, so it reaches everyone on the ride over cell or LoRa. The report from the car 400 yards ahead is the one that matters on a back road.</li>
<li><b>And out to the road.</b> With the WzSabre proxy installed, the same tap also files it to Waze, so it reaches drivers who aren't on your ride. Every kind goes, animals included: a deer on the shoulder is filed as Waze's own animal hazard, not kept to the group. Taking from a crowd-sourced feed without ever adding to it is a poor way to use one.</li>
<li><b>Duplicates collapse.</b> The same kind within 150 feet is the same thing seen twice, so the car behind filing the same speed trap is one pin.</li>
<li>Each expires on its own clock: police at 25 minutes, a closed road at six hours. They survive a restart mid-ride.</li>
</ul>
</div>
</div>

<span class="k">How positions travel</span>
<div class="wire">
<svg viewBox="0 0 900 330" role="img" aria-label="Group positions travel phone to radio over Bluetooth, radio to radio over a 2.4 GHz lane about once a second and LoRa every five seconds, with the server over cell as the fallback">
<defs>
<marker id="pg" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#2e9e4f"/></marker>
<marker id="pb" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#1e88e5"/></marker>
<marker id="po" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#e08a1e"/></marker>
<marker id="pw" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#9aa3ad"/></marker>
</defs>
<rect x="0" y="0" width="900" height="330" fill="#14181e"/>
<g font-family="system-ui,Segoe UI,Roboto,sans-serif" font-size="14" fill="#e9edf2">
<rect x="15" y="40" width="150" height="70" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="90" y="68" text-anchor="middle">Your phone</text><text x="90" y="90" text-anchor="middle" fill="#9aa3ad" font-size="12">GPS fix to the radio, 1 Hz</text>
<rect x="215" y="40" width="150" height="70" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="290" y="68" text-anchor="middle">Your radio</text><text x="290" y="90" text-anchor="middle" fill="#9aa3ad" font-size="12">ESP32-S3 + SX1262</text>
<rect x="535" y="40" width="150" height="70" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="610" y="68" text-anchor="middle">Their radio</text><text x="610" y="90" text-anchor="middle" fill="#9aa3ad" font-size="12">relays both lanes</text>
<rect x="735" y="40" width="150" height="70" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="810" y="68" text-anchor="middle">Their phone</text><text x="810" y="90" text-anchor="middle" fill="#9aa3ad" font-size="12">map, card, chat</text>
<line x1="165" y1="75" x2="213" y2="75" stroke="#9aa3ad" stroke-width="3" marker-end="url(#pw)"/><text x="189" y="64" text-anchor="middle" fill="#9aa3ad" font-size="11">BLE</text>
<line x1="685" y1="75" x2="733" y2="75" stroke="#9aa3ad" stroke-width="3" marker-end="url(#pw)"/><text x="709" y="64" text-anchor="middle" fill="#9aa3ad" font-size="11">BLE</text>
<line x1="365" y1="58" x2="533" y2="58" stroke="#2e9e4f" stroke-width="4" marker-end="url(#pg)"/>
<text x="450" y="34" text-anchor="middle" fill="#2e9e4f" font-size="12">2.4 GHz · about 1 Hz</text>
<text x="450" y="49" text-anchor="middle" fill="#9aa3ad" font-size="11">ESP-NOW, line of sight</text>
<line x1="365" y1="92" x2="533" y2="92" stroke="#1e88e5" stroke-width="3" stroke-dasharray="8 6" marker-end="url(#pb)"/>
<text x="450" y="112" text-anchor="middle" fill="#1e88e5" font-size="12">LoRa · every 5 s</text>
<text x="450" y="127" text-anchor="middle" fill="#9aa3ad" font-size="11">915 MHz, further</text>
<rect x="360" y="205" width="180" height="62" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="450" y="231" text-anchor="middle">Your server</text><text x="450" y="252" text-anchor="middle" fill="#9aa3ad" font-size="12">signed, no accounts</text>
<path d="M90,110 C90,236 220,236 358,236" fill="none" stroke="#e08a1e" stroke-width="3" marker-end="url(#po)"/>
<path d="M542,236 C690,236 810,236 810,112" fill="none" stroke="#e08a1e" stroke-width="3" marker-end="url(#po)"/>
<text x="450" y="178" text-anchor="middle" fill="#9aa3ad" font-size="12">newest fix per rider wins, the radio beats the server</text>
<text x="120" y="292" fill="#e08a1e" font-size="12">cell: every 5 s with no radio</text>
<text x="120" y="310" fill="#e08a1e" font-size="12">every 30 s with one, to find a lost car</text>
<text x="580" y="292" fill="#9aa3ad" font-size="12">colours match the icons on the group card:</text>
<text x="580" y="310" font-size="12"><tspan fill="#2e9e4f">green 2.4 GHz</tspan><tspan fill="#9aa3ad"> · </tspan><tspan fill="#1e88e5">blue LoRa</tspan><tspan fill="#9aa3ad"> · </tspan><tspan fill="#e08a1e">amber server</tspan></text>
</g>
</svg>
</div>

<p class="lead">Three ways between cars, fastest first. The app runs all of them at once and keeps whichever fix for each car is newest.</p>

<ul class="tight">
<li><b>Phone to radio is Bluetooth.</b> Touge talks Meshtastic's own BLE service and protobufs. Once a second it writes the phone's GPS fix to its own radio, addressed to the radio itself, so it costs no airtime. The radio needs no GPS of its own, and whichever lane fires next sends the latest position rather than one from twenty seconds ago.</li>
<li><b>2.4 GHz, the fast lane.</b> On a Heltec V3 or V4 running the Touge firmware module, the ESP32's own WiFi radio carries positions over ESP-NOW in long-range mode (250 kbit/s) on channel 1, 6 or 11. A 250 ms cycle is split into nine slots so cars take turns instead of colliding. A car transmits after 20 m of travel, or once a second when parked, and each car is handed to the phone at most once a second. Measured on the bench: about 1.0 Hz per car. Range is roughly what you can see, with two hops to reach the back of a strung-out group. Encrypted with a key derived from the ride's channel key, and signed.</li>
<li><b>LoRa, the long lane.</b> The same position goes out over 915 MHz every 5 seconds, which is as often as the radio will send one. It reaches further and gets over terrain where 2.4 GHz can't, relays up to three hops, and the gap grows with the group so the channel stays under 30% busy (capped at 20 seconds). When a car has been heard on 2.4 GHz in the last 3 seconds, its slower LoRa copy doesn't overwrite it.</li>
<li><b>Cell, the fallback.</b> With no radio, the fix goes to <code>server/convoy.py</code> beside Valhalla every 5 seconds, signed with HMAC-SHA256 under the ride key. No database, no accounts, positions pruned after thirty minutes, plain http refused. With a radio, the server is still asked every 30 seconds, and every cycle once the radio has heard nobody for 30 seconds, so a car that drove out of radio range is found from either side.</li>
<li><b>The radio outranks the server.</b> Every fix carries the time it was measured, so a late cell packet never overwrites a newer radio one. A server copy has to be five seconds newer before it takes a car off the radio, which stops a car sitting two lengths ahead from flipping between sources on timestamp noise.</li>
<li><b>Name, colour and car icon travel with the fix</b> on every link, so what you set is what the others see.</li>
<li><b>The ping interval is a setting,</b> 1 second to a minute, 5 by default. On cell it's what you get. On LoRa it's a request: the air decides.</li>
</ul>

<div class="row">
<figure><img src="/assets/images/touge/v2/chat-lanes.png" alt="Chat screen: the 2.4 GHz lane card reading channel 11, one car on it, slot 3, and one row for mattpixel reading 2.4 0.7 Hz"><figcaption>The chat screen on a bench test. The fast lane's own report on top, then one row per car.</figcaption></figure>
<div>
<span class="k">Signal, on the chat screen</span>
<ul class="tight">
<li><b>The lane card</b> is the radio's own account of its 2.4 GHz side: which channel it settled on, how many cars it can hear on it, its slot, and what the cycle is timed by. If the report stops for 16 seconds the card turns red and says so, because a silent fast lane otherwise looks exactly like a quiet road.</li>
<li><b>One row per car, one lane per row.</b> Only the lane carrying that car right now is shown. <code>2.4 1.0 Hz -48 dBm</code> while the fast lane is live (heard in the last 5 seconds); otherwise <code>LoRa 13 dB SNR -62 dBm 2 hops</code> while LoRa is (heard in the last 30). Heard on neither lately, it greys out to <code>last heard 45s ago</code>.</li>
<li><b>Your own car isn't listed.</b> The radio hands back your own positions too, and a row saying how well you hear yourself is noise.</li>
<li><b>The colour is the signal strength.</b> Dark green at -55 dBm or better, light green to -68, yellow to -78, orange to -88, red below that. A car's name turns red when its packets arrive having used all three hops: nothing is left in the budget and the next ridge drops it.</li>
<li><b>What the numbers mean.</b> -90 dBm is a healthy LoRa link and a dead 2.4 GHz one, so they don't read the same. On 2.4 GHz, -67 or better is good, -75 is okay, -85 is weak. LoRa decodes below the noise floor: -100 is still good and -120 is the edge. SNR is LoRa only: 5 dB or more is excellent, zero is fine, under -10 is about to drop. The Range button reads it out in words per lane: Excellent, Good, Okay, Weak, About to drop.</li>
<li><b>Messages carry their route too.</b> Each one has a dot, blue for the radio and grey for the server, and says "direct", "3 hops" or "server".</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/group-links.png" alt="Group card with two cars: mattsam with an amber cell icon, 20s old, and mattpixel with a green antenna, now"><figcaption>mattsam over the server, 20 seconds old. mattpixel over 2.4 GHz, now.</figcaption></figure>
<div>
<span class="k">Colours on the map and the card</span>
<ul class="tight">
<li><b>The icon beside each car says which link it came over.</b> A green antenna is the 2.4 GHz lane, a blue antenna is LoRa, amber bars are the server. Green reads as the good one and amber as the fallback, which is the order they rank in.</li>
<li><b>The status chip does the same for your own radio:</b> green on 2.4 GHz, blue on LoRa only, red when it isn't connected.</li>
<li><b>On the map</b> each car is drawn as its own model in its rider's colour, with its name in that colour. Silent for two minutes, the car and its name fade to half.</li>
<li><b>On the compact card</b> a name chip stays plain while it's fresh. From 20 seconds old it tints light orange and shows its age, heading to light red by two minutes.</li>
</ul>
</div>
</div>

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
<span class="k">Standard Meshtastic on LoRa</span>
<ul class="tight">
<li><b>Pairing:</b> bonded radios are listed, a scan finds ones not yet paired, a tap starts the bond. PIN is the Meshtastic default, 123456.</li>
<li><b>Configure this radio for the ride</b> writes the radio's settings over BLE in one edit: region from the phone's country (US is 915 MHz), the radio speed, hop limit 3, your name as the radio's owner, and a primary channel named for the ride (<code>tg-</code> plus six characters of the key's hash). The channel is encrypted with a 32-byte key hashed from the ride key a different way, so the public channel name gives nothing away. The radio applies it and restarts. Every phone on the ride derives the same name and key, so nothing is typed.</li>
<li><b>Radio speed</b> is a setting: Turbo, Short, Medium or Long. Short is the default. A position is about 58 ms of airtime on Short and 760 ms on Long, thirteen times as much, and every car relays every other car's packets, so the air fills as the square of the group. Held to 30% of the channel, Short carries about ten cars on LoRa alone and Long about two. Past that the 2.4 GHz lane is doing the work, which is why it exists.</li>
<li><b>Discovery</b> is Meshtastic's own: every radio on the channel rebroadcasts up to three hops and keeps a node list. Touge reads that list and the standard Position and NodeInfo packets. Only the car icon and color go on a private port (256) that other apps ignore.</li>
<li><b>Any stock Meshtastic node</b> set to the same channel name and key sees every car as a node on its map, and its position shows on ours, as a generic car. It just won't get the 2.4 GHz lane.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/phone.png" alt="Phone layout: map, next turn, group card and three buttons in portrait"><figcaption>Phone layout: the basics, in portrait.</figcaption></figure>
<div>
<span class="k">Riding along on a phone</span>
<ul class="tight">
<li>Under 600 dp of width the app switches to one column: map, next turn, the group, three buttons.</li>
<li><b>Background presence</b> keeps sending your position with the app closed, at the same rate and over the same links as the screen, under one notification, for the rider who only wants to be on the map.</li>
<li><b>The turn card</b> puts the distance big beside the arrow with the time under it, and the road name on its own line across the full width, so "US 15 / US 501" isn't cut to "US 15 / US 50...".</li>
</ul>
</div>
</div>


### The radios

<p class="lead">Any Meshtastic radio on 915 MHz works. These are the two being tested here.</p>

<div class="grid3">
<div class="tile"><b><a href="https://www.seeedstudio.com/SenseCAP-Card-Tracker-T1000-E-for-Meshtastic-p-5913.html">SenseCAP Card Tracker T1000-E</a></b><p>Credit-card sized, sealed, with its own GPS and a battery measured in days. The one to hand a passenger or drop in a door pocket — nothing to mount and nothing to plug in.</p></div>
<div class="tile"><b><a href="https://meshnology.com/">Meshnology N30</a></b><p>A cased Heltec LoRa V3 — ESP32-S3 and an SX1262. Three of them here for the group test. Bigger than the card, and it takes an external antenna, which is what matters between ridges.</p></div>
<div class="tile"><b>What the app needs</b><p>Bluetooth pairing and nothing else for LoRa. The app speaks Meshtastic's own BLE service and writes its positions on a private port, so the radios stay ordinary Meshtastic nodes you can still use for text. The 2.4 GHz lane needs an ESP32 board, a Heltec V3 or V4, flashed with the Touge module. The T1000-E has no ESP32, so it stays on LoRa.</p></div>
</div>

<p class="lead">Neither has been up a mountain yet, so the range numbers above are the protocol's rather than mine.</p>

### Getting everyone in

<p class="lead">Three questions on first run, and the middle one is the group. Every one of them was already answerable from Setup, which was the problem: a new rider got a map with a nameless orange car and twenty sections to search through to find out why nobody could see them.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/wizard-ride.png" alt="Setup wizard step two: Ride together, with buttons to start a ride or join someone else's"><figcaption>Step two of three. Start one, or join one.</figcaption></figure>
<div>
<span class="k">First run</span>
<ul class="tight">
<li><b>What are you driving</b> — name, car and colour, which is what everyone else sees on their map.</li>
<li><b>Ride together</b>, before the map download, on purpose: a group works fine on a streamed map, and making somebody wait for 400 MB before they can join their friends is the wrong order.</li>
<li><b>If a radio hears a ride already running, it is one tap.</b> The invite travels over the mesh, so three cars in a car park need no key typed and no QR held up to a windscreen.</li>
<li>The person organising it hits <b>Advertise</b> and the invite goes back out every 45 seconds, so latecomers are offered the ride as they pull in. It stops itself after fifteen minutes — the broadcast carries the key, and a ride advertising all afternoon is one anybody in radio range can walk into.</li>
<li>Maps last. Every step skips, nothing is modal afterwards, and an existing profile never sees it at all.</li>
</ul>
</div>
</div>

## Plan the road, not the arrival time

<p class="lead">Every other nav app offers alternatives as minutes saved. Touge offers them as corners.</p>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/route-picker.png" alt="Four routes from Sparta to Boone, North Carolina, each in its own color with time, distance and twist score"><figcaption>Sparta to Boone, 53 miles. Four routes, four colors, one card per route in the same color.</figcaption></figure>
<div>
<span class="k">Route picker</span>
<ul class="tight">
<li>The TWIST score is measured off the geometry: heading change per mile, share of distance in real corners, median corner radius, junctions per mile. No model guesses which road sounds scenic.</li>
<li><b>Five ways, not one.</b> Every leg is asked three times at different highway tolerances and the answers merged by shape, so the set spans refusing the slab, tolerating it, and taking it. The fastest is always one of the five.</li>
<li>It asks <b>my own Valhalla</b>, four states of tiles, in well under a second — with Touge's preferences baked into the request rather than applied afterwards. Asking for a normal route and re-sorting by curviness gets a worse set to sort: the engine has already decided the interstate is the answer and offered three variations on it.</li>
<li>Old NC 16 scores 28 and costs half an hour. NC 88 scores 24 and costs eight minutes. That is the whole decision, in two numbers.</li>
<li>Each card sits where its route is furthest from the others and wears the route's color. The selected one is outlined.</li>
<li><b>Tap the name to change it.</b> Picking the wrong town is a tap to fix, not a reason to plan the ride again.</li>
</ul>
</div>
</div>

## Highway out, back roads home

<p class="lead">Every other routing app takes one instruction for the whole trip. A real ride is slab until the good roads start, then nothing but corners, then the quick way home — and that is three instructions, not one.</p>

<div class="row">
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

## Shape the line yourself

<p class="lead">Search finds roads by name — type "bledsoe" and Bledsoe Creek Road comes back, 1.5 miles out. What it cannot do is say <em>which way</em>: up this one, over the gap, down the other side, in that order. That is a shape, not a name, and the place to draw a shape is a map.</p>

<div class="row flip">
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

<div class="row">
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

## Drive it

<div class="row flip">
<figure><img src="/assets/images/touge/v2/driving.png" alt="Driving screen with the turn card, a Ka band radar alert, the group table, a tire pressure warning and the weather radar disc"><figcaption>One screen, everything at once: the next turn, a Ka alert with its bearing, four cars up the road, a rear-left tire going down, and 100 miles of weather.</figcaption></figure>
<div>
<span class="k">Turn by turn</span>
<ul class="tight">
<li>Position snaps to the route. The heading tolerance widens with the bend, so a hairpin, where the road sits 90 degrees off the car, still matches instead of calling you off route.</li>
<li><b>Off route</b> is more than 40 m (130 feet) from the line for three seconds, or off it and facing more than 60 degrees away for a second and a half. One bad fix under a bridge does neither. What happens next is below.</li>
<li><b>Heading comes from the road, not the phone, when slow.</b> Above 12 mph the GPS bearing is used as is. Below that it's the direction of the track the car has actually driven, once it has covered 15 m or twice the fix's accuracy. Picking the phone up at a light and putting it back no longer spins the map.</li>
<li><b>The speed limit sign works without a route.</b> On a route the limits come with it and work with no signal. Without one, every 150 m of travel the server is asked what the road under the car is posted at, with the heading picking the carriageway. The last answer holds through a dead spot and is dropped after a minute without a new one.</li>
<li>Lane guidance draws the lanes to be in when the map data carries them.</li>
<li>Calls land about eight seconds out, so they scale with speed. Music ducks, never pauses.</li>
<li>The strip counts down. Arrival, time left and distance left come from where you actually are, not from the plan the route was fetched with.</li>
<li>The voice is a button on the rail, not a trip into settings. Silent is one press from anything.</li>
</ul>
</div>
</div>

## Off the route

<p class="lead">A full reroute answers "the best way from here". After a missed turn on a planned ride that's the wrong question, because the best way from here usually skips the twisty section that was the reason for picking the route.</p>

Nothing pops up for a wobble. The offer only appears once the car is a quarter mile off the line, moving at 10 mph or more, with a destination still ahead. Parked beside yesterday's route, it stays quiet. It sits over whatever screen is up, with big targets for a thumb on a bumpy road.

<div class="grid3">
<div class="tile"><b>Back to my route</b><p>The default, and what happens on its own after a 30 second countdown. A short connector from the car to a point further along the route you picked, then the rest of that route exactly as it was: same roads, same turns, same stops.</p></div>
<div class="tile"><b>New route to Boone</b><p>Named for the last stop. A fresh route from here, with stops already behind you dropped. It keeps your taste per leg: if you picked the quickest option, each leg's new answer is the quickest again, and if you picked the twistiest, the twistiest.</p></div>
<div class="tile"><b>Keep going</b><p>Dismisses it. Still well off the line ten seconds later, it asks again. Taking a different road on purpose is the point of this app, and a nav that keeps dragging you back is one people switch off.</p></div>
</div>

<ul class="tight">
<li><b>The rejoin point is never behind the car.</b> It's at least 800 m (half a mile) past where you left the route, or as far past it as you've strayed, whichever is more. Aiming for the nearest bit of line is how a nav tells someone who drove two miles past their turn to make a U-turn.</li>
<li>Within the next 8 km after that, the point closest to the car wins, with a small pull towards earlier ones so a parallel road doesn't skip ten miles of the ride.</li>
<li><b>The connector is costed like the leg it rejoins.</b> Getting back onto a back-roads leg doesn't go by the interstate. The router is also told which way the route runs at the join, so you arrive going the right direction rather than facing the way you came.</li>
<li>The connector's "you have arrived" is dropped. Its last real turn is the one onto the route, and that's what gets said.</li>
<li>The countdown only stops for three things: getting back on the line, arriving, or Keep going. Driving back towards the route doesn't cancel it halfway.</li>
<li>No way back onto the route from here, and it says so and works out a new one instead.</li>
</ul>

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

## Weather, and where the police are

<p class="lead">Two things worth knowing before you get to them: what the sky is doing for a hundred miles, and who is sitting on the next straight.</p>

<div class="grid3">
<div class="tile"><b>Waze's reports, Waze's colours</b><p>Hazards come in through WzSabre and are drawn as round badges in the map itself, so they move with it. The colours match Waze so a pin reads the same in both apps: police blue, crashes red, hazards amber, closed roads red, traffic orange. Each subtype has its own symbol: stopped vehicle, construction, pothole, ice, fog, flood, animal, speed camera.</p></div>
<div class="tile"><b>Every badge says how old</b><p>The label under it reads "Police · 21 min ago". The badge fades as it ages and police shrink as well. Each kind has its own lifetime: police 25 minutes, traffic 20, animals 30, crashes 90, hazards two hours, closed roads six.</p></div>
<div class="tile"><b>Whether the feed is alive</b><p>A chip on the map reads "Waze · just now · 14": when the last update landed and how many reports it held. Asked every two minutes, around the car and every five miles along the route out to 25. Green while that keeps up, amber once an update is overdue, red past six minutes, so a feed the battery optimiser quietly killed doesn't look like a quiet road.</p></div>
<div class="tile"><b>Police on the radar disc</b><p>Police reports are plotted on the weather disc too, north up, where they are. A fresh one is a big bright red dot that fades and shrinks toward pale orange. Under the disc the nearest three are listed with distance and age: "14 mi · 4 min".</p></div>
<div class="tile"><b>Tap for the detail</b><p>Distance, how old, the band if it is radar, and how many people have confirmed it. The same numbers the report carried, none of them invented.</p></div>
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

## Alerts you control

<div class="row flip">
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

## Valentine One Gen 2

<p class="lead">The detector already beeps. What it cannot do is tell you where, on a screen, while you drive.</p>

<div class="row">
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

## Tire pressure

<p class="lead">A slow leak is the failure that ends a mountain day, and it announces itself an hour before it strands you.</p>

<div class="row flip">
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
<figure><img src="/assets/images/touge/v2/offline-routing.png" alt="Setting: Route without a connection, switched on, with its explanation"><figcaption>Off by default, and the screen says what it costs before you turn it on.</figcaption></figure>
<div>
<span class="k">Routing with no signal</span>
<ul class="tight">
<li><b>A route worked out on the phone, from the pack you already downloaded.</b> Nothing else to fetch. The roads this app is for are the ones with no bars, and until now losing signal meant it could draw exactly where you were and nothing about how to get anywhere.</li>
<li>North Carolina takes about a minute to build once, and a route comes back in about a tenth of a second after that.</li>
<li>Off by default because it is not free: another 300 MB beside the pack, and a minute of the phone's attention.</li>
<li>There is no offline mode to remember to switch on. The server is asked first whenever there is signal, because it knows things the phone cannot &mdash; turn restrictions, which way a one-way runs at a junction, roads closed today. When the bars come back, a route worked out on the device is quietly replaced by the server's.</li>
<li><b>It says which one you are looking at.</b> The route screen runs a progress bar while it works the answer out, then names the source: found on the server, or found on this device. Those are different answers and you should not have to guess which one is on the screen.</li>
</ul>
</div>
</div>

<div class="row">
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
<div class="tile"><b>Waze reports</b><p>Over SABRE, an open Android protocol, through the WzSabre proxy app, the same one JBV1 uses. The proxy owns the Waze side; Touge scrapes nothing. With no proxy installed the feature is simply absent.</p></div>
<div class="tile"><b>Routing server</b><p>Routing goes to my Valhalla box first — well under a second, four states of tiles — with the public FOSSGIS instance behind it as a fallback for anywhere outside them. A list, not one address, because the day the public instance stopped answering it took every device with it. A small monthly subscription (about $3) covers the server. Settings take any Valhalla URL of your own.</p></div>
<div class="tile"><b>Android Auto</b><p>The tablet's map on the head unit: same style and pack, the route, the group's cars with their icons, the radar disc as an inset, heading up. The turn card with lanes and ETA; Skip, Later and Go on when a stop is ahead; Group, Routes, Search and Tires as car screens; tire and radar alerts as car toasts. The phone app does the work and the head unit shows it.</p></div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/settings-traffic.png" alt="Settings: live traffic switch and TomTom key field"><figcaption>Traffic is one switch and a free TomTom key.</figcaption></figure>
<div>
<span class="k">Live traffic</span>
<ul class="tight">
<li>Flow is drawn over the roads, green through red, from TomTom's traffic tiles.</li>
<li><b>Incidents wear the same badges as Waze reports.</b> A TomTom crash is a red crash badge, a jam is an orange traffic badge, and fog, ice, flooding, lane closures, construction and stopped vehicles each get their Waze symbol. A road is only drawn closed when TomTom's category says closed, not whenever the delay is open-ended.</li>
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
<li>One switch for a tablet whose Bluetooth cannot hold the fast connection interval: turn high-throughput off and it uses the slower one it can.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/settings-mapview.png" alt="Settings: map view, with how much road to show at speed, a zoom in when slowing switch, and 3D tilt in degrees"><figcaption>How much road, whether it closes in when you slow, and the tilt in degrees.</figcaption></figure>
<div>
<span class="k">Map view</span>
<ul class="tight">
<li><b>How much road to show at speed</b>, a quarter mile to five. A distance, not a zoom number, so half a mile is half a mile on any panel and at any latitude. The range button on the driving screen steps the same setting.</li>
<li><b>Zoom in when slowing.</b> Coming to a stop closes the map in on the junction, which in 3D is what shows you which way the road actually goes. Off, the range you picked is the range at any speed.</li>
<li><b>3D tilt in degrees</b>, 30 through 60, held at any speed rather than only once you are moving. Sixty is the map engine's limit.</li>
<li><b>Where your car sits</b> down the screen. Running tail, everyone is in front and the screen wants road ahead; running lead, what you want to know is whether you have dropped anyone.</li>
</ul>
</div>
</div>

<div class="row">
<figure><img src="/assets/images/touge/v2/nav-3d.png" alt="The map tilted to sixty degrees, roads running away toward a horizon, weather radar disc top right"><figcaption>Sixty degrees, held at a standstill. The horizon is on screen and the next corner is still in the picture.</figcaption></figure>
<div>
<span class="k">What the tilt buys</span>
<ul class="tight">
<li>Perspective puts the road you are about to drive in the top half of the screen rather than the field beside you.</li>
<li>Flat is one tap away and shows the same amount of road, because the zoom compensates either way.</li>
</ul>
</div>
</div>

<div class="last"></div>

</div>
