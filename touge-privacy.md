---
layout: article
title: Touge Privacy Policy
permalink: /touge/privacy/
key: touge-privacy
aside:
  toc: true
---

# Touge Privacy Policy

**Last updated: 18 September 2026**

Touge is a navigation and drive-recording app for Android, published by
Geekopolis. This policy describes exactly what the app does with your data.

There is no account, no sign-in, no advertising, no analytics and no crash
reporting. Nothing is collected about you in the background.

---

## The short version

Your video, audio and recorded drives **stay on your device**. The app has no
upload path for them and no server to send them to.

Your location leaves the device **only when you use a feature that needs a
server** — searching for a place, asking for a route, or turning on traffic or
weather. Those requests go to the services listed below and are not stored by
Touge.

---

## What stays on your device

Never transmitted anywhere by this app:

- **Video and audio recordings** made by the app, written to your device's own
  storage or a memory card you choose.
- **Recorded drives, routes and trip history.**
- **Offline map packs** you download.
- **Your settings**, including vehicle profiles.
- **Media information** read through the notification listener — see below.

Deleting the app, or clearing its storage, removes all of it. There is no copy
elsewhere.

---

## What leaves your device, and when

Each of these happens only while you are using the feature it belongs to.

| Feature | What is sent | Where |
|---|---|---|
| Routing | Start, destination and waypoint coordinates | `route.geekopolis.com` (operated by us) or `valhalla1.openstreetmap.de` |
| Place search | Your search text and your approximate position, to rank results by distance | `photon.komoot.io` (Komoot) |
| Live traffic | The map area you are viewing | `api.tomtom.com` (TomTom) |
| Weather radar | The map area you are viewing | `mesonet.agron.iastate.edu` (Iowa State University) |
| Offline map downloads | The region you asked for | `build.protomaps.com` (Protomaps) |

These requests carry no name, no account and no device identifier. They are not
logged against you by Touge, and nothing about them is retained after the
response is used.

Turn the corresponding feature off and the request is not made.

Each of those services has its own privacy policy, and your request is subject
to it once it arrives:
[Komoot](https://www.komoot.com/privacy),
[TomTom](https://www.tomtom.com/privacy/),
[Protomaps](https://protomaps.com/),
[Iowa State Mesonet](https://mesonet.agron.iastate.edu/disclaimer.php).

---

## Riding together

When you share your position with a group, it is sent **over a Meshtastic LoRa
radio, directly between devices**. It does not go over the internet and it does
not pass through any server we operate.

Position sharing runs only while you have joined a group and only for as long
as you stay in it.

---

## Permissions, and why each one exists

- **Location (precise and approximate)** — to show where you are on the map,
  navigate, record a drive, and share your position with a group when you ask
  it to. The app does **not** request background location; it uses a
  foreground service with a visible notification while recording or sharing,
  so you can always see when it is active.
- **Camera** — to record video of a drive, when you start a recording.
- **Microphone** — to record audio alongside that video, when you start a
  recording.
- **Bluetooth (connect and scan)** — to talk to a Meshtastic radio and to tyre
  pressure sensors you have paired. The scan permission is declared
  `neverForLocation`: the app does not derive your position from Bluetooth.
- **Notification access** — to read the title, artist and app name of whatever
  is currently playing, so it can be shown on the driving display. The app does
  not read the content of any other notification, and this information never
  leaves your device.
- **Notifications** — to show the foreground-service notification while
  recording or sharing, and to show alerts.
- **Network access** — for the features in the table above.

---

## Children

Touge is not directed at children and does not knowingly collect information
from anyone under 13.

---

## Changes

If this policy changes, the date at the top changes with it, and the previous
text remains in the page's history on
[GitHub](https://github.com/MrBlahhhh/MrBlahhhh.github.io).

---

## Contact

Questions about this policy, or about data in the app:

**matt@geekopolis.com**
