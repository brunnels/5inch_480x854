#### Display Rotation

Modify /boot/config.txt add following
```
#display_rotate 1=90, 2=180, 3=270
display_rotate=1
```

#### Touchscreen touch rotation
If your touchscreen isn't registering touches properly after the screen has been rotated, you will need to apply a
transformation matrix.

First you will need your device name.

Run: `DISPLAY=:0 xinput`

Output
```
⎡ Virtual core pointer                          id=2    [master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer                id=4    [slave  pointer  (2)]
⎜   ↳ EP0110M09                                 id=6    [slave  pointer  (2)]
⎣ Virtual core keyboard                         id=3    [master keyboard (2)]
    ↳ Virtual core XTEST keyboard               id=5    [slave  keyboard (3)]
```
In this case the device is the EP0110M09 Touchscreen, yours may be different

You can test a change by running:

`DISPLAY=:0 xinput set-prop "<device name>" 'Coordinate Transformation Matrix' <matrix>`

Where the matrix can be one of the following options:

* 0°: `1 0 0 0 1 0 0 0 1`
* 90° Clockwise: `0 -1 1 1 0 0 0 0 1`
* 90° Counter-Clockwise: `0 1 0 -1 0 1 0 0 1`
* 180° (Inverts X and Y): `-1 0 1 0 -1 1 0 0 1`
* invert Y: `-1 0 1 1 1 0 0 0 1`
* invert X: `-1 0 1 0 1 0 0 0 1`

For example:

`DISPLAY=:0 xinput set-prop "EP0110M09" 'Coordinate Transformation Matrix' 0 1 0 -1 0 1 0 0 1`

To make this permanent, modify the file `/etc/udev/rules.d/51-touchscreen.rules` and add following line:

```
ACTION=="add", ATTRS{name}=="EP0110M09", ENV{LIBINPUT_CALIBRATION_MATRIX}="0 1 0 -1 0 1 0 0 1"
```


