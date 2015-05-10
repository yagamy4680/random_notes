[LINUX 下离线烧写 SPI 闪存](http://blog.dword1511.info/?p=4107)

> coreboot （原 LinuxBIOS ）项目开发的 flashrom 工具居然带有离线编程功能，除了支持一票比较贵的专用编程器/基于 FTDIxxxxH 的编程器外，还支持一种叫做 serprog 的协议，可以通过串口操作单片机来给 SPI 闪存编程。这样一来就方便了。flashrom 的 wiki 里给了[基于 Arduino 的 serprog 的固件源码](http://www.flashrom.org/Serprog#Arduino_flasher_by_GNUtoo)，其实就是为 ATMEGA 系列 AVR 单片机设计的。


[Embedded Programming with the GNU Toolchain](http://www.bravegnu.org/gnu-eprog-dist.pdf) (pdf)


### Utilities

[binwalk](https://github.com/devttys0/binwalk), Firmware Analysis Tool

```text
$ binwalk firmware.bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             TRX firmware header, little endian, header size: 28 bytes, image size: 14766080 bytes, CRC32: 0x6980E553 flags: 0x0, version: 1
28            0x1C            LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 5494368 bytes
2319004       0x23629C        Squashfs filesystem, little endian, version 4.0, compression: xz, size: 12442471 bytes, 3158 inodes, blocksize: 131072 bytes, blocksize: 131072 bytes, created: 2014-05-21 22:38:47
```