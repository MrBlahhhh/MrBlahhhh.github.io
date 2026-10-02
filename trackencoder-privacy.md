---
layout: article
title: TrackEncoder Privacy Policy
permalink: /trackencoder/privacy/
key: trackencoder-privacy
aside:
  toc: true
---

# TrackEncoder Privacy Policy

**Last updated: 2 October 2026**

TrackEncoder is a track-day video and telemetry recorder for Android, published
by Geekopolis. This policy describes exactly what the app does with your data.

There is no account, no sign-in, no advertising, no analytics and no crash
reporting. Nothing the app records is ever sent to us.

---

## The short version

Your video, audio and telemetry **stay on your phone**, or on the memory card
or drive you record to. The app has no upload path for them.

Two optional features send data off the phone, and only after you set them up
with your own credentials: the **AI coach** and the **Telegram bot**. Both are
described below. Until you set them up, nothing leaves the phone.

---

## What stays on your device

Never transmitted anywhere by this app:

- **Video and audio recordings**, written to a memory card or USB drive you
  choose, or to the app's own storage.
- **Telemetry** from your car and data logger: GPS position, speed, g-forces,
  and engine and chassis channels.
- **Lap times, corner analysis, coaching notes and grip history.**
- **Your settings**, car profiles and channel setup.

Recordings on a card or drive you chose stay there until you delete them.
Uninstalling the app removes its own storage.

---

## What leaves your device, and when

| Feature | What is sent | Where |
|---|---|---|
| AI coach (optional) | A written summary of a session: corner-by-corner analysis, lap times, the track name, and the car and conditions labels. No video, no raw telemetry, no GPS trace. | The AI provider you set up with your own API key: Anthropic (`api.anthropic.com`) by default, or another provider's address you enter |
| Telegram bot (optional) | Session status, lap times, coaching notes and alerts, and a session data file when you ask for one. Commands you send the bot come back to the phone. | Telegram (`api.telegram.org`), to your own bot and your own chat |

Both are off until you set them up on the settings page. The AI coach sends
its summary after a session, and only while the end-of-session summary is
switched on. Remove the key or the bot token and that feature stops sending
anything.

When you enter an Anthropic key or a Telegram bot token, the app checks it with
that service before saving it. Those checks carry only the key or token.

Your API key and bot token are stored only on the phone and are sent only to
the service they belong to. The settings page shows which provider or bot is
linked, never the key or token itself.

Each of those services has its own privacy policy, and what you send is
subject to it once it arrives:
[Anthropic](https://www.anthropic.com/legal/privacy),
[Telegram](https://telegram.org/privacy).
If you point the AI coach at another provider, that provider's policy applies.

---

## On your local network

The app serves a settings and control page on the Wi-Fi network the phone is
connected to, so a second phone or tablet in the car can change settings and
start or stop recording. It is not reachable from the internet, but anyone on
the same Wi-Fi network can open it, so use a network you control. The page uses
plain HTTP: a key or token typed into it crosses that local network once and is
never shown back.

Telemetry from a RaceCapture unit arrives over the same local network.
Bluetooth devices such as GPS data loggers and OBD adapters talk to the phone
directly.

---

## Permissions, and why each one exists

- **Camera**: to record video from a USB or built-in camera.
- **Microphone**: to record audio with that video.
- **Bluetooth (scan and connect)**: to connect to GPS data loggers, OBD
  adapters and other in-car devices.
- **Location**: requested only on Android 11 and older, where Android requires
  it for Bluetooth scanning. The app never reads the phone's own location; the
  position on your video comes from your data logger.
- **Wi-Fi state and multicast**: to receive telemetry broadcast on the local
  network.
- **Network access**: for the local settings page, and for the AI coach and
  Telegram once you set them up.
- **Notifications and foreground service**: to keep recording with the screen
  off, with a notification that shows it is running.
- **Run at startup**: to open the recorder when the phone in the car powers on.
- **USB**: to use a USB camera and USB storage.
- **Device administrator (lock screen only)**: to switch the screen off while a
  session records, which keeps the phone cooler. It cannot erase data or change
  passwords, and you can turn it off in Android's settings.

---

## Children

TrackEncoder is not directed at children and does not knowingly collect
information from anyone under 13.

---

## Changes

If this policy changes, the date at the top changes with it, and the previous
text remains in the page's history on
[GitHub](https://github.com/MrBlahhhh/MrBlahhhh.github.io).

---

## Contact

Questions about this policy, or about data in the app:

**matt@geekopolis.com**
