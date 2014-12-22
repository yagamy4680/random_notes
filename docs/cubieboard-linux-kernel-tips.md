### Cubieboard Linux Kernel Compilation Tips

gcc compiler and linker options
```
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/lib/gcc/arm-linux-gnueabihf/4.7/lto-wrapper
Target: arm-linux-gnueabihf
Configured with: ../src/configure -v 
    --with-pkgversion='Ubuntu/Linaro 4.7.2-2ubuntu1' 
    --with-bugurl=file:///usr/share/doc/gcc-4.7/README.Bugs 
    --enable-languages=c,c++,go,fortran,objc,obj-c++ 
    --prefix=/usr 
    --program-suffix=-4.7 
    --enable-shared 
    --enable-linker-build-id 
    --with-system-zlib
    --libexecdir=/usr/lib 
    --without-included-gettext 
    --enable-threads=posix 
    --with-gxx-include-dir=/usr/include/c++/4.7 
    --libdir=/usr/lib 
    --enable-nls 
    --with-sysroot=/ 
    --enable-clocale=gnu 
    --enable-libstdcxx-debug 
    --enable-libstdcxx-time=yes 
    --enable-gnu-unique-object 
    --disable-libitm 
    --enable-plugin 
    --enable-objc-gc 
    --enable-multilib 
    --disable-sjlj-exceptions 
    --with-arch=armv7-a 
    --with-fpu=vfpv3-d16 
    --with-float=hard 
    --with-mode=thumb 
    --disable-werror 
    --enable-checking=release 
    --build=arm-linux-gnueabihf 
    --host=arm-linux-gnueabihf 
    --target=arm-linux-gnueabihf
Thread model: posix
gcc version 4.7.2 (Ubuntu/Linaro 4.7.2-2ubuntu1)
```

Default architecture options:
```
COLLECT_GCC_OPTIONS=
    '-march=armv7-a' 
    '-mfloat-abi=hard' 
    '-mfpu=vfpv3-d16' 
    '-mtls-dialect=gnu'
 as -v -march=armv7-a -mfloat-abi=hard -mfpu=vfpv3-d16 -meabi=5
GNU assembler version 2.22.90 (arm-linux-gnueabihf) using BFD version (GNU Binutils for Ubuntu) 2.22.90.20120924
COMPILER_PATH=
    /usr/lib/gcc/arm-linux-gnueabihf/4.7/:
    /usr/lib/gcc/arm-linux-gnueabihf/4.7/:
    /usr/lib/gcc/arm-linux-gnueabihf/:
    /usr/lib/gcc/arm-linux-gnueabihf/4.7/:
    /usr/lib/gcc/arm-linux-gnueabihf/
LIBRARY_PATH=
    /usr/lib/gcc/arm-linux-gnueabihf/4.7/:
    /usr/lib/gcc/arm-linux-gnueabihf/4.7/../../../arm-linux-gnueabihf/:
    /usr/lib/gcc/arm-linux-gnueabihf/4.7/../../../../lib/:
    /lib/arm-linux-gnueabihf/:
    /lib/../lib/:
    /usr/lib/arm-linux-gnueabihf/:
    /usr/lib/../lib/:
    ./:
    /usr/lib/arm-linux-gnueabihf/:
    /usr/lib/gcc/arm-linux-gnueabihf/4.7/../../../:
    /lib/:
    /usr/lib/
```

### References

Debian testing with 3D and CedarX (via libhybris) howto on CubieTruck (CubieBoard3)
[http://sigpipe.me/?p=10](http://sigpipe.me/?p=10)

ARM Chromebook下实现一个最小的Hypervisior 之一Secure World 到Non Secure World的动态切换
http://hi.baidu.com/cinifr/item/02dd23d5526d4c3ce3108f86

http://api.ning.com/files/zogvUtrhf2vp4fPyfifwj-Vg0q5SRKbPEXezEWuY6sEetN0UAaWzmSZA9oEboi7NxPu0OCnhH0W90dYoOSEYVVjm5mCVYsPv/cubieboard2linux_kernel.pdf

Cubieboard2 TF卡Linux系统安装(非镜像)
http://forum.cubietech.com/archiver/?tid-894.html

http://dl.cubieforums.com/patwood/

介紹如何在 debian wheezy amd64 上 cross compile Raspberry Pi 執行檔
http://guildwar23.blogspot.tw/2012/11/debian-wheezy-amd64-cross-compile.html

Cubieboard2 移植到最新内核 Linux 3.11+ 与ARM VIRTUALIZATION
http://cubietech.com/forum.php?mod=viewthread&tid=1078

Setting up the kernel modules.
https://github.com/StephanvanSchaik/gentoo-on-arm/blob/master/cubieboard2/video-decoding.md

Debian testing with 3D and CedarX (via libhybris) howto on CubieTruck (CubieBoard3)
http://sigpipe.me/?p=10

Experimental VDPAU for Allwinner sunxi SoCs (WiP)
https://github.com/linux-sunxi/libvdpau-sunxi

Cedrus
http://linux-sunxi.org/Cedrus

libcedarx
https://github.com/willswang/libcedarx

VLC
http://linux-sunxi.org/VLC

VLC Streaming
http://plnx.nl/wiki/index.php/VLC_Streaming

How to enable VDPAU for gstreamer?
https://forums.gentoo.org/viewtopic-t-912868-start-0.html

GStreamer with VDPAU (h264 acceleration with nVidia cards)
http://stackoverflow.com/questions/4205515/gstreamer-with-vdpau-h264-acceleration-with-nvidia-cards
`gst-launch-0.10 filesrc location=/home/manu/big.mov ! ffdemux_mov_mp4_m4a_3gp_3g2_mj2 ! vdpauh264dec ! vdpauvideopostprocess ! vdpausink`

compiling kernel modules
https://github.com/cubieplayer/Cubian/issues/27

CedarX/Reverse Engineering
http://linux-sunxi.org/Reverse_Engineering

Cubieboard 2 - Linaro + OpenCV
http://www.cubieforums.com/index.php?topic=694.0

关于CubieBoard自编译Node.js 0.10.5最新版(二楼补充0.8.22和0.6.19)
http://forum.cubietech.com/forum.php?mod=viewthread&tid=393&reltid=1147&pre_thread_id=0&pre_pos=5&ext=

Linux Kernel
http://linux-sunxi.org/Linux_Kernel#Compilation


BeagleBoard/gst-openmax
http://elinux.org/BeagleBoard/gst-openmax

3.4.75+ kernel for CB2 and CT
http://www.cubieforums.com/index.php/topic,1412.0.html

```text
    I've uploaded a build of the 3.4.75 kernel that runs on both the Cubieboard2 and Cubietruck.  It's merged from 
    the linux-sunxi/sunxi-3.4 branch of about December 23rd, and includes the Broadcom driver from 
    Benn's 3.4.67 kernel (sunxi-3.4-ct-v101): http://dl.cubieforums.com/patwood/A20-kernel-3.4.75-ct.tar.gz

    The git repository is here: https://github.com/patrickhwood/linux-sunxi/tree/pat-3.4.75-ct

    What's working:
        Gbit ethernet and wifi on CT (put bcmdhd in /etc/modules to auto load the driver)
        100Mbit ethernet on CB2
        CedarX
        Mali
        USB OTG (and g_ether)
        GPIO, LED, all the other usual stuff
```

[Xen ARM with Virtualization Extensions/Allwinner](http://wiki.xen.org/wiki/Xen_ARM_with_Virtualization_Extensions/Allwinner)