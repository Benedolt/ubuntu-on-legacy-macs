# Ubuntu 24.04
*updated April 2025*

I have tested both point releases 24.04.1 and 24.04.2 and you can get basic functionality, but it's really a far cry from the 22.04 experience... When running 24.04 compared to 22.04:
* booting takes a whole lot longer
* the desktop is considerably more choppy
* Youtube is unusable, even when playing back low resolution videos

## Initial installation and fixes to boot
So how did I get there? The installation runs smooth just like with 22.04. (And the new installer looks great!) But on first boot we're left at a black screen. We need the grub boot option `nomodeset` - but it's difficult to set, because grub is running in graphical mode and therefore isn't accessible...
So I booted the live session back up, used chroot to get into the installation on disk. ([This](https://livesys.se/posts/the-chroot-technique/) is a good guide on how to do that.) From the chroot environment, I edited `/etc/defaults/grub`: 
* I set `GRUB_TERMINAL=console` and `GRUB_TIMEOUT_STYLE=menu` to be able to access grub on boot
* I added `nomodeset` as kernel parameter to boot with "safe graphics"
* Never forget running `sudo update-grub` after updating the grub configuration

Now we have a booting system, however, as mentionned above the performance is just bad. Also brightness controls don't work, since we're booting with `nomodeset`.

These issues might be fixable - I haven't gotten into that though (yet - at least) and will stick to 22.04 for the time being.