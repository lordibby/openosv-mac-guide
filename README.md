# OpenOSV on Mac: a beginner's guide for DaVinci Resolve

Use DJI Osmo 360 `.OSV` files in DaVinci Resolve on an Apple Silicon Mac, using
[OpenOSV](https://github.com/Kemerd/OpenOSV) (Apache-2.0, by Kemerd).

This guide is unofficial. It contains no OpenOSV code, only plain-English steps.
All credit for the plug-in goes to its author.

## Status (be honest about what's verified)

| Step | Status |
|---|---|
| Build + install on MacBook Pro (M5 Pro) | Worked |
| Appears in Resolve, reframing an .OSV on a timeline | Worked (confirmed on M5 Pro MacBook Pro) |
| Audio extraction, HDR colour modes | Not yet tested |

OpenOSV's own docs call Mac support a community-tested preview. If something
breaks, report it at https://github.com/Kemerd/OpenOSV/issues

## What you need

- Apple Silicon Mac (M1 or newer), macOS 13.3 or newer
- DaVinci Resolve (free or Studio)
- About 30 minutes. The first build compiles FFmpeg.
- Mac only builds the **Resolve** plug-in. Premiere on Mac is untested and needs
  Adobe SDKs. VEGAS has no Mac version.

## Why a build is needed

The project's prebuilt downloads are Windows-only, so on a Mac you build it once.

## Install

1. Open **Terminal** (Cmd+Space, type Terminal).
2. Install Apple's developer tools (click Install in the popup):
   ```bash
   xcode-select --install
   ```
3. Install [Homebrew](https://brew.sh) if `brew --version` fails.
4. Download and build. Enter your Mac password when asked.
   ```bash
   git clone https://github.com/Kemerd/OpenOSV.git
   cd OpenOSV
   scripts/build_mac.sh --install-ofx
   ```
   Lots of yellow "warning" lines are normal. Success ends with `==> Done.`
5. Quit Resolve fully (Cmd+Q) and reopen it.

## Use it

Resolve cannot import `.OSV` files into the Media Pool. Use the generator:

1. **Edit** page, **Effects** panel, **Open FX**, search `OpenOSV`.
2. Drag **OpenOSV Source** onto the timeline.
3. Inspector, **Choose .OSV File...**, pick your clip.
4. Drag the generator's end to match the **Clip** length shown in the Inspector.
5. Frame the shot with **Camera**: Pan, Tilt, Roll, FOV, Zoom (all keyframeable).

### Audio

A generator has no audio. Extract it and place it under the clip:
```bash
build/macos-release/bin/osvtool extract /path/to/CLIP.OSV --audio ~/Desktop/CLIP.wav
```
Run that from the `OpenOSV` folder.

## Tips

- Choppy playback: set **Sphere Size** to 4K or 2K, or use proxies.
- Colour defaults to Rec. 709. Change **Colour Output** only for HDR timelines.
- Reframing is CPU-based on Mac, so it is slower than on a Windows NVIDIA PC.

## Uninstall

```bash
cd OpenOSV
scripts/install_ofx.sh --uninstall
```

## Troubleshooting

| Problem | Try |
|---|---|
| OpenOSV not in Open FX | Fully quit and reopen Resolve; select **Open FX** before searching |
| Build stops with an error | Copy the last ~20 lines into an issue |
| Picture is black past the clip | Normal: trim the generator to the clip length |
