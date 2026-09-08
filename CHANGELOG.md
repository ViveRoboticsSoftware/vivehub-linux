# Changelog

## v1.0.3

- The packages now include HTC's licence notice (`LICENSE.txt`) and the
  third-party licence terms (`ThirdPartyLicenses.txt`).

## v1.0.2

- Faster map sync between the host tracker and its clients.
- The host tracker is marked with a `*` after its serial in the UI.
- Fixed a garbled build-environment line in the arm64 package's `README.txt`.

## v1.0.1

- SDK renamed to VUT (VIVE Ultimate Tracker).
- Fixed client trackers getting stuck at "Setup required" or "Syncing": the
  host WiFi query is now retried, and a stale `map_state` is discarded instead
  of being trusted.

## v1.0.0

First Linux beta of VIVE Hub for VIVE Ultimate Tracker.

- Tracker pairing and status monitoring over the USB dongle.
- Tracking-map build wizard, launched from **Start setup**.
- Multi-tracker support: one tracker acts as the map host, the rest sync from it.
- Per-tracker power off, and factory reset.
- C++ SDK (`sdk/`) for reading tracker poses from your own application, with a
  runnable example.
- Packages for x86_64 and ARM64. On ARM64 the x86_64 map wizard runs under
  box64, installed offline by `install-box64.sh`.
- Bundled `libglfw` and `libhidapi-hidraw`, so only the system graphics stack
  is required.
- Timestamped per-launch log files under
  `~/.local/share/HTC/VIVEUltimateTracker/logs/`, newest 10 kept per component.

Built on Ubuntu 20.04.6 (glibc 2.31); requires that version or newer.
