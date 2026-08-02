# Options

Proton-Anime inherits all GE-Proton features: media foundation patches for video playback
(the anime games rely heavily on this for cutscenes), AMD FSR, NVIDIA CUDA/NVAPI support,
raw input, the protonfixes system, NTSync, wine-wayland, and HDR.

Useful launch options (set in Steam launch options or the environment):

| Compat config string | Environment Variable | Description |
| :-------------------- | :----------------------------- | :----------- |
|                       | <tt>PROTON_LOG</tt>            | Dump a debug log to `$HOME/steam-$APPID.log`. |
| <tt>wined3d</tt>      | <tt>PROTON_USE_WINED3D</tt>    | Use OpenGL-based wined3d instead of Vulkan-based DXVK. |
| <tt>nod3d12</tt>      | <tt>PROTON_NO_D3D12</tt>       | Disables DX12. |
| <tt>nod3d11</tt>      | <tt>PROTON_NO_D3D11</tt>       | Disables DX11. |
| <tt>noesync</tt>      | <tt>PROTON_NO_ESYNC</tt>       | Disable eventfd-based in-process synchronization. |
| <tt>nofsync</tt>      | <tt>PROTON_NO_FSYNC</tt>       | Disable futex-based in-process synchronization. |
| <tt>nontsync</tt>     | <tt>PROTON_NO_NTSYNC</tt>      | Do not use the ntsync kernel module. |
| <tt>forcelgadd</tt>   | <tt>PROTON_FORCE_LARGE_ADDRESS_AWARE</tt> | Force LARGE_ADDRESS_AWARE for all executables. |
| <tt>enablenvapi</tt>  | <tt>PROTON_ENABLE_NVAPI</tt>   | Enable NVIDIA's NVAPI GPU support library. |
| <tt>seccomp</tt>      | <tt>PROTON_USE_SECCOMP</tt>    | Enable seccomp-bpf filter to emulate native syscalls, required for some DRM protections. |
|                       | <tt>WINE_FULLSCREEN_FSR</tt>   | Enable AMD FidelityFX Super Resolution. Combine with `WINE_FULLSCREEN_FSR_STRENGTH` (0–5). |
|                       | <tt>WINE_FULLSCREEN_FSR_CUSTOM_MODE</tt> | Fake screen resolution, format `WIDTHxHEIGHT`. |
|                       | <tt>WINE_DO_NOT_CREATE_DXGI_DEVICE_MANAGER</tt> | Set to 1 if video playback is miscolored (pink tint). |
| `fsr4`                | `PROTON_FSR4_UPGRADE`          | Upgrade games with FSR 3.1 to FSR 4 (downloads `amdxcffx64.dll`). |
| `dlss`                | `PROTON_DLSS_UPGRADE`          | Automatically use newer `nvngx_dlss*.dll` versions. |
| `sdlinput`            | `PROTON_USE_SDL` / `PROTON_PREFER_SDL` | Use SDL input instead of HIDRAW/Steam Input. |
| `wayland`             | `PROTON_USE_WAYLAND` / `PROTON_ENABLE_WAYLAND` | Enable the Wayland driver. |
|                       | `PROTON_ENABLE_HDR`            | Enable HDR (requires HDR-capable compositor, game and monitor, auto-enables Wayland). |
| `wow64`               | `PROTON_USE_WOW64`             | Enable wow64. |

For the full list of options see [`proton`](proton) and the
[GE-Proton README](https://github.com/GloriousEggroll/proton-ge-custom#options).

## NTSync

For NTSync your kernel must be 6.14+ with `CONFIG_NTSYNC=y` or `=m`. If built as a module,
load it with `sudo modprobe ntsync` and persist it via `/etc/modules-load.d/ntsync.conf`
containing the line `ntsync`.
