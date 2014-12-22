
### Player

VLC

- start mjpg-streamer
- open url: `http://server:8080/?action=stream`


### Commands

Find all supported formats of one specific webcam:

```bash
root@nuc54250:/home/smith# v4l2-ctl --list-formats -d /dev/video0
 ioctl: VIDIOC_ENUM_FMT
       Index       : 0
       Type        : Video Capture
       Pixel Format: 'YUYV'
       Name        : YUV 4:2:2 (YUYV)
 
       Index       : 1
       Type        : Video Capture
       Pixel Format: 'H264' (compressed)
       Name        : H.264
 
       Index       : 2
       Type        : Video Capture
       Pixel Format: 'MJPG' (compressed)
       Name        : MJPEG
       
# You can use `v4l2-ctl --list-formats-ext -d /dev/video0` to get more detailed format information.
```

### GStreamer commands

Capture H.264 frame stream from Logitech C910/C920, decode frames, and output to X-Window:
```bash
./capture -c 100000 -o | gst-launch -v -e filesrc location=/dev/fd/0 ! h264parse ! decodebin2 ! xvimagesink sync=false
```


### Open Source Projects

#### [v4l2grab](https://github.com/twam/v4l2grab/tree/master)
This is a small command line utility for grabbing JPEGs form V4L2 devices (e.g. USB webcams).


### Articles

[Capture images using V4L2 on Linux](http://www.jayrambhia.com/blog/capture-v4l2/)

- Open the capture device

```c
int fd;
fd = open("/dev/video0", O_RDWR);
if (fd == -1)
{
    // couldn't find capture device
    perror("Opening Video device");
    return 1;
}
```

- Query the capture device

```c
struct v4l2_capability caps = {0};
if (-1 == xioctl(fd, VIDIOC_QUERYCAP, &caps))
{
    perror("Querying Capabilites");
    return 1;
}
```


