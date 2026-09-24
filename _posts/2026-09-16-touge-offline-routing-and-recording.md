---
title: "Touge"
date: 2026-09-16 00:00:00 -0400
last_modified_at: 2026-09-24 10:05:00 -0400
categories: car tech
tags: [touge, android, navigation, offline, maplibre, pmtiles, valhalla, meshtastic, lora, esp-now, heltec, tpms, radar, valentine-one, waze, android-auto, kotlin, compose, openstreetmap, motorcycle, bronco, back-roads]
cover: /assets/images/touge/v3/drive-cover.jpg
lightbox: true
excerpt: "The whole group on one map with no signal, car to car over its own radios about once a second. Eight ways to anywhere, ranked by corners. Police only when they're on your road, and a Valentine One on the screen."
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
<span class="k">Android · offline · for the pass</span>
<h1>Touge</h1>
<p>Keeps the whole group on one map when the bars run out, car to car over its own radios. Then it plans the twisty way there and only warns you about police who are actually on your road. A paid app, with an optional small monthly subscription for routing on my server.</p>
<figure class="reel">
<video controls playsinline preload="metadata" poster="/assets/images/touge/v3/touge-intro-poster.jpg">
<source src="/assets/images/touge/v3/touge-intro.mp4" type="video/mp4">
</video>
<figcaption>Eighteen seconds, sound on.</figcaption>
</figure>
<div class="stats">
<div class="stat"><b>1 s</b><span>car to car on 2.4 GHz</span></div>
<div class="stat"><b>32</b><span>radio slots per ride</span></div>
<div class="stat"><b>8</b><span>routes per leg</span></div>
<div class="stat"><b>½ mi</b><span>police corridor</span></div>
<div class="stat"><b>96</b><span>cars and bikes</span></div>
<div class="stat"><b>100 mi</b><span>radar disc</span></div>
<div class="stat"><b>0</b><span>accounts</span></div>
<div class="stat"><b>1,101</b><span>automated tests</span></div>
</div>
</div>

Every screenshot below is the current app on a Samsung tablet and a moto phone. The map screens use the app's demo ride, which drives this car and three made-up cars (Dave, Ana and Kev) along a real route, so the DEMO badge is on. The radio screens are three real radios on the bench. Nothing is a mockup and nothing is a render.

## Ride together

<p class="lead">A group spread over eight miles of ridge road isn't a question of pins on a map. It's who, how far, and how old that fix is.</p>

<div class="row">
<figure class="tall"><img src="/assets/images/touge/v3/phone-group.webp" alt="Phone layout leaving Brevard, North Carolina: this car and three others on the blue route line, a turn card, the group card listing Dave, Ana and Kev with distance back, and the status chip reading LoRa and the time" loading="lazy"><figcaption>Leaving Brevard on the phone layout. Every car in its own model and colour, the group card underneath.</figcaption></figure>
<div>
<span class="k">Group card</span>
<ul class="tight">
<li><b>Gap is measured along the road.</b> On a switchback the car two hairpins back is 1,000 feet away and four minutes behind, and the card says four minutes.</li>
<li><b>Age is the age of the fix</b>, not of the packet. A car that stops reporting greys out after 30 seconds at the default rate and stays on the card with its age counting up.</li>
<li>A car more than about 400 feet off the shared route falls back to straight-line distance.</li>
<li>The worst gap and any silent car are spoken.</li>
<li><b>No teleporting.</b> Between fixes each car is carried along the route at its last speed, and when the real fix lands the icon eases onto it in about a third of a second. The guess stops after ten seconds on the route and three off it. Past that the car sits where it was last heard and its age counts up, because that's the truth.</li>
<li><b>The phone gets the same buttons as the tablet,</b> in one column, with a More menu for the rest. Background sharing keeps your position going out with the app closed, under one notification.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v3/report.webp" alt="Tablet in 3D near Boone with the report tray open: Police, Hazard, Crash, Heavy traffic, Road closed and Animal, each with a full-colour icon" loading="lazy"><figcaption>One tap opens six targets. The second files it and closes.</figcaption></figure>
<div>
<span class="k">Tap to report</span>
<ul class="tight">
<li><b>Two taps, not five.</b> Waze asks for a category, then a subtype, then a confirmation, with small targets. Here every target is 96 dp with a full-colour icon and one or two words. No subtype. The undo lives in the toast that follows, not in a dialog before it.</li>
<li><b>It goes to the group.</b> A report rides the same authenticated exchange as the positions, so it reaches everyone on the ride over radio or cell. The report from the car 400 yards ahead is the one that matters on a back road.</li>
<li><b>And out to the road.</b> With the WzSabre proxy installed, the same tap also files it to Waze, so it reaches drivers who aren't on your ride. Every kind goes, animals included: a deer on the shoulder is filed as Waze's own animal hazard. Taking from a crowd-sourced feed without ever adding to it is a poor way to use one.</li>
<li><b>Duplicates collapse.</b> The same kind within about 500 feet is the same thing seen twice, so the car behind filing the same speed trap is one pin.</li>
<li>Each expires on its own clock: police at 25 minutes, a closed road at six hours. They survive a restart mid-ride.</li>
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
<text x="450" y="112" text-anchor="middle" fill="#1e88e5" font-size="12">LoRa · every 5 s</text>
<text x="450" y="127" text-anchor="middle" fill="#9aa3ad" font-size="11">915 MHz, over the ridge</text>
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
<li><b>Phone to radio is Bluetooth.</b> Touge talks Meshtastic's own BLE service and protobufs. Each new GPS fix goes to its own radio within a quarter of a second, addressed to the radio itself, so it costs no airtime. Coming back, the radio packs up to nine cars into one Bluetooth read, keeps only the newest position per car, and names any write it had to drop, so a lost fix is counted, not guessed.</li>
<li><b>2.4 GHz, the fast lane.</b> On a Heltec V3 or V4 running the Touge firmware, the ESP32's own WiFi radio carries positions over ESP-NOW in long-range mode (250 kbit/s) on channel 1, 6 or 11. Range is roughly what you can see, with two hops to reach the back of a strung-out group. Encrypted with a key derived from the ride's key, and signed.</li>
<li><b>Built for a big group.</b> A one-second schedule is cut into 32 slots of 27 ms, and each car leases one. Every frame carries the slot map, so two cars that land on the same slot notice and one moves, with no coordinator to lose. In simulation a car park of 25 to 29 cars settles, and so do joins, drops, reboots and two groups merging. Each car sends about one new position a second, which is as often as most phone GPS makes one. A small group gets up to four slots per car per second, so a new fix doesn't wait for its turn: in the schedule simulations it's on the air about 70 ms after it's taken with three cars, and about half a second on a full ride. A GNSS that updates faster than once a second gets all four.</li>
<li><b>LoRa, the long lane.</b> The same position goes out over 915 MHz every 5 seconds. It reaches further and gets over terrain where 2.4 GHz can't, relays up to three hops, and the gap grows with the group so the channel stays under 30% busy (capped at 20 seconds).</li>
<li><b>Cell, the fallback.</b> With no radio, the fix goes to <code>server/convoy.py</code> beside Valhalla every 5 seconds, signed with HMAC-SHA256 under the ride key. No database, no accounts, positions pruned after thirty minutes, plain http refused. With a radio, the server is still asked every 30 seconds, and fully once the radio has heard nobody for 30 seconds, so a car that drove out of radio range is found from either side.</li>
<li><b>The radio outranks the server.</b> Every fix carries the time it was measured, so a late cell packet never overwrites a newer radio one. A server copy has to be five seconds newer before it takes a car off the radio, which stops a car sitting two lengths ahead from flipping between sources on timestamp noise.</li>
<li><b>Name, colour and car icon travel with the fix</b> on every link, so what you set is what the others see.</li>
<li><b>The LoRa and cell rate is a setting,</b> 1 second to a minute, 5 by default. On cell it's what you get. On LoRa it's a request: the air decides. The 2.4 GHz lane runs on its own schedule.</li>
</ul>

<div class="row">
<figure class="small"><img src="/assets/images/touge/v3/chat-lanes.webp" alt="Chat screen top: the ride name, then the 2.4 GHz lane card reading channel 6, 1 car on it, slot 0, knows 2 and its clock source, and one row for mattsam reading 2.4, 0.7 Hz, -92 dBm" loading="lazy"><figcaption>The chat screen with two radios on the bench. The fast lane's own report on top, then one row per car.</figcaption></figure>
<div>
<span class="k">Signal, on the chat screen</span>
<ul class="tight">
<li><b>The lane card</b> is the radio's own account of its 2.4 GHz side: which channel it settled on, how many cars it can hear on it, which slot it holds, how many cars it knows, and what its clock is locked to. If the report stops the card turns red and says so, because a silent fast lane otherwise looks exactly like a quiet road.</li>
<li><b>One row per car, one lane per row.</b> Only the lane carrying that car right now is shown: <code>2.4 1.0 Hz -48 dBm</code> while the fast lane is live, otherwise <code>LoRa 13 dB SNR -62 dBm</code>. Heard on neither lately, it greys out to its last-heard time.</li>
<li><b>Your own car isn't listed.</b> The radio hands back your own positions too, and a row saying how well you hear yourself is noise.</li>
<li><b>The colour is the signal strength.</b> Green when it's strong, through yellow and orange to red. A car's name turns red when its packets arrive having used every hop: nothing is left in the budget and the next ridge drops it.</li>
<li><b>What the numbers mean.</b> -90 dBm is a healthy LoRa link and a dead 2.4 GHz one, so they don't read the same. On 2.4 GHz, -67 or better is good, -75 is okay, -85 is weak. LoRa decodes below the noise floor: -100 is still good and -120 is the edge. SNR is LoRa only: 5 dB or more is excellent, zero is fine, under -10 is about to drop. The Range button reads it out in words per lane.</li>
<li><b>Messages carry their route too.</b> Each one says "direct", "3 hops" or "server".</li>
</ul>
</div>
</div>

<div class="row flip">
<figure class="small"><img src="/assets/images/touge/v3/group-links.webp" alt="Group card with two cars: mattsam with a green antenna, 40 ft, now, and mattpixel with amber cell bars, 60 ft, 15 seconds" loading="lazy"><figcaption>mattsam over 2.4 GHz, now. mattpixel over the server, 15 seconds old.</figcaption></figure>
<div>
<span class="k">Colours on the map and the card</span>
<ul class="tight">
<li><b>The icon beside each car says which link it came over.</b> A green antenna is the 2.4 GHz lane, a blue antenna is LoRa, amber bars are the server. Green reads as the good one and amber as the fallback, which is the order they rank in.</li>
<li><b>On the map</b> each car is drawn as its own model in its rider's colour, with its name in that colour. A car that goes silent fades.</li>
<li><b>On the compact card</b> a name chip stays plain while it's fresh. From 20 seconds old it tints orange and shows its age, heading to red by two minutes.</li>
</ul>
</div>
</div>

<div class="row">
<figure><img src="/assets/images/touge/v3/link-diagnostics.webp" alt="Link diagnostics: phone reads 7.2 a second, positions 1.2 a second in 1.2 batches, 0 batches missed, writes 1.4 a second with 0 lost, per car 12a9 on 2.4 GHz at 1.2 Hz, radio 2.4 counters, radio LoRa counters, phone queue, and radio dropped 0 phone writes" loading="lazy"><figcaption>Setup › Group &amp; radio › Advanced. Live counters from both ends of the Bluetooth link.</figcaption></figure>
<div>
<span class="k">Link diagnostics</span>
<ul class="tight">
<li><b>Measured, not claimed.</b> The phone and the radio both count every lane and every queue, and this screen shows them side by side: reads a second, positions a second and in how many batches, writes sent and lost, and the rate for each car on each lane.</li>
<li><b>Per car, per lane.</b> <code>12a9 2.4 1.2</code> is one car heard over 2.4 GHz 1.2 times a second. That's the number the fast lane is judged by.</li>
<li><b>Lost means lost.</b> A write only counts as dropped when the radio names it. Zero here is a real zero.</li>
<li>The same numbers go to the log every five seconds, so a ride can be measured afterwards instead of remembered.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure class="tall"><img src="/assets/images/touge/v3/invite.webp" alt="Ride screen on the phone: ride name thurs, Start a new ride, Lock everyone out, Share link, and the ride's QR code, blurred here" loading="lazy"><figcaption>One key per ride. The QR code is blurred here because it carries the key.</figcaption></figure>
<div>
<span class="k">Start or join a ride</span>
<ul class="tight">
<li>Start a ride and the app makes a key. Share it as a QR code for the phone in the next car, as a <code>touge://join</code> link, or over the mesh, which pops a join prompt on every Touge radio in range.</li>
<li>Join by scanning, by opening the link, or by answering the prompt.</li>
<li>A second QR puts a stock Meshtastic radio on the ride's channel from the Meshtastic app.</li>
<li><b>Lock everyone out</b>, tapped twice, makes a fresh key. Every old invite is dead, and the new QR is ready for whoever is still coming.</li>
<li><b>Send to group</b> on the route screen hands everyone your road in one radio packet: the stops, plus a point every mile and a half lifted off the route you picked. Their routers have almost no freedom left, so five cars end up on the same way over the ridge instead of five answers to "Tazewell".</li>
</ul>
</div>
</div>

<div class="row">
<figure><img src="/assets/images/touge/v3/icons.webp" alt="Profile screen: name, seventeen colour swatches, a search box and a grid of bikes: generic motorcycle, adventure bike, Ninja, R 1250 GS, R 1250 RT, Street Glide, Sportster, 690, Tracer 9 GT, Spyder RT, Ryker and Slingshot" loading="lazy"><figcaption>Ninety-six cars and bikes, grouped. This is the bottom of the list.</figcaption></figure>
<div>
<span class="k">Your car</span>
<ul class="tight">
<li><b>Ninety-six models, drawn not photographed,</b> under Garage, Sports, Off-road, Trucks &amp; vans and Bikes. Each one is a description: nose shape, where the cabin sits, what the headlights look like, and its own details. The R53's contrast roof and bonnet stripes, the NA Miata's pop-up lights, the 911's rear grille, the Bronco's spare on the tailgate, the G-Class's boxy flanks.</li>
<li><b>Thirteen bikes and trikes,</b> the MT-07, GS, Street Glide, Sportster and KTM 690 among them, plus a Spyder, a Ryker and a Slingshot, so the three-wheeled rider isn't a car on everyone's map.</li>
<li>Search by make or model: Mini, GT86, Bronco, 911.</li>
<li>Pick a swatch and every icon repaints in your colour. The preview is exactly what the group sees on their map.</li>
</ul>
</div>
</div>

### The radio

<div class="row flip">
<figure><img src="/assets/images/touge/v3/group-radio.webp" alt="Setup, Group and radio: Bluetooth to the radio reading Radio ready, Meshtastic radio, Group radio chat and SOS, Done with this radio, 2.4 GHz lane seen every 1.0 s from mattmoto, LoRa relay seen every 5.8 s" loading="lazy"><figcaption>Group &amp; radio, live: the other car on 2.4 GHz every 1.0 s, on LoRa every 5.8 s.</figcaption></figure>
<div>
<span class="k">Standard Meshtastic on LoRa</span>
<ul class="tight">
<li><b>Pairing:</b> bonded radios are listed, a scan finds ones not yet paired, a tap starts the bond. Pair with the PIN shown on the radio's screen.</li>
<li><b>Configure this radio for the ride</b> writes the radio's settings over BLE in one edit: region from the phone's country (US is 915 MHz), the radio speed, hop limit 3, your name as the radio's owner, and a primary channel for the ride, encrypted with a key derived from the ride key. It quiets the radio's own chatter, keeps its GPS on, and restarts it. Every phone on the ride derives the same channel, so nothing is typed.</li>
<li><b>Radio speed</b> is a setting: Turbo, Short, Medium or Long. Short is the default. A position is about 58 ms of airtime on Short and 760 ms on Long, thirteen times as much, and every car relays every other car's packets, so the air fills as the square of the group. Held to 30% of the channel, Short carries about ten cars on LoRa alone and Long about two. Past that the 2.4 GHz lane does the work, which is why it exists.</li>
<li><b>Done with this radio</b> hands a radio on. It tells the ride this car is leaving, puts the radio back to its stock name and the stock LongFast channel, and unpairs it from this phone. The next rider gets a clean radio.</li>
<li><b>Hold the button four seconds</b> and the radio's screen says Shutting Down, then it sleeps.</li>
<li><b>Any stock Meshtastic node</b> on the ride's channel sees every car as a node on its map, and its position shows on ours as a generic car. It just won't get the 2.4 GHz lane.</li>
</ul>
</div>
</div>

<div class="grid3 four">
<div class="tile"><b>Buy the V4, high-power</b><p>A Heltec WiFi LoRa 32 V4, the 28 dBm high-power version, not the 22 dBm one. Six decibels is roughly double the LoRa range, and it has a proper u.FL connector for a 2.4 GHz antenna. Listings that don't say which they ship are usually the 22. A V3 (or the cased Meshnology N30) is supported too, but it's the lower-spec board: no PSRAM, less range, and a fixed 2.4 GHz antenna.</p></div>
<div class="tile"><b>Flash it in the browser</b><p>The <a href="https://mrblahhhh.github.io/Touge-mesh-firmware/">web flasher</a> runs in Chrome or Edge over USB, spots a V3 or a V4 by its chip and refuses anything else. Update keeps the radio's settings and pairing; Fresh install wipes them. Source is on <a href="https://github.com/MrBlahhhh/Touge-mesh-firmware">GitHub</a>. The app tells you when a radio is too old for it.</p></div>
<div class="tile"><b>A tablet with no GPS</b><p>Plenty of tablets have none. With the GNSS module from the V4 expansion kit, the radio sends its own fix to the phone once a second, off the air. The app uses its own GPS while it has a good one, the radio's otherwise, and the status chip says <code>GPS: radio</code>.</p></div>
<div class="tile"><b>No ESP32, no fast lane</b><p>Any Meshtastic radio on 915 MHz carries LoRa, the SenseCAP T1000-E card included. The 2.4 GHz lane needs a V3 or V4 on the Touge firmware.</p></div>
</div>

### Getting everyone in

<p class="lead">Three questions on first run, and the middle one is the group. A new rider shouldn't get a map with a nameless orange car and seven categories to search through to find out why nobody can see them.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/wizard-ride.png" alt="Setup wizard step two: Ride together, with buttons to start a ride or join someone else's" loading="lazy"><figcaption>Step two of three. Start one, or join one.</figcaption></figure>
<div>
<span class="k">First run</span>
<ul class="tight">
<li><b>What are you driving:</b> name, car and colour, which is what everyone else sees on their map.</li>
<li><b>Ride together</b>, before the map download, on purpose: a group works fine on a streamed map, and making somebody wait for 400 MB before they can join their friends is the wrong order.</li>
<li><b>If a radio hears a ride already running, it's one tap.</b> The invite travels over the mesh, so three cars in a car park need no key typed and no QR held up to a windscreen.</li>
<li>The person organising it hits <b>Advertise</b> and the invite goes back out every 45 seconds, so latecomers are offered the ride as they pull in. It stops itself after fifteen minutes. The broadcast carries the key, and a ride advertising all afternoon is one anybody in radio range can walk into.</li>
<li>Maps last. Every step skips, nothing is modal afterwards, and an existing profile never sees it at all.</li>
</ul>
</div>
</div>

## Plan the road, not the arrival time

<p class="lead">Every other nav app offers alternatives as minutes saved. Touge offers them as corners.</p>

<div class="row flip">
<figure><img src="/assets/images/touge/v3/routes.webp" alt="Boone to Asheville, North Carolina: nine numbered routes in their own colours, each labelled with time and twist score, route 1 timed on live traffic and route 4 marked surface unknown" loading="lazy"><figcaption>Boone to Asheville on Back roads. Eight ways in eight colours, numbered, plus one timed on live traffic.</figcaption></figure>
<div>
<span class="k">Route choice</span>
<ul class="tight">
<li><b>Up to eight ways per leg, not one.</b> Each leg is asked at up to four highway tolerances and the answers merged by shape, so the set spans refusing the slab, tolerating it, and taking it. The fastest is always in there.</li>
<li><b>Eight colours, one number each.</b> Blue, orange, magenta, teal, violet, yellow, pink and cyan, with its rank on every label and card. Red is kept for a closed road and brown for gravel, so neither is ever just a route's colour.</li>
<li>The TWIST score is measured off the geometry: heading change per mile, share of distance in real corners, median corner radius, junctions per mile. No model guesses which road sounds scenic.</li>
<li>Flat Top Road and the Blue Ridge Parkway score 24 and take 2 h 09. US 221 scores 9 and takes 1 h 31. Thirty-eight minutes buys nearly three times the corners, and that's the whole decision in two numbers.</li>
<li>It asks <b>my own Valhalla</b>, four states of tiles, with Touge's preferences baked into the request rather than applied afterwards. Asking for a normal route and re-sorting by curviness gets a worse set to sort: the engine has already decided the interstate is the answer and offered three variations on it.</li>
<li><b>Tap the name to change it.</b> Picking the wrong town is a tap to fix, not a reason to plan the ride again.</li>
</ul>
</div>
</div>

<div class="row">
<figure class="tall"><img src="/assets/images/touge/v3/phone-routes.webp" alt="Phone route choice out of Brevard: eight numbered routes in eight colours around Asheville and Marion, with times and twist scores" loading="lazy"><figcaption>Eight routes out of Brevard on the phone, Twistiest first.</figcaption></figure>
<div>
<span class="k">What's on the road</span>
<ul class="tight">
<li><b>Paved or not, per route.</b> Every route is split into paved, unpaved and unknown from the server's own road data. In Asphalt and Twisties, the car and street-bike modes, any real stretch of gravel gets a brown <b>Gravel</b> badge, is drawn dashed brown on the line, and asks <b>Start anyway</b> or <b>Pick another</b> before you go. On the road, the voice says "Gravel ahead" before you reach it. Backcountry and Off-road show the split up front, because there gravel is the point.</li>
<li><b>Traffic and closures.</b> One more request per leg asks the server for a traffic-aware answer, and it comes back labelled Live traffic. A route along a road the server has marked closed is drawn red, says which road, and sorts last.</li>
<li><b>Unknown says unknown.</b> Where the map has no surface tag the card says <code>surface ?</code> rather than guessing paved.</li>
</ul>
</div>
</div>

## Highway out, back roads home

<p class="lead">Every other routing app takes one instruction for the whole trip. A real ride is slab until the good roads start, then nothing but corners, then the quick way home. That's three instructions, not one.</p>

<div class="row flip">
<figure><img src="/assets/images/touge/v3/trip.webp" alt="Trip screen starting at Boone with one stop, Asheville, the Back roads style chosen, and nine timed options for the leg with road names and twist scores" loading="lazy"><figcaption>Every leg gets its own style and its own set of timed options.</figcaption></figure>
<div>
<span class="k">Trips</span>
<ul class="tight">
<li>Each stop carries the style of the leg that arrives at it: Highway, Mixed, Back roads, Twistiest.</li>
<li>One request per leg, stitched, so the engine does what you asked instead of averaging two wishes into a compromise.</li>
<li>The start is the car, or Home, or anywhere you pick. Planning Sunday's ride from the sofa is a real thing people do, and it clears itself once you drive off.</li>
</ul>
</div>
</div>

## Shape the line yourself

<p class="lead">Search finds roads by name: type "bledsoe" and Bledsoe Creek Road comes back. What it can't do is say <em>which way</em>: up this one, over the gap, down the other side, in that order. That's a shape, not a name, and the place to draw a shape is a map.</p>

<div class="row">
<figure><img src="/assets/images/touge/v2/editor.png" alt="Route editor with three numbered pins dropped on back roads near Sparta and the routed line running through them" loading="lazy"><figcaption>Three pins, dragged onto the roads I meant. The engine joins them up; the line is the answer, not a sketch.</figcaption></figure>
<div>
<span class="k">Route editor</span>
<ul class="tight">
<li>Long-press to drop a pin. Drag it to move it. Tap it to change or remove it.</li>
<li>Dragging is the point. "Not that road, the one a ridge over" is a decision you make by looking, and you can see immediately whether the line went where you meant.</li>
<li>A pin doesn't need to be anywhere named: a pull-off, a gate, the car park everyone meets in.</li>
<li>Reverse rides it the other way, and the leg styles travel with the legs. Highway out stays highway out; it doesn't become highway home.</li>
<li>Close loop brings you back to the first place you chose, not to wherever the car was parked.</li>
<li>Save GPX, or Start and drive it.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/library.png" alt="Rides and routes library listing recorded rides, each with Follow, Route it, share and delete" loading="lazy"><figcaption>Every ride it recorded, and every GPX you brought in. One flat list.</figcaption></figure>
<div>
<span class="k">Rides and routes</span>
<ul class="tight">
<li><b>Follow</b> pins you to the line exactly as recorded, with no router involved. That's how a forest road the map has never heard of stays the route.</li>
<li><b>Route it</b> hands the same line to the engine as shaping points and gives back street names, lanes and rerouting.</li>
<li>A recorded ride defaults to Follow. Something shared out of Maps defaults to Route it. They're different questions.</li>
<li>Import a GPX, share one out. A plan is written as a route, a recording as a track. It won't pass off a computed line as something the wheels did.</li>
<li>Recorded rides prune to the newest thirty. Files you imported are never touched.</li>
</ul>
</div>
</div>

<div class="quote">A ride you liked is a file. Ride it again, hand it to somebody, or open it and move three pins.</div>

## Drive it

<div class="row">
<figure><img src="/assets/images/touge/v3/drive.webp" alt="Tablet in 3D on Flat Top Road south of Boone: turn card, group card with Dave, Ana and Kev behind, weather radar disc, status chip, Waze chip, the blue route winding through the hills, and the time, distance and twist strip" loading="lazy"><figcaption>South of Boone on the demo ride: the next turn, three cars behind, 100 miles of weather, and the twist score of the road ahead.</figcaption></figure>
<div>
<span class="k">Turn by turn</span>
<ul class="tight">
<li>Position snaps to the route. The heading tolerance widens with the bend, so a hairpin, where the road sits 90 degrees off the car, still matches instead of calling you off route.</li>
<li><b>Off route</b> is more than 40 m (130 feet) from the line for three seconds, or off it and facing more than 60 degrees away for a second and a half. One bad fix under a bridge does neither.</li>
<li><b>Heading comes from the road, not the phone, when slow.</b> Above 12 mph the GPS bearing is used as is. Below that it's the direction of the track the car has actually driven. Picking the phone up at a light and putting it back doesn't spin the map.</li>
<li><b>The speed limit sign works without a route.</b> On a route the limits come with it and work with no signal. Without one, every 150 m the server is asked what the road under the car is posted at, with the heading picking the carriageway.</li>
<li>Lane guidance draws the lanes to be in when the map data carries them.</li>
<li>Calls land about eight seconds out, so they scale with speed. Music ducks, never pauses.</li>
<li>Arrival, time left, distance left and the twist score of what's left come from where you actually are, not from the plan the route was fetched with.</li>
</ul>
</div>
</div>

## Off the route

<p class="lead">A full reroute answers "the best way from here". After a missed turn on a planned ride that's the wrong question, because the best way from here usually skips the twisty section that was the reason for picking the route.</p>

Nothing pops up for a wobble. The offer only appears once the car is a quarter mile off the line, moving at 10 mph or more, with a destination still ahead. Parked beside yesterday's route, it stays quiet. It sits over whatever screen is up, with big targets for a thumb on a bumpy road.

<div class="grid3">
<div class="tile"><b>Back to my route</b><p>The default, and what happens on its own after a 30 second countdown. A short connector from the car to a point further along the route you picked, then the rest of that route exactly as it was: same roads, same turns, same stops.</p></div>
<div class="tile"><b>New route to Asheville</b><p>Named for the last stop. A fresh route from here, with stops already behind you dropped. It keeps your taste per leg: if you picked the quickest option, each leg's new answer is the quickest again, and if you picked the twistiest, the twistiest.</p></div>
<div class="tile"><b>Keep going</b><p>Dismisses it. Still well off the line ten seconds later, it asks again. Taking a different road on purpose is the point of this app, and a nav that keeps dragging you back is one people switch off.</p></div>
</div>

<ul class="tight">
<li><b>The rejoin point is never behind the car.</b> It's at least 800 m (half a mile) past where you left the route, or as far past it as you've strayed, whichever is more. Aiming for the nearest bit of line is how a nav tells someone who drove two miles past their turn to make a U-turn.</li>
<li><b>It never skips a stop.</b> The join is never past the next stop you haven't reached, so a missed turn just before the fuel stop takes you to the fuel stop, not past it.</li>
<li><b>It honours closures.</b> The way back is checked against the roads the server knows are closed. If it runs along one, the join moves further on; if there's no open way back, it says so and works out a new route instead.</li>
<li><b>The connector is costed like the leg it rejoins.</b> Getting back onto a back-roads leg doesn't go by the interstate, and you arrive going the right direction rather than facing the way you came.</li>
<li>The countdown only stops for three things: getting back on the line, arriving, or Keep going.</li>
</ul>

## Skipping a stop without stopping

<p class="lead">Google's version is a small dialog at the moment you're looking for a parking space. This one comes up two miles out, with three big buttons, and needs no answer.</p>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/stop-ahead.png" alt="Stop ahead card 1.3 miles out with Skip, Later and Go on" loading="lazy"><figcaption>1.3 miles from Marion, an intermediate stop. Skip drops it, Later moves it to the end of the trip, Go on keeps it.</figcaption></figure>
<div>
<span class="k">Stop ahead</span>
<ul class="tight">
<li><b>Skip</b> drops the stop and the route goes straight on to the next one, so a town you only meant to pass doesn't pull you into its centre and back out.</li>
<li><b>Later</b> moves it to the end of the trip.</li>
<li><b>Go on</b> keeps it as planned. Nothing is modal; ignore the card and the trip advances on its own when you arrive under 5 mph.</li>
</ul>
</div>
</div>

<div class="grid3 four">
<div class="tile"><b>Silent</b><p>The turn card still counts down.</p></div>
<div class="tile"><b>Calm</b><p>Valhalla's own wording.</p></div>
<div class="tile"><b>Brief</b><p>Direction and road, nothing else.</p></div>
<div class="tile"><b>Touge</b><p>Japanese, shouted, about the corner. <code>ヘアピン左！落とせ！</code> for a hairpin left. Needs a Japanese voice installed and says so if there's none.</p></div>
</div>

## Weather, and where the police are

<p class="lead">Two things worth knowing before you get to them: what the sky is doing for a hundred miles, and who's sitting on your road. Not the next county's road. Yours.</p>

<div class="row">
<figure class="small"><img src="/assets/images/touge/v3/corner.webp" alt="Top right corner of the tablet: status chip reading LoRa, 9:28, wifi and 100 percent, the Waze chip reading just now and 77 reports, and the 100 mile weather radar disc over the hills near Boone" loading="lazy"><figcaption>The top corner: radio lane and clock, the Waze feed's age and count, and 100 miles of NEXRAD.</figcaption></figure>
<div>
<span class="k">The top corner</span>
<ul class="tight">
<li><b>The status chip</b> reads the radio lane first: <code>2.4 GHz</code> in green, <code>LoRa</code> in blue, <code>Radio · alone</code> when your radio is up but hears nobody, <code>No radio</code> in red. Then <code>GPS: radio</code> if the fix is coming from the radio, the time in your phone's 12 or 24 hour format, signal, and battery.</li>
<li>On a phone with a camera hole where the chip would sit, the antenna drops to its own badge below it and nothing else moves.</li>
<li><b>The Waze chip</b> says when the last update landed and how many reports the app holds: "Waze · just now · 77". Green while the two-minute poll keeps up, amber once one is overdue, red past six minutes, so a feed the battery optimiser quietly killed doesn't look like a quiet road. <b>Tap it</b> for every report on the map and in a list, nearest first; tap a row to go to it.</li>
<li><b>The radar disc</b> is the Iowa Environmental Mesonet's national NEXRAD composite, 100 miles around the car, rings at 25, 50 and 75. No key, no account, refreshed every five minutes or five miles. Any <code>{z}/{x}/{y}</code> tile feed can be pasted in instead.</li>
</ul>
</div>
</div>

<div class="grid3">
<div class="tile"><b>Police on your way, not everywhere</b><p>A police report counts only when it's within half a mile of the route ahead, or of the road ahead with no route. Beside you or behind you, it's gone. The same rule filters the map pins, the radar disc and Android Auto, so a speed trap two valleys over never lights up the screen.</p></div>
<div class="tile"><b>A banner, JBV1 style</b><p>When one is ahead, a banner across the top gives it all at once: how reliable it is on a red-to-green bar, the road, how many people confirmed it, how old it is, which way, the distance to a hundredth of a mile, and the time until you're there.</p></div>
<div class="tile"><b>Police cards under the disc</b><p>The nearest three on your way are small cards under the radar disc, each with road, confirmations, age, direction and distance. They fade as they age. Police are plotted on the disc too, north up, where they are.</p></div>
<div class="tile"><b>Hazards on your side only</b><p>A spoken hazard has to be on the road you're driving, and when the report has a heading, on your carriageway. A stopped car on the other side of a divided highway stays a pin and stays quiet.</p></div>
<div class="tile"><b>Every badge says how old</b><p>Waze's colours, so a pin reads the same in both apps, with full-colour icons per subtype: stopped car, cones, pothole, ice, fog, flood, animal, speed camera. The label reads "Police · 21 min ago", the badge fades as it ages, and each kind has its own lifetime: police 25 minutes, traffic 20, animals 30, crashes 90, hazards two hours, closed roads six.</p></div>
<div class="tile"><b>Tap for the detail</b><p>Distance, how old, and how many people have confirmed it. The same numbers the report carried, none of them invented.</p></div>
</div>

## Alerts you control

<div class="row flip">
<figure><img src="/assets/images/touge/v2/alerts.png" alt="Alerts screen: one row per kind with a map toggle and a voice toggle" loading="lazy"><figcaption>One row per kind, both switches on the row.</figcaption></figure>
<div>
<span class="k">Map and voice, separately</span>
<ul class="tight">
<li>Waze puts every report type behind its own page with the same two switches on each. Here it's one screen: every kind is a row, and the pin toggle and the speaker toggle sit on it.</li>
<li>They answer different questions. A pin is <em>what's out there</em>, worth a glance in traffic; a spoken warning is <em>act now</em>, and far fewer things earn one. Traffic drawn and silent is the setting most drivers land on, and one switch can't say that.</li>
<li>Warning distance is ½, 1, 1.5 or 2 miles, never less than 20 seconds of notice at speed, and a chime can lead the words so the first syllable isn't the warning.</li>
<li>Voice follows the voice mode, so Silent stays silent.</li>
</ul>
</div>
</div>

## Valentine One Gen 2

<p class="lead">The detector already beeps. What it can't do is tell you where, on a screen, while you drive, or pick its own logic mode as the town changes.</p>

<div class="row">
<figure><img src="/assets/images/touge/v3/v1-explainer.webp" alt="Setup, Devices and sensors: the Valentine One Gen 2 switch and the How the V1 is driven explainer covering the three modes, when Touge switches, what it mutes on top, and the card" loading="lazy"><figcaption>Setup explains how the V1 is driven, in the V1's own letters.</figcaption></figure>
<div>
<span class="k">Valentine One</span>
<ul class="tight">
<li>A V1 Gen 2 over Bluetooth on the same protocol JBV1 uses. A ring around the car points where the signal comes from, with band, frequency and bars beside it. Ka and laser are red.</li>
<li><b>It sets the logic mode for you.</b> Capital <b>L</b>, Advanced Logic, in a city: 150 or more places within a mile. Lowercase <b>l</b>, Logic, everywhere else. Never <b>A</b>. The card shows the mode Touge last sent, in the V1's own lettering.</li>
<li>It sends a mode only when its choice changes, and again on reconnect. Press the V1's own button and your choice stands until then.</li>
<li><b>What Touge mutes on top:</b> a spot you've locked out as a false alert, anything under 10 over a known limit, and anything under two bars. X and K in town is a switch of its own, off by default, because the logic mode already filters them.</li>
<li>A Gen 2 pairs with one app at a time, so JBV1 is closed while Touge owns the detector.</li>
</ul>
</div>
</div>

## Tire pressure

<p class="lead">A slow leak is the failure that ends a mountain day, and it announces itself an hour before it strands you.</p>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/tires.png" alt="Tires screen with four wheel tiles; rear left is leaking" loading="lazy"><figcaption>Four tiles, one leaking. The rate is a fitted slope, not two samples.</figcaption></figure>
<div>
<span class="k">Tire pressure</span>
<ul class="tight">
<li>Bluetooth TPMS sensors (Zeepin/TPMSII, DJTPMS, Tesla) bind to a wheel by tapping the wheel, then the sensor.</li>
<li>Alarms in order: under 20 psi; losing 2 psi a minute (a least-squares slope over two minutes, so a puncture trips it and quantization doesn't); 10 psi below the tire's peak; over 158°F.</li>
<li>A red strip on the driving screen and one spoken line naming the corner.</li>
</ul>
</div>
</div>

## Maps, search, and your own server

<div class="row">
<figure><img src="/assets/images/touge/v2/packs.png" alt="Map packs screen listing regions with measured sizes and installed state" loading="lazy"><figcaption>Measured, not estimated. North Carolina is 88,675 tiles and 392 MB.</figcaption></figure>
<div>
<span class="k">Map packs</span>
<ul class="tight">
<li>Any of the fifty states, a few ready-made regions (Blue Ridge, Southern Appalachia), a radius around you, or the corridor from here to where you're going.</li>
<li>PMTiles is read in place, so the app walks the planet file's directory and adds up exactly the bytes it needs before fetching any.</li>
<li><b>Keep as many as you have room for.</b> Every pack is drawn as its own layer, so North Carolina, Virginia and West Virginia are one map with no seam and no switching.</li>
<li>The search index is built on the device from the same tiles and spans every pack, so a Virginia town is findable from a North Carolina car park.</li>
<li>A pack you've downloaded never expires and needs no signal.</li>
</ul>
</div>
</div>

<div class="grid3">
<div class="tile"><b>Routing with no signal</b><p>A route worked out on the phone from the pack you already downloaded. North Carolina takes about a minute to build once, then a route comes back in about a tenth of a second. It's off by default because it isn't free: another 300 MB beside the pack. There's no offline mode to remember: the server is asked first whenever there's signal, and a route worked out on the device is quietly replaced by the server's when the bars come back. The route screen always says which one you're looking at.</p></div>
<div class="tile"><b>Live traffic</b><p>Flow drawn over the roads, green through red, from TomTom's traffic tiles, with a free key from developer.tomtom.com. TomTom incidents wear the same badges as Waze reports, and a closure both feeds report shows once. Refreshed every two minutes around the car, well inside the free tier.</p></div>
<div class="tile"><b>Off-road mode</b><p>A fourth vehicle mode, for the Bronco. Forest roads and tracks are routed on and drawn brown, even offline. Leaving the planned line is the plan, so there's no reroute nag, and the breadcrumb trail records where the wheels went.</p></div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v2/search.png" alt="Search results for Sparta, the town first" loading="lazy"><figcaption>Search ranks by name, then kind, then distance. A town beats a road of the same name.</figcaption></figure>
<div>
<span class="k">Search</span>
<ul class="tight">
<li>Runs over the on-device index, not the tiles on screen. A diner 120 miles ahead is as findable as one in view.</li>
<li>Fuel, food, coffee and views are one tap each. On a route they're ordered by how soon you reach them, not by how near they are: the closest pump is often twenty minutes behind you. Food hides chains by brand tag and checks opening hours, overnight spans included.</li>
<li>The last places you picked, and any you starred, come up on an empty box. Typing a town name in a moving car is the most expensive thing this app asks for.</li>
</ul>
</div>
</div>

<div class="grid3 four">
<div class="tile"><b>Breadcrumbs</b><p>Every ride is recorded as GPX with no button. Points closer than 50 feet are dropped unless the heading moved 12 degrees, so the corners are kept. They land in Rides and routes, ready to Follow.</p></div>
<div class="tile"><b>Waze reports</b><p>Over SABRE, an open Android protocol, through the WzSabre proxy app, the same one JBV1 uses. The proxy owns the Waze side; Touge scrapes nothing. With no proxy installed the feature is simply absent.</p></div>
<div class="tile"><b>Routing server</b><p>Routing goes to my Valhalla box first, four states of tiles that can carry closures and traffic speeds, with the public FOSSGIS instance behind it as a fallback for anywhere outside them. A small monthly subscription (about $3) covers the server. Settings take any Valhalla URL of your own.</p></div>
<div class="tile"><b>Android Auto</b><p>The map on the head unit: same style and pack, the route, the group's cars with their icons, the radar disc as an inset, heading up, and only the police on your way. The turn card with lanes and ETA; Skip, Later and Go on when a stop is ahead; Group, Routes, Search and Tires as car screens. The phone does the work and the head unit shows it.</p></div>
</div>

## The settings screen

<p class="lead">Seven categories, each with a one-line summary of what it's set to, and a search box that finds any setting by what it's called or what it's currently set to.</p>

<div class="row">
<figure><img src="/assets/images/touge/v3/settings.webp" alt="Setup on the tablet: the You card, then seven categories with live summaries: Maps and navigation, Group and radio, Sound and alerts, Display and layout, Devices and sensors, Recording, App and data; Maps and navigation open on the right" loading="lazy"><figcaption>Seven categories, each summarised on its header.</figcaption></figure>
<div>
<span class="k">Categories</span>
<ul class="tight">
<li><b>Maps &amp; navigation, Group &amp; radio, Sound &amp; alerts, Display &amp; layout, Devices &amp; sensors, Recording, App &amp; data.</b> Each header says what's set ("In a ride: thurs · Radio ready · Sharing on") and flags a problem before you open it, like a traffic key that's missing.</li>
<li><b>Advanced is one level down</b> in each category, so the radio's modem preset and the Link diagnostics are there when you want them and out of the way when you don't.</li>
<li>URLs and keys get Save and Cancel and a check on their shape, so a half-pasted key doesn't quietly break traffic.</li>
<li><b>Backup and restore</b> writes every setting to one file, ride key included, so a second tablet becomes this one in a tap.</li>
</ul>
</div>
</div>

<div class="row flip">
<figure><img src="/assets/images/touge/v3/settings-search.webp" alt="Setup search for radio, listing Bluetooth to the radio, Server check-in while on the radio, Start or join a ride, Share my position, Keep sharing in the background, 2.4 GHz lane, LoRa relay, Send my position every, and Cellular fallback, each with its category and current value" loading="lazy"><figcaption>Type "radio". Every match comes back with where it lives and what it's set to.</figcaption></figure>
<div>
<span class="k">Search</span>
<ul class="tight">
<li>Search matches the setting's name, its category, words people actually use for it, and its current value, so "tomtom", "zoom" and "30 s" all find something.</li>
<li>Each result says where it lives, <code>Group &amp; radio › Advanced</code>, so next time you know the way.</li>
</ul>
</div>
</div>

<div class="grid3 four">
<div class="tile"><b>How much road</b><p>A quarter mile to five miles at speed. A distance, not a zoom number, so half a mile is half a mile on any screen and at any latitude.</p></div>
<div class="tile"><b>3D tilt</b><p>30 to 60 degrees, held at any speed. Perspective puts the road you're about to drive in the top half of the screen rather than the field beside you. Flat is one tap away.</p></div>
<div class="tile"><b>Zoom in when slowing</b><p>Coming to a stop closes the map in on the junction, which in 3D shows you which way the road actually goes.</p></div>
<div class="tile"><b>Where your car sits</b><p>Running tail, everyone is in front and the screen wants road ahead. Running lead, what you want to know is whether you've dropped anyone.</p></div>
</div>

<div class="appicon">
<img src="/assets/images/touge/v3/app-icon.svg" alt="Touge app icon: an orange hairpin road with a dashed centre line, a white car with its headlights on carving the top bend, under a navy night sky with a moon and stars">
<p><b>A new icon.</b> A car carving a night-time switchback with its headlights on, drawn so a round or a squircle launcher mask keeps the car and the bend, with a monochrome layer for themed icons.</p>
</div>

<div class="last"></div>

</div>
