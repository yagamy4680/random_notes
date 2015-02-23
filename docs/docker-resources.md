[[_TOC_]]

All Docker resources found from Internet.

### Guides
[The Docker Guidebook](http://kencochrane.net/blog/2013/08/the-docker-guidebook/) is a very comphrensive guidebook.


### Examples
[Public docker images](https://github.com/dotcloud/docker/wiki/Public-docker-images)

NodeJS + OpenCV 

```shell
docker run shykes/node-opencv node \
  -e 'console.log(require("opencv").version)'
```

EtherCalc

```shell
# Runs at port 6967 (default)
docker run audreyt/ethercalc

# Runs at another port, for example 8080
docker run -p 8080:6967 audreyt/ethercalc
```

GitLab

```shell
git clone https://github.com/yep/gitlab-docker.git
cd gitlab-docker
docker build -t gitlab .
cd PATH/TO/gitlab-docker
docker run -d -v PATH/TO/gitlab-docker:/srv/gitlab gitlab
```


### Registry and Index Spec

[Registry & Index Spec](http://docs.docker.io/en/latest/api/registry_index_spec/) describe the 3 roles (Index, Registry, and Docker):

**Pull**

![Pull](http://docs.docker.io/en/latest/_images/docker_pull_chart.png)

1. Contact the Index to know where I should download “samalba/busybox”
2. Index replies: a. samalba/busybox is on Registry A b. here are the checksums for samalba/busybox (for all layers) c. token
3. Contact Registry A to receive the layers for samalba/busybox (all of them to the base image). Registry A is authoritative for “samalba/busybox” but keeps a copy of all inherited layers and serve them all from the same location.
4. registry contacts index to verify if token/user is allowed to download images
5. Index returns true/false lettings registry know if it should proceed or error out
6. Get the payload for all layers

**Push**

![Push](http://docs.docker.io/en/latest/_images/docker_push_chart.png)

1. Contact the index to allocate the repository name “samalba/busybox” (authentication required with user credentials)
2. If authentication works and namespace available, “samalba/busybox” is allocated and a temporary token is returned (namespace is marked as initialized in index)
3. Push the image on the registry (along with the token)
4. Registry A contacts the Index to verify the token (token must corresponds to the repository name)
5. Index validates the token. Registry A starts reading the stream pushed by docker and store the repository (with its images)
6. docker contacts the index to give checksums for upload images

### Private Repository

- [Share Images via Repositories](http://docs.docker.io/en/latest/use/workingwithrepository/)
- [HOW TO USE YOUR OWN REGISTRY](http://blog.docker.io/2013/07/how-to-use-your-own-registry/)
- [docker enterprise registry discussions on Github](https://github.com/dotcloud/docker/issues/1988), need to read when I have time.
- [利用官方私人 registry image 架設私人 registry (CentOS 6 host)](https://dockers.hackpad.com/-registry-image-registry-CentOS-6-host-xtp4N9JFMuC)


### Storage
[Where are Docker images stored?](http://blog.thoward37.me/articles/where-are-docker-images-stored/), very good article to describe the storage layout in Docker.

local storage on Docker host is `/var/lib/docker`:
```text
drwxr-xr-x    5 root     root           100 Feb 12 13:24 aufs/
drwx------    2 root     root            40 Feb 12 13:24 containers/
drwx------    2 root     root            40 Feb 12 13:24 graph/
drwx------    2 root     root            60 Feb 12 13:24 init/
-rw-r--r--    1 root     root          5120 Feb 12 13:24 linkgraph.db
lrwxrwxrwx    1 root     root            14 Feb 12 13:24 lxc-start-unconfined -> /bin/lxc-start
-rw-------    1 root     root            19 Feb 12 13:24 repositories-aufs
drwx------    2 root     root            40 Feb 12 13:24 volumes/
```

### Github Resources
- [docker-registry](https://github.com/dotcloud/docker-registry), registry server for Docker (hosting/delivering of repositories and images).
- [docker-registry-cookbook](https://github.com/ismell/docker-registry-cookbook), this cookbook deployes the dotCloud docker-registry.
- [docker-spotter](https://github.com/discordianfish/docker-spotter), Hook into docker event stream and execute commands on container events (yagamy: really cool for automation on Host OS). This example will execute `pipework eth0 <id> 192.168.242.1/24` when a container named 'pxe-server' starts and `echo gone` when it stops.
```bash
./spotter \
  -e 'pxe-server:start:pipework:eth0:{{ID}}:192.168.242.1/24' \
  -e 'pxe-server:stop:echo:gone'
```

- [docker-osx](https://github.com/noplay/docker-osx), an unofficial docker helper made to simplify docker usage on OSX.
- [cadvisor](https://github.com/google/cadvisor), Analyzes resource usage and performance characteristics of running containers.


### Docker on Hypbervisor

[docker-vm](https://github.com/pinireznik/docker-vm)
This project was created as a placeholder for transparent hypervisor for running Docker on any platform in addition to Linux.

The goal is to create a new command for Docker (docker.io) "docker setup" which will install some kind of hypervisor is a silent mode and configure everything needed to run docker inside a VM on that hypervison. For example we can use QEMU


[boot2docker-vagrant-box](https://github.com/mitchellh/boot2docker-vagrant-box), packer scripts to build a Vagrant-compatible boot2docker box.

Good example to initiate a VirtualBox with boot2docker iso image:
https://github.com/mitchellh/boot2docker-vagrant-box/blob/master/vagrantfile.tpl

```ruby
Vagrant.configure("2") do |config|
  config.ssh.shell = "sh -l"
  config.ssh.username = "docker"

  # Disable synced folders because guest additions aren't available
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Expose the Docker port
  config.vm.network "forwarded_port", guest: 4243, host: 4243

  # Attach the ISO
  config.vm.provider "virtualbox" do |v|
    v.customize "pre-boot", [
      "storageattach", :id,
      "--storagectl", "IDE Controller",
      "--port", "0",
      "--device", "1",
      "--type", "dvddrive",
      "--medium", File.expand_path("../boot2docker-vagrant.iso", __FILE__),
    ]
  end
end
```

Good example to add SSH public keys into boot2docker iso image:
https://github.com/mitchellh/boot2docker-vagrant-box/blob/master/build-iso.sh

[boot2docker-qemu](https://github.com/steeve/boot2docker/issues/31)

- `qemu -cdrom boot2docker.iso -m 1024`
- `qemu -hda boot2docker.iso -m 1024`


TinyCoreLinux installation for running [term.js](https://github.com/chjj/term.js/):

```bash
tce-load -wi python
tce-load -wi compiletc
tce-load -wi git
wget http://distro.ibiblio.org/tinycorelinux/4.x/x86/tcz/nodejs.tcz
tce-load -i nodejs.tcz
git clone https://github.com/chjj/term.js.git
cd term.js
npm install
cd example
node index.js -n
```


### PXE Booting

https://news.ycombinator.com/item?id=7185432

I have my router serving up TFTP with the following pxelinux.cfg (and the appropriate vmlinuz64 and initrd.img extracted from the b2d ISO):

```text
LABEL boot2docker
          MENU LABEL boot2docker v0.5.2
          KERNEL boot2docker/v0.5.2/vmlinuz64
          APPEND initrd=boot2docker/v0.5.2/initrd.img loglevel=3 user=docker
```


https://index.docker.io/u/jpetazzo/pxe/

1. Of course you need Docker first!
2. Clone this repo and cd into the repo checkout.
3. Build the container with docker build -t pxe .
4. Run the container with PXECID=$(docker run -d pxe)
5. Give it an extra network interface with ./pipework br0 $PXECID 192.168.242.1/24
6. Put the network interface connected to your machines on the same bridge with e.g. brctl addif br0 eth0 (don't forget to move eth0 IP address to br0 if there is one).
7. You can now boot PXE machines on the network connected to eth0! Alternatively, you can put VMs on br0 and achieve the same result.

(yagamy: This is something I want to implement, too)

1. Burn a boot2docker ISO on a blank CD.
2. With that CD, boot a physical machine.
3. Run the PXE container on Docker on the physical machine.
4. Pull the ubuntu container, start it in privileged mode, apt-get install QEMU in it, and start a QEMU VM, mapping its hard disks to the real hard disk of the machine, and bridging it with the PXE container.
6. The QEMU VM will netboot from the PXE container. Install Debian.
7. Reboot the physical machine -- it now boots on Debian.
8. Repeat steps but install Windows for trolling purposes.


### TinyCoreLinux

[Tiny Core Linux Remote Desktop Kiosk](http://itekblog.com/tiny-core-linux-remote-desktop-kiosk/)

[nodejs package for TinyCoreLinux](http://forum.tinycorelinux.net/index.php?topic=12287.0)

http://distro.ibiblio.org/tinycorelinux/5.x/armv7/


### Articles

- [Docker Misconceptions](https://devopsu.com/blog/docker-misconceptions/)
- [Microservice not a free lunch](http://highscalability.com/blog/2014/4/8/microservices-not-a-free-lunch.html)
- [DOCKERCON TALKS](https://www.dockboard.org/dockercon-talks-recap/)
- [Comprehensive Overview of Storage Scalability in Docker](http://developerblog.redhat.com/2014/09/30/overview-storage-scalability-docker/)
- [Docker Image Insecurity](https://titanous.com/posts/docker-insecurity), Dec/23, 2014
- [Real-World Docker: 10 Things We've Learned](http://www.slideshare.net/rightscale/webinar-real-world-docker-2014-1209-v3-1) (slide)

### Tips

- [24 random docker tips](http://csaba.palfi.me/random-docker-tips/)

### Ideas

- [On Docker Container Composition](https://medium.com/@allingeek/on-docker-container-composition-a98788f1aa3c)

### Misc

[Spoon](https://spoon.net/), Use your applications instantly, anywhere.
Run in isolated containers. Share with collaborators. Migrate between devices. (yagamy: docker-like on Windows!!)


### Best Practices & Principles

[程式碼準則 - 開發出靈活、穩定、可持續發展的 HTML 與 CSS 標準。](http://lisp.es/code-guide/)