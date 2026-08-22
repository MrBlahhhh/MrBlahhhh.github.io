---
title: "TrackEncoder — 16 of my 27 track videos were unplayable, and nothing told me"
date: 2026-08-22 00:00:00 -0400
categories: car tech
tags: [trackencoder, android, video, mp4, reliability, sd-card, usb, magisk, telegram, track]
cover: /assets/images/trackencoder-metrics/hud-full.jpg
lightbox: true
excerpt: "The overlay worked. The recording light was on. The file was growing. And more than half of what I'd recorded couldn't be opened — because of where I'd put the SD card."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/trackencoder-metrics/hud-full.jpg
---

<!--more-->

Last month I wrote up [what every number on the TrackEncoder overlay means](/car/tech/2026/08/21/trackencoder-metrics-explained.html). The metrics were the interesting part. This month the interesting part was finding out that more than half my recordings couldn't be played, that the app had been telling me everything was fine the whole time, and that the cause was a decision about *where the SD card lived*.

Nothing about the overlay was wrong. Every metric in that post still works exactly as described. The video around them just wasn't openable.

## How it looked from the inside

Perfect. That's the problem.

`CAM: USB 1920x1080`. `REC: CARD`. File on the card, growing at 1.7 MB/s. Camera at 30 fps. No errors in the log, no warning on the screen, nothing in the notification. Every signal the app had said the session was fine.

Then I went to check a file and it wouldn't open. Then another. In the end, of 27 recordings I checked, **16 had no index and no player would touch them.**

## What actually broke

An MP4 keeps its index — the `moov` atom — at the *end* of the file, written when recording stops. Everything before it is `mdat`: the raw frames, no table of contents. Kill the writer before `stop()` runs and you get a large file full of perfectly good video that nothing can play, because nothing knows where anything is.

I knew that. It's why the app writes ~1.8 GB segments instead of one giant file, and why it closes the file when the car's power is cut. Android announces the cut as `ACTION_POWER_DISCONNECTED` with the phone still very much alive on its own battery, which is plenty of time to finalise properly.

The part I had wrong was the card.

I'd built the obvious thing: one PD powered USB hub carrying the camera and a USB SD card reader. Tidy, one cable to the phone. It works perfectly on the bench.

In the car, the supply to that hub is switched — I cut power in the grid queue while waiting to go out. So I cut it, on purpose, with a recording running, and watched the kernel log:

```
[1007.359052] usb 1-1.3.1: USB disconnect, device number 5
[1010.046684] hub 1-1.3:1.0: USB hub found      <- the hub itself re-enumerated
[1262.448452] sdh: sdh1                          <- card back as a NEW device
[1264.853491] FAT-fs (sdg1): bread failed in fat_clusters_flush
```

The hub **does not** fail over to bus power from the phone. It power-cycles, and takes everything downstream with it for about three seconds. Look at the last two lines: the card comes back as `sdh`, but the filesystem is still mounted on `sdg` — a device that no longer exists. That's the `bread failed`: the mount thrashing against dead hardware.

Which means at the exact moment the app is trying to close the file properly, **the storage it's writing to has vanished.** There is no version of "handle the power cut gracefully" that survives that. You cannot write an index onto a disk that isn't there.

Every single power cut cost me the session it interrupted, and told me nothing.

## The fix is a SIM tray

The Moto G Stylus has a microSD slot in the SIM tray, and the vendor's own `fstab` already manages it:

```
/devices/platform/soc/8804000.sdhci/mmc_host*  auto auto defaults
                                    wait,voldmanaged=sdcard1:auto
```

Move the card there and storage is on the phone's power rail. It cannot re-enumerate, cannot get renamed, cannot disappear when the hub blinks. The hub still drops the camera — nothing to be done about that, it's the hub's power — but the file stays open on storage that's still there, and closes properly.

Cut the power mid-recording now and you get a finalised, playable file. I've since done it repeatedly. Every recording since the card moved has been fine.

It's a slightly worse-looking build. One more thing in the phone, and the card is behind a SIM tool instead of accessible on the hub. It's a much better recorder.

## The check that lied to me

Here's the bit that stings, and the reason this went on so long.

I'd been verifying files with `grep -c moov`. Search the binary for those four bytes; if they're there, the index is there. It's fast, it works over adb, and it is **completely wrong**.

"moov" is four bytes. In a 172 MB video file, those four bytes turn up by chance. My check was returning "fine" on files that had no `moov` atom anywhere in them. I'd used it to declare several power-cut tests successful. They hadn't been tested at all.

The right way is to walk the atom chain, which takes about fifteen lines of Python. A good file:

```
ftyp  24 bytes
free  3,327 bytes
mdat  172,401,056        <- sane size
moov  26,535             <- present
                            sizes sum to the file length exactly
```

A broken one:

```
ftyp  24 bytes
free  3,327 bytes        <- reserved for moov, still empty
mdat  4,557,430,888,798,830,399    <- garbage; never closed
                            no moov at all
```

That `free` block is the giveaway. The muxer reserves space for the index up front and never gets to fill it in.

A check that can only produce false *positives* is worse than having no check, because it turns an open question into a confident wrong answer. I had one of those for weeks.

## Making it refuse

Fixing the card fixed the corruption. It didn't fix the other half — the app cheerfully recording things that were never going to be worth anything.

At one point I watched it record for four minutes at 1920x1080, 30 fps, file growing, every status green, while the camera had actually stopped delivering frames. It was encoding **one frozen image** for the whole file. And a still frame compresses so well that even the file size looks unremarkable.

The status text said 30 fps because that's the last thing the camera *announced*. It's not a measurement of anything.

So there are two guards now, and both sit in one place that every route to starting a recording passes through — the on-screen button, the notification shade, the web page and the Telegram bot:

- **No external power, no recording.** The supply is switched; a session that starts on battery is one that was never meant to be running.
- **No camera frame in the last two seconds, no recording.** Not "is the camera plugged in" — is it actually delivering.

There's a `FORCE` button to override both, for when the refusal is wrong. It's off at every launch and deliberately not remembered, because an override that persists between sessions is off on the morning you needed it, with nothing on screen to say so.

And when power comes back, it starts itself again — retrying every couple of seconds for forty-five, because the hub takes a few seconds to re-enumerate and bring the camera with it. Measured gap on the bench was about 16 seconds, most of which was me. Nobody has to reach into the glovebox when the car gets waved back out.

## Getting the video off a card you can't reach

Putting the card in the SIM tray is right for reliability and miserable for access. Nobody wants to pull a SIM tray in a paddock with a session's footage on it.

So the files come off over the network. The phone runs a small web page; tick what you want, and it hands back a command to paste:

```
curl.exe -C - -o "2026-08-22_1532_CMP_000.mp4" "http://192.168.1.88:8080/dl/2026-08-22_1532_CMP_000.mp4"
```

One line per file so they come down in sequence, and every one resumes — the server answers `206 Partial Content`. A download that dies at 80% over paddock wifi picks up where it stopped instead of starting again, and re-running the block skips whatever already finished. That matters more than convenience when the files are gigabytes and the session can't be recorded twice.

I looked at zipping the selection instead. One click, no command to paste. But a zip can't resume, so a dropped link costs the whole thing — the wrong trade here.

The phone's address is a button on the Telegram card, so there's nothing to remember or retype.

## The screen was cooking it

One more, because it took three separate fixes and each one hid the others.

The phone sits in a glovebox with the screen off. Except it wasn't off:

1. **Ambient display.** On an OLED that's a few pixels. This is an IPS LCD, so "always-on" means the *backlight* is on, for a clock nobody is looking at.
2. **Developer options → Stay awake.** Keeps the screen on whenever the phone is charging — and the recorder is on USB power for the entire session. It overrides the screen timeout completely.
3. **The camera re-attaching turns the screen on.** When the hub's power returns, Android launches the app to hand it the camera, and the app asks for the screen. So the phone that just resumed a session is also a phone lit up for the rest of it.

That third one needed the app to put the panel out itself once recording is confirmed running. It uses `DevicePolicyManager.lockNow()`, which needs a device admin — declared, activated, and reporting success, while `isAdminActive` returned false forever.

That one was mine. The receiver class shares its name with the framework class it extends, and in Kotlin an explicit import outranks the class's own declaration:

```kotlin
import android.app.admin.DeviceAdminReceiver
class DeviceAdminReceiver : DeviceAdminReceiver() {
    fun getComponentName(c: Context) =
        ComponentName(c, DeviceAdminReceiver::class.java)   // the FRAMEWORK one
}
```

It names a component that was never registered. It compiles, it runs, `dumpsys` happily lists the admin as enabled, and every symptom points at the activation rather than at the lookup. Aliasing the import fixes it.

## While I was in there

The phone is rooted now, which bought three things worth having: a charge cap at 80% so it isn't sitting full and hot all day, adb over wifi that survives reboots instead of needing re-enabling every time, and the ability to recover storage without touching the phone.

The charge cap has a trap in it. The controller picks its own method, and on Qualcomm it usually picks `input_suspend` — which cuts USB input entirely, so the phone holds 80% by **running down its own battery**. The one you want is `battery_charging_enabled`, which parks the battery and lets the charger carry the load. Measured, plugged in, at the cap: `-282 mA` versus `0 mA`. Opposite behaviours, same "Not charging" on screen.

I also turned *off* its thermal shutdown. It powers the phone off at 55 °C, and a hard poweroff is precisely the one ending a recording can't survive — it would have reintroduced the exact failure I'd just spent a day removing. There's a Telegram warning at 129 °F instead, which says plainly that nothing is going to stop it.

<img src="/assets/images/trackencoder-reliability/card-menu.png" class="img-lg" alt="The card menu on the phone">
*Tap CARD in the status row. Minutes remaining rather than gigabytes free, because when the question is "does this last the session", one of those answers it. The FAT32 note is stated against the actual segment size, so it reads as a fact and not a warning with no consequence.*

## What I'd tell myself a month ago

**Storage does not belong on switched power.** Everything downstream of that hub is expendable — the camera drops and comes back, no harm done. Storage isn't expendable, because the file being written is the entire point.

**A green light is not evidence.** The app said recording, the file grew, the camera reported 30 fps, and all three were true while producing something worthless. Every one of those signals was reporting what it had been *told*, not what was *happening*.

**Verify the thing, not a proxy for it.** `grep -c moov` was a proxy. It agreed with me for weeks.

The overlay in [the previous post](/car/tech/2026/08/21/trackencoder-metrics-explained.html) hasn't changed. It just gets recorded properly now.

Source is at [github.com/MrBlahhhh/TrackEncoder](https://github.com/MrBlahhhh/TrackEncoder).
