[[_TOC_]]

### Command-Line Tools

http://www.brendangregg.com/linuxperf.html
![Linux Performance](http://www.brendangregg.com/Perf/linuxperftools_1000.png)

#### [Find Out Which Process Is Listening Upon a Port](http://www.cyberciti.biz/faq/what-process-has-open-linux-port/)

```bash
$ netstat -tulpn

Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      1138/mysqld
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      850/portmap
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1607/apache2
tcp        0      0 0.0.0.0:55091           0.0.0.0:*               LISTEN      910/rpc.statd
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      1467/dnsmasq
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      992/sshd
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      1565/cupsd
tcp        0      0 0.0.0.0:7000            0.0.0.0:*               LISTEN      3813/transmission
tcp6       0      0 :::22                   :::*                    LISTEN      992/sshd
tcp6       0      0 ::1:631                 :::*                    LISTEN      1565/cupsd
tcp6       0      0 :::7000                 :::*                    LISTEN      3813/transmission
udp        0      0 0.0.0.0:111             0.0.0.0:*                           850/portmap
udp        0      0 0.0.0.0:662             0.0.0.0:*                           910/rpc.statd
udp        0      0 192.168.122.1:53        0.0.0.0:*                           1467/dnsmasq
udp        0      0 0.0.0.0:67              0.0.0.0:*                           1467/dnsmasq
udp        0      0 0.0.0.0:68              0.0.0.0:*                           3697/dhclient
udp        0      0 0.0.0.0:7000            0.0.0.0:*                           3813/transmission
udp        0      0 0.0.0.0:54746           0.0.0.0:*                           910/rpc.statd
```

TCP port 3306 was opened by mysqld process having PID # 1138. You can verify this using /proc, enter:

```bash
$ ls -l /proc/1138/exe
lrwxrwxrwx 1 root root 0 2010-10-29 10:20 /proc/1138/exe -> /usr/sbin/mysqld
```

#### Lint your shell script

[ShellCheck](http://www.shellcheck.net/), A shell script static analysis tool ([github](https://github.com/koalaman/shellcheck))

#### Find all mounted USB devices

```bash
root@nuc54250:/home/smith# lsusb
Bus 001 Device 002: ID 8087:8000 Intel Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 002 Device 008: ID 0458:708c KYE Systems Corp. (Mouse Systems)
Bus 002 Device 007: ID 0458:708c KYE Systems Corp. (Mouse Systems)
Bus 002 Device 006: ID 1d57:f833 Xenta
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

# You can use `lsusb -v` to get more detailed information
```

#### [How to check hard disk performance!?](http://askubuntu.com/questions/87035/how-to-check-hard-disk-performance)

hdparm:

```shell
sudo hdparm -Tt /dev/sda

/dev/sda:
Timing cached reads:   12540 MB in  2.00 seconds = 6277.67 MB/sec
Timing buffered disk reads: 234 MB in  3.00 seconds =  77.98 MB/sec
```

dd:

```shell
dd if=/dev/zero of=/tmp/output bs=8k count=10k; rm -f /tmp/output

10240+0 records in
10240+0 records out
83886080 bytes (84 MB) copied, 1.08009 s, 77.7 MB/s
```

#### [於 Linux 如何查看 詳細的硬體資訊、型號](http://blog.longwin.com.tw/2014/02/linux-query-hardware-2014/)

```text
# apt-get install smartmontools
# smartctl -i /dev/sda

Model Family:     Seagate Barracuda 7200.14 (AF)
Device Model:     ST2000DM001-1CH164
Serial Number:    Z1E3NQMJ
LU WWN Device Id: 5 000c50 050533951
Firmware Version: CC24
User Capacity:    2,000,398,934,016 bytes [2.00 TB]
Sector Sizes:     512 bytes logical, 4096 bytes physical
Rotation Rate:    7200 rpm
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   ATA8-ACS T13/1699-D revision 4
SATA Version is:  SATA 3.0, 6.0 Gb/s (current: 6.0 Gb/s)
```


```text
# sudo hdparm -i /dev/sda1

/dev/sda1:

Model=ST2000DM001-1CH164, FwRev=CC29, SerialNo=Z340PKFV
Config={ HardSect NotMFM HdSw>15uSec Fixed DTR>10Mbs RotSpdTol>.5% }
RawCHS=16383/16/63, TrkSize=0, SectSize=0, ECCbytes=4
BuffType=unknown, BuffSize=unknown, MaxMultSect=16, MultSect=16
CurCHS=16383/16/63, CurSects=16514064, LBA=yes, LBAsects=3907029168
IORDY=on/off, tPIO={min:120,w/IORDY:120}, tDMA={min:120,rec:120}
PIO modes:  pio0 pio1 pio2 pio3 pio4
DMA modes:  mdma0 mdma1 mdma2
UDMA modes: udma0 udma1 udma2 udma3 udma4 udma5 *udma6
AdvancedPM=yes: unknown setting WriteCache=enabled
Drive conforms to: Reserved:  ATA/ATAPI-4,5,6,7

```

sudo dmidecode -t # 後面可以接下述的參數

- bios
- system
- baseboard
- chassis
- processor
- memory
- cache
- connector
- slot


### Rsyslog

Install rsyslog on Ubuntu 12.04 (precise) with following steps:

```bash
apt-get install -y libjson0 libjson0-dev
apt-get install -y uuid-dev
apt-get install -y python-docutils
apt-get install -y byacc flex

wget http://libestr.adiscon.com/files/download/libestr-0.1.9.tar.gz
tar xvf libestr-0.1.9.tar.gz
cd libestr-0.1.9
./configure
make
make install

wget http://www.rsyslog.com/files/download/rsyslog/rsyslog-7.4.7.tar.gz
tar xvf rsyslog-7.4.7.tar.gz
cd rsyslog-7.4.7

./configure \
  --enable-gssapi-krb5 \
  --enable-kmsg \
  --enable-mysql \
  --enable-elasticsearch \
  --enable-imfile \
  --enable-imptcp \
  --enable-omstdout \
  --enable-omuxsock

make
make install
```



### Nginx

http://stackoverflow.com/questions/1627901/remote-addr-not-getting-sent-to-django-using-nginx-tornado

```text
location / {
    proxy_pass http://frontends;
    proxy_pass_header Server;
    proxy_redirect off;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Scheme $scheme;
    proxy_set_header REMOTE_ADDR $remote_addr;
}
```

[Nginx Pitfalls](http://wiki.nginx.org/Pitfalls) shows you what kinds of Nginx configurations are good or bad.


### GUI Utilities

[Xshell 4 超好用的 SSH 連線工具](http://evil-ms.blogspot.tw/2012/06/xshell-4-ssh.html)

- multiple terminals in same window
- remember password, also keep public/private keys
- drop and copy to remote server


[csshx](https://code.google.com/p/csshx/), a tool to allow simultaneous control of multiple SSH sessions. csshX will attempt to create an SSH session to each remote host in separate Terminal.app windows. A master window will also be created. All keyboard input in the master will be sent to all the slave windows.


### Global Web Infrastructure
[SmokePing](http://oss.oetiker.ch/smokeping/), keeps track of your network latency:

- Best of breed latency visualisation.
- Interactive graph explorer.
- Wide range of latency measurment plugins.
- Master/Slave System for distributed measurement.
- Highly configurable alerting system.
- Live Latency Charts with the most 'interesting' graphs.
- Free and OpenSource Software written in Perl written by Tobi Oetiker, the creator of MRTG and RRDtoo


### Remote Desktop
Install VNC server on Ubuntu:
http://forum.ovh.co.uk/showthread.php?7720-desktop-gui-on-ubuntu-13-10-with-remote
```bash
sudo apt-get -y install xfce4 xfce4-goodies vnc4server
mkdir ~/.vnc
echo "startxfce4 &" > ~/.vnc/xstartup
chmod +x ~/.vnc/xstartup
vncserver :1
```


### Experiences

[10 Things We Forgot to Monitor](http://word.bitly.com/post/74839060954/ten-things-to-monitor)

- Fork Rate
- flow control packets
- Swap In/Out Rate
- Server Boot Notification
- NTP Clock Offset
- DNS Resolutions
- SSL Expiration
- DELL OpenManage Server Administrator (OMSA)
- Connection Limits
- Load Balancer Status.

[Linux Container (lxc) 與 QEMU 搭配使用](https://tossug.hackpad.com/Linux-Container-lxc-QEMU--v388MYVKAIn)

1. 安裝 lxc 以及 qemu-user-static
```bash
sudo apt-get install lxc qemu-user-static:i386 debian-archive-keyring debian-ports-archive-keyring
```
2. 修改 /usr/share/lxc/templates/lxc-debian (workaround)
  - 將 36 行附近的變數改成 `MIRROR=${MIRROR:-http://download.si-linux.co.jp/debian-sh/wheezy-sh4/}`
  - 將 190 行附近的 `debootstrap --verbose` 改成 `qemu-debootstrap --verbose --no-check-gpg`
  - 將 163 行附近的 download_debian() 裡面的 packages 變數中的 dialog 那行移除掉 # 因為目前 si-linux 沒有提供 dialog 這個套件
3. 產生 Linux Container:
```bash
sudo lxc-create -t debian -n wheezy -- -a sh4 -r wheezy -c 2>&1 | tee sh4-debug.log
```
4. 啟動 Linux Container
```bash
sudo lxc-start -n wheezy
```
5. 修改 /etc/apt/sources.list
```text
deb http://download.si-linux.co.jp/debian-sh/wheezy-sh4/ wheezy main contrib non-free
deb-src http://free.nchc.org.tw/debian/ wheezy main contrib non-free
deb http://people.linux.org.tw/~fourdollars/debian/ wheezy main contrib non-free
deb-src http://people.linux.org.tw/~fourdollars/debian/ wheezy main contrib non-free
```
6. 設定 Linux Container 的網路
```bash
ifconfig eth0 10.0.3.2 # 指定 Static IP
route add default gw 10.0.3.1 # 設定 Gateway
echo "nameserver 8.8.8.8" > /etc/resolv.conf # 設定 DNS
```
7. 更新 apt index
```bash
apt-get update
```
8. 關掉 gpg signature checks
```bash
echo 'APT::Get::AllowUnauthenticated "true";' > /etc/apt/apt.conf # 因為 si-linux 沒有 sign deb 所以乾脆關掉
```
接下來就是跟使用一般的 Debian 系統一樣了。 Have fun. :D

### iSCSI

http://jonmccune.wordpress.com/2011/12/19/diskless-windows-7-with-iscsi-and-gpxe/

Configurations on isc-dhcp-server
```conf
option space gpxe;
option gpxe.keep-san code 8 = unsigned integer 8;
option iscsi-initiator-iqn code 203 = string;
option routers 10.0.0.0;
allow booting;
allow bootp;
next-server 10.0.0.1; # this server
if exists user-class and option user-class = "gPXE" {
	filename "http://10.0.0.1/gpxe_boot_script.html";
} else {
	filename "undionly.kpxe";
}

range 10.0.0.100 10.0.0.200;
option root-path "iscsi:10.0.0.1::::iqn.2011-12.com.example:storage.iscsiboot";
option gpxe.keep-san 1;
```

Configurations on iPXE
```text
#!gpxe
dhcp net0
set keep-san 1
sanboot iscsi:10.0.0.1::::iqn.2011-12.com.example:storage.iscsiboot
exit
```

My experiments
```text
# using sanhook and grub.exe to boot
```

Useful iscsi initiator commands:
```bash
# Find all available iscsi targets on the server with specified IP
iscsiadm -m discovery -t sendtargets -p 192.168.100.5

# Show iSCSI target status
iscsiadm -m node -o show

# Login all iSCSI target nodes
iscsiadm -m node --login

# Show all iSCSI sessions' status
iscsiadm -m session -o show

# Logout all iSCSI target nodes
iscsiadm -m node --logout
```

[在Ubuntu 12.04上配置iSCSI Target服务](http://www.qyjohn.net/?p=3104), using a physical partition for iSCSI server to share with other PCs:

```text
Target iqn.2013-03.world.server:target0
	Lun 0 Path=/dev/mapper/vg_target00-lv_target00,Type=blockio
```

[Setup an Ubuntu iSCSI target (server)](http://www.heath-bar.com/blog/?p=203), using an image file as partition for iSCSI server to share:

```bash
# create an image file as 20G partition
dd if=/dev/zero of=/mnt/media/iscsi/xbmc.img bs=1M count=20000

# configuration /etc/iet/ietd.conf
Target iqn.2013-03.world.server:target0
	Lun 0 Path=/mnt/media/iscsi/xmbc.img,Type=fileio
```

[Work Around BIOS Halting on iPXE Exit](http://ipxe.org/appnote/work_around_bios_halting_on_ipxe_exit)

```bash
# boot hard disk 0 (MBR)
chain http://server/grub4dos/grub.exe --config-file="rootnoverify (hd0);chainloader +1"

# boot hard disk 0, partition 0 (VBR)
chain http://server/grub4dos/grub.exe --config-file="root (hd0,0);chainloader +1"

# boot CD/DVD 0
chain http://server/grub4dos/grub.exe --config-file="cdrom --init;map --hook;root (cd0);chainloader (cd0)"
```

[Grub4dos Guide - Boot Options](http://diddy.boot-land.net/grub4dos/files/boot.htm)


[Making a bootable USB key from an .iso image on Mac OS X](http://borgstrom.ca/2010/10/14/os-x-bootable-usb.html)

```bash
# convert ISO file to a READ/WRITE Universal Disk Image Format (UDRW)
hdiutil convert -format UDRW -o debian-6.0.7-amd64-netinst.img debian-6.0.7-amd64-netinst.iso

# list disks
diskutil list

# write the UDRW image onto USB stick
dd if=./xbmc-9.11-live-repack.img.dmg of=/dev/disk1 bs=1m
```

[Write to stdin of a running process using pipe](http://serverfault.com/questions/443297/write-to-stdin-of-a-running-process-using-pipe)

```bash
# Start up your program in background with stdin from a FIFO file
mkfifo yourfifo
cat > yourfifo &
mypid=$!
yourprogram < yourfifo

# In separate terminal, write data to FIFO, then background program can receive
echo "Hello World" > yourfifo
```

### Log

[Logstalgia](https://code.google.com/p/logstalgia/) is a website traffic visualization that replays or streams web-server access logs as a pong-like battle between the web server and an never ending torrent of requests.

![](http://logstalgia.googlecode.com/svn/trunk/screenshots/logstalgia.png)


### NFS
[Optimizing NFS Performance](http://nfs.sourceforge.net/nfs-howto/ar01s05.html)


### Operations

Ubuntu 13.10 slow boot

I modified `/etc/init/failsafe.conf`, and comment out all `sleep` statements, then matrix.local  can boot in 3 seconds.

### Performance

[PHP5-FPM with Nginx 效能調教 (1)](http://blog.hothero.org/posts/2014/05/24/php5-fpm-with-nginx-performance-tuned-1)


### Chat

[http://sdelements.github.io/lets-chat/](http://sdelements.github.io/lets-chat/)

[https://github.com/sdelements/lets-chat](https://github.com/sdelements/lets-chat)

![](http://sdelements.github.io/lets-chat/assets/img/devices.png)


### References

[isc-dhcp-server options](http://www.ipamworldwide.com/dhcp-options/isc-dhcpv4-options.html)


### Misc

#### [How to setup an Access Point mode Wi-Fi Hotspot?](http://askubuntu.com/questions/180733/how-to-setup-an-access-point-mode-wi-fi-hotspot)

`echo 1| sudo tee /proc/sys/net/ipv4/ip_forward`
`sudo iptables -t nat -A POSTROUTING -s 10.10.0.0/16 -o ppp0 -j MASQUERADE`

