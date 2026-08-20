# Proton-Anime

My [GE-Proton](https://github.com/GloriousEggroll/proton-ge-custom) fork, based on
Valve's [Proton](https://github.com/ValveSoftware/Proton), the Windows compatibility
layer for Steam. It improves support for anime-themed gacha games and fixes their
quirky launchers. Several helpful features and integrations are included on top.

> [!Note]
> This project is not affiliated with Valve or with GloriousEggroll.
> Do **not** report issues with Proton-Anime to the Valve or GE-Proton bug trackers.
> If an issue also happens on Proton Experimental or GE-Proton, report it upstream instead.

> [!Warning]
> **Running non-Steam games with Proton-Anime outside of Steam is only supported using
> [umu](https://github.com/Open-Wine-Components/umu-launcher).**
>
> Proton runs in a container with a runtime built specifically for it. Launching it any other
> way makes Wine pick up your system libraries instead, which breaks in
> distribution-specific ways. umu-launcher mimics Steam's containerized runtime so Proton runs
> exactly as it does under Steam. This matters especially for the launcher-based games below
> (HYP titles): use umu, [Lutris](https://lutris.net/) or
> [Heroic](https://heroicgameslauncher.com/), both of which use umu as their backend for
> Proton builds.

## Table of contents

- [Supported games](#supported-games)
- [Installation](#installation)
- [Enabling in Steam](#enabling-in-steam)
- [Building locally](#building-locally)
- [Anime-game specific fixes](#anime-game-specific-fixes)
- [Useful additions](#useful-additions)
- [Options](#options)
- [Credits](#credits)

## Supported games

These are the games this fork is tested against. Status reflects the most recent
Proton-Anime release.

Legend: ✅ working &nbsp;|&nbsp; ⚠️ working with issues &nbsp;|&nbsp; 🧪 untested / needs verification &nbsp;|&nbsp; ❌ broken

| Game | Distribution | Status | Known issues |
| ---- | ------------ | ------ | ------------ |
| WuWa     | Steam / Epic / launcher  | ✅ | — |
| GI       | HYP launcher / Epic      | ✅ | — |
| HSR      | HYP launcher / Epic      | ✅ | — |
| ZZZ      | Steam / HYP launcher / Epic | ✅ | — |
| HI3      | Steam / HYP launcher / Epic | ✅ | — |
| Endfield | Epic / launcher          | ✅ | — |
| PGR      | Steam / launcher         | ✅ | — |
| DNA      | Steam / Epic / launcher  | ✅ | — |
| NTE      | Steam / Epic / launcher  | ✅ | — |
| Nikki    | Steam / Epic / launcher  | 🧪 | — |
| ToF      | Steam / launcher         | 🧪 | — |
| NIKKE    | launcher                 | ⚠️ | Launcher updates probably broken, game works |

> [!Note]
> None of these games officially support Linux, so a residual risk always remains. Whether
> an account is flagged for playing through a compatibility layer is up to the game's
> publisher. Issues are practically unheard of, but use at your own discretion.

Games that are not on Steam (e.g. the HYP titles) must be run through umu, Lutris or
Heroic. See the warning at the top of this README.

## Installation

You must have the proper Vulkan drivers installed on your system. VKD3D on AMD requires
Mesa 22.0.0 or higher. See [here](https://github.com/lutris/docs/blob/master/InstallingDrivers.md)
for general driver installation guidance.

### Steam (native)

1. Download a release tarball from the [Releases](https://github.com/MrLGamer/proton-anime/releases) page.
2. Create the compatibility tools directory if it does not exist:
   ```bash
   mkdir -p ~/.steam/steam/compatibilitytools.d
   ```
3. Extract the release tarball into it:
   ```bash
   tar -xf Anime-Proton*.tar.gz -C ~/.steam/steam/compatibilitytools.d/
   ```
4. Restart Steam.
5. [Enable Proton-Anime in Steam](#enabling-in-steam).

For the Steam Flatpak, use `~/.var/app/com.valvesoftware.Steam/data/Steam/compatibilitytools.d/`
instead. For the Steam Snap it is `~/snap/steam/common/.steam/steam/compatibilitytools.d/`.
Please don't use Steam Snap unless you absolutely have to. 

### Lutris / Heroic

Extract the release tarball into `~/.steam/steam/compatibilitytools.d/` as above and select
Proton-Anime as the Proton/runner version in the game's settings. Both launchers will run it
through umu automatically.

## Enabling in Steam

1. Right click a game in Steam and click `Properties`.
2. In the `Compatibility` tab, check `Force the use of a specific Steam Play compatibility tool`
   and select `Anime-ProtonX-Y`.
3. Launch the game.

## Building locally

If you'd rather build Proton-Anime yourself instead of using a release, this is all it takes.

Requirements: `git`, `make`, `fontforge`, and a container engine (`docker` or `podman`).
The build itself runs inside Valve's Steam Runtime SDK container. Expect the sources and
build to take a lot of disk space and the first build to take a long time.

```sh
# 1. Clone the repository and fetch the submodules (several GB)
git clone https://github.com/MrLGamer/proton-anime
cd proton-anime
git submodule update --init --filter=tree:0 --recursive

# 2. Apply the patches
./patches/protonprep-valve-staging.sh &> patchlog.txt

# 3. Check the patch log for failures (no output = all good)
grep -i -e fail -e error patchlog.txt

# 4. Configure and build from a sibling build directory
cd .. && mkdir build && cd build
../proton-anime/configure.sh --build-name=anime-proton-localbuild
make redist

# 5. Install the finished build into Steam
tar -xf anime-proton-localbuild.tar.gz -C ~/.steam/steam/compatibilitytools.d/
```

Restart Steam afterwards and select `anime-proton-localbuild` as described in
[Enabling in Steam](#enabling-in-steam).

If you want to add your own Wine patches: drop them into `patches/`, add a patch line for
them in `patches/protonprep-valve-staging.sh` under `#WINE CUSTOM PATCHES` in the same way
the others are done, and rebuild.

## Anime-game specific fixes

This fork's per-game hacks come with their own environment variables, separate from
the inherited GE-Proton options below:

- **NTE** launcher hiding
- **HSR** and **GI** frame rate unlocks
- **WuWa** `UserEngine.ini` console variables

See **[Anime-game specific fixes](GAMEFIXES.md)** for the full list.

## Useful additions

Proton-Anime can also download and set up some useful third-party tools automatically,
with no manual DLL copying or injectors needed:

- **OptiScaler** replaces a game's built-in upscaler, including FSR 4 on RDNA4 cards
- **XXMI model importers** run 3dmigoto-based model mod loaders natively
- **ReShade** adds post-processing effects, standalone or alongside XXMI

See **[Useful additions](ADDITIONS.md)** for setup and options.

## Options

Proton-Anime inherits all GE-Proton features: media foundation patches for video playback
(the anime games rely heavily on this for cutscenes), AMD FSR, NVIDIA CUDA/NVAPI support,
raw input, the protonfixes system, NTSync, wine-wayland, and HDR.

See **[Options](OPTIONS.md)** for the useful launch options and NTSync setup.

## Credits

Proton-Anime is a fork of [GE-Proton](https://github.com/GloriousEggroll/proton-ge-custom)
by **GloriousEggroll (Thomas Crider)**. Virtually all of the heavy lifting in this
repository is his and the upstream Proton/Wine communities' work. If you find this useful,
consider supporting him: https://www.patreon.com/gloriouseggroll

Further credits inherited from upstream:

- **Valve**: [Proton](https://github.com/ValveSoftware/Proton) itself
- **TKG (Etienne Juvigny)**: Wine/Proton patch rebases ([wine-tkg](https://github.com/Frogging-Family/wine-tkg-git))
- **doitsujin (Philip Rebohle)**: [DXVK](https://github.com/doitsujin/dxvk)
- **HansKristian-Work (Hans-Kristian Arntzen)**: [vkd3d-proton](https://github.com/HansKristian-Work/vkd3d-proton)
- **Joshua Ashton**: D9VK and DXVK contributions
- **Guy1524 (Derek Lesho)**: raw input and Media Foundation work
- **flibitijibibo (Ethan Lee)**: [FAudio](https://fna-xna.github.io/)
- **simons-public (Chris Simmons)**: the original [protonfixes](https://github.com/simons-public/protonfixes)
- **The wine-staging maintainers**: Alistair Leslie-Hughes, Zebediah Figura, Paul Gofman

See [LICENSE](LICENSE) and [LICENSE.proton](LICENSE.proton) for licensing.

Patches and code original to this repository are provided under the license of
the component they modify (LGPL-2.1-or-later for Wine, BSD for Proton and
protonfixes).
