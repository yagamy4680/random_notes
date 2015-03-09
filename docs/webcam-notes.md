# webcam-notes

### Show all Webcam Settings

```bash
root@viao0:/home/smith# v4l2-ctl -d 1 --all
Driver Info (not using libv4l2):
  Driver name   : uvcvideo
  Card type     : HD Pro Webcam C920
  Bus info      : usb-0000:00:1d.7-3
  Driver version: 3.13.11
  Capabilities  : 0x84000001
    Video Capture
    Streaming
    Device Capabilities
  Device Caps   : 0x04000001
    Video Capture
    Streaming
Priority: 2
Video input : 0 (Camera 1: ok)
Format Video Capture:
  Width/Height  : 1920/1080
  Pixel Format  : 'H264'
  Field         : None
  Bytes per Line: 3840
  Size Image    : 4147200
  Colorspace    : SRGB
Crop Capability Video Capture:
  Bounds      : Left 0, Top 0, Width 1920, Height 1080
  Default     : Left 0, Top 0, Width 1920, Height 1080
  Pixel Aspect: 1/1
Streaming Parameters Video Capture:
  Capabilities     : timeperframe
  Frames per second: 30.000 (30/1)
  Read buffers     : 0
                     brightness (int)    : min=0 max=255 step=1 default=128 value=128
                       contrast (int)    : min=0 max=255 step=1 default=128 value=128
                     saturation (int)    : min=0 max=255 step=1 default=128 value=128
 white_balance_temperature_auto (bool)   : default=1 value=1
                           gain (int)    : min=0 max=255 step=1 default=0 value=0
           power_line_frequency (menu)   : min=0 max=2 default=2 value=2
      white_balance_temperature (int)    : min=2000 max=6500 step=1 default=4000 value=5220 flags=inactive
                      sharpness (int)    : min=0 max=255 step=1 default=128 value=128
         backlight_compensation (int)    : min=0 max=1 step=1 default=0 value=0
                  exposure_auto (menu)   : min=0 max=3 default=3 value=3
              exposure_absolute (int)    : min=3 max=2047 step=1 default=250 value=250 flags=inactive
         exposure_auto_priority (bool)   : default=0 value=1
                   pan_absolute (int)    : min=-36000 max=36000 step=3600 default=0 value=0
                  tilt_absolute (int)    : min=-36000 max=36000 step=3600 default=0 value=0
                 focus_absolute (int)    : min=0 max=250 step=5 default=0 value=0 flags=inactive
                     focus_auto (bool)   : default=1 value=1
                  zoom_absolute (int)    : min=100 max=500 step=1 default=100 value=100
```

### Adjust Webcam Setting

```bash
root@viao0:/home/smith# v4l2-ctl -d 1 -c zoom_absolute=400
```