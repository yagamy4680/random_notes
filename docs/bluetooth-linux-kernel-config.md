## Linux Kernel Configurations for Bluetooth MOD

[How to configure the Linux kernel/drivers/bluetooth](http://how-to.wikia.com/wiki/How_to_configure_the_Linux_kernel/drivers/bluetooth)

| Config    | Description |
|-----------|--------------------------|
| `BT_HCIUSB` | Bluetooth HCI USB driver |
| `BT_HCIUSB_SCO` | This option enables the SCO support in the HCI USB driver. You need this to transmit voice data with your Bluetooth USB device. |
| `BT_HCIUART` | Bluetooth HCI UART driver. This driver is required if you want to use Bluetooth devices with serial port interface. You will also need this driver if you have UART based Bluetooth PCMCIA and CF devices like Xircom Credit Card adapter and BrainBoxes Bluetooth PC Card. |
| `BT_HCIUART_H4` | UART (H4) is serial protocol for communication between Bluetooth device and host. This protocol is required for most Bluetooth devices with UART interface, including PCMCIA and CF cards. |
| `BT_HCIUART_BCSP` | BCSP (BlueCore Serial Protocol) is serial protocol for communication between Bluetooth device and host. This protocol is required for non USB Bluetooth devices based on CSR BlueCore chip, including PCMCIA and CF cards. |
| `BT_HCIBCM203X` | Bluetooth HCI BCM203x USB driver. This driver provides the firmware loading mechanism for the Broadcom Blutonium based devices. |
| `BT_HCIBPA10X` | Bluetooth HCI BPA10x USB driver. This driver provides support for the Digianswer BPA 100/105 Bluetooth sniffer devices. |
| `BT_HCIBFUSB` | Bluetooth HCI BlueFRITZ! USB driver. This driver provides support for Bluetooth USB devices with AVM interface: AVM BlueFRITZ! USB | 
| `BT_HCIDTL1` | Bluetooth HCI DTL1 (PC Card) driver. This driver provides support for Bluetooth PCMCIA devices with Nokia DTL1 interface: Nokia Bluetooth Card Socket Bluetooth CF Card |
| `BT_HCIBT3C` | Bluetooth HCI BT3C (PC Card) driver. This driver provides support for Bluetooth PCMCIA devices with 3Com BT3C interface: 3Com Bluetooth Card (3CRWB6096) HP Bluetooth Card |
| `BT_HCIBLUECARD` | Bluetooth HCI BlueCard (PC Card) driver. This driver provides support for Bluetooth PCMCIA devices with Anycom BlueCard interface: Anycom Bluetooth PC Card Anycom Bluetooth CF Card
Say Y here to compile support for HCI BlueCard devices into the kernel or say M to compile it as module (bluecard_cs). |
| `BT_HCIBTUART` | Bluetooth HCI UART (PC Card) driver. This driver provides support for Bluetooth PCMCIA devices with an UART interface: Xircom CreditCard Bluetooth Adapter Xircom RealPort2 Bluetooth Adapter Sphinx PICO Card H-Soft blue+Card Cyber-blue Compact Flash Card |
| `BT_HCIVHCI` | Bluetooth Virtual HCI device driver. This driver is required if you want to use HCI Emulation software. |



http://landley.net/kdocs/menuconfig/powerpc.html
(todo: find x86 architecture settings)

## BlueZ
### Default Configurations

Installation Directories:

```text
--prefix=/usr \
--sysconfdir=/etc \
--localstatedir=/var \
--mandir=/usr/share/man \
--disable-cups \
--with-udevdir=/usr/lib \
--enable-library \
--with-systemdsystemunitdir=/lib/system/system \
--with-systemduserunitdir=/usr/lib/systemd/user
```