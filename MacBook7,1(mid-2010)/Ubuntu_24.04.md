# Ubuntu 24.04

This is still very much a work in progress...

## Initial findings
- Needs `nomodeset` to even boot. (must `chroot` to edit grub config...)
- Wake from sleep doesn't work
- Brightness controls don't work (b/c of `nomodeset` I believe)
- Performance is generally very choppy, YouTube playback unusable, even at 360p/AVC
- Wayland/Xorg makes no difference with nouveau
- nvidia-340 install from PPA works fine, but blackscreens (forgot to change PCIE registers though, so that might be it)