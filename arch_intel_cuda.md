# Using CUDA without enabling nvidia graphics on Arch Linux

## Step 1. Install CUDA

~~~
sudo pacman -Sy cuda
~~~

If you want Tensorflow to work on top of CUDA:

~~~
pacman -Sy python-tensorflow-opt-cuda
~~~

## Step 2. Install nvidia proprietary driver

~~~
sudo pacman -Sy nvidia
~~~

If you reboot now, the graphic server may not be able to start. 

## Step 3. Tell Xorg to use intel

~~~
# /etc/X11/xorg.conf.d/20-nvidia.conf
Section "ServerLayout"
     Identifier "layout"
     Screen 0 "intel"
     Screen 1 "nvidia"
EndSection

Section "Screen"
     Identifier "intel"
     Device "intel"
EndSection

Section "Device"
     Identifier "intel"
     Driver "modesetting"
     BusID "PCI:0@0:2:0"
#     Option "AccelMethod" "glamor"
#     Option "DRI" "3"
EndSection

Section "Device"
     Identifier "nvidia"
     Driver "nvidia"
     BusID "PCI:2@0:0:0"
     Option "ConstrainCursor" "off"
EndSection

Section "Screen"
     Identifier "nvidia"
     Device "nvidia"
     Option "AllowEmptyInitialConfiguration" "on"
     Option "IgnoreDisplayDevices" "CRT"
EndSection
~~~

[[source](https://gist.github.com/alexlee-gk/76a409f62a53883971a18a11af93241b)]

## Step 4. Recover GLX capabilities

GLX may be broken (you can test with `glxgears`) if you don't have bumblebee installed:

~~~
sudo pacman -Sy bumblebee
~~~

[[source](https://bbs.archlinux.org/viewtopic.php?pid=1476069#p1476069)]
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU0NDI4MTA0Ml19
-->