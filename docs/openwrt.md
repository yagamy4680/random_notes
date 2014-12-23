[[_TOC_]]

## Software

### OpenWRT Histories

[OpenWRT History](http://wiki.openwrt.org/about/history)

| openwrt | nickname | release date | linux kernel | remarks |
|--------|--------|--------|--------|--------|
| OpenWrt 10.03 | Backfire 10.03 | April 2010 | 2.6.32 | supports Netgear WNDR3800 |
| OpenWrt 12.09 | Attitude Adjustment 12.09 | April 2013 | 3.3 | |
| OpenWrt 14.07 | Barrier Breaker 14.07 | 14th July 2014 | 3.10 | |
| OpenWrt 14.12 | Chaos Chalmer 14.11 or 14.12 | 2014/E | 3.14 or later TLS version | under development |


### CeroWRT

[CeroWRT](https://www.bufferbloat.net/projects/cerowrt) is a project built on the OpenWrt firmware to resolve the endemic problems of bufferbloat in home networking today, and to push forward the state of the art of edge networks and routers.



## Hardware

### Arduino Yun

Hardware
  - AVR Arduino microcontroller
    - ATmega32u4
  - Linux microprocessor
    - Atheros AR9331 400MHz
    - 64 MiB DDR RAM
    - 16 MiB Flash

[Linino](http://www.linino.org/modules/yun/) distribution (based on OpenWRT 12.09 with kernel 3.3). According to [Stackoverflow](http://stackoverflow.com/questions/22052030/yun-openssh-sftp-server-gone-from-linino-opkg-repository), Linino latest development image shall be based on OpenWRT Barrier Breaker 14.07 with kernel 3.10. => Needs to verify.

Linino has some attractive features:
  - Node.js/javascript support (on MIPS CPUs)
  - AllJoyn support
  - Ideino Dev (web-browser based IDE)

[openwrt-yun](https://github.com/arduino/openwrt-yun) is a custom version of OpenWrt, targeted to the Arduino Yún. (derived from Linino)


### Netgear WNDR3800

Hardware 
  - Atheros AR7161 rev2 680MHz
  - 128 MiB RAM
  -  16 MiB Flash

Just use latest OpenWRT release (Barrier Breaker 14.07) with kernel 3.10.

Yet another choice is [CeroWRT 3.10](https://www.bufferbloat.net/projects/cerowrt) that also uses kernel 3.10. Or its derive [OpenWireless](https://openwireless.org/).


### [Netgear WNDR4300](http://wiki.openwrt.org/toh/netgear/wndr4300)

Hardware
  - Atheros AR9344 @ 560MHz
  - 128 MiB RAM
  - 128 MiB NAND Flash

[Compiled Firmware for Netgear WNDR4300](https://forum.openwrt.org/viewtopic.php?id=49189)

[WNDRv4 fit for openwrt](https://forum.openwrt.org/viewtopic.php?id=41094&p=12)

[安裝 Ubuntu 下使用的tftp server](http://blog.xuite.net/arieslin2005blog/blog/53375149-%E5%AE%89%E8%A3%9D+Ubuntu+%E4%B8%8B%E4%BD%BF%E7%94%A8%E7%9A%84tftp+server+(tftpd-hpa+%E8%88%87+tftp-hpa))

## Hints

Q: How to change HOSTNAME?
A: Edit `/etc/config/system` file, change as `hostname` option:

```text
config 'system'
	option 'hostname'	'OpenWrt'
	option 'timezone'	'UTC'
```

Q: How to enable SSH login from WAN?
A: Edit `/etc/config/firewall` by adding following lines:

```text
config rule
	option name 'ssh-from-wan'
	option src 'wan'
	option dest_port '22'
	option target 'ACCEPT'
	option proto 'tcp'
```

Q: How to enable Bonjour?
A: For Netgear WNDR4300, here are the steps:

```bash
wget http://downloads.openwrt.org/barrier_breaker/14.07-rc2/ar71xx/nand/packages/libdaemon_0.14-4_ar71xx.ipk
wget http://downloads.openwrt.org/barrier_breaker/14.07-rc2/ar71xx/nand/packages/libavahi_0.6.31-6_ar71xx.ipk
wget http://downloads.openwrt.org/barrier_breaker/14.07-rc2/ar71xx/nand/packages/avahi-daemon_0.6.31-6_ar71xx.ipk
opkg install libdaemon_0.14-4_ar71xx.ipk
opkg install libavahi_0.6.31-6_ar71xx.ipk
opkg install avahi-daemon_0.6.31-6_ar71xx.ipk
opkg install libgdbm_1.11-1_ar71xx.ipk
opkg install libavahi-dbus-support_0.6.31-6_ar71xx.ipk
opkg install libavahi-client_0.6.31-6_ar71xx.ipk
opkg install avahi-utils_0.6.31-6_ar71xx.ipk
/etc/init.d/avahi-daemon enable

# Modify `/etc/dbus-1/system.conf`
# 
#   <deny own="*"/> 
#   <allow own="org.freedesktop.Avahi"/> <!-- add this line -->
#   <deny send_type="method_call"/>
# 
# Refer to https://dev.openwrt.org/ticket/12971
# 
/etc/init.d/dbus restart
/etc/init.d/avahi-daemon start

# Modify `/etc/config/firewall` to also allow PCs on WAN to 
# find the OpenWRT router via bonjour protocol.
# 
# Add following lines:
#
# -------------
# config rule
#	option name 'bonjour-mdns'
#	option src 'wan'
#	option dest_port '5353'
#	option target 'ACCEPT'
#	option proto 'udp'
# -------------
#
/etc/init.d/firewall restart

# Prevent any custom config from being overwritten on upgrade 
#
echo "/etc/config/firewall" >> /etc/sysupgrade.conf
echo "/etc/avahi/avahi-daeon.conf" >> /etc/sysupgrade.conf
```

Q: Any pre-built binaries for OpenWRT to download
A: [http://downloads.openwrt.org/](http://downloads.openwrt.org/) has many pre-built binaries to download.

For WNDR4300, please download softwares from here:
- [http://downloads.openwrt.org/barrier_breaker/14.07-rc2/ar71xx/nand/packages/](http://downloads.openwrt.org/barrier_breaker/14.07-rc2/ar71xx/nand/packages/)
- [http://downloads.openwrt.org/snapshots/trunk/ar71xx.nand/packages/](http://downloads.openwrt.org/snapshots/trunk/ar71xx.nand/packages/)

Q: How to setup wireless AP on OpenWRT?
A: [http://wiki.openwrt.org/doc/recipes/routedap](http://wiki.openwrt.org/doc/recipes/routedap) specifies following files to be modified:
- /etc/config/network
- /etc/config/wireless
- /etc/config/dhcp
- /etc/config/firewall

On Barrier Breaker (14.07) branch, you just simply edit `/etc/config/wireless`, comment `option disabled 1` lines, and run following commands to restart wifi:

```bash
ifup lan
/etc/init.d/firewall restart
/etc/init.d/dnsmasq restart
```

Q: How to reset OpenWRT settings?
A: According to the [article](http://wiki.villagetelco.org/OpenWrt_Failsafe_Mode_and_Flash_Recovery#2a._Run_.27firstboot.27_Command), just simply run following commands to reset OpenWRT settings (including cleaning JFFS2 partition and `/overlay`:

```bash
firstboot

# After running the command, check /etc/config/network to see what IP address will be used on reboot.
# and then reboot.
reboot -f
```

Q: What's the boot process of OpenWRT?
A: As follows [process.boot](http://wiki.openwrt.org/doc/techref/process.boot):

1. kernel boots from a known raw partition (without a FS), scans mtd partition rootfs for a valid superblock and mounts the SquashFS partition (containing `/etc`) then runs `/etc/preinit`.
2. `/etc/preinit` runs [/sbin/mount_root](https://dev.openwrt.org/browser/trunk/package/base-files/files/sbin/mount_root)
3. `mount_root` mounts the JFFS2 partition (`/overlay`) and combines it with the SquashFS partition (`/rom`) to create a new virtual root filesystem (`/`)
4. bootup continues with `/sbin/init`





## References

- OpenWRT Build Guide: Start To Finish: [1](http://www.thepowerbase.com/2012/01/openwrt-build-guide-start-to-finish/), [2](http://www.thepowerbase.com/2012/01/openwrt-build-guide-start-to-finish/2/), [3](http://www.thepowerbase.com/2012/01/openwrt-build-guide-start-to-finish/3/)
- [The OpenWrt Flash Layout](http://wiki.openwrt.org/doc/techref/flash.layout)
- [Image Generator (Image Builder)](http://wiki.openwrt.org/doc/howto/obtain.firmware.generate)
- [OpenWRT helper softwares](http://downloads.openwrt.org.cn/software/)
  - WinPE
  - WinPM
  - PL-2303 driver
  - ...

- [Partitioning, Formatting and Mounting Storage Devices](http://wiki.openwrt.org/doc/howto/storage)
- [The OpenWrt Flash Layout](http://wiki.openwrt.org/doc/techref/flash.layout)
- [Rootfs on External Storage (extroot)](http://wiki.openwrt.org/doc/howto/extroot)
- [OpenWrt	Development	Guide (pdf)](http://www.ccs.neu.edu/home/noubir/Courses/CS6710/S12/material/OpenWrt_Dev_Tutorial.pdf)