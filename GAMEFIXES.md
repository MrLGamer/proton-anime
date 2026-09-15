# Anime-game specific fixes

Environment variables for this fork's per-game hacks. They only affect the named game
and are separate from the inherited GE-Proton options in
[Options](OPTIONS.md). Fixes that need no configuration (launcher repairs,
cutscene playback, startup timeouts) are applied automatically and are not listed here.

| Environment Variable | Description |
| :-------------------- | :----------- |
| <tt>PROTON_NTE_HIDE_LAUNCHER</tt> | Set to `1` to hide the NTE launcher. It auto-starts the game and is closed once `HTGame.exe` exits. |
| <tt>PROTON_NTE_HIDE_LAUNCHER_TIMEOUT</tt> | Seconds to wait for `HTGame.exe` before closing the launcher anyway (default `300`, `0` waits forever). |
| <tt>PROTON_HSR_FPS</tt> | Unlock the HSR frame rate cap, e.g. `PROTON_HSR_FPS=120`. Needs one prior game launch. Changing in-game graphics settings reverts the cap until the next launch. |
| <tt>PROTON_GI_FPS</tt> | Unlock the GI frame rate cap, e.g. `PROTON_GI_FPS=120`. Works for `GenshinImpact.exe` and `YuanShen.exe`. |
| <tt>PROTON_WUWA_ENGINE_VARS</tt> | Comma-separated console variables written to WuWa's `UserEngine.ini` (LOD tweaks some WWMI mods ask for). The file is rewritten with exactly these variables. When unset it is left untouched. |

Example for the WuWa engine variables:

```sh
PROTON_WUWA_ENGINE_VARS="r.Streaming.HiddenPrimitiveScale=25,r.Kuro.SkeletalMesh.DistanceLODBaseFOV=165"
```
