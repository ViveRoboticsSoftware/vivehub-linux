# VIVE Hub for Linux (Beta)

*VIVE Hub for Linux* is the desktop app, currently released as a beta, for setting up and monitoring HTC VIVE Ultimate
Tracker on Linux both x86 and arm64. Supports pairing trackers to the dongle, building the tracking map,
and exposing tracker poses to your own application through a C++ SDK.

The beta is being developed on Ubuntu, so we recommend an Ubuntu setup (see below). Other distros should work, but they haven't been tested.

This repository is used only for issue tracking and publishing releases.
Pre-built packages are attached to each [release](../../releases/latest).

## Download

| Platform | Package |
|---|---|
| Intel/AMD 64-bit | `VIVEHub-Linux-<version>-x86_64.tar.gz` |
| ARM 64-bit | `VIVEHub-Linux-<version>-arm64.tar.gz` |

Not sure which one? Run `uname -m` — `x86_64` means the Intel/AMD package,
`aarch64` means the ARM package.

Get them from the [latest release](../../releases/latest). To verify a
download against the `.sha256` file published alongside it:

```sh
sha256sum -c VIVEHub-Linux-*.sha256
```

## Recommended Setup

Ubuntu 20.04 or newer, on a desktop (graphical) session.

`libglfw` and `libhidapi-hidraw` are bundled inside the package — you do not
need to install them. Everything else is deliberately **not** bundled and must
come from the system: the C++ runtime (`libstdc++`/`libgcc`) and the
GPU/display stack (`libgl1`, `libx11-6`, the Mesa DRI drivers, `libudev`,
glibc). Bundling those breaks GL driver loading.

On a minimal or headless image, install the graphics stack first:

```sh
sudo apt install libgl1 libx11-6 libgl1-mesa-dri
```

The packages are built on Ubuntu 20.04.6 (glibc 2.31) and require that glibc /
libstdc++ version or newer on the target machine.

## Install

```sh
tar xzf VIVEHub-Linux-*-x86_64.tar.gz
cd VIVEHub-Linux
sudo ./install-udev.sh        # one-time USB permission setup
```

Then **unplug and replug the tracker dongle** so the new permission applies.

On ARM64 there is one extra one-time step. The tracking-map wizard is an
x86_64 Unity build, so it runs under box64:

```sh
sudo ./install-box64.sh       # ARM64 only, works offline
```

## Run

```sh
./run-vivehub.sh
```

Always launch through `run-vivehub.sh` — it points the loader at the bundled
`lib/`. The UI starts the `tracker_server` dongle daemon for you; do not run
the two separately.

Pressing **Start setup** in the UI opens the map-build wizard, which connects
back to the daemon.

## Reading tracker poses from your own application

The package ships the VUT (VIVE Ultimate Tracker) C++ SDK under `sdk/` —
headers in `sdk/include/vut/`, static library `sdk/lib/libvut_sdk.a`. With
VIVE Hub running and a tracker tracking:

```sh
cd sdk/examples && make && ./vut_demo
```

See `sdk/README.md` in the package for the API.

## Reporting a problem

Please [open an issue](../../issues/new) and attach logs. VIVE Hub writes a new
timestamped log file per launch to:

```
~/.local/share/HTC/VIVEUltimateTracker/logs/
```

Attach the newest file of each component:

```
vivehub_YYYYMMDD_HHMMSS_<pid>.log         the UI
tracker_server_YYYYMMDD_HHMMSS_<pid>.log  the dongle daemon
```

Log retention will keep the latest 10 log files for each component and older ones are deleted, so
please grab them before launching again a few more times. To watch a run live,
`tail -f` the newest file.

Include your distribution and version, the output of `uname -m`, and which
package you installed.

## Licensing

Your use of VIVE Hub is governed by the **VIVE Product EULA**, linked from
`LICENSE.txt`.

- `LICENSE.txt` — HTC's copyright and licence notice for VIVE Hub.
- `ThirdPartyLicenses.txt` — the third-party components the Linux packages
  include and their respective licence terms. You must comply with those terms
  while using the identified third-party software.

Both files also ship inside every release tarball.
