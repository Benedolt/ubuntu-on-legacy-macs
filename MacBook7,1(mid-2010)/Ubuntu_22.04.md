# Ubuntu 22.04 on the MacBook 7,1 (Mid-2010 Polycarbonate)
*last updated April 2025*

As of Ubuntu 22.04.2 this is super easy and straight-forward. Simply create a USB installer and boot from it. (Hold down the alt/option key after the chime to access the boot menu.) Install, boot into your new OS afterwards and everything should work fine.

There are a couple quality of life fixes though:

## Get Grub to display
Out of the box grub doesn't show up on this MacBook. In the grub config file `/etc/default/grub` uncommenting the line `GRUB_TERMINAL=console` and set `GRUB_TIMEOUT_STYLE=menu` to fixes. (Don't forget to run `sudo update-grub` after changing the the config file.)

## Switch "fn" und left "ctrl" key
With all my other keyboards I'm used to the `ctrl` key being the leftmost key in the bottom row on the keyboard, on MacBooks that's where the `fn` key is however. Either you retrain your muscle memory or you just switch the keys back the way they're meant to be. :) In order to do that:
```
$ sudo nano /etc/modprobe.d/hid_apple.conf
```
Then put `options hid_apple swap_fn_leftctrl=1` in the file, save it and run `sudo update-initramfs -u`.

After a reboot Ubuntu will believe the leftmost key in the bottom row is the `ctrl` key. I also always physically switch the keys. To do that just gently pry up the keys, lifting the top left corner, switch `ctrl` and `fn`, click them into place and voilà.

**Note:** I also like to add `options hid_apple fnmode=2` to that file in order to default to F# keys instead of to media keys.

# Install nvidia drivers
Switching from the nouveau to the nvidia drivers is not really neccessary anymore. As of the point release of Ubuntu 22.04.5 (kernel 6.8) nouveau runs really well. Before the point release of Ubuntu 22.04.2 that was different: nouveau/wayland would freeze constantly. This could be fixed by installing the proprietary nvidia drivers. Another argument for the proprietary driver is video acceleration, which doesn't seem to work with nouveau/wayland. So here's how you do it: 

The MacBook7,1 uses an Nvidia GeForce 320M which is only supported in the legacy nvidia-driver version 340. This version isn't available in Ubuntu anymore so we have to jump through a few hoops:

### 1. Make sure you're running a compatible kernel
We're going to get the legacy `nvidia-340` driver from [Butterfly/kelebek333's PPA](https://launchpad.net/~kelebek333/+archive/ubuntu/nvidia-legacy). When the kernel changes, the driver in that PPA sometimes breaks, as was the case around early 2023. If that's still the case, you'd now have to downgrade from your HWE 6.x to the stock 5.15 kernel that shipped with Ubuntu 22.04. 

First use `uname -r` to check which kernel version you're on and then downgrade using `sudo apt install --install-recommends linux-generic`. This will switch you from the HWE to the original/stock kernel. Note, that this kernel is still supported until the end of life of Ubuntu 22.04 - so you're still getting all security updates, you're just missing out on new kernel features.

### 2. Set PCI-E bus registers
Because of a quirk with UEFI and the way the Mac sets its PCI-E registers the nvidia driver won't work out of the box but will lead to a black screen. Find out how to do this over at [AskUbuntu](https://askubuntu.com/a/613573/21008).

### 3. Install the `nvidia-340` driver
Finally it's time to actually install the nvidia driver. As the driver is not available in the official repositories anymore we can't just use the Ubuntu driver installer but need to add a PPA instead:
```
$ sudo add-apt-repository ppa:kelebek333/nvidia-legacy
```

Installing the package `xorg-modulepath-fix` from that PPA executes a fix we need and then pulls in the correct `nvidia-340` package: 
```
$ sudo apt install xorg-modulepath-fix
```
### 4. Enjoy
If `apt` finishes without errors you're ready to reboot. You'll notice that the boot animation is somewhat garbled, but apart from that you're now running the correct nvidia driver and a stable X.org session.

**Note:** Brightness controls is the only feature that doesn't work out of the box. Brightness is always at the max value.

## Quality of Life Fixes when running with nvidia drivers/X.org

### Fix multitouch gestures
Unsurprisingly the legacy nvidia driver only supports legacy display servers, so you're now running traditional X11 instead of Wayland. That means that Gnome's nifty touchpad gestures (e.g. three finger swipe-up) don't work, because they are Wayland-only. I find it jarring to not have my touchpad gestures on a Mac and luckily you can re-enable them using José Expósito's `touchegg` and the `X11 Gestures` Gnome extension. 

Install touchegg from José Expósito's PPA:
```
$ sudo add-apt-repository ppa:touchegg/stable
$ sudo apt update
$ sudo apt install touchegg
```

Then install the `X11 Gestures` extension from [here](https://extensions.gnome.org/extension/4033/x11-gestures/) and you're good to go.

**Additional tip**: I find installing Gnome extensions from the browser cumbersome. I prefer to use the Gnome Extension Manager, which you can get from the repo:
```
$ sudo apt install gnome-shell-extension-manager
```
Now you can lauch the extension manager from the dash and then search for and install the extension from there.

### Get the delete key back

Using X.org you can use this method in order to convert the eject key in the top right of the keyboard to be the delete key. Again this is totally optional - I just never need an eject key, but often need to delete stuff. :)

To do that, install AutoKey from the repos: `sudo apt install autokey-gtk` Then run AutoKey, create a new Phrase, put `<delete>` in the big white text entry box and set the hotkey to `<code169>` and save. (Use "press to set".) In the preferences window make sure that AutoKey is set to startup on login and that's that. :)

## Ressources
- [MacBook Specs on Everymac](https://everymac.com/systems/apple/macbook/specs/macbook-core-2-duo-2.4-white-13-polycarbonate-unibody-mid-2010-specs.html)
