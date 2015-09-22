### RPi - Bare Metal Programming in C

- [part1](http://www.valvers.com/open-software/raspberry-pi/step01-bare-metal-programming-in-cpt1/), toolchain
- [part2](http://www.valvers.com/open-software/raspberry-pi/step02-bare-metal-programming-in-c-pt2/), c runtime
- [part3](http://www.valvers.com/open-software/raspberry-pi/step03-bare-metal-programming-in-c-pt3/), cmake
- [part4](http://www.valvers.com/open-software/raspberry-pi/step04-bare-metal-programming-in-c-pt4/), interrupts

### Blog Posts

[Bare Metal](https://balau82.wordpress.com/tag/bare-metal/)

- [Flashing the STM32-P152 board with OpenOCD](https://balau82.wordpress.com/2013/08/14/flashing-the-stm32-p152-board-with-openocd/)
- [ARM926 interrupts in QEMU](https://balau82.wordpress.com/2012/04/15/arm926-interrupts-in-qemu/)
- [Using Newlib in ARM bare metal programs](https://balau82.wordpress.com/2010/12/16/using-newlib-in-arm-bare-metal-programs/)
- [QEMU ARM semihosting](https://balau82.wordpress.com/2010/11/04/qemu-arm-semihosting/)

[Emulating ARM PL011 serial ports](https://balau82.wordpress.com/2010/11/30/emulating-arm-pl011-serial-ports/)

```c
#include <stdint.h>
 
typedef volatile struct {
 uint32_t DR;
 uint32_t RSR_ECR;
 uint8_t reserved1[0x10];
 const uint32_t FR;
 uint8_t reserved2[0x4];
 uint32_t LPR;
 uint32_t IBRD;
 uint32_t FBRD;
 uint32_t LCR_H;
 uint32_t CR;
 uint32_t IFLS;
 uint32_t IMSC;
 const uint32_t RIS;
 const uint32_t MIS;
 uint32_t ICR;
 uint32_t DMACR;
} pl011_T;
 
enum {
 RXFE = 0x10,
 TXFF = 0x20,
};
 
pl011_T * const UART0 = (pl011_T *)0x101f1000;
pl011_T * const UART1 = (pl011_T *)0x101f2000;
pl011_T * const UART2 = (pl011_T *)0x101f3000;
 
static inline char upperchar(char c) {
 if((c >= 'a') && (c <= 'z')) {
  return c - 'a' + 'A';
 } else {
  return c;
 }
}
 
static void uart_echo(pl011_T *uart) {
 if ((uart->FR & RXFE) == 0) {
  while(uart->FR & TXFF);
  uart->DR = upperchar(uart->DR);
 }
}
 
void c_entry() {
 for(;;) {
  uart_echo(UART0);
  uart_echo(UART1);
  uart_echo(UART2);
 }
}
```

[Arm Cortex M3 Bare Metal With Newlib](http://sushihangover.github.io/arm-cortex-m3-bare-metal-with-newlib/)

[An ARM Bare Metal Hello World Using Musl](http://ellcc.org/blog/?p=2389)

[Installing QEMU on OS X](https://github.com/psema4/pine/wiki/Installing-QEMU-on-OS-X)

[Llvm, Cmsis Dsp And Cortex M3 & M0](http://sushihangover.github.io/llvm-cmsis-dsp-and-cortex-m3-and-m0/)


[SushiHangover - Bare Metal](http://sushihangover.github.io/blog/categories/bare-metal/)

- [LLVM Bare Metal GitSlave](https://github.com/sushihangover/llvm_baremetal) (github)
- [QEMU mirror with Cortex-Mx additions](https://github.com/sushihangover/qemu) (github)
- [Bkpt: Printf Service Calls On The Cortex M0](http://sushihangover.github.io/bkpt-service-calls-on-the-cortex-m0/)


### Cross Compiler Tips

[LLVM Cross-Compiler](http://wiki.osdev.org/LLVM_Cross-Compiler)

For example, for cross compiling to ARM, you can use:

```bash
-march=armv7-a -mfloat-abi=soft -ccc-host-triple arm-elf
```

Since 3.1, it can be shortened to

```bash
-target armv7--eabi -mcpu=cortex-a9
```

- `-ffreestanding`, Indicated that the file should be compiled for a freestanding enviroment (like a kernel), not a hosted (userspace), environment.
- `-fno-builtin`, Disable special handling and optimizations of builtin functions like strlen and malloc.
- `-nostdlib`, Disables standard library
- `-nostdinc`, Makes sure the standard library headers are not included.
- `-nostdinc++`, Makes sure the standard C++ library headers are not included. This makes sense if you build a custom version of libc++ and want to avoid including system one.

[Cortex-M0 Linker Script for CMSIS](https://github.com/sushihangover/llvm_baremetal/blob/master/examples/arm/M0_CMSIS_DSP/src/cortex_M0.ld)




[ARM Embedded (Bare Metal)](http://www.cryptopp.com/wiki/ARM_Embedded_(Bare_Metal))

Architecture Options for `arm-none-eabi-gcc`:

```text
--------------------------------------------------------------------
| ARM Core | Command Line Options                       | multilib |
|----------|--------------------------------------------|----------|
|Cortex-M0+| -mthumb -mcpu=cortex-m0plus                | armv6-m  |
|Cortex-M0 | -mthumb -mcpu=cortex-m0                    |          |
|Cortex-M1 | -mthumb -mcpu=cortex-m1                    |          |
|          |--------------------------------------------|          |
|          | -mthumb -march=armv6-m                     |          |
|----------|--------------------------------------------|----------|
|Cortex-M3 | -mthumb -mcpu=cortex-m3                    | armv7-m  |
|          |--------------------------------------------|          |
|          | -mthumb -march=armv7-m                     |          |
|----------|--------------------------------------------|----------|
|Cortex-M4 | -mthumb -mcpu=cortex-m4                    | armv7e-m |
|(No FP)   |--------------------------------------------|          |
|          | -mthumb -march=armv7e-m                    |          |
|----------|--------------------------------------------|----------|
|Cortex-M4 | -mthumb -mcpu=cortex-m4 -mfloat-abi=softfp | armv7e-m |
|(Soft FP) | -mfpu=fpv4-sp-d16                          | /softfp  |
|          |--------------------------------------------|          |
|          | -mthumb -march=armv7e-m -mfloat-abi=softfp |          |
|          | -mfpu=fpv4-sp-d16                          |          |
|----------|--------------------------------------------|----------|
|Cortex-M4 | -mthumb -mcpu=cortex-m4 -mfloat-abi=hard   | armv7e-m |
|(Hard FP) | -mfpu=fpv4-sp-d16                          | /fpu     |
|          |--------------------------------------------|          |
|          | -mthumb -march=armv7e-m -mfloat-abi=hard   |          |
|          | -mfpu=fpv4-sp-d16                          |          |
|----------|--------------------------------------------|----------|
|Cortex-R4 | [-mthumb] -march=armv7-r                   | armv7-ar |
|Cortex-R5 |                                            | /thumb   |
|Cortex-R7 |                                            | 	   |
|(No FP)   |                                            |          |
|----------|--------------------------------------------|----------|
|Cortex-R4 | [-mthumb] -march=armv7-r -mfloat-abi=softfp| armv7-ar |
|Cortex-R5 | -mfpu=vfpv3-d16                            | /thumb   |
|Cortex-R7 |                                            | /softfp  |
|(Soft FP) |                                            |          |
|----------|--------------------------------------------|----------|
|Cortex-R4 | [-mthumb] -march=armv7-r -mfloat-abi=hard  | armv7-ar |
|Cortex-R5 | -mfpu=vfpv3-d16                            | /thumb   |
|Cortex-R7 |                                            | /fpu     |
|(Hard FP) |                                            |          |
|----------|--------------------------------------------|----------|
|Cortex-A* | [-mthumb] -march=armv7-a                   | armv7-ar |
|(No FP)   |                                            | /thumb   |
|----------|--------------------------------------------|----------|
|Cortex-A* | [-mthumb] -march=armv7-a -mfloat-abi=softfp| armv7-ar |
|(Soft FP) | -mfpu=vfpv3-d16                            | /thumb   |
|          |                                            | /softfp  |
|----------|--------------------------------------------|----------|
|Cortex-A* | [-mthumb] -march=armv7-a -mfloat-abi=hard  | armv7-ar |
|(Hard FP) | -mfpu=vfpv3-d16                            | /thumb   |
|          |                                            | /fpu     |
--------------------------------------------------------------------
```

[Linker script manual](http://www.math.utah.edu/docs/info/ld_3.html)


### Cortex-M

[Arm Cortex M3 Bare Metal With Newlib](http://sushihangover.github.io/arm-cortex-m3-bare-metal-with-newlib/)

I am working on a custom NEWLIB but first I wanted to make sure that NEWLIB compiled for ARM-NONE-EABI works out of the box with my ARM bare-metal Clang/LLVM build and qemu.

[Semihosting with ARM, GCC, and OpenOCD](http://bgamari.github.io/posts/2014-10-31-semihosting.html)

[Tail-Chaining ARM Cortex-M0 Interrupts](https://embeddedfreak.wordpress.com/2010/10/19/tail-chaining-arm-cortex-m0-interrupts/)

[Cortex-M0 memory map](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.dui0182n/CHDHCADA.html)


[在 Cortex-M3 上運用 CodeSourcery 裸機工具鏈](http://cms.mcuapps.com/tooltips/tt0004/)

```bash
$ arm-none-eabi-gcc -mthumb -march=armv7 -mfix-cortex-m3-ldrd \
  -T lm3s6965.ld main.c reset.S syscalls.c -o main
  
$ arm-none-eabi-objcopy -O binary main main.bin

$ qemu-system-arm -M lm3s6965evb --kernel main.bin --serial stdio
Hello World!
```

[在 SRAM 上執行 ARM Cortex-Mx 程式碼](http://cms.mcuapps.com/tooltips/tt0002/)

[How to create bare minimum Debian Wheezy rootfs from scratch](http://balau82.wordpress.com/2014/07/21/how-to-create-bare-minimum-debian-wheezy-rootfs-from-scratch/)

```bash
# First of all you need to install the support packages on your pc
sudo apt-get install qemu-user-static debootstrap binfmt-support

# Next you need to choose the version of Debian in this case we are building a wheezy image.
targetdir=rootfs
distro=wheezy

# Now we will build first stage of Debian rootfs :
mkdir $targetdir
sudo debootstrap --arch=armhf --foreign $distro $targetdir

# Next copy the qemu-arm-static binary into the right place for the binfmt packages to find it and copy in resolv.conf from the host.
sudo cp /usr/bin/qemu-arm-static $targetdir/usr/bin/
sudo cp /etc/resolv.conf $targetdir/etc

# If everything is right we now have a minimal Debian Rootfs
sudo chroot $targetdir

# Inside the chroot we need to set up the environment again
distro=wheezy
export LANG=C

# Now we are setup the second stage of debootstrap needs to run install 
# the packages downloaded earlier
/debootstrap/debootstrap --second-stage

# Once the package installation has finished, setup some support 
# files and apt configuration.
cat <<EOT > /etc/apt/sources.list
deb http://ftp.uk.debian.org/debian $distro main contrib non-free
deb-src http://ftp.uk.debian.org/debian $distro main contrib non-free
deb http://ftp.uk.debian.org/debian $distro-updates main contrib non-free
deb-src http://ftp.uk.debian.org/debian $distro-updates main contrib non-free
deb http://security.debian.org/debian-security $distro/updates main contrib non-free
deb-src http://security.debian.org/debian-security $distro/updates main contrib non-free
EOT

# Update Debian package database:
apt-get update

# set up locales dpkg scripts tend to complain otherwise, note in 
# jessie you will also need to install the dialog package as well.
apt-get install locales dialog
dpkg-reconfigure locales

# Install some useful packages inside the chroot
apt-get install openssh-server ntpdate
 
# Set a root password so you can login
passwd

# Build a basic network interface file so that the board will DHCP on eth0
echo <<EOT >> /etc/network/interfaces
allow-hotplug eth0
iface eth0 inet static
	address 192.168.1.254
	netmask 255.255.255.248
	gateway 192.168.1.1	
EOT

# Set the hostname
echo nameme > /etc/hostname
 
# Enable the serial console, Debian sysvinit way
echo T0:2345:respawn:/sbin/getty -L ttyS0 115200 vt100 >> /etc/inittab
 
# We are done inside the chroot, so quit the chroot shell
exit
 
# Tidy up the support files
sudo rm $targetdir/etc/resolv.conf
sudo rm $targetdir/usr/bin/qemu-arm-static
```

### Linker Script

- [rtenv的linker script解釋](http://wen00072-blog.logdown.com/posts/247207-rtenv-linker-script-explained)
- [GNU ld的linker script簡介](http://www.slideshare.net/zzz00072/gnu-ldlinker-script)
- [窺探 .bss section](http://blog.linux.org.tw/~jserv/archives/002030.html)
- [.bss section 的觀念：執行時期的長度](http://www.jollen.org/blog/2006/12/bss_section_2.html)
- [GNU LD 手冊略讀 (1): Chapter 3 ~ Chapter 3.5](http://wen00072-blog.logdown.com/posts/246069-study-on-the-linker-script-1)
- [GNU LD 手冊略讀 (2): Chapter 3.6 SETCIONS](http://wen00072-blog.logdown.com/posts/246070-study-on-the-linker-script-2-setcion-command)
- [GNU LD 手冊略讀 (3): Chapter 3.7 ~ Chapter 3.11](http://wen00072-blog.logdown.com/posts/246071-study-on-the-linker-script-3)
- [Linux中使用C語言載入data object 檔案資料](http://wen00072-blog.logdown.com/posts/194317-loads-the-data-object-using-the-c-language-archives-data-in-linux)
- [Linux中使用C語言載入data object 檔案資料 (續）](http://wen00072-blog.logdown.com/posts/194370-load-linux-using-c-language-data-object-archives-data-continued)
- [The Dark Art of Linker Scripts](http://blogs.bu.edu/md/2011/11/15/the-dark-art-of-linker-scripts/)


[Linker Script: Put a particular file at a later position](http://stackoverflow.com/questions/21418593/linker-script-put-a-particular-file-at-a-later-position)

```c
void foo() __attribute__ ((section(".specialmem")));
```

```text
.specialmem:
{
    *(.specialmem)
} >specialMemBlock
```


http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.dui0474i/CHDBIJJD.html

Example 3. Importing a linker-defined symbol

```c
extern unsigned int Image$$ZI$$Limit;
config.heap_base = (unsigned int) &Image$$ZI$$Limit;
```

Example 4. Importing symbols that define a ZI output section

```c
extern unsigned int Image$$ZI$$Length;
extern char Image$$ZI$$Base[];
memset(Image$$ZI$$Base, 0, (unsigned int)&Image$$Length);
```

### libc

[Comparison of C/POSIX standard library implementations for Linux](http://www.etalabs.net/compare_libcs.html)

| Library | Size | Licenses |
|---|---|---|
| [musl](http://www.musl-libc.org/) | 426K | [MIT](http://git.musl-libc.org/cgit/musl/tree/COPYRIGHT) |
| [uClibc](http://www.uclibc.org/) | 500K | |
| [dietlibc](http://www.fefe.de/dietlibc/) | 120K | [GPL v2](https://github.com/ensc/dietlibc/blob/master/COPYING) |
| [glibc](http://www.gnu.org/software/libc/) | 2.0M | GPL |
| [bionic](https://github.com/android/platform_bionic) | ... | Apache |

