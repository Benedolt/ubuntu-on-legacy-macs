# Ubuntu 26.04 on the iMac13,1 (Mid-2012, 21")
*created May 8 2026*

This is the first of the tapered-edge iMacs, and it runs Ubuntu easily.

```

                             ....              user@iMac13-1
              .',:clooo:  .:looooo:.           -----------------
           .;looooooooc  .oooooooooo'          OS: Ubuntu 26.04 (Resolute Raccoon) x86_64
        .;looooool:,''.  :ooooooooooc          Host: iMac13,1 (1.0)
       ;looool;.         'oooooooooo,          Kernel: Linux 7.0.0-15-generic
      ;clool'             .cooooooc.  ,,       Uptime: 13 mins
         ...                ......  .:oo,      Packages: 1651 (dpkg), 12 (snap)
  .;clol:,.                        .loooo'     Shell: fish 4.2.1
 :ooooooooo,                        'ooool     Display (iMac): 1920x1080 in 22", 60 Hz [External]
'ooooooooooo.                        loooo.    DE: GNOME 50.1
'ooooooooool                         coooo.    WM: Mutter (Wayland)
 ,loooooooc.                        .loooo.    WM Theme: Yaru
   .,;;;'.                          ;ooooc     Theme: Yaru [GTK2/3/4]
       ...                         ,ooool.     Icons: Yaru [GTK2/3/4]
    .cooooc.              ..',,'.  .cooo.      Font: Ubuntu Sans (11pt) [GTK2/3/4]
      ;ooooo:.           ;oooooooc.  :l.       Cursor: Yaru (24px)
       .coooooc,..      coooooooooo.           Terminal: Ptyxis 50.1
         .:ooooooolc:. .ooooooooooo'           Terminal Font: Ubuntu Sans Mono (11pt)
           .':loooooo;  ,oooooooooc            CPU: Intel(R) Core(TM) i5-3330S (4) @ 3.20 GHz
               ..';::c'  .;loooo:'             GPU: NVIDIA GeForce GT 640M Mac Edition [Discrete]
                                               Memory: 2.29 GiB / 7.19 GiB (32%)
                                               Swap: 0 B / 4.00 GiB (0%)
                                               Disk (/): 11.40 GiB / 914.78 GiB (1%) - ext4
                                               Local IP (wlp4s0b1): 192.168.0.51/24
                                               Locale: de_DE.UTF-8
```

## Installation

To install Ubuntu 26.04 all you need to do is create your install USB drive, stick it into the MacBook, hold down the `Alt` key and select `EFI Boot`. Then install normally.

## Fixing WIFI

WIFI doesn't work out of the box. We're dealing with another of the ever-wonky Broadcom cards, the `BCM4331` specifically. In its initial release of Ubuntu 26.04 neither the `Install third party drivers` option in the installer nor the driver utility doesn't seem to be able to deal with this conundrum and you have to manually install the Broadcom firmware by plugging in a network cable and running:

`sudo apt install b43-fwcutter firmware-b43-installer`

Then reboot and you should be all set. 

### Notes 
- [Here](https://wiki.ubuntuusers.de/WLAN/Karten/Broadcom/) you can find a handy (albeit German language) list of what Broadcom card needs which driver... 
