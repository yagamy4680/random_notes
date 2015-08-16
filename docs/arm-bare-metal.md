### RPi - Bare Metal Programming in C

- [part1](http://www.valvers.com/open-software/raspberry-pi/step01-bare-metal-programming-in-cpt1/), toolchain
- [part2](http://www.valvers.com/open-software/raspberry-pi/step02-bare-metal-programming-in-c-pt2/), c runtime
- [part3](http://www.valvers.com/open-software/raspberry-pi/step03-bare-metal-programming-in-c-pt3/), cmake
- [part4](http://www.valvers.com/open-software/raspberry-pi/step04-bare-metal-programming-in-c-pt4/), interrupts





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

