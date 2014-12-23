[LINUX 下离线烧写 SPI 闪存](http://blog.dword1511.info/?p=4107)

> coreboot （原 LinuxBIOS ）项目开发的 flashrom 工具居然带有离线编程功能，除了支持一票比较贵的专用编程器/基于 FTDIxxxxH 的编程器外，还支持一种叫做 serprog 的协议，可以通过串口操作单片机来给 SPI 闪存编程。这样一来就方便了。flashrom 的 wiki 里给了[基于 Arduino 的 serprog 的固件源码](http://www.flashrom.org/Serprog#Arduino_flasher_by_GNUtoo)，其实就是为 ATMEGA 系列 AVR 单片机设计的。


