# Ubuntu 22.04 on the MacBook 7,1 (Mid-2010 Polycarbonate)

The Mid-2010 white polycarbonate MacBook is an iconic device. Sure, with its Core 2 Duo P8600 it's not going to break any speed records, but if you adjust expectations accordingly it's still a very competent machine for webbrowsing and some media comsumption. Installing an SSD and doing a RAM upgrade (it can go up to 16GB - double that of the Pro models from the era!) helps of course. This document outlines how to get a modern Ubuntu OS up and running.
  
## Basic Installation 
This is very straight-forward. Simply create a USB installer and boot from it. (Hold down the alt/option key after the chime to access the boot menu.) 

## Install nvidia drivers
You should now be able to boot to the desktop. Ubuntu will run with the open source nouveau driver however, which means the Macbook runs pretty hot and will randomly lock up. These lock-ups only affect the graphical stack and not the underlying OS so it's a good idea to quickly set up a ssh server (`sudo apt install openssh-server`) and execute the following steps remotely. 

The MacBook7,1 uses an Nvidia GeForce 320M which is only supported in the legacy nvidia-driver version 340. This version isn't available in Ubuntu anymore so we have to jump through a few hoops:

### 1. Make sure you're running a compatible kernel
As of now the legacy driver `nvidia-340` doesn't work with the newest hardware-enablement (HWE) kernel in Ubuntu 22.04. So use `uname -r` to check with version you're on. If you're running the original 5.15 kernel you're good and can move on to the next step. If you're running anything newer than 5.15 you need to downgrade kernel using `sudo apt install --install-recommends linux-generic`. This will switch you from the HWE to the original kernel. Note, that this kernel is still supported until the end of life of Ubuntu 20.04 - so you're still secure.

### 2. Set PCI-E bus registers
Because of a quirk with UEFI and the way the Mac sets its PCI-E registers the nvidia driver won't work out of the box but will lead to a black screen.
Find out how to do this over at [AskUbuntu](https://askubuntu.com/a/613573/21008).

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
  
## Quality of Life Changes
These steps are completely optional, but they considerably improve the usability of our venerable MacBook.

### Gesten nachinstallieren
Unsurprisingly the legacy nvidia driver only supports legacy display servers, so you're now running traditional X11 instead of Wayland. That means that Gnome's nifty touchpad gestures (e.g. three finger swipe-up) don't work, because they are Wayland-only. I find it jarring to not have my touchpad gestures on a Mac and luckily you can re-enable them using José Expósito's `touchegg` and the `X11 Gestures` Gnome extension. 

Install touchegg from José Expósito's PPA:
```
$ sudo add-apt-repository ppa:touchegg/stable
$ sudo apt update
$ sudo apt install touchegg
```

Then install the `X11 Gestures` extension from [here](https://extensions.gnome.org/extension/4033/x11-gestures/) and you're good to go.

**Additional tip**: I find installing Gnome extensions from the browser cumbersome. An IMO better way is to use the Gnome Extension Manager, which you can get from the repo:
```
$ sudo apt install gnome-shell-extension-manager
```
Now you can lauch the extension manager from the dash and then search for and install the extension from there.

### Switch "fn" und left "ctrl" key
With all my other keyboards I'm used to the `ctrl` key being the leftmost key in the bottom row on the keyboard, on MacBooks that's where the `fn` key is however. Either you retrain your muscle memory or you just switch the keys back the way they're meant to be. :) In order to do that:
```
$ sudo nano /etc/modprobe.d/hid_apple.conf
```
Then put `options hid_apple swap_fn_leftctrl=1` in the file, save it and run `sudo update-initramfs -u`.

After a reboot Ubuntu will believe the leftmost key in the bottom row is the `ctrl` key. I also always physically switch the keys. To do that just gently pry up the keys, lifting the top left corner, switch `ctrl` and `fn`, click them into place and voilà.

### Get the delete key back
And the last keyboard related hack that I always execute is converting the `eject` key in the top right of the keyboard to be the `delete` key. Again this is totally optional - I just never need an eject key, but often need to delete stuff. :)

To do that, install `AutoKey` from the repos: `$ sudo apt install autokey-gtk`
Run AutoKey, create a new `Phrase`, put `<delete>` in the big white text entry box and set the hotkey to `<code169>`. In the preferences window make sure that AutoKey is set to startup on login and that's that. :)

### Two finger gestures for Firefox
Under MacOS even Firefox supports two-finger swipes to jump back and forth. Under Ubuntu only Chromium-based browsers support that. I always install the Firefox extension [Two Finger History Jump](https://addons.mozilla.org/de/firefox/addon/two-finger-history-jump/) to get that feature back.

## TODO

- [ ] PPA sichern 
- [ ] Is the PCI-E register hack really neccessary?

## Ressources
- [MacBook Specs on Everymac](https://everymac.com/systems/apple/macbook/specs/macbook-core-2-duo-2.4-white-13-polycarbonate-unibody-mid-2010-specs.html)
