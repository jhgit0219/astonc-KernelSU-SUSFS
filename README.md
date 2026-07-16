# astonc KernelSU-Next + SUSFS kernel (PixelOS, OnePlus Ace 3 / 12R)

A reproducible GitHub Actions build of a **KernelSU-Next + SUSFS** kernel for the
**OnePlus Ace 3 (codename `astonc`) / OnePlus 12R**, targeting the **unofficial PixelOS 16**
ROM. Built from the ROM's *own* kernel source so it matches the device's vendor/KMI exactly.

## Why this exists
The prebuilt community kernels (e.g. Wild Kernels `OP-ACE-3`) are built against **OxygenOS**
and **bootloop on PixelOS** — a different kernel base. This repo instead builds from the
PixelOS source tree, adds root (KernelSU-Next) and root-hiding (SUSFS), and produces a
flashable kernel that boots on PixelOS and passes Play Integrity when paired with a Play
Integrity fix.

## Build details
| | |
|---|---|
| Kernel source | [`OnePlus12R-development/android_kernel_oneplus_sm8550`](https://github.com/OnePlus12R-development/android_kernel_oneplus_sm8550) @ `sixteen-qpr2` |
| Version | `5.15.207` (android13-5.15 GKI, SoC SM8550 / kalama) |
| Toolchain | AOSP clang `r450784e` (tree-pinned), LLVM/LTO + CFI |
| Root | [KernelSU-Next](https://github.com/KernelSU-Next/KernelSU-Next) (`next`) |
| Root hiding | [SUSFS](https://gitlab.com/simonpunk/susfs4ksu) (`gki-android13-5.15`) |
| Packaging | AnyKernel3 |

## How to build
Push to this repo or run the **Build astonc KSU-Next + SUSFS** workflow from the Actions tab.
Artifacts: the raw `Image` and an AnyKernel3 zip.

## How to flash (fastboot, boot partition only)
> Only the `boot` partition is touched. Do **not** flash vbmeta and do **not** also keep a
> KernelSU-patched `init_boot` (double-KSU bootloops). Keep a stock `boot` backup to roll back.

```
adb reboot bootloader
fastboot flash boot boot_ksun_susfs_5.15.207.img
fastboot reboot
```
Verify: `adb shell cat /proc/version` → `5.15.207 ... (KSU)`, then the KernelSU-Next Manager
shows "working". Add the **SUSFS** module + a **Play Integrity fix** to pass app integrity.

## Credits
KernelSU-Next (rifsxd), SUSFS (simonpunk), PixelOS OnePlus12R-development, AnyKernel3 (osm0sis).
