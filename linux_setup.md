---
title: My Arch Linux setup
---

## Operating system

My laptop (Dell XPS 15 9530) is running a Manjaro-based system created with [Manjaro Architect](https://manjaro.org/2017/03/27/install-manjaro-as-you-want-it-with-architect/), which means I was too lazy to learn to properly install Arch Linux. I chose an Arch Linux derivative essentially for its extensive documentation and because [the AUR](https://aur.archlinux.org/) exists.

I use [fish](https://fishshell.com/) as my default shell, more for its convenience than for its scripting language, which I hear is pretty nice, but I use Ruby for that anyway.

## Desktop

I use i3 as my main window manager -- GNOME as a backup, just in case everything br. i3 is a tiling window manager, which means windows are generally not allowed to overlap each other. This makes organizing your desktop way easier than using floating windows, but takes a while to learn.

One cool thing about simple window managers such as i3 is that you get to extend them via whatever programs you want. If you want a futuristic information display with all the readings from your hardware, you can use conky! Whether you want a full-fledged panel and app launcher or a minimalistic bar, you have the options! This way, you can put together the pieces you want in order to make a perfect desktop experience, tailored just for you.

![](desktop_empty.png)

This is what my desktop looks like when no apps are running. That bar up there is managed by [yabar](https://github.com/geommer/yabar), and shows the current desktop and active window, the date and time, and some hardware info.

I have defined keyboard bindings that allow me to quickly launch a terminal window, a file browser or an editor window. Every other app can be accessed from [rofi](https://github.com/DaveDavenport/rofi/), a highly customizable launcher, with <kbd>Super</kbd>+<kbd>Space</kbd> (macOS users will feel at home here).

## Themes and icons

- Flat-Plat
- Moka

## Other configuration

### Fonts

- Noto Sans/Cantarell
- SourceCodePro-Powerline-Awesome

### HiDPI management

- Xresources
- gnome gsd-xsettings

### Exposé-like view

- skippy-xd

### Hot corners and screen edged

- brightside
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkzNTQ5MTY4XX0=
-->