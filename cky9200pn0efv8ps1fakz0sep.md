---
title: "Alacritty terminal emulator"
datePublished: Mon Jan 10 2022 19:03:57 GMT+0000 (Coordinated Universal Time)
cuid: cky9200pn0efv8ps1fakz0sep
slug: alacritty-terminal-emulator
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1639762371087/TZjvfgrmN.png
tags: linux, terminal

---

If you're following along with [this series](https://travishorn.com/series/arch-linux), we've already installed Arch Linux and xmonad window manager. Now, I think it's time to install a new terminal emulator.

In the last article, we installed **xterm** when we installed xmonad. xterm is the default terminal in xmonad, but there are better options. Like [Alacritty](https://github.com/alacritty/alacritty).

> Alacritty is a modern terminal emulator that comes with sensible defaults, but allows for extensive configuration. By integrating with other applications, rather than reimplementing their functionality, it manages to provide a flexible set of features with high performance.

# Installation

Install Alacritty with pacman

```
sudo pacman -S alacritty
```

Since we want this to be the default terminal in xmonad, edit `~/.xmonad/xmonad.hs`

Change the `myTerminal` line to

```
myTerminal = "alacritty"
```

Exit and save the file, restart xmonad (Alt + q), and launch a new terminal (Alt + Shift + Enter)

You should see a noticable difference in the new terminal that launches. Alacritty's font, size, and colors are different than xterm's.

# Configuration

Copy the default configuration

```
mkdir -p ~/.config/alacritty
cp /usr/share/doc/alacritty/example/alacritty.yml ~/.config/alacritty/alacritty.yml
```

Edit alacritty's config file at `~/.config/alacritty/alacritty.yml`

You'll see that this file has a ton of options ready for you to customize them. By default, most options are commented out. In order to set an option, uncomment the line and change the value.

Some options change immediately on save. Others may require a restart of the terminal. Others still may require a restart of the window manager.

My minimal configuration looks something like this (I've removed all of the comments for brevity)

```
window:
  padding:
    x: 6
    y: 6

font:
  offset:
    y: 10

  glyph_offset:
    y: 5

colors:
  primary:
    foreground: '#ffffff'

background_opacity: 0.8
```

My configuration might not be exactly what you like, so feel free to play around and configure Alacritty how you like.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639760975613/PQjeJgsl2.png)

Note: Although I set the background opacity to 80%, I haven't been able to get Alacritty to actually show this effect, yet. After digging around, it looks like it intermittently has this problem depending on changes between updates. Perhaps in the next update, it will be working for my setup.

The system is really starting to come together. In future article's I'll go over to set up a status bar (xmobar) and a new text editor (Vim).