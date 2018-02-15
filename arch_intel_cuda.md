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

## Step 4. Recover GLX capabilities

GLX will be broken (you can test wil if you don't have bumblebee installed:

~~~
sudo pacman -Sy bumblebee
~~~
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzUxMzQ1NzgxXX0=
-->