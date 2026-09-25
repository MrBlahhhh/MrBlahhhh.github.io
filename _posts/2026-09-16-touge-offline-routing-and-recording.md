---
title: "Touge"
date: 2026-09-16 00:00:00 -0400
last_modified_at: 2026-09-25 00:00:00 -0400
categories: car tech
tags: [touge, android, navigation, offline, maplibre, pmtiles, valhalla, meshtastic, lora, esp-now, heltec, tpms, radar, valentine-one, waze, android-auto, kotlin, compose, openstreetmap, motorcycle, bronco, back-roads]
cover: /assets/images/touge/v3/drive-cover.jpg
lightbox: true
excerpt: "An Android app for back-road drives: offline maps, twisty routes, group tracking over radio or cell, and automatic GPX recording."
article_header:
  type: overlay
  theme: dark
  background_color: "#0B0D10"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .25), rgba(0, 0, 0, .75))"
    src: /assets/images/touge/v3/drive-cover.jpg
---

<style>
.tg{--ink:#e9edf2;--mute:#9aa3ad;--acc:#ff3366;--acc2:#ff8a00;--bg:#0b0d10;--card:#14181e;--line:#232a33}
.tg *{box-sizing:border-box}
.tg .hero{margin:0 0 28px;padding:28px 0 8px;border-bottom:1px solid var(--line)}
.tg .hero h1{font-size:clamp(34px,6vw,64px);line-height:1;margin:0 0 10px;letter-spacing:-.02em}
.tg .hero p{font-size:clamp(17px,2.2vw,21px);color:var(--mute);max-width:52ch;margin:0}
.tg .reel{margin:22px 0 0;background:#000;border:1px solid var(--line);border-radius:16px;overflow:hidden;box-shadow:0 12px 34px rgba(0,0,0,.4)}
.tg .reel video{display:block;width:100%;height:auto;aspect-ratio:16/9;background:#000}
.tg .reel figcaption{background:var(--card)}
.tg .stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:12px;margin:22px 0 0}
.tg .stat{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:14px 16px}
.tg .stat b{display:block;font-size:28px;line-height:1;font-variant-numeric:tabular-nums;color:var(--ink)}
.tg .stat span{color:var(--mute);font-size:13px;letter-spacing:.04em;text-transform:uppercase}
.tg .k{display:inline-block;font-size:12px;letter-spacing:.12em;text-transform:uppercase;color:var(--acc);margin:0 0 6px}
.tg h2{font-size:clamp(26px,3.6vw,40px);letter-spacing:-.01em;margin:44px 0 6px}
.tg .lead{font-size:18px;color:var(--mute);margin:0 0 18px;max-width:64ch}
.tg .row{display:grid;grid-template-columns:1.25fr 1fr;gap:22px;align-items:center;margin:18px 0 26px}
.tg .row.flip{grid-template-columns:1fr 1.25fr}
.tg .row.flip figure{order:2}
.tg .row > *{min-width:0}
.tg figure{margin:0;background:var(--card);border:1px solid var(--line);border-radius:16px;overflow:hidden}
.tg figure img{display:block;width:100%;height:auto}
.tg figure.tall{max-width:380px;width:100%;justify-self:center}
.tg figure.small{max-width:640px;width:100%;justify-self:center}
.tg figcaption{padding:10px 14px;font-size:13px;color:var(--mute);border-top:1px solid var(--line)}
.tg ul.tight{margin:0;padding-left:18px}
.tg ul.tight li{margin:4px 0}
.tg code{word-break:break-word}
.tg .grid3{display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:14px;margin:14px 0 22px}
.tg .grid3.four{grid-template-columns:repeat(auto-fit,minmax(300px,1fr))}
.tg .tile{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:16px}
.tg .tile b{display:block;font-size:17px;margin-bottom:4px;color:var(--ink)}
.tg .tile p{margin:0;color:var(--mute);font-size:15px}
.tg .tile a{color:var(--acc2)}
.tg .quote{border-left:4px solid var(--acc);padding:6px 16px;margin:18px 0;font-size:19px}
.tg .wire{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:16px;margin:14px 0 22px}
.tg .wire svg{width:100%;height:auto;display:block}
.tg .appicon{display:flex;gap:20px;align-items:center;background:var(--card);border:1px solid var(--line);border-radius:16px;padding:18px;margin:14px 0 22px}
.tg .appicon img{width:112px;height:112px;border-radius:26px;flex:none;box-shadow:0 6px 18px rgba(0,0,0,.45)}
.tg .appicon p{margin:0;color:var(--mute)}
.tg .appicon b{color:var(--ink)}
.tg .last{margin-top:56px;padding-top:24px;border-top:1px solid var(--line)}
@media (max-width:820px){.tg .row,.tg .row.flip{grid-template-columns:1fr}.tg .row.flip figure{order:0}}
@media (max-width:480px){.tg .appicon{flex-direction:column;align-items:flex-start}}
</style>

<div class="tg" markdown="1">

<div class="hero">
<span class="k">Android · maps · group drives</span>
<h1>Touge</h1>
<p>I built Touge for back-road drives. It plans twisty routes, works with downloaded maps, records the trip and shows where the rest of the group is, even without cell service when you have radios.</p>
<figure class="reel">
<video controls playsinline preload="metadata" poster="/assets/images/touge/v3/touge-intro-poster.jpg">
<source src="/assets/images/touge/v3/touge-intro.mp4" type="video/mp4">
</video>
<figcaption>An 18-second preview, with sound.</figcaption>
</figure>
<div class="stats">
<div class="stat"><b>1 s</b><span>2.4 GHz update target</span></div>
<div class="stat"><b>32</b><span>radio slots per ride</span></div>
<div class="stat"><b>8</b><span>routes per leg</span></div>
<div class="stat"><b>½ mi</b><span>police corridor</span></div>
<div class="stat"><b>96</b><span>cars and bikes</span></div>
<div class="stat"><b>100 mi</b><span>radar disc</span></div>
<div class="stat"><b>0</b><span>accounts</span></div>
</div>
</div>

These are screenshots from a Samsung tablet and a Moto phone. Map screens show a demo ride with three simulated cars; radio screens show real radios on the bench.

## Ride together

<p class="lead">See who is ahead, who is behind and when each car last reported.</p>

<div class="row">
<figure class="tall"><img src="/assets/images/touge/v3/phone-group.webp" alt="Phone layout leaving Brevard, North Carolina: this car and three others on the blue route line, a turn card, the group card listing Dave, Ana and Kev with distance back, and the status chip reading LoRa and the time" loading="lazy"><figcaption>The phone layout, with the route and group positions.</figcaption></figure>
<div>
<span class="k">Group card</span>
<ul class="tight">
<li>See each car's distance along the route and how old its position is. Off-route cars use straight-line distance.</li>
<li>Cars move smoothly between updates, then stop and fade if updates stop. Voice alerts warn about large gaps and silent cars.</li>
<li>The phone and tablet have the same controls. Background sharing keeps your position updating while you use another app.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v3/report.webp" alt="Tablet in 3D near Boone with the report tray open: Police, Hazard, Crash, Heavy traffic, Road closed and Animal, each with a full-colour icon" loading="lazy"><figcaption>Two taps to report a problem.</figcaption></figure>
<div>
<span class="k">Tap to report</span>
<ul class="tight">
<li>Report police, hazards, crashes, traffic, closures or animals in two taps. Undo is available afterwards.</li>
<li>Reports go to your ride over radio or cell. With WzSabre installed, they also go to Waze.</li>
<li>Nearby duplicates merge into one pin. Reports expire by type and survive an app restart.</li>
</ul>
</div>
</div>

<span class="k">How positions travel</span>
<div class="wire">
<svg viewBox="0 0 900 330" role="img" aria-label="Group positions travel phone to radio over Bluetooth, radio to radio over a 2.4 GHz lane about once a second per car and over LoRa every five seconds, with the server over cell as the fallback">
<defs>
<marker id="pg" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#2e9e4f"/></marker>
<marker id="pb" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#1e88e5"/></marker>
<marker id="po" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#e08a1e"/></marker>
<marker id="pw" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#9aa3ad"/></marker>
</defs>
<rect x="0" y="0" width="900" height="330" fill="#14181e"/>
<g font-family="system-ui,Segoe UI,Roboto,sans-serif" font-size="14" fill="#e9edf2">
<rect x="15" y="40" width="150" height="70" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="90" y="68" text-anchor="middle">Your phone</text><text x="90" y="90" text-anchor="middle" fill="#9aa3ad" font-size="12">each new GPS fix</text>
<rect x="215" y="40" width="150" height="70" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="290" y="68" text-anchor="middle">Your radio</text><text x="290" y="90" text-anchor="middle" fill="#9aa3ad" font-size="12">ESP32-S3 + SX1262</text>
<rect x="535" y="40" width="150" height="70" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="610" y="68" text-anchor="middle">Their radio</text><text x="610" y="90" text-anchor="middle" fill="#9aa3ad" font-size="12">relays 2 hops / 3 hops</text>
<rect x="735" y="40" width="150" height="70" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="810" y="68" text-anchor="middle">Their phone</text><text x="810" y="90" text-anchor="middle" fill="#9aa3ad" font-size="12">map, card, chat</text>
<line x1="165" y1="75" x2="213" y2="75" stroke="#9aa3ad" stroke-width="3" marker-end="url(#pw)"/><text x="189" y="64" text-anchor="middle" fill="#9aa3ad" font-size="11">BLE</text>
<line x1="685" y1="75" x2="733" y2="75" stroke="#9aa3ad" stroke-width="3" marker-end="url(#pw)"/><text x="709" y="64" text-anchor="middle" fill="#9aa3ad" font-size="11">BLE</text><text x="709" y="96" text-anchor="middle" fill="#9aa3ad" font-size="10">batched</text>
<line x1="365" y1="58" x2="533" y2="58" stroke="#2e9e4f" stroke-width="4" marker-end="url(#pg)"/>
<text x="450" y="18" text-anchor="middle" fill="#2e9e4f" font-size="12">2.4 GHz · about 1 new fix/s per car</text>
<text x="450" y="34" text-anchor="middle" fill="#9aa3ad" font-size="11">ESP-NOW · 32 slots · line of sight</text>
<line x1="365" y1="92" x2="533" y2="92" stroke="#1e88e5" stroke-width="3" stroke-dasharray="8 6" marker-end="url(#pb)"/>
<text x="450" y="112" text-anchor="middle" fill="#1e88e5" font-size="12">LoRa · 5 s target</text>
<text x="450" y="127" text-anchor="middle" fill="#9aa3ad" font-size="11">915 MHz · longer range</text>
<rect x="360" y="205" width="180" height="62" rx="12" fill="#0b0d10" stroke="#232a33"/><text x="450" y="231" text-anchor="middle">Your server</text><text x="450" y="252" text-anchor="middle" fill="#9aa3ad" font-size="12">signed, no accounts</text>
<path d="M90,110 C90,236 220,236 358,236" fill="none" stroke="#e08a1e" stroke-width="3" marker-end="url(#po)"/>
<path d="M542,236 C690,236 810,236 810,112" fill="none" stroke="#e08a1e" stroke-width="3" marker-end="url(#po)"/>
<text x="450" y="178" text-anchor="middle" fill="#9aa3ad" font-size="12">fresh radio positions preferred; cell fills gaps</text>
<text x="120" y="292" fill="#e08a1e" font-size="12">cell: every 5 s with no radio</text>
<text x="120" y="310" fill="#e08a1e" font-size="12">every 30 s with one, to find a lost car</text>
<text x="580" y="292" fill="#9aa3ad" font-size="12">colours match the icons on the group card:</text>
<text x="580" y="310" font-size="12"><tspan fill="#2e9e4f">green 2.4 GHz</tspan><tspan fill="#9aa3ad"> · </tspan><tspan fill="#1e88e5">blue LoRa</tspan><tspan fill="#9aa3ad"> · </tspan><tspan fill="#e08a1e">amber server</tspan></text>
</g>
</svg>
</div>

<p class="lead">Bluetooth connects the phone to its radio. Between cars, Touge uses 2.4 GHz, LoRa and cell.</p>

<ul class="tight">
<li><b>Bluetooth:</b> connects each phone to its radio. Incoming car positions are grouped into batches to reduce reads.</li>
<li><b>2.4 GHz:</b> Heltec V3 and V4 radios with Touge firmware share positions over ESP-NOW, targeting one update per second per car. They can relay through other cars.</li>
<li><b>Group size:</b> the schedule has 32 slots. Simulations cover 25 to 29 cars, including joins, reboots and groups merging. Small groups can send up to four times a second when fresh GPS fixes are available.</li>
<li><b>LoRa:</b> provides more range and up to three relay hops. The default target is five seconds; larger groups need longer intervals.</li>
<li><b>Cell:</b> shares positions through the ride server when radios are unavailable. With radios connected, periodic server checks help find cars outside radio range.</li>
<li>Updates use the ride's key. The app prefers fresh radio positions and rejects older copies from other links.</li>
</ul>

<div class="row">
<figure class="small"><img src="/assets/images/touge/v3/chat-lanes.webp" alt="Chat screen top: the ride name, then the 2.4 GHz lane card reading channel 6, 1 car on it, slot 0, knows 2 and its clock source, and one row for mattsam reading 2.4, 0.7 Hz, -92 dBm" loading="lazy"><figcaption>Radio status and per-car signal readings from the bench test.</figcaption></figure>
<div>
<span class="k">Signal, on the chat screen</span>
<ul class="tight">
<li>The chat screen shows the 2.4 GHz channel, slot and nearby cars. A missing radio status report turns the card red.</li>
<li>Each car shows its active link, update rate and signal strength. LoRa also shows signal-to-noise ratio.</li>
<li>Colours show signal quality. Red names warn that a packet used the full hop allowance. The Range screen explains the readings.</li>
<li>Messages show whether they arrived directly, through relays or through the server.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure class="small"><img src="/assets/images/touge/v3/group-links.webp" alt="Group card with two cars: mattsam with a green antenna, 40 ft, now, and mattpixel with amber cell bars, 60 ft, 15 seconds" loading="lazy"><figcaption>Green: 2.4 GHz. Amber: server. Each position shows its age.</figcaption></figure>
<div>
<span class="k">Colours on the map and the card</span>
<ul class="tight">
<li>Green antennas mean 2.4 GHz, blue antennas mean LoRa, and amber bars mean the server.</li>
<li>Each car uses its rider's chosen model and colour. Old positions fade and show their age.</li>
</ul>
</div>
</div>

<div class="row">
<figure><img src="/assets/images/touge/v3/link-diagnostics.webp" alt="Link diagnostics: phone reads 7.2 a second, positions 1.2 a second in 1.2 batches, 0 batches missed, writes 1.4 a second with 0 lost, per car 12a9 on 2.4 GHz at 1.2 Hz, radio 2.4 counters, radio LoRa counters, phone queue, and radio dropped 0 phone writes" loading="lazy"><figcaption>Live counters in Setup › Group &amp; radio › Advanced.</figcaption></figure>
<div>
<span class="k">Link diagnostics</span>
<ul class="tight">
<li>Link diagnostics shows Bluetooth reads, position updates, batches, dropped writes and each car's update rate.</li>
<li>A write is counted as dropped when the radio identifies it. The same counters are logged every five seconds.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure class="tall"><img src="/assets/images/touge/v3/invite.webp" alt="Ride screen on the phone: ride name thurs, Start a new ride, Lock everyone out, Share link, and the ride's QR code, blurred here" loading="lazy"><figcaption>The QR code contains the ride key, so it is blurred here.</figcaption></figure>
<div>
<span class="k">Start or join a ride</span>
<ul class="tight">
<li>Start a ride, then invite people by QR code, link or radio. Each ride has its own key.</li>
<li>A separate QR code configures stock Meshtastic radios for the ride's channel.</li>
<li><b>Lock everyone out</b> replaces the key and invalidates old invites.</li>
<li><b>Send to group</b> shares your stops and points along your chosen route so the group follows the same roads.</li>
</ul>
</div>
</div>

<div class="row">
<figure><img src="/assets/images/touge/v3/icons.webp" alt="Profile screen: name, seventeen colour swatches, a search box and a grid of bikes: generic motorcycle, adventure bike, Ninja, R 1250 GS, R 1250 RT, Street Glide, Sportster, 690, Tracer 9 GT, Spyder RT, Ryker and Slingshot" loading="lazy"><figcaption>Choose from 96 car and bike icons.</figcaption></figure>
<div>
<span class="k">Your car</span>
<ul class="tight">
<li>Choose from 96 car and bike icons, including 13 bikes and trikes.</li>
<li>Search by make or model, then pick a colour. The preview shows what others will see on their map.</li>
</ul>
</div>
</div>

### The radio

<div class="row flip">
<figure><img src="/assets/images/touge/v3/group-radio.webp" alt="Setup, Group and radio: Bluetooth to the radio reading Radio ready, Meshtastic radio, Group radio chat and SOS, Done with this radio, 2.4 GHz lane seen every 1.0 s from mattmoto, LoRa relay seen every 5.8 s" loading="lazy"><figcaption>This bench test: 2.4 GHz updates every 1.0 s; LoRa every 5.8 s.</figcaption></figure>
<div>
<span class="k">Standard Meshtastic on LoRa</span>
<ul class="tight">
<li>Select a radio and pair using the PIN on its screen.</li>
<li><b>Configure this radio for the ride</b> sets the region, speed, three-hop limit, owner name and encrypted ride channel.</li>
<li>Choose Turbo, Short, Medium or Long. Short is the default; slower settings trade airtime for range. Larger groups and relays need more airtime.</li>
<li><b>Done with this radio</b> leaves the ride, restores the stock name and channel, and unpairs it.</li>
<li>Hold the radio's button for four seconds to shut it down.</li>
<li>Stock Meshtastic nodes on the same channel can exchange LoRa positions. Touge firmware adds the 2.4 GHz link.</li>
</ul>
</div>
</div>

<div class="grid3 four">
<div class="tile"><b>Radio hardware</b><p>Touge supports the Heltec V3, V4 and Meshnology N30. For a V4, check whether the listing is the 22 or 28 dBm version and which antenna connections it includes.</p></div>
<div class="tile"><b>Install the firmware</b><p>Use the <a href="https://mrblahhhh.github.io/Touge-mesh-firmware/">web flasher</a> in Chrome or Edge over USB. Update preserves settings; Fresh install clears them. The firmware source is on <a href="https://github.com/MrBlahhhh/Touge-mesh-firmware">GitHub</a>.</p></div>
<div class="tile"><b>Radio GPS</b><p>A V4 with the GNSS expansion module can supply location to a tablet without GPS. Touge uses it when the device has no good fix.</p></div>
<div class="tile"><b>Other radios</b><p>Other compatible Meshtastic radios, including the T1000-E, provide LoRa. The 2.4 GHz link requires a supported V3 or V4 running Touge firmware.</p></div>
</div>

### Getting everyone in

<p class="lead">First setup covers your profile, the ride and offline maps.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/wizard-ride.png" alt="Setup wizard step two: Ride together, with buttons to start a ride or join someone else's" loading="lazy"><figcaption>Start or join a ride during setup.</figcaption></figure>
<div>
<span class="k">First run</span>
<ul class="tight">
<li>Set your name, vehicle and colour.</li>
<li>Start or join a ride by link, QR code or radio invite. <b>Advertise</b> sends invites every 45 seconds for up to 15 minutes; anyone in radio range can join while it's running.</li>
<li>Download maps if you want them offline. You can skip any setup step.</li>
</ul>
</div>
</div>

## Choose a route

<p class="lead">Compare travel time with how twisty each route is.</p>

<div class="row flip">
<figure><img src="/assets/images/touge/v3/routes.webp" alt="Boone to Asheville, North Carolina: nine numbered routes in their own colours, each labelled with time and twist score, route 1 timed on live traffic and route 4 marked surface unknown" loading="lazy"><figcaption>Boone to Asheville: eight alternatives plus a live-traffic option.</figcaption></figure>
<div>
<span class="k">Route choice</span>
<ul class="tight">
<li>Compare up to eight routes per leg, from highways to back roads. Each has a colour, number, travel time and TWIST score.</li>
<li>The TWIST score uses bends, corner radius and junctions along the route.</li>
<li>For example, Flat Top Road and the Blue Ridge Parkway score 24 at 2 h 09. US 221 scores 9 at 1 h 31.</li>
<li>Routes come from Valhalla using your driving preferences. Tap a destination name to change it.</li>
</ul>
</div>
</div>

<div class="row">
<figure class="tall"><img src="/assets/images/touge/v3/phone-routes.webp" alt="Phone route choice out of Brevard: eight numbered routes in eight colours around Asheville and Marion, with times and twist scores" loading="lazy"><figcaption>Phone route choices, sorted by TWIST score.</figcaption></figure>
<div>
<span class="k">What's on the road</span>
<ul class="tight">
<li>Each route shows paved, unpaved and unknown surfaces. Gravel is marked brown, with a warning before starting in a road-focused mode.</li>
<li>Live traffic adds another route option. Known closures are marked red and ranked last.</li>
<li>Missing surface data is labelled unknown.</li>
</ul>
</div>
</div>

## Highway out, back roads home

<p class="lead">Use highways to get there, back roads for the ride and the quickest way home.</p>

<div class="row flip">
<figure><img src="/assets/images/touge/v3/trip.webp" alt="Trip screen starting at Boone with one stop, Asheville, the Back roads style chosen, and nine timed options for the leg with road names and twist scores" loading="lazy"><figcaption>Choose a driving style for each leg.</figcaption></figure>
<div>
<span class="k">Trips</span>
<ul class="tight">
<li>Choose Highway, Mixed, Back roads or Twistiest for each leg.</li>
<li>Start from your current location, Home or any place you choose.</li>
</ul>
</div>
</div>

## Edit a route

<p class="lead">Pick the roads you want by adding points on the map.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/editor.png" alt="Route editor with three numbered pins dropped on back roads near Sparta and the routed line running through them" loading="lazy"><figcaption>Drag points onto the roads you want to take.</figcaption></figure>
<div>
<span class="k">Route editor</span>
<ul class="tight">
<li>Long-press to add a stop or shaping point. Drag it to another road, or tap to edit or remove it.</li>
<li>Reverse the route or close it into a loop.</li>
<li>Save it as GPX or start navigation.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/library.png" alt="Rides and routes library listing recorded rides, each with Follow, Route it, share and delete" loading="lazy"><figcaption>Recorded rides and imported GPX files.</figcaption></figure>
<div>
<span class="k">Rides and routes</span>
<ul class="tight">
<li><b>Follow</b> follows the recorded line, including roads missing from the map.</li>
<li><b>Route it</b> calculates a navigable route with road names, lane guidance and rerouting.</li>
<li>Import and share GPX files. The app keeps the latest 30 recordings; imported files stay until you delete them.</li>
</ul>
</div>
</div>

## Drive it

<div class="row">
<figure><img src="/assets/images/touge/v3/drive.webp" alt="Tablet in 3D on Flat Top Road south of Boone: turn card, group card with Dave, Ana and Kev behind, weather radar disc, status chip, Waze chip, the blue route winding through the hills, and the time, distance and twist strip" loading="lazy"><figcaption>The tablet layout during the demo ride near Boone.</figcaption></figure>
<div>
<span class="k">Turn by turn</span>
<ul class="tight">
<li>Turn guidance includes lane arrows where map data is available. Spoken directions lower the music volume briefly.</li>
<li>The map uses your direction of travel, so moving the phone at a stop doesn't rotate it.</li>
<li>Speed limits work offline with a route. Without a route, the app asks the server for the road's limit.</li>
<li>ETA, remaining distance and TWIST score update as you drive.</li>
</ul>
</div>
</div>

## Off the route

<p class="lead">Miss a turn and you can rejoin your planned route or choose a new one.</p>

The prompt appears when you are at least a quarter mile off route, travelling at 10 mph or more, with a destination ahead.

<div class="grid3">
<div class="tile"><b>Back to my route</b><p>After a 30-second countdown, Touge connects you to the route ahead and keeps your remaining stops.</p></div>
<div class="tile"><b>New route to Asheville</b><p>Recalculate to the destination, keeping your preferences for each remaining leg.</p></div>
<div class="tile"><b>Keep going</b><p>Dismiss the prompt. It returns after ten seconds if you are still well off route.</p></div>
</div>

<ul class="tight">
<li>Rejoining keeps the remaining route and stops, checks known closures, and uses that leg's driving style.</li>
<li>If no open connection is available, Touge offers a new route.</li>
</ul>

## Stops

<p class="lead">A card appears two miles before a stop with three choices.</p>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/stop-ahead.png" alt="Stop ahead card 1.3 miles out with Skip, Later and Go on" loading="lazy"><figcaption>Skip, postpone or keep the next stop.</figcaption></figure>
<div>
<span class="k">Stop ahead</span>
<ul class="tight">
<li><b>Skip</b> removes the stop and continues to the next one.</li>
<li><b>Later</b> moves it to the end of the trip.</li>
<li><b>Go on</b> keeps it. Ignore the card to continue as planned.</li>
</ul>
</div>
</div>

<div class="grid3 four">
<div class="tile"><b>Silent</b><p>On-screen directions only.</p></div>
<div class="tile"><b>Calm</b><p>Standard spoken directions.</p></div>
<div class="tile"><b>Brief</b><p>Just the direction and road name.</p></div>
<div class="tile"><b>Touge</b><p>Short, shouted Japanese corner calls. Requires a Japanese voice installed.</p></div>
</div>

## Weather and road reports

<p class="lead">Weather radar and reports of police and hazards ahead.</p>

<div class="row">
<figure class="small"><img src="/assets/images/touge/v3/corner.webp" alt="Top right corner of the tablet: status chip reading LoRa, 9:28, wifi and 100 percent, the Waze chip reading just now and 77 reports, and the 100 mile weather radar disc over the hills near Boone" loading="lazy"><figcaption>Connection status, Waze updates and weather radar.</figcaption></figure>
<div>
<span class="k">The top corner</span>
<ul class="tight">
<li>The status chip shows the active radio link, time, signal and battery. <code>GPS: radio</code> means the radio is supplying your location.</li>
<li>The Waze chip shows the last update and report count. It changes colour when updates are late; tap it to browse reports.</li>
<li>The weather radar shows NEXRAD within 100 miles, with distance rings. It refreshes every five minutes or five miles, and supports a custom tile feed.</li>
</ul>
</div>
</div>

<div class="grid3">
<div class="tile"><b>Police ahead</b><p>Police alerts are filtered to within half a mile of the route or road ahead.</p></div>
<div class="tile"><b>Alert banner</b><p>See the road, distance, estimated arrival time, report age and confirmations.</p></div>
<div class="tile"><b>Nearby reports</b><p>The three nearest relevant police reports appear below the weather radar.</p></div>
<div class="tile"><b>Hazard warnings</b><p>Spoken warnings are filtered to your road and, when a heading is supplied, your direction of travel.</p></div>
<div class="tile"><b>Report age</b><p>Pins show their type and age, fade over time and expire according to the report type.</p></div>
<div class="tile"><b>Report details</b><p>Tap a pin for distance, age and confirmations.</p></div>
</div>

## Alerts you control

<div class="row flip">
<figure><img src="/assets/images/touge/v2/alerts.png" alt="Alerts screen: one row per kind with a map toggle and a voice toggle" loading="lazy"><figcaption>Separate map and voice switches for each alert type.</figcaption></figure>
<div>
<span class="k">Map and voice, separately</span>
<ul class="tight">
<li>Each report type has separate map and voice switches on one screen.</li>
<li>Choose a warning distance from half a mile to two miles, with at least 20 seconds of notice at speed.</li>
<li>An optional chime comes before speech. Silent mode disables spoken alerts.</li>
</ul>
</div>
</div>

## Valentine One Gen 2

<p class="lead">Show detector alerts on the map and change its logic mode automatically.</p>

<div class="row">
<figure><img src="/assets/images/touge/v3/v1-explainer.webp" alt="Setup, Devices and sensors: the Valentine One Gen 2 switch and the How the V1 is driven explainer covering the three modes, when Touge switches, what it mutes on top, and the card" loading="lazy"><figcaption>Valentine One controls and mode explanations.</figcaption></figure>
<div>
<span class="k">Valentine One</span>
<ul class="tight">
<li>Connect a Valentine One Gen 2 over Bluetooth to show signal direction, band, frequency and strength on the map.</li>
<li>Touge selects Advanced Logic (<b>L</b>) in dense urban areas and Logic (<b>l</b>) elsewhere. A manual change on the detector lasts until Touge changes modes or reconnects.</li>
<li>Extra muting covers saved false alerts, weak signals and speeds less than 10 mph over a known limit. X/K muting in town is optional.</li>
<li>Close JBV1 first: the detector connects to one app at a time.</li>
</ul>
</div>
</div>

## Tire pressure

<p class="lead">Watch tire pressure and temperature, with alerts for leaks.</p>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/tires.png" alt="Tires screen with four wheel tiles; rear left is leaking" loading="lazy"><figcaption>Pressure and leak warnings for each tire.</figcaption></figure>
<div>
<span class="k">Tire pressure</span>
<ul class="tight">
<li>Assign supported Bluetooth sensors to each wheel: Zeepin/TPMSII, DJTPMS or Tesla.</li>
<li>Alerts cover pressure below 20 psi, a 2 psi/minute leak, a 10 psi drop from peak pressure, or temperatures above 158°F.</li>
<li>A red warning strip and spoken alert identify the tire.</li>
</ul>
</div>
</div>

## Maps, search, and your own server

<div class="row">
<figure><img src="/assets/images/touge/v2/packs.png" alt="Map packs screen listing regions with measured sizes and installed state" loading="lazy"><figcaption>This North Carolina pack is 392 MB.</figcaption></figure>
<div>
<span class="k">Map packs</span>
<ul class="tight">
<li>Download a state, a region such as Blue Ridge, an area around you or a corridor along your trip.</li>
<li>The app calculates the download size first. Multiple packs display together and share an offline search index.</li>
<li>Downloaded maps don't expire.</li>
</ul>
</div>
</div>

<div class="grid3">
<div class="tile"><b>Offline routing</b><p>Enable on-device routing to calculate routes without signal. It needs extra storage: about 300 MB for North Carolina. Touge uses the server when available and labels the route source.</p></div>
<div class="tile"><b>Live traffic</b><p>TomTom supplies traffic colours and incidents, refreshed every two minutes. Add a TomTom API key in settings. Duplicate closures from TomTom and Waze merge.</p></div>
<div class="tile"><b>Off-road mode</b><p>Routes on forest roads and tracks, including offline. It records your trail and lets you leave the planned route without reroute prompts.</p></div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/search.png" alt="Search results for Sparta, the town first" loading="lazy"><figcaption>Search results for Sparta.</figcaption></figure>
<div>
<span class="k">Search</span>
<ul class="tight">
<li>Search all downloaded packs, including places outside the current map view.</li>
<li>Fuel, food, coffee and viewpoints have shortcuts. Along a route, results are ordered by when you'll reach them.</li>
<li>Recent and starred places appear before you type. Food search filters chains and checks opening hours when available.</li>
</ul>
</div>
</div>

<div class="grid3 four">
<div class="tile"><b>Ride recording</b><p>Rides are recorded automatically as GPX and saved in Rides and routes.</p></div>
<div class="tile"><b>Waze reports</b><p>Install the WzSabre proxy app for Waze reports through SABRE, the same connection used by JBV1.</p></div>
<div class="tile"><b>Routing server</b><p>My Valhalla server covers four states, with FOSSGIS as a fallback elsewhere. A subscription of about $3/month covers the server, or you can enter your own Valhalla URL.</p></div>
<div class="tile"><b>Android Auto</b><p>Show navigation, group positions, weather and relevant police reports on the head unit. Group, Routes, Search and Tires are available as car screens.</p></div>
</div>

## The settings screen

<p class="lead">Seven categories, searchable settings and a summary of each category.</p>

<div class="row">
<figure><img src="/assets/images/touge/v3/settings.webp" alt="Setup on the tablet: the You card, then seven categories with live summaries: Maps and navigation, Group and radio, Sound and alerts, Display and layout, Devices and sensors, Recording, App and data; Maps and navigation open on the right" loading="lazy"><figcaption>Settings grouped into seven categories.</figcaption></figure>
<div>
<span class="k">Categories</span>
<ul class="tight">
<li>Settings are grouped into navigation, group and radio, sound, display, devices, recording, and app data.</li>
<li>Each category shows its current setup and any missing configuration. Less-used controls sit under Advanced.</li>
<li>Server addresses and keys have Save and Cancel buttons. Backup and restore transfers settings, including the ride key, to another device.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v3/settings-search.webp" alt="Setup search for radio, listing Bluetooth to the radio, Server check-in while on the radio, Start or join a ride, Share my position, Keep sharing in the background, 2.4 GHz lane, LoRa relay, Send my position every, and Cellular fallback, each with its category and current value" loading="lazy"><figcaption>Search results for "radio", with current values.</figcaption></figure>
<div>
<span class="k">Search</span>
<ul class="tight">
<li>Search by setting name, category or current value.</li>
<li>Results show where the setting lives and what it's set to.</li>
</ul>
</div>
</div>

<div class="grid3 four">
<div class="tile"><b>Map distance</b><p>Choose how much road is visible, from a quarter mile to five miles.</p></div>
<div class="tile"><b>3D tilt</b><p>Set the tilt from 30 to 60 degrees, or switch to a flat map.</p></div>
<div class="tile"><b>Zoom when slowing</b><p>Automatically zoom closer as you approach a junction or stop.</p></div>
<div class="tile"><b>Car position</b><p>Move your car on the screen to leave more room ahead or behind.</p></div>
</div>

<div class="appicon">
<img src="/assets/images/touge/v3/app-icon.svg" alt="Touge app icon: an orange hairpin road with a dashed centre line, a white car with its headlights on carving the top bend, under a navy night sky with a moon and stars">
<p><b>The Touge icon:</b> a car on a mountain switchback at night.</p>
</div>

<div class="last"></div>

</div>
