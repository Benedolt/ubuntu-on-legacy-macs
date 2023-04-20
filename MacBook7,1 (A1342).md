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
sudo add-apt-repository ppa:kelebek333/nvidia-legacy
```

Installing the package `xorg-modulepath-fix` from that PPA executes a fix we need and then pulls in the correct `nvidia-340` package: 
```
sudo apt install xorg-modulepath-fix
```
If `apt` finishes without errors you're ready to reboot. You'll notice that the boot animation is somewhat garbled, but apart from that you're now running the correct nvidia driver and a stable X.org session.
  
## Quality of Life Changes
These steps are completely optional, but they considerably improve the usability of our venerable MacBook.

### Gesten nachinstallieren
  Mithilfe einer Gnome Shell Extension (X11 Gestures) können wir die Gesten wieder aktivieren.
  
  Extension Manager von hier installieren:
  https://flathub.org/apps/details/com.mattjakeman.ExtensionManager
  (Vorher apt install flatpak)  
  
  Touchégg installieren
  sudo apt install touchegg
  
  Extension "X11 Gestures" über den Extension Manager installieren
  Fertig.
  
### "fn" und linke "ctrl" Taste tauschen
  Erstelle: /etc/modprobe.d/hid_apple.conf
  Inhalt: options hid_apple swap_fn_leftctrl=1
  Ausführen: sudo update-initramfs -u
  Neustarten, Tasten physisch tauschen - fertig

### Autohotkey um Delete-Taste zu simulieren

## TODO

- [ ] PPA sichern 
- [ ] Helligkeitskontrolltasten wieder herstellen 
- [ ] gesture: 2-finger back and forth in browser
- [ ] Is the PCI-E register hack really neccessary?
