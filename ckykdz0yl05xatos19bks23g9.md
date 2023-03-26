---
title: "xmobar"
datePublished: Tue Jan 18 2022 17:24:33 GMT+0000 (Coordinated Universal Time)
cuid: ckykdz0yl05xatos19bks23g9
slug: xmobar
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1639762498470/KXf6Jn0Wq.png
tags: linux

---

So far in [this series](https://travishorn.com/series/arch-linux), we've installed Arch Linux, the xmonad window manager, and Alacritty terminal emulator. The system is actually pretty usable at this point. However, a very common UI element for most desktops is some sort of status bar to display things like system information, the date/time, and much more.

xmobar is a status bar written in Haskell that plays nicely with our currently installed window manager. Like everything we've installed, it's extremely customizable. In this article, I'll go over a basic setup. But know that there are endless features to this program. You can do almost anything with it if you put in the effort.

Follow along and we'll install and configure xmobar.

# Installation

Install xmobar and, if you don't already have one installed, a monospace font. I like **Fira Mono**, but you may want to search and install one that you like instead.

```
sudo pacman -S xmobar ttf-fira-mono
```

Download a standard configuration

```
curl -o ~/.xmobarrc https://raw.githubusercontent.com/jaor/xmobar/master/examples/xmobar.config
```

Before we configure xmobar, we need to configure xmonad to launch it.

Edit `~/.xmonad/xmonad.hs`

In the imports section, add the following line to import modules for managing docking programs

```
import XMonad.Hooks.ManageDocks
```

Change the line that says `myLayout = [...]` to

```
myLayout = mySpacing $ avoidStruts tiled ||| Mirror tiled ||| Full
```

If you didn't configure gaps in the last article, you might not have the `mySpacing $` part. In that case, that's okay. Just leave that part out.

The line above makes sure that your status bar isn't hidden behind your open windows.

Change the `main` function to

```
main = do
  xmproc <- spawnPipe "~/.fehbg"
  xmproc <- spawnPipe "picom"
  xmproc <- spawnPipe "xmobar"
  xmonad $ docks defaults
```

If you didn't install **feh** or **picom** in the earlier articles, that's okay. Just leave out those lines again. But note that if you haven't yet imported it, you'll need to make sure `import XMonad.Util.Run` is in your imports section at the top.

Pay extra attention that we didn't only add the xmobar line above. We also changed the call to xmonad so that it takes our docks into account (the last line).

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639761196091/nR6KurIvT.png)

Exit and save the file

## Set the font

I haven't had much luck running xmobar with the default font setting. My solution is to replace it with a known good setting.

Edit `~/.xmobarrc`

Replace everything in quotes after `font =` to

```
xft:Fira Mono:pixelsize=12:antialias=true:hinting=true
```

That's it! xmobar is ready to go. Press Alt + q to restart xmonad. You should see the status bar at the top of your screen.

# Configuration

By default, xmobar shows a lot of information:
 - CPU usage
 - Memory usage
 - Swap usage
 - Network adapter usage
 - The date and time
 - The temperature outside
 - The version of Linux you're running

I don't necessarily need all that information and I remove a lot of it. Feel free to add or remove anything you like. The [xmobar homepage](https://xmobar.org/) has a ton of information about using it.

Edit the configuration file at `~/.xmobarrc`

The weather module uses ICAO codes for identifying the weather station. You can search the internet for `icao [your city here` to find a nearby station and it's ICAO code. The closest one to me is `KSTL`, so I'll replace `EGPF` (in two places) in the configuration file.

I'm only going to show the temperature in one area, so I don't need to see the name of the station. I remove the `<station>: ` part of the file.

Since I'm used to seeing the weather in Fahrenheit, I change `<tempC>C` to `<tempF>F`.

Personally, I don't want the temperature display to change colors based on the temperature itself, so I remove the options for `-L`, `-H`, `--normal`, `--high`, and `--low` in the weather module.

I don't care about network usage so I remove all 4 lines that have to do with the `eth0` and `eth1` modules.

I also don't care about the swap or Linux version, so I remove the lines regarding `Swap` and `Com "uname"`.

The date display is nice, but I want it to be formatted a little differently. You can see the different options on Haskell.org's [Date.Time.Format](https://hackage.haskell.org/package/time-1.13/docs/Data-Time-Format.html) page. I change the format string to `%b %_d %-l:%M %p` but you will probably have your own preferences.

Finally, the template needs to be modified to remove the unnecessary parts and reorder a few things. The new template will look like this:

```
"%cpu% | %memory% }{ %KSTL% <fc=#ffffff>%date%</fc>"
```

Exit and save the file

In order to see the changes, kill the current xmobar process

```
killall xmobar
```

Restart xmonad with Alt + q, which will relaunch xmobar during startup.

If, for some reason, it doesn't pop back up, try running `xmobar` by itself and see if any errors appear in your terminal.

My minimal `.xmobarrc` looks like this:

```
Config { font = "xft:Fira Mono:pixelsize=12:antialias=true:hinting=true"
       , additionalFonts = []
       , borderColor = "black"
       , border = TopB
       , bgColor = "black"
       , fgColor = "grey"
       , alpha = 255
       , position = Top
       , textOffset = -1
       , iconOffset = -1
       , lowerOnStart = True
       , pickBroadest = False
       , persistent = False
       , hideOnStart = False
       , iconRoot = "."
       , allDesktops = True
       , overrideRedirect = True
       , commands = [ Run Weather "KSTL" ["-t","<tempF>F"] 36000
                    , Run Cpu ["-L","3","-H","50",
                               "--normal","green","--high","red"] 10
                    , Run Memory ["-t","Mem: <usedratio>%"] 10
                    , Run Date "%b %_d %-l:%M %p" "date" 10
                    ]
       , sepChar = "%"
       , alignSep = "}{"
       , template = "%cpu% | %memory% }{ %KSTL% <fc=#ffffff>%date%</fc>"
       }
```

You can use it, modify it, or use something completely different. It's up to you how you want your status bar to appear. Don't forget to check the [xmobar homepage](https://xmobar.org/) for all the different configuration options.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639761573968/YsK8VjgZr.png)

Nano is really easy to use for new users, but I'm getting tired of using it. I'm more comfortable in Vim and can get things done faster with it. In the next article, I'll walk through installing, configuring, and using it instead of Nano.