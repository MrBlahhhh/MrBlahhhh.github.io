---
layout: article
title: F5X Code Privacy Policy
permalink: /f5xcode/privacy/
key: f5xcode-privacy
aside:
  toc: true
---

# F5X Code Privacy Policy

**Last updated: 28 October 2026**

F5X Code is a diagnostics, logging and coding app for the MINI F54, F55, F56
and F57, published by Geekopolis. This policy says exactly what the app does
with your data.

There is no account, no sign-in, no advertising, no analytics and no crash
reporting. The app only goes online when you choose to upload a log to
Geekopolis Logs, described below.

---

## The short version

Everything the app reads from your car stays on your phone. It only leaves
when you tap **Send report**, **Share** or **Share log link**, and only what
you picked.

---

## What stays on your device

Never transmitted anywhere by this app:

- **Your car's VIN**, module identities and software versions.
- **Fault codes** and fault reports.
- **Logs** recorded by the logger, and their graphs, unless you upload one.
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
| **Share log link**, then **Upload** | The one log you chose, with its notes, its title and the app version | logs.geekopolis.com, run by Geekopolis. See below. |

A report you email is used only to answer you and fix the app. It isn't sold,
shared or used for advertising. Ask and it will be deleted.

---

## Uploading a log to Geekopolis Logs

**Share log link** in the graph screen uploads one saved log to
[logs.geekopolis.com](https://logs.geekopolis.com/) so it can be viewed,
graphed and analysed there. Nothing is uploaded until you confirm, and a log
that is still recording can't be uploaded.

- **Who can see it:** anyone with the log's link can view and download the
  whole file. The link isn't listed or searchable, but treat it like the file
  itself. Logs hold engine data only: no VIN and no location.
- **What the site keeps:** the log file and its title and notes, which app
  and version sent it, when, and a one-way hash of the network address it came
  from, used only to limit abuse. The site doesn't keep your address itself.
- **Checking your purchase:** uploads come with the unlock. The first time
  you upload, the app sends the site your Google Play purchase receipt (order
  number, product, purchase token and app name, signed by Google) so the site
  can confirm the purchase. The site keeps the order number and a one-way hash
  of the purchase token, and gives the app an access code it stores and sends
  with each upload. If Matt gave you an access code instead, that code is sent.
- **Editing and deleting:** after an upload the app saves a private edit key
  on your phone. **Manage upload** opens the log on the site, where that key
  lets you change its title and notes or delete it. Deleted logs stop being
  available straight away. Email matt@geekopolis.com and an upload, or
  everything tied to your purchase, will be removed.

Uploads aren't sold, shared or used for advertising. They're used to show you
your log and, if you ask for help, to look at it with you.

---

## Your car and your adapter

The app talks to your car through the adapter you connect: an ENET cable, a
Wi-Fi adapter, or a Bluetooth OBD adapter. That traffic stays between the
phone, the adapter and the car. With a Wi-Fi adapter the phone joins the
adapter's own network, and the app talks only to the adapter.

---

## Purchases

The one-time unlock is bought through Google Play. Google handles the payment;
the app learns whether the unlock is owned, and keeps Google's receipt for it
so a log upload can show the purchase to Geekopolis Logs. See
[Google's privacy policy](https://policies.google.com/privacy).

---

## Permissions, and why each one exists

- **Bluetooth (connect and scan)**: to find and talk to a Bluetooth OBD
  adapter. The scan permission is declared `neverForLocation`: the app does not
  work out your position from Bluetooth.
- **Location (Android 11 and older only)**: those Android versions require it
  for any Bluetooth scan. The app never reads your location.
- **Local network and network access**: to reach a Wi-Fi adapter or an ENET
  cable on the phone's own network, and, only when you upload a log, to reach
  logs.geekopolis.com.
- **Network state**: to hold the wired Ethernet connection while an ENET cable
  is plugged in.
- **Notifications, foreground service and wake lock**: to keep a recording
  running with the screen off, with a visible notification that has Mark and
  Stop on it.

---

## Children

F5X Code is not directed at children and does not knowingly collect
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
