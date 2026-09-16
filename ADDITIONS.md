# Useful additions

Optional quality-of-life tools Proton-Anime can download and set up automatically, with
no manual DLL copying and no injectors. All of them hook into the game's graphics
pipeline at runtime and only change what ends up rendered on screen. Game files are
never modified. Nothing here interacts with gameplay, game logic or network
traffic. These are not cheating tools:

- [OptiScaler](#optiscaler) brings modern upscalers (FSR 4, DLSS, XeSS) to GPUs and
  games that don't officially support them
- [XXMI model importers](#xxmi-model-importers) swap character
  models and textures on the GPU, including higher resolution assets than the anime
  games' mobile-derived defaults
- [ReShade](#reshade) adds post-processing to make often washed-out games look pretty
  again

Everything is opt-in via environment variables. See
[Anime-game specific fixes](GAMEFIXES.md) for the per-game hacks and
[Options](OPTIONS.md) for the inherited GE-Proton options.

> [!Note]
> Third-party tools like these are almost never officially supported by any game.
> Contrary to recurring rumors, there is no confirmed case of the tools listed here
> being the root cause of an account suspension. Cheating tools and anti-cheat bypasses were,
> and none of the tools here is one of them. Using them through Proton-Anime is no
> different from using them on Windows, where issues are practically unheard of. Still,
> use them at your own discretion. Should a game ever start taking action against one
> of these tools, it will be blacklisted for that game.

## OptiScaler

[OptiScaler](https://github.com/optiscaler/OptiScaler) replaces a game's built-in upscaler
(DLSS/FSR/XeSS inputs) with any other upscaler. Proton-Anime ships GE-Proton's native 
OptiScaler integration, so no manual DLL setup is needed:

| Environment Variable | Description |
| :-------------------- | :----------- |
| <tt>PROTON_USE_OPTISCALER</tt> | Set to `1` to download OptiScaler and load it into the game (deployed to `<prefix>/drive_c/windows/system32/umu/`, loaded as `dxgi.dll`). Set to a version number to pin an OptiScaler release. Automatically pulls in the DLSS/XeSS/FFX upgrade DLLs. |
| <tt>PROTON_OPTISCALER_NAME</tt> | DLL name OptiScaler is loaded as (default `dxgi.dll`). Change it if a game needs a different shim, e.g. `winmm.dll`. |
| <tt>PROTON_OPTISCALER_CONFIG</tt> | Semicolon-separated `Section.Option=Value` pairs applied to `OptiScaler.ini`, e.g. `Upscalers.Dx11Upscaler=fsr31;Upscalers.Dx12Upscaler=dlss`. Only options that already exist in the ini are accepted, and the changes persist in the prefix. |

Press `Insert` in-game to open the OptiScaler overlay.

## XXMI model importers

Proton-Anime loads the [XXMI](https://github.com/SpectrumQT/XXMI-Launcher) model
importers (3dmigoto-based mod loaders) natively. No XXMI Launcher, no injector, no
files in the game folder. Set `PROTON_USE_XXMI=1` and the next launch detects the game,
downloads the matching importer plus the shared
[XXMI libs](https://github.com/SpectrumQT/XXMI-Libs-Package) from GitHub, deploys them
into the prefix at `drive_c/xxmi/<IMPORTER>/` and loads 3dmigoto in place of the game's
`d3d11.dll`. Each game also has its own variable for cases auto-detection cannot tell
apart (several games under one multi-game HYP launcher):

| Environment Variable | Game |
| :-------------------- | :---- |
| <tt>PROTON_USE_GIMI</tt> | GI ([GIMI](https://github.com/SilentNightSound/GIMI-Package)) |
| <tt>PROTON_USE_SRMI</tt> | HSR ([SRMI](https://github.com/SpectrumQT/SRMI-Package)) |
| <tt>PROTON_USE_ZZMI</tt> | ZZZ ([ZZMI](https://github.com/leotorrez/ZZMI-Package)) |
| <tt>PROTON_USE_HIMI</tt> | HI3 ([HIMI](https://github.com/leotorrez/HIMI-Package)) |
| <tt>PROTON_USE_WWMI</tt> | WuWa ([WWMI](https://github.com/SpectrumQT/WWMI-Package)) |
| <tt>PROTON_USE_EFMI</tt> | Endfield ([EFMI](https://github.com/SpectrumQT/EFMI-Package)) |

3dmigoto only works under D3D11. WuWa and Endfield are handled automatically when the
game exe is launched directly, but when going through their launchers you have to
enable the DX11 compatibility checkmark in the launcher settings. The other games run
D3D11 by default.

**Mods** go into `~/.local/share/xxmi/<IMPORTER>/Mods/` (under Flatpak Steam,
`~/.var/app/com.valvesoftware.Steam/data/xxmi/`). The folder is shared between prefixes
and survives prefix deletion. The deployed importer itself (`d3dx.ini`, `ShaderFixes/`,
logs) is symlinked next to it as `Importer`. Press `F10` in-game to hot-reload mods. Set
`PROTON_XXMI_MODS_PATH=/some/dir` for a custom location, or `=0` to keep mods in a
plain folder inside the prefix.

**Updates** are automatic: every launch checks GitHub and redeploys newer importer or
libs releases. Your `Mods/` folder and `d3dx_user.ini` are never touched. To freeze or
roll back, set the variable to a version number instead of `1`
(e.g. `PROTON_USE_EFMI=1.3.0`), and `PROTON_XXMI_LIBS=<version>` pins the shared libs.

**In-game settings** most mods expect: for GIMI turn `Dynamic Character Resolution`
off, for ZZMI set `Character Quality` to High and disable `High-Precision Character
Animation`, and for WWMI disable the Wounded Effect if modded textures break after
taking hits. WWMI mods may also ask for LOD variables in the game's `UserEngine.ini`,
which are not written automatically. Set them through the `PROTON_WUWA_ENGINE_VARS`
game fix (see [Anime-game specific fixes](GAMEFIXES.md)).

**For modders**, the 3dmigoto settings from the XXMI Launcher settings screen exist as
environment variables, applied on every launch (unset means the launcher defaults):

| Environment Variable | XXMI Launcher setting | Effect |
| :-------------------- | :--------------------- | :------ |
| <tt>PROTON_XXMI_MUTE_WARNINGS=0</tt> | Mute Warnings (default on) | Show 3dmigoto's ini parser warning overlay again |
| <tt>PROTON_XXMI_HUNTING=1</tt> | Enable Hunting | Shader hunting mode + overlay (`Numpad 0` toggles it in-game) |
| <tt>PROTON_XXMI_DUMP_SHADERS=1</tt> | Dump Shaders | Marking a shader while hunting also dumps its HLSL/ASM to `ShaderFixes` |
| <tt>PROTON_XXMI_CACHE_SHADERS=1</tt> | Cache Shaders | Cache patched shaders as `.bin` in `ShaderCache`, removes shader loading stalls |
| <tt>PROTON_XXMI_CALLS_LOGGING=1</tt> | Calls Logging | Log D3D11 calls to `d3d11_log.txt` |
| <tt>PROTON_XXMI_DEBUG_LOGGING=1</tt> | Debug Logging | Verbose debug output in `d3d11_log.txt` |
| <tt>PROTON_XXMI_DELAY=&lt;ms&gt;</tt> | XXMI Delay | 3dmigoto initialization delay in ms (defaults: WWMI `500`, others `0`) |

`PROTON_USE_GIMI_DEV=1` instead deploys SilentNightSound's original
[GIMI v7 dev build](https://github.com/SilentNightSound/GI-Model-Importer/releases/tag/v7.0)
as a game-agnostic debug injector with hunting already enabled (the `PROTON_XXMI_*`
toggles above don't apply to it, mods live in `.../xxmi/GIMI-DEV/Mods/`).

## ReShade

[ReShade](https://reshade.me/) adds post-processing effects (color grading, sharpening,
etc.) to any game. Proton-Anime extracts it from the official setup exe and deploys it
into the prefix at `drive_c/reshade/`:

| Environment Variable | Description |
| :-------------------- | :----------- |
| <tt>PROTON_USE_RESHADE</tt> | Set to `1` to download ReShade and load it into the game. Set to a version number instead to pin a ReShade release. |
| <tt>PROTON_RESHADE_ADDON</tt> | Set to `1` to deploy the full add-on variant instead, which skips ReShade's network-activity add-on lockout. |
| <tt>PROTON_RESHADE_PATH</tt> | Custom host-side location for shaders, textures and presets (see below), or `0` to keep them inside the prefix instead. |

Press `Home` in-game to open the ReShade overlay.

Shaders, textures and presets live host-side in `~/.local/share/reshade/` (honors
`$XDG_DATA_HOME`) and are symlinked into the prefix, so presets survive prefix deletion
and are shared between games. The standard effect pack is installed on first use.

ReShade combines with the XXMI importers above: with an importer active it is injected
next to 3dmigoto automatically. Without one it takes the `dxgi.dll` slot, so it cannot
be combined with OptiScaler. That conflict does not apply while an importer is loaded.
