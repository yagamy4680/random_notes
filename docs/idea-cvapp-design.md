### Requirements

There are 2 types of deployment environments: `Developer's MBP` and `Dedicated Servers`:

- Development Laptop
- Dedicated Servers
  - TCE Dedicated Server
  - Ubuntu Dedicated Server

cvapp is a webapp, listen to a specified port, use opencv (or other cv libraries) to transform an image frame retrieved from a ipcam server, and output the transformed image frame with recognized information via http protocol.

#### Development Laptop
Assuming the developer's laptop is Mac Book Pro embedded with FaceTime webcam. Here are the environment assumptions:
1. Mac OS X
2. vagrant + virtualbox for virtualization (vmware's usb host emulation has bug for webcam)

Requirements:
1. be able to run an ipcam server by utilizing any mounted USB webcams or image/video resources
    - from usb webcam
      - /dev/video0, http://0.0.0.0:9000/?action=snapshot
      - /dev/video1, http://0.0.0.0:9001/?action=snapshot
      - ...
      - http://0.0.0.0:9020/ is the management console
    - from multiple image files
      - image resource files are many image files stored in host system (Mac OS X),
      - image resource files are mounted at /data/images on guest system (Ubuntu 14.04) via nfs
      - /data/images, http://0.0.0.0:9010/?action=snapshot
      - when starting to process frames, the ipcam server retrieves image file from /data/images one-by-one and in the loop
      - the delay of each frame is runtime configurable
    - from a video file
      - the video file is stored on host system (Mac OS X)
      - the video file is mounted at /data/video/input.mp4 (or other video format)
      - /data/video/input.[mp4|ogg|avi|wmv|...], http://0.0.0.0:9011/?action=snapshot
      - time accuracy between frames in server and client sides are the same
      - (maybe use gstreamer and v4l2loopback to implement)
    - networking
      - use public network (eth1)
      - use a static ip on private network (eth2), 172.0.0.10
      - bonjour daemon is installed
      - hostname `ipcams`
    - (optional)
      - run above service inside a docker container (all `/dev/videoX` are mounted into the docker container)
      - follow `it4t2t` conventions
2. be able to develop cvapps
    - all source codes of cvapp are stored on host system (Mac OS X)
    - all source codes of cvapp are mounted via nfs on guest system (Ubuntu 14.04). E.g. /cvapps/apps/a, /cvapps/apps/b, /cvapps/apps/c, ...
    - cvapp and prerequisited softwares are installed on guest system. E.g. opencv, python bindings, ...
    - networking
      - use public network (eth1)
      - use a static ip on private network (eth2), 172.0.0.11
      - bonjour daemon is installed
      - hostname `cvrunner`
    - entire cvapp repository are
    - cvapp settings are initial-time and runtime configurable, including:
      - ipcam-url
      - the selected root path of cvapp to be executed, e.g. /cvapps/apps/a
      - public port to listen


#### TCE Dedicated Server
The server is booted via dhcp and ipxe with a customized TinyCoreLinux image
that can serve Docker services. The hardware might be Dell 760 or Intel NUC(s).

Here are the environment assumptions:
1. DockerLinux OS (a customized Boot2Docker based on TinyCoreLinux distribution)
2. mounted webcam devices are /dev/video0, video1, ...
3. each service is running on a docker container

1. be able to run an ipcam server by utilizing any mounted USB webcams
    - from usb webcam
      - /dev/videoX is mounted from host to guest.
      - /dev/video0, http://0.0.0.0:9000/?action=snapshot
      - /dev/video1, http://0.0.0.0:9001/?action=snapshot
      - ...
      - http://0.0.0.0:9020/ is the management console
    - networking
      - port mapping from host to guest, in 9000, 9001, 9002, ...
      - hostname `XXX` (depends on network configuration)


#### ArchLinux SBC

Every
docker images are downloaded from `ground.local` server with `docker pull`.

  TCE Dedicated Server
    a. TinyCoreLinux
    b. boot via dhcp / ipxe / iso-image
    c. be able to run cvapp
          each cvapp is running inside a docker container
          the entire cvapp repository are checked out, and stored at /cvapps inside docker's data container
          the initial settings (ipcam-url, selected cvapp, public port) is set


  Ubuntu Dedicated Server
	a. ubuntu 14.04 desktop
    b. boot via dhcp / ipxe / iscsi (or nfs)
    c. be able to transform all mounted USB webcams to several ipcam servers
          ...
    d. be able to run cvapp



### Experiments

1. Add entire `/dev` into a docker container, especially for all `/dev/videoX`

```bash
# At virtualbox (/home/vagrant/mjpg-streamer/mjpg-streamer)
docker run -t -i -p 9000:9000 -v /home/vagrant/mjpg-streamer/mjpg-streamer:/mjpg-streamer --rm=true --privileged ubuntu /bin/bash

# Inside docker
#
apt-get install -y libexif-dev libjpeg-dev pkg-config make
cd /mjpg_streamer
make clean; make
./mjpg_streamer -i "./input_uvc.so -f 25 -d /dev/video0 -r 640x480" -o "./output_http.so -w ./www -p 9000"
```

With `--privileged`, the docker container can access all devices available on host system.
