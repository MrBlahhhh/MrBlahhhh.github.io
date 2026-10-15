---
layout: article
title: F56 Code Privacy Policy
permalink: /f56code/privacy/
key: f56code-privacy
aside:
  toc: true
---

# F56 Code Privacy Policy

**Last updated: 15 October 2026**

F56 Code is a diagnostics, logging and coding app for the MINI F56, published
by Geekopolis. This policy says exactly what the app does with your data.

There is no account, no sign-in, no advertising, no analytics and no crash
reporting. The app has no server and makes no internet requests of its own.

---

## The short version

Everything the app reads from your car stays on your phone. The only way any
of it leaves is when you tap **Send report** or **Share**, and then it goes
where you send it, through an app you pick.

---

## What stays on your device

Never transmitted anywhere by this app:

- **Your car's VIN**, module identities and software versions.
- **Fault codes** and fault reports.
- **Logs** recorded by the logger, and their graphs.
- **Coding backups** of your modules, and the history of every change.
- **Your settings**, including saved adapters and oil level calibration.

Deleting the app, or clearing its storage, removes all of it. There is no copy
elsewhere.

---

## What leaves your device, and when

Only when you ask, and only to where you choose:

| You tap | What is sent | Where |
|---|---|---|
| **Send report** | The report you're looking at: logs, the communication journal, and for coding reports the backups involved, which carry the VIN | Your email app opens with it addressed to matt@geekopolis.com. Nothing is sent until you send that email. |
| **Share** | The files you picked | Whatever app you choose from Android's share sheet |

A report you email is used only to answer you and fix the app. It isn't sold,
shared or used for advertising. Ask and it will be deleted.

---

## Your car and your adapter

The app talks to your car through the adapter you connect: an ENET cable, a
Wi-Fi adapter, or a Bluetooth OBD adapter. That traffic stays between the
phone, the adapter and the car. With a Wi-Fi adapter the phone joins the
adapter's own network, and the app talks only to the adapter.

---

## Purchases

The one-time unlock is bought through Google Play. Google handles the payment;
the app only learns whether the unlock is owned. See
[Google's privacy policy](https://policies.google.com/privacy).

---

## Permissions, and why each one exists

- **Bluetooth (connect and scan)**: to find and talk to a Bluetooth OBD
  adapter. The scan permission is declared `neverForLocation`: the app does not
  work out your position from Bluetooth.
- **Location (Android 11 and older only)**: those Android versions require it
  for any Bluetooth scan. The app never reads your location.
- **Local network and network access**: to reach a Wi-Fi adapter or an ENET
  cable on the phone's own network. No internet service is contacted.
- **Network state**: to hold the wired Ethernet connection while an ENET cable
  is plugged in.
- **Notifications, foreground service and wake lock**: to keep a recording
  running with the screen off, with a visible notification that has Mark and
  Stop on it.

---

## Children

F56 Code is not directed at children and does not knowingly collect
information from anyone under 13.

---

## Changes

If this policy changes, the date at the top changes with it, and the previous
text stays in the page's history on
[GitHub](https://github.com/MrBlahhhh/MrBlahhhh.github.io).

---

## Contact

Questions about this policy, or about data you've sent:

**matt@geekopolis.com**
