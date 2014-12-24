### [Xenomai](http://wiki.csie.ncku.edu.tw/embedded/xenomai)

Architecture

![](http://wiki.csie.ncku.edu.tw/embedded/xenomai/xenomai_arch.jpg)

----
iPIPE

![](http://wiki.csie.ncku.edu.tw/embedded/xenomai/adeos.jpg)

[Install Xenomai on the Beaglebone black(Angstrom and Debian)](http://emplearn.blogspot.tw/2014/11/xenomai-on-beaglebone-black.html)

市面上許多的產品，如果內部的作業系統不能夠做到即時處理，就會造成許多人眼視覺上、使用上的誤差，例如要將馬達操控的桿子維持在水平狀，如果不能夠即時的PID迴授控制，那麼就會導致用力過度的狀況。...

[Xenomai on the Beaglebone Black in 14 easy steps](http://brunosmartins.info/xenomai-on-the-beaglebone-black-in-14-easy-steps/)

[EBC Xenomai](http://elinux.org/EBC_Xenomai)

[Installing xenomai and debian on beaglebone black](http://sagar.se/xenomai-debian-on-bbb.html) (yagamy: super simple procedure to enable Xenomai on bbb kernel 3.8)

```bash
git clone https://github.com/cdsteinkuehler/linux-dev
cd linux-dev
./build_kernel.sh
./tools/install_kernel.sh
```

### AdeOS

[Adaptive Domain Environment for Operating System](http://www.opersys.com/ftp/pub/Adeos/adeos.pdf) (pdf)

[Life With Adeos](http://www.xenomai.org/documentation/branches/v2.3.x/pdf/Life-with-Adeos-rev-B.pdf) (pdf)


### Articles

[BEAGLEBONE BLACK 及其超频](http://blog.dword1511.info/?p=4549#more-4549)

[Cool New Phone With Dual OS, Coolpad Bodun](https://storify.com/redbendsoftware/cool-new-phone-coolpad-v1)

### Code Snippets

#### [bb-show-serial](https://github.com/gkaindl/beaglebone-ubuntu-scripts/blob/master/bb-show-serial.sh)

This script reads the serial number from the i2c-connected eeprom available
on BeagleBone (Black). It should work both on device-tree and pre-device-tree kernels.

The serial number is unique for each BeagleBone (Black) and also includes
the week/year of manufacture in the first 4 digits.

Here are the sample outputs:

```text
root@bbb3:/home/smith# ./bb-show-serial.sh
beaglebone serial number: 2614BBBK0368
```

#### [mmc-detect](https://gist.github.com/dword1511/7178095)

A shell script that collects information about MMC/SD cards present. Here are the outputs for the script running on my Beagle Bone Black:

```text
MMC Host Controller "mmc0":
  Driver: omap_hsmmc
  Alias : platform:mmc.5
  =========================================
  Card @ 0x59b4:
    Card Type             : SD
    Card Name             : USD
    Card Size             : 0 MiB
    Card Serial           : 0x243189ea
    Block Device          : mmcblk0
    OEM ID                : 0x4a45 (JE)
    Manufacturer ID       : 0x000074 (Transcend?)
    Manufacture Date      : 07/2014
    Hardware Revision     : 0x1
    Firmware Revision     : 0x0
    Product Revision      : N/A
    Prefered Erase Size   : 4096 KiB
    Mode                  : Block-Addressed
    CID Register          : 744a45555344202010243189ea00e789
    CSD Register          : 400e00325b590000ebdd7f800a400053
    SCR Register          : 0235800100000000
  =========================================

MMC Host Controller "mmc1":
  Driver: omap_hsmmc
  Alias : platform:mmc.11
  =========================================
  Card @ 0x0001:
    Card Type             : MMC
    Card Name             : MMC04G
    Card Size             : 3688 MiB
    Card Serial           : 0x0563bfd8
    Block Device          : mmcblk1
    OEM ID                : 0x0100
    Manufacturer ID       : 0x000070 (*Unknown*)
    Manufacture Date      : 03/1998
    Hardware Revision     : 0x0
    Firmware Revision     : 0x0
    Product Revision      : N/A
    Prefered Erase Size   : 4096 KiB
    Erase Group Size      : 4096 KiB
    Enhanced Area Offset  : 18446744073709551594
    Enhanced Area Size    : 4095 MiB
    RPMB Partition Size   : 128 KiB
    Reliable Write Sectors: 1
    CID Register          : 7001004d4d43303447580563bfd831c5
    CSD Register          : d04f01320f5903ffffffffe78a400051
  =========================================
```


