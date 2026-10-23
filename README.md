# selene_vzw-kernel

Stock kernel-side prebuilts for the Verizon TCL Tab 8 NxtPaper 5G (8189S,
selene_vzw), firmware TP1A.220624.014 4PEQ (2026-08-05). Used by device/tcl/selene_vzw
(BoardConfig.mk / device.mk) to build boot, vendor_boot and the A/B OTA.

| File | Source |
|---|---|
| `boot.img` | stock `boot_b`, AVB footer stripped (first 25022464 bytes); header unchanged (no OS version/patch level) |
| `Image.lz4` | kernel of stock `boot_b` (GKI android13-5.15, `unpack_bootimg`) |
| `dtb/mt6835.dtb` | dtb of stock `vendor_boot_b` |
| `dtbo.img` | stock `dtbo_b`, AVB footer stripped (first 134976 bytes) |
| `vendor_ramdisk/lib/modules/` | stock `vendor_boot_b` ramdisk, verbatim (221 modules, `modules.load`, `modules.load.recovery`, ...) |
| `vendor_ramdisk/first_stage_ramdisk/` | stock first-stage files, for reference (the build installs the device tree's identical fstabs) |
| `images/{vendor,odm,system}_dlkm.img` | stock `*_b` logical partitions |

The images under `images/` had TCL's AVB footer removed (`avbtool
erase_footer`) and an unsigned hashtree footer added at the stock partition
size, so their descriptors can go into our vbmeta:

    avbtool add_hashtree_footer --image <p>.img --partition_name <p> \
        --partition_size <size> --hash_algorithm sha256 --salt <random>

Sizes: vendor_dlkm 14077952, odm_dlkm 348160, system_dlkm 348160.
