# TWRP device tree for Lenovo Smart Tab M10 FHD Plus (TB-X606FA/FX)

## Release info
This is an unofficial build.  Install at your own risk.

Build with TWRP for Android 10.0.  See "MediaTek HW FDE" below for cherrypick instructions.

### About Device

![Lenovo Smart Tab M10 HD](https://download.lenovo.com/images/ProdImageSmart/amazon_alexa.jpg "Lenovo Smart Tab M10 FHD Plus (TB-X606FA)")

Recovery Device Tree for Lenovo Smart Tab M10 FHD Plus wifi (TB-X606FA)
=======================================================================
Component   | Specs
-------:|:-------------------------
Chipset| MediaTek Helio P22T
CPU | ARM Cortex-A53, Octo-Core, 2.3 GHz
GPU     | IMG PowerVR GE8320, 650 MHz
Memory  | 2GB or 4GB (soldered)
Shipped Android Version | 9.0 (Pie), upgrade to 10.0
Storage | 32GB or 64GB eMMC
MicroSD | Up to 256 GB
Battery | 5000 mAh, Li-Po (non-removable)
Display | 1920x1200 pixels, 10.3"
Front Camera | 5.0 MP
Rear Camera  | 8.0 MP
Wifi | dual band, 802.11a/b/g/n/ac
WLAN | 4G LTE   (TB-X606X only)
Bluetooth | v5
USB | USB-C (micro USB)
Release Date | August 2020


## To build
```
. build/envsetup.sh
lunch omni_X606FA-eng
make clean 
mka recoveryimage
```

## MediaTek HW FDE
Until it's merged into android-10.0 branch, you'll need to cherrypick [patch 3237](https://gerrit.twrp.me/c/android_bootable_recovery/+/3237)
```
git fetch ssh://<your_userid>@gerrit.twrp.me:29418/android_bootable_recovery refs/changes/37/3237/3 && git cherry-pick FETCH_HEAD
```
The reason for this is that MTK HW FDE support in the kernel expects the blkdev path to have the string "bootdevice" in it.  If you don't include this patch, TWRP will convert the /data blkdev path to the real block device name, which doesn't include the "bootdevice" string.  This breaks dm-crypt in the kernel, which breaks decryption in twrp.

**Note:** "dm_use_original_path" twrp flag for /data in twrp.fstab is required to make MTK HW FDE work.
