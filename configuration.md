# Configuration

## Install a window manager

## Setup wireless networking

To make the WiFi card work reliably, the official Marvell drivers have be installed.
Clone the git repository at `git://git.marvell.com/mwifiex-firmware.git` and copy the `mrvl` folder to `/lib/firmware/mrvl`.

Since Network Manager does not work reliably, it is recommended to use `netctl` instead.
Instructions can be found in the relevant [Arch wiki article](https://wiki.archlinux.org/index.php/netctl).

At the point of this writing, the official Marvell drivers cannot wake up the network card from sleep. To circumvent this problem, it might help to disable the power management for this device by adding the following lines to a new file called `/etc/udev/rules.de/70-wifi-powersafe.rules`:

```
# Disable power management for wifi card
ACTION=="add", SUBSYSTEM=="net", KERNEL=="wlp2s0", RUN+="iwconfig wlp2s0 power off"
```

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

When using i3, the brightness and volume keys can be enabled by the following lines:

```
# Volume and screen brightness
# http://askubuntu.com/a/823691

# Pulse Audio controls
bindsym XF86AudioRaiseVolume exec --no-startup-id pactl set-sink-volume 0 +5% # increase sound volume
bindsym XF86AudioLowerVolume exec --no-startup-id pactl set-sink-volume 0 -5% # decrease sound volume
bindsym XF86AudioMute exec --no-startup-id pactl set-sink-mute 0 toggle # mute sound

# Sreen brightness controls
bindsym XF86MonBrightnessUp exec xbacklight -inc 10 # increase screen brightness
bindsym XF86MonBrightnessDown exec xbacklight -dec 10 # decrease screen brightness
```

Note that the brightness keys are actually not the alternative keys behind the `F1` and `F2` keys. These only control the keyboard backlight brightness. The display brightness can be changed using `Fn+Del` and `Fn+Backspace`.

## Connect to bluetooth devices

To manage bluetooth devices using `bluetoothctl`, install the *bluez* and *bluez-utils* packages and follow the instructions from the [Arch wiki article](https://wiki.archlinux.org/index.php/Bluetooth).
