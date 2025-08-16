---
title: "Streamline Your Linux Experience with Automatic Updates"
datePublished: Wed Aug 16 2023 11:00:08 GMT+0000 (Coordinated Universal Time)
cuid: clldmfh5x000309l2htx880xy
slug: streamline-your-linux-experience-with-automatic-updates
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689274407972/c959546a-dde5-4c83-8df7-8bba1e942809.png
tags: linux, security, automation, debian, unattendedupgrades

---

Staying up-to-date with software updates and security patches is important for a secure computing experience. However, manually keeping track of updates and performing system reboots can be time-consuming and prone to oversight. Enter **UnattendedUpgrades**, a powerful utility for Debian that automates the update process, keeping your system current. In this guide, we will walk through the installation and customization of UnattendedUpgrades on Debian Linux, exploring features like modifying update schedules and scheduling reboots.

## Installation

Install `unattended-upgrades`.

```bash
sudo apt install -y unattended-upgrades
```

Activate it.

```bash
sudo dpkg-reconfigure -plow unattended-upgrades
```

By default, the system will check for and download upgrades twice a day:

* A random time in a 12 hour window starting at 6 AM
    
* A random time in a 12 hour window starting at 6 PM
    

It will then apply the upgrades once per day in a random time in a 60 minute window starting at 6 AM.

## **Modify the Schedule**

You can override the default schedule when upgrades are *downloaded*.

```bash
sudo systemctl edit apt-daily.timer
```

And editor will open. In the top section, enter the following.

```bash
[Timer]
OnCalendar=
OnCalendar=01:00
```

The above example sets the upgrades to download around 1 AM every day.

The line `OnCalendar=` (blank) is necessary because, without it, any additional `OnCalendar` lines simply **add** another time to the existing defaults. This line clears the defaults first.

Once the overrides are in place, restart the timer.

```bash
sudo systemctl restart apt-daily.timer
```

You can override the default schedule when upgrades are *applied*, as well.

```bash
sudo systemctl edit apt-daily-upgrade.timer
```

Follow the same procedure as when you edited the `apt-daily.timer` above: Write the timer configuration lines and restart the timer.

## **Scheduled Reboots**

Some upgrades require rebooting the system. One strategy is to set a specific day of the month to check and perform a reboot if required.

Create the script to check and reboot if required at `/usr/local/bin/reboot_if_required.sh`.

```bash
#!/bin/bash

if [ -f /var/run/reboot-required ]; then
  echo "Reboot required. Initiating reboot..."
  /sbin/shutdown -r now
else
  echo "No reboot required."
fi
```

Make the script executable.

```bash
sudo chmod +x reboot_if_required.sh
```

Create a systemd service for the script at `/etc/systemd/system/unattended_reboot.service`.

```bash
[Unit]
Description=Reboot (if required)

[Service]
Type=oneshot
ExecStart=/usr/local/bin/reboot_if_required.sh

[Install]
WantedBy=default.target
```

Create a matching timer for the service at `/etc/systemd/system/unattended_reboot.timer`.

```bash
[Unit]
Description=Reboot (if required) once per month on the 15th at 11 PM

[Timer]
OnCalendar=*-15 23:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

Enable the timer.

```bash
sudo systemctl enable unattended_reboot.timer
```

Start the timer.

```bash
sudo systemctl start unattended_reboot.timer
```

## **Rebooting Immediately After Upgrades**

You could take a more aggressive rebooting strategy instead. You can set the system to automatically reboot after UnattendedUpgrades detects that one is pending.

Edit `/etc/apt/apt.conf.d/50unattended-upgrades`. Uncomment this line:

```bash
//Unattended-Upgrade::Automatic-Reboot "false";
```

And change `false` to `true`.

```bash
Unattended-Upgrade::Automatic-Reboot "true";
```

## **Manual Rebooting**

If you don't use either of the two automatic options above, you can manually reboot when it works for you. You can check if a reboot is pending by checking for the presence of `/var/run/reboot-required`.

```bash
ls /var/run/reboot-required
```

If the file exists, consider rebooting at some point.

```bash
sudo shutdown -r now
```

Your server is now set up to apply automatic updates.

UnattendedUpgrades on Debian Linux is a game-changer when it comes to automating the update process and enhancing the overall stability and security of your system. By following the steps outlined in this article, you've learned how to install and activate UnattendedUpgrades, customize the update schedule to fit your needs, schedule reboots for minimal disruption, immediately reboot after upgrades, and manually reboot when necessary. Automation frees up your time, reduces the risk of missing critical updates, and ensures your Debian Linux system remains resilient and up-to-date. With this setup, you always know that your system is fortified against emerging vulnerabilities.

Cover photo by [Mohamed Nohassi](https://unsplash.com/@coopery?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-long-tunnel-with-a-light-at-the-end-of-it-YvoedPdh9JM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).