# Changelog

All notable changes to PortScope are documented here.

This project follows [Semantic Versioning](https://semver.org): a license covers
one full major version (every 1.x release), so the major number changes only when
a new, separately-licensed version ships.

---

## [1.0.6] — 2026-10-16

### Speed Test

The Speed Test was rebuilt around one question: how fast does this drive go,
for how long, and what stops it?

- **Accurate on fast drives.** The video test now keeps several requests in
  flight on APFS volumes, the way a real copy does. One request at a time held
  fast NVMe drives back: an internal SSD that reads 6.9 GB/s measured 5.8. On a
  WD_BLACK SN850X over a 40 Gbps link the results now match AmorphousDiskMark's
  queue-depth-8 figures within 0.5% for reads and 2.7% for writes. Other file
  systems keep writing one request at a time, which measured faster on exFAT
  and Mac OS Extended.
- **Sustained runs stop on their own.** Choose how much the test may write
  ("Write at most"). It stops sooner once the speed has settled after the
  drive's cache fills, keeps going while the drive is still heating up, then
  reads for 30 seconds.
- **See where the drop happens.** The throughput chart marks each drop in write
  speed with the time, how much had been written, the drive's temperature and
  the speed either side. A second drop (a drive slowing again as it starts
  folding data out of its cache) gets its own marker, and the point where
  thermal throttling began is marked in orange. Write and read both start at
  zero so their lengths compare at a glance, with each pass's duration and the
  total underneath.
- **Burst or sustained, never confused.** A quick test fits inside the drive's
  fast cache, so its results are labelled "Burst" and only a sustained run's
  are called "Sustained".
- **Judged against what the connection can really carry.** The verdict now
  finds the slowest part of the chain (the port's link, the drive's own USB or
  PCIe connection, or a hub) and compares the result with what that kind of
  link typically delivers: "3,433 MB/s, about 99% of what a 40 Gbps link
  delivers", or "the drive is the limit, not the cable" for a 10 Gbps drive
  on a faster port. The internal drive is described as a baseline instead of
  asking which port its cable is in.
- **Warns when a result may be off.** A run is flagged when something else
  used the drive at the same time (another app, Spotlight, Time Machine),
  when some reads came from memory, or when Low Power Mode, battery power or a
  hot Mac may have held it back. Low Power Mode is also flagged before you
  start.
- **Records the drive's state.** Each result notes the file system, how full
  the drive was, encryption and TRIM, and adds a note when one of them changes
  how to read the result (an SSD's fast cache shrinks as it fills).
- **Drives that don't report temperature are named as such**, on the results
  and in history, instead of leaving the reading blank.
- **No more "0 W" for drives powered by a dock** or their own supply.
- **Rescan clears the last result** off the Speed Test page.
- **Short tests run long enough to measure.** Each pass now runs for at least
  six seconds, so a 1 GB test on a fast drive is no longer over before it
  takes a reading.

### Speed Test History

- **Renamed from "History"**, since it only holds speed tests.
- **Tick boxes and Select All.** Export the ticked tests as one PDF report
  (a page per test) or one CSV, or delete them.
- **Laid out like the rest of the app**, in cards: write and read up top in
  the chart's colours, then the run, the connection, the drive and the
  verdict. The expanded charts have their own cards and titles.
- **Curves keep their dips.** Long runs are stored and drawn with every stall
  intact instead of being thinned past them.
- **Older results are marked.** Video tests saved before this version ran one
  request at a time and read low on fast drives, so they carry a note saying
  to compare them loosely.

### Fixed

- **Tether Throughput no longer fails silently.** A capture folder that has
  gone missing (typically an external drive that remounted under a new name,
  such as `Backup 1` beside `Backup`) or that macOS won't let PortScope read
  now shows a warning instead of a card reading 0 MB/s forever. When the folder
  returns, files already in it are no longer counted as newly arrived frames.
- **A very large capture folder** (over 20,000 files) now says that newer
  frames may be missed, rather than quietly under-counting.
- **An idle Thunderbolt port no longer reads as linked.** A port whose
  Thunderbolt/USB4 link has gone inactive, or that has nothing connected, no
  longer shows its last link speed.
- **"No e-marker" no longer implies a cheap cable.** Through a charger macOS
  usually can't read a cable's e-marker at all, and the Cable card now says so
  instead of suggesting a USB 2.0 or charge-only cable. With a device
  attached, it notes that ordinary 3 A cables aren't required to carry one.
- **Devices behind older Thunderbolt 3 docks appear on their port again.**
  Docks such as the CalDigit TS3 Plus bring their own USB controllers, and
  everything plugged into them (drives, card readers, audio) was missing from
  the port's device list.
- **No false "DSC on" for TVs.** A display taking a non-RGB signal (TVs often
  use reduced colour such as 4:2:0) is no longer assumed to need full RGB
  bandwidth, so PortScope no longer infers compression it can't see.

### Added

- **No-frames hint:** if a camera has been connected for a minute and not one
  frame has landed in the capture folder, the Tether Throughput card suggests
  checking that your tethering app is saving to it.
- **A charger's contract can prove the cable.** Chargers may only offer more
  than 3 A over a cable they've confirmed is rated for 5 A, so a contract
  above 3 A now shows the cable as "At least 5 A" on the Power Contract card,
  even when macOS never read its e-marker.
- **Docks named by Thunderbolt.** A Thunderbolt dock that doesn't announce
  itself over USB (such as the CalDigit TS3 Plus) now gets its own row, with
  everything plugged into it listed underneath. This only happens when it's
  the one Thunderbolt device on the port, so a daisy chain never files one
  box's devices under another.
- **"Linked to another Mac".** A Thunderbolt cable to another Mac now says so
  in the sidebar and names the far end's model on the Negotiated Link card,
  instead of reading like a cable waiting for a device.

---

## [1.0.5] — 2026-09-29

### Fixed

- **Cable no longer blamed for a slower device.** A 5 Gbps drive on a
  10 Gbps-rated cable is now reported as the device being the limit, not a
  cable fault, and a 10 Gbps USB drive on a 40/80 Gbps cable is no longer
  graded "limited" by the speed test.
- **Untrained USB links** no longer read as the connector's capability rate
  (a USB 2.0 device could show "10 Gbps link up").
- **Cable power rating** now follows the spec: above 20 V only counts for an
  EPR-marked 5 A cable, and the 30 V / 40 V codes are decoded. A cable's
  length is no longer shown for the open-ended "long" latency code.
- **Cable health** keeps a separate baseline per port, so two identical
  cables plugged in together both record errors.
- **Random-write speed** no longer includes the final flush in its timing.
- **History files** that can't be read are set aside (`*.corrupt-…json`)
  instead of being overwritten by the next result.
- **Licence check** no longer revokes a licence on a server error reply.
- Charger profiles show fractional amps correctly (2.25 A, not 2.2 A).

### Added

- **Reference intro:** the first time the Reference page opens, a short card
  says what it's for (speeds and names, power, glossary), with a "Don't show
  this again" checkbox.
- **System notifications**, off by default, in Settings ▸ Notifications. A
  master switch (which is what asks macOS for permission) plus a switch for
  each kind: device or display connected / disconnected, cable warnings (an
  e-marker that contradicts itself, or a cable rated below the charger),
  link errors and overcurrent, power adapter connected or removed, an adapter
  too weak to charge, a drive passing 65 °C, a speed test finishing in the
  background, and a new PortScope release. Repeats are held back, a
  dock plugging in gives one banner rather than one per device, and clicking
  a banner brings the window back.
- **Automatic update check**, off by default: Settings ▸ About. Once a day
  while PortScope runs; a new version is announced once.
- **Erase** for speed test history and cable tester history in Settings ▸
  Data, alongside the cable health record and display names.
- **Speed Test keeps the Mac awake** for the length of a run, so a long
  sustained test isn't cut short by idle sleep (and App Nap can't skew the
  timing).
- **Speed Test clears leftovers:** test folders left on a drive by a run that
  crashed or was force-quit are deleted before the next run.
- **Battery warning** on the Speed Test page when the Mac is running on
  battery, stronger for a sustained test.

### Changed

- **Buy** buttons now open the checkout page directly.
- **Text is a little larger** throughout: small captions went from 10 to 11 pt
  and supporting text from 12 to 13 pt, closer to the system's own size. The
  menu-bar popover and Settings window are slightly wider to match.
- **Reference** corrected after an accuracy review: the 40 and 80 Gbps rows use
  the USB-IF's current names ("USB 40Gbps", "USB 80Gbps"), the 80 Gbps cable
  note no longer implies any 40 Gbps cable will do, the Thunderbolt/USB4
  storage speed is ~3,000 MB/s rather than 4,000, charge-only cables are no
  longer said to carry USB 2.0, and the XID, e-marker, billboard, connect
  count and SuperSpeed glossary entries were corrected.
- **Settings ▸ Speed Test** offers the same test sizes as the Speed Test page
  (it listed 32 GB, which the page doesn't, and had no sustained sizes), and
  hides size and sustained for the 4K responsiveness test.
- **Settings ▸ Data** lists your saved display names and can forget them.
- **Help** corrected: the Cable Tester never runs over Wi-Fi or a VPN, both
  Macs need the same version, and the privacy note now mentions licence
  validation. New entries for the wiggle test, unusual e-marker values,
  tethered shooting and Export Report.
- **Settings** now has a left sidebar (General, Notifications, Help, About)
  instead of tabs across the top, without a collapse button.
- **Contrast:** de-emphasised text, yellow status text and the throttle-chart
  labels now use the AA-checked palette in light and dark mode.
- About now links to the Faini Made Software Policy.

---

## [1.0.4] — 2026-09-23

### Added

- **Vendor names** — cable, device and charger vendor IDs now show the
  company name (e.g. "Apple Inc. (0x05AC)"): first from the USB-IF list
  macOS ships, then from the Linux USB ID Repository (bundled, used under
  the BSD 3-clause licence; see Settings ▸ About ▸ Acknowledgments), and
  otherwise the hex ID.
- **Charger profiles** — every voltage the charger offers is listed, with the
  one in use marked and any the cable can't carry at full current flagged.
- **Battery state** — charging, full, charging paused (Optimized Battery
  Charging, a charge limit or temperature), or not charging because the
  adapter is too weak. Shown on the port page, in the sidebar and in the
  menu bar.
- **Unusual e-marker values** — the Cable card flags a cable whose chip
  contradicts itself: no vendor ID, EPR claimed on a 3 A or 20 V cable, a
  passive 40/80 Gbps cable claiming over 2 m, reserved codes, or a chip that
  doesn't identify as a cable.
- **Display signal** — for external monitors: the resolution actually being
  sent (not just the desktop mode), whether it's the panel's native
  resolution, bit depth, and whether DSC compression is on, with the
  bandwidth the mode needs against what the link carries.
- **Charging-path resistance** — an estimate of the resistance between the
  charger and the Mac, from how the voltage sags as the load changes.
- **Per-row sources** — the Power and Power Contract cards now tag each row
  with where it came from (charger, cable chip, negotiated, measured).

---

## [1.0.3] — 2026-09-23

### Added

- **Cable Tester results saved on both Macs** — after a test, either Mac can
  name and save the result, and it's recorded in the history on both. The
  host now gets the Save card and verdict too (previously only the Mac that
  started the test did).

### Fixed

- **Tether "Average frame" too small** — a frame was sized while its file was
  still being written, and sidecar files (Capture One settings/thumbnails,
  `.xmp`) and RAW+JPEG pairs counted as extra frames. Frames now use their
  finished size, only image files count, and a RAW+JPEG pair is one frame.
- **Tether "Per frame" inflated by breaks** — a pause between sets no longer
  skews the time between shots.
- **Cable Tester directions on the host** — the host showed Send and Receive
  swapped; both Macs now report them from their own side.

> Both Macs must run 1.0.3 to use the Cable Tester together.

---

## [1.0.2] — 2026-08-30

### Added

- **Cable Tester troubleshooting** — a new section in Settings ▸ Help covering
  the common two-Mac test failures, plus a standalone guide at
  [portscope.fainimade.com/cable-tester-troubleshooting](https://portscope.fainimade.com/cable-tester-troubleshooting)
  (source in `docs/cable-tester-troubleshooting.html`).
- **Cable Tester wired-link check** — the page warns when there's no
  Thunderbolt / USB4 link to another Mac, and flags a host that was found
  over Wi-Fi only (the test can't use it).

### Fixed

- **USB 3 link speed wrong on some Macs** — a 10 Gbps drive could show as
  "5 Gbps" (and, on another port, a 5 Gbps hub as "10 Gbps"). The negotiated
  link was read from the USB-C connector's capability rather than the rate
  the link actually trained at; it now reads the real link rate. This
  corrects the port's speed everywhere it appears — the sidebar, the
  Negotiated Link card, "why is this slow?", the bandwidth budget, and the
  Speed Test ceiling. Seen on Mac Studio; any Mac with standalone USB 3
  ports could be affected.
- **Cable Tester reliability** — the test now pins its connection to the exact
  wired interface the host was discovered on, instead of letting the OS pick
  among every wired interface (or drift toward Wi-Fi). Failure messages name
  the real cause — link renegotiating, no wired route yet, connection refused —
  rather than always suggesting a role swap.

---

## [1.0.1] — 2026-08-20

### Added

- **Live Data usage percentages** — each storage/network row now shows its
  share of the port's own link ceiling, plus a total-usage bar for everything
  moving through the port at once.
- **Live Data volume names** — a storage row now leads with its mounted
  volume name ("2TB") instead of the drive's model, with the model demoted
  to a subtitle, matching the menu bar.

### Fixed

- **Menu bar eject button** — was missing for Thunderbolt/USB NVMe drives
  (the kind of external SSD enclosure macOS reports as "Fixed" rather than
  removable media); it now appears for any external drive, matching the
  main window's behavior.

---

## [1.0.0] — 2026-08-18

First public release. Everything below is free unless marked **Licensed**.

### Identity and inspection

- **Cable identity** — reads a USB-C cable's e-marker and USB Power Delivery
  identity (SOP/SOP′/SOP″): passive or active, per-lane speed, maximum current
  and voltage, vendor and cable ID.
- **Port inventory** — every physical port, including USB-C, HDMI, SD card,
  MagSafe, wired Ethernet, and the headphone jack, each with the link it
  negotiated.
- **Device topology** — a Compact view that folds a dock's internal hub stages
  into one row, and a Detailed view showing every node. Devices are classified
  by USB-IF class code rather than product name.
- **Live data monitor** — per-device read/write throughput, live network bytes,
  and DisplayPort lane allocation, all passive.
- **Power** — adapter rating, cable ceiling, negotiated PD contract, and live
  draw kept distinct, plus each device's requested USB current budget.
- **Cable data path** — distinguishes a data cable from a charge-only cable and
  a plain charger, and flags a link stuck at USB 2.0 when the e-marker promised
  more.
- **Dock bandwidth budget** — shows whether video is tunnelled, sharing the data
  budget, or on dedicated lanes.
- **Bottleneck verdict** — one sentence naming the binding cause of a slow link,
  including a fast drive sitting in a dock's USB 2.0 socket.
- **Displays** — EDID identity, resolution and refresh joined to the exact
  DisplayPort link, and full EDID 1.4 decode with hex dump.
- **Volumes and drive health** — mounted volumes, free space, drive temperature,
  and NVMe health for internal and Thunderbolt/USB4 NVMe drives.
- **Raw data** — USB device and interface descriptors.
- **Reference library** — USB and Thunderbolt speed tiers with their full rename
  history, the USB-PD power ladder from 5 W to 240 W, and a searchable glossary,
  with rows matching your live hardware badged.
- **Menu bar** — a live tree of everything connected, grouped by hub, with power,
  throughput, and volume names, plus one-click eject for removable drives.
- **Diagnostic report** — a one-page shareable Markdown summary with identifying
  details redacted by default.
- **First-run tour and inline help** throughout.

### Verification and testing

- **Speed Test** *(Licensed)* — writes then reads a real file to measure true
  sustained throughput, with a live curve, drive-temperature telemetry, SLC
  write-cache sizing, thermal-throttle detection, saved history, and CSV export.
- **Cable Tester** *(Licensed)* — streams data memory-to-memory between two Macs
  over the cable under test, so on a Thunderbolt port the cable is the only
  possible bottleneck. Requires a license on both Macs.
- **Cable Health over time** *(Licensed)* — tracks each cable's link-error
  counters across sessions, correctly handling counter resets on re-enumeration.
- **Wiggle Test** *(Licensed)* — a guided intermittent-fault test sampling at
  5 Hz across every port stage, localizing a fault to a connector.
- **Rename Display** *(Licensed)* — your own name for a monitor, keyed to its
  EDID so it survives replugs. Never writes to the display.

### Licensing

- One license activates on up to two Macs and covers one full major version for
  life, with no subscription.
- A Mac can be deactivated from Settings to free its seat for another machine.
- Online activation with a 14-day offline grace period.

### Requirements

- Apple Silicon Mac (M1 or later), macOS 15 (Sequoia) or newer.

---

<!--
Template for future releases:

## [1.1.0] — YYYY-MM-DD

### Added
### Changed
### Fixed
### Removed
-->
