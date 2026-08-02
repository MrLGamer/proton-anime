---
name: Compatibility Report
about: Report an issue with a game from the supported games list.
---

THIS ISSUE TRACKER ONLY COVERS THE GAMES LISTED IN THE SUPPORTED GAMES TABLE IN THE README.

IF YOUR GAME IS NOT ON THAT LIST, PLEASE OPEN A "GAME SUPPORT REQUEST" INSTEAD OF A COMPATIBILITY REPORT.
FOR ISSUES WITH ANY OTHER GAME, PLEASE USE THE UPSTREAM PROJECTS INSTEAD:
- GE-Proton: https://github.com/GloriousEggroll/proton-ge-custom/issues
- Valve Proton: https://github.com/ValveSoftware/Proton/issues

# Compatibility Report
- Name of the game (must be on the supported games list):
- Steam AppID of the game (if on Steam):
- Launcher used (Steam / umu / Lutris / Heroic / other):

## System Information
- GPU: <!-- e.g. RX 580, RX 7900 XT, RTX 4070 -->
- Driver/LLVM version: <!-- e.g. Mesa 25.2.6 / LLVM 21.0.0 or NVIDIA 575.64 -->
- Kernel version: <!-- e.g. 6.15.6 -->
- Distro version: <!-- e.g. Fedora 42, Bazzite, Arch, Nobara, Ubuntu 26.04 -->
- Desktop session: <!-- e.g. KDE Wayland, GNOME Wayland, X11 -->
- Link to full system information report as [Gist](https://gist.github.com/):
- Proton-Anime version:

## Proton comparison
<!-- Please test with a clean prefix where possible. If a game has DRM/anti-tamper limits, mention that. -->
- Works with Proton Experimental: <!-- yes/no/not tested -->
- Works with Proton Experimental bleeding-edge: <!-- yes/no/not tested -->
- Works with latest Valve Proton stable: <!-- yes/no/not tested/version -->
- Works with previous GE-Proton version: <!-- yes/no/not tested/version -->
- Last known working GE-Proton version, if any:
- First known broken GE-Proton version, if any:

## Launch options
```text
<!-- Paste the exact Steam launch options used, e.g. PROTON_LOG=1 %command% -->
```

## I confirm:
- [ ] that the game is listed in the supported games table in the README.
- [ ] that I haven't found an existing compatibility report for this game.
- [ ] that I have checked whether there are updates for my system available.
- [ ] that I have tested with a clean prefix or explained why I could not.

<!-- Please add `PROTON_LOG=1 %command%` to the game's launch options and drag
and drop the generated `$HOME/steam-$APPID.log` into this issue report. -->

<!-- For media/audio/video regressions, please consider adding a focused WINEDEBUG log if requested by the maintainer, for example:
PROTON_LOG=1 WINEDEBUG="+seh,+dmo,+mfplat,+quartz,+strmbase,+mfreadwrite,+xaudio2,+mmdevapi,+dsound,+winmm,+pulse,+err" %command%
-->

## Symptoms <!-- What's the problem? -->


## Reproduction


<!--
1. You can find the Steam AppID in the URL of the shop page of the game.
   e.g. for `The Witcher 3: Wild Hunt` the AppID is `292030`.
2. You can find your driver and Linux version, as well as your graphics
   processor's name in the system information report of Steam.
3. You can retrieve a full system information report by clicking
   `Help` > `System Information` in the Steam client on your machine.
4. Please copy it to your clipboard by pressing `Ctrl+A` and then `Ctrl+C`.
   Then paste it in a [Gist](https://gist.github.com/) and post the link in
   this issue.
-->
