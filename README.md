# Spreadtrum WCN Bluetooth / FM for the iBall Slide Skye 03

Spreadtrum/Unisoc marlin2 Bluetooth (`sprdbt_tty`) and FM (`sprd_fm`) drivers
for the integrated WCN of the SC7731E, imported unmodified from
[kanadenadi/android_kernel_sprd_sc9832e](https://github.com/kanadenadi/android_kernel_sprd_sc9832e)
(`drivers/misc/sprdwcn_bsp/sipc_modules`). The SC7731E uses the SIPC variants.

`compat/misc/` holds two one-line wrappers so `<misc/marlin_platform.h>` and
`<misc/wcn_integrate_platform.h>` resolve to the 4.4 kernel's `include/linux`
headers.

## Build

```bash
for d in bluetooth/driver fm/driver; do
  make -C <kernel> O=out M=$PWD/$d BSP_BOARD_UNISOC_WCN_SOCKET=sipc \
       ARCH=arm CROSS_COMPILE=arm-linux-androideabi- modules
done
arm-linux-androideabi-strip --strip-debug bluetooth/driver/sprdbt_tty.ko fm/driver/sprd_fm.ko
```

Install as `/lib/modules/sprdbt_tty.ko` and `/lib/modules/sprd_fm.ko` in the
boot ramdisk (loaded from `init.sp7731e_1h10.rc`).

Tested with the KGV-Pro-X kernel: Bluetooth turns on, loads its firmware
configuration over /dev/ttyBT0 and discovers nearby devices; /dev/fm is created.
