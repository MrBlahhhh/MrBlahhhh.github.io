# F5X Code post: running order

Not a post. The running order for `2026-10-15-f5x-code.md`, whose page is
`_includes/f5xcode-feature.html`. Add to this structure instead of appending
to the end.

**The rule: what it is, what it costs and what hardware it needs come first,
then how to hook it up and what it can't do, then the feature detail.**
(Reordered 2026-10-17 at Matt's request, the way the R53 Logger page was.
Rewritten 2026-11-20 for build 248 at Matt's request: fresh screenshots, the
service tools, eighteen presets and the GP3 steering flash.)

1. **Hero**: what it is (the five cars, the jobs, the connection, and the GP3
   line), stats strip (Free engine fault codes, $45 once, 11 modules,
   18 presets, 14 service tools), Join the testers / $45 · what you need /
   GP3 steering flash.
2. **What it does**: five tiles (diagnose, log, code, service, and flashing
   tagged developer build) and three screenshots under them.
3. **What it costs**: the engine's fault codes (read and clear) are free;
   everything else, service tools included, is the one-time $45 unlock
   (`codingunlock`).
4. **What you need**: ENET cable plus USB-C Ethernet adapter, or the red MHD
   Universal adapter (both recommended, both ENET); Android 7+, and 11+ for
   the cable because it needs Ethernet tethering; OBDLink CX/MX+ and Wi-Fi
   OBD2 optional.
5. **Hook up the ENET cable** (`#connect`): five steps, tethering called out,
   the chooser and ENET dialog screenshots, then the MHD adapter's three
   steps.
6. **Find your way around** (`#tabs`, added 2026-11-20 for build 249): the
   five tabs (Coding · Tools · Logger · Backups · Reports, R53 Code's layout)
   as tiles, then the Tools and Coding tab shots. Every other section says
   which tab its feature is on; keep that in step with `ui/MainMenu.kt`.
7. **What it can and can't do**. The Play version doesn't flash: say so here.
   Keep the line that a listed option isn't always accepted.
8. **Flashing** (`#gp3`): the developer-build band first, then why the GP3
   file fits (spec tiles) beside the post-install steering log, what the app
   did (four steps), the app's result block, the return-to-centre bars and
   what changes, what isn't known, then Engine calibration with the Tools →
   Flashing shot.
9. **What it logs** (`#logging`): the eighteen presets from `LogPresets.kt`
   in four group tiles, the note that several read inside this car's exact
   software or need the CAN board, then the graph notes beside the preset
   picker screenshot.
10. **Coding and more** (`#coding`): oil level row, Quick codings (from
   `CodingPresets.kt`) beside its shot, Track settings
   (`#track`, three shots), the coding tiles (Factory LED headlights among
   them), then the eleven modules and the option-size note.
11. **Service tools** (`#service`): the fourteen services from `MainMenu.kt`
    (Tools → Service) in three group tiles beside the Tools → Service shot.
12. **Get it**: closed testing, three steps (group, opt-in, install). Switch
    to one Play button when it goes to production.
13. Screenshot note, privacy line and the independent-app note.

## Standing rules

- Present tense, current build only. No "coming soon".
- **Flashing is debug-build only** (`EpsProgrammingActivity`,
  `SameVersionFlashActivity`, `TcuProgrammingActivity` finish() unless
  `BuildConfig.DEBUG`). Every mention says developer build, on Matt's car.
  If flashing ships in the Play build, change the band, the can't list and
  the What it does tile together.
- GP3 facts come from `docs/EPS-GP3-PROFILE-2026-11-19.md` and the phone's
  `files/eps-programming/activity.log` (installed 2026-11-19 11:37, build 243).
  Don't claim how it feels or that it's safe on the Variant A rack; the doc
  lists that as not established.
- Presets, modules and services must match the app: `LogPresets.kt`
  (18 presets), `ModuleSigningProfiles.kt` (11 modules), `CodingPresets.kt`
  (10 quick codings: 4 cluster, 4 body, 2 head unit), and the service
  chooser (Engine learning, Steering relearn and the 12 `MaintenanceGroup`s).
- The price is set in Play Console; the page says $45 (Matt, 2026-10-17).
- Connection labels match the app's chooser: ENET cable / USB-C, MHD Wi-Fi /
  ENET, OBDLink CX / Bluetooth LE, Paired Bluetooth adapter, Wi-Fi OBD2
  adapter.
- No protocol internals on the page (service IDs, routine numbers, coding
  block numbers). The result block has its internal codes taken out and
  says so.

## Screenshots

In `assets/images/f5xcode/v3/` (2026-11-20, build 248 debug), 720 px wide
(dialog crops 640), dark mode, status and nav bars cropped. Shot on the
`methanol_blog_34` AVD (Pixel 6, API 34) on hermes, with Matt's phone data
copied off by `adb run-as`, read-only:

- `faults.jpg`: the 2026-11-19 16:00 Check all report
  (`reports/1795122001547-fault-check-…`) played back with the debug
  `ReplayTransport` (`--es cmd connect --es adapter replay --es file <log>`,
  then `--es cmd faultscan`). Replay answers each request with its first
  recorded reply, so it shows the scan from just after the engine flash.
  The replay swaps the VIN for `WMW00000000000000`.
- `oil-level.jpg`: the 2026-11-08 oil read
  (`reports/1794159698553-oil-level-…`), with the phone's own full mark
  (69 mm, `shared_prefs/oil_level.xml`) copied in.
- `graph-tuning.jpg`: `logs/f56-tuningmodel-20261119-125936-428.csv` pushed
  into `files/logs`, opened with GraphActivity `--ez replay true`, scrolled
  to the RPM/boost charts and tapped at the 4,274 rpm peak.
- `graph-steering-gp3.jpg`: `logs/f56-eps-20261119-143034-468.csv`, the
  EPS return to center log recorded after the GP3 install, Lines view.
- `logger-presets.jpg`: the preset picker, top and bottom of the list
  stitched.
- `coding-bdc.jpg`, `quick-codings.jpg` and the `track-*.jpg` shots: the phone's `files/backups` with the VIN replaced in
  every file name and file (`WMW00000000000000`), pushed into the AVD.
  `track-brakes.jpg` is `--es cmd codingview --es addr 29 --es scroll
  3000/0042`, `track-ediff.jpg` the same with `--es scroll 3000/0048`,
  `track-edlc.jpg` `--es review 3000/0065 --es value Off`; `coding-bdc.jpg`
  is `--es cmd codingview --es addr 40`; `quick-codings.jpg` `--es cmd quick`.
- `enet-chooser.jpg` and `enet-dialog.jpg`: the screens themselves, no car.
- `tools.jpg`, `coding-modules.jpg`, `tools-service.jpg` and
  `tools-flashing.jpg` (2026-11-20, build 249 debug, the same AVD and
  backups): the Tools tab at the top, the Coding tab, and Tools scrolled to
  the Service and Flashing labels. The debug build shows Flashing and
  Developer; crop Developer out, and keep Flashing only in `#gp3`.
- `cover.jpg` (one level up, 1600 x 900): the faults, tuning and steering
  shots as three phone cards on the right, the left kept dark for the
  title overlay.

The guide's names map onto the F56's DSC as: e-diff 3000/0065, FLR 3000/0045,
fade compensation 3000/0048, HBA = Dynamic Brake Control 3000/0042, brake
standby 3000/0046, DPC = Performance Control 3000/0067; Maximum Brake Support
has no switch.
