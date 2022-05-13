***How to rotate the display 180 degrees***

modify /boot/config.txt add follow line
```
display_rotate=1 #1:90;2: 180; 3: 270, Need to modify 40-libinput.conf:"0 1 0 -1 0 1 0 0 1” ->90 "-1 0 1 0 -1 1 0 0 1”->180 "0 -1 1 1 0 0 0 0 1”->270
```

then copy 40-libinput-conf as follows:
```
sudo apt-get install xserver-xorg-input-libinput
sudo mkdir /etc/X11/xorg.conf.d
sudo cp /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf.d/
```

next modify 40-libinput-conf and add "Option "Cali..." line as follows:
```
Section "InputClass"
Identifier "libinput touchscreen catchall"
MatchIsTouchscreen "on"
Option "CalibrationMatrix" "0 1 0 -1 0 1 0 0 1"
```

**If RPI4 is used in landscape, modify config.txt as follows (854-->856):**
```
dpi_timings=480 0 60 2 55 856 0 16 2 22 0 0 0 60 0 32000000 6
```
