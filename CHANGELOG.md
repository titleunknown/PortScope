# Changelog

All notable changes to PortScope are documented here.

This project follows [Semantic Versioning](https://semver.org): a license covers
one full major version (every 1.x release), so the major number changes only when
a new, separately-licensed version ships.

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
