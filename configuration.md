# Configuration

## Install a window manager

## Setup networking

## Reconnect the type cover

The attachable keyboard should work out of the box.
However, once it gets disconnected it is not possible to reconnect the keyboard again
since one does not have an input device any more.
To overcome this problem, one can add the following lines to a new file called `/etc/udev/rules.d/90-local.rules`:

```
# handle typing cover disconnects
# https://www.reddit.com/r/SurfaceLinux/comments/6axyer/working_sp4_typecover_plug_and_play/
ACTION=="add", SUBSYSTEM=="usb", ATTR{product}=="Surface Type Cover", RUN+="/sbin/modprobe -r i2c_hid && /sbin/modprobe i2c_hid"
```

## Enable the brightness and volume keys
