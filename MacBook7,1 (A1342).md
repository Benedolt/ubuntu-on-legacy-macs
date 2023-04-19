## Running Ubuntu 22.04 on the Macbook 7,1 (Mid-2010 Polycarbonate)

# Grundinstallation 
  ganz normal

# PCIE-Register hin und herschieben. xD
  https://askubuntu.com/questions/264247/proprietary-nvidia-drivers-with-efi-on-mac-to-prevent-overheating
  
# Nvidia-Treiber
  Standardmäßig läuft Ubuntu mit nouveau und Wayland. Das hängt sich ab und zu auf. 
  Die nVidia-Treiber sind nicht mehr im Repo und müssen über ein PPA installiert werden. 
  
  sudo add-apt-repository ppa:kelebek333/nvidia-legacy
  sudo apt install xorg-modulepath-fix
  
  Letzteres zieht auch die richtigen Treiber mit rein. (340)
  
  Die Boot-Animation ist jetzt nicht mehr fehlerfrei, dafür läuft Gnome stabil.
  Da Gnome jetzt allerdings auf X11 läuft, funktionieren die Gesten nicht mehr.
  
# Gesten nachinstallieren
  Mithilfe einer Gnome Shell Extension (X11 Gestures) können wir die Gesten wieder aktivieren.
  
  Extension Manager von hier installieren:
  https://flathub.org/apps/details/com.mattjakeman.ExtensionManager
  (Vorher apt install flatpak)  
  
  Touchégg installieren
  sudo apt install touchegg
  
  Extension "X11 Gestures" über den Extension Manager installieren
  Fertig.
  
# "fn" und linke "ctrl" Taste tauschen
  Erstelle: /etc/modprobe.d/hid_apple.conf
  Inhalt: options hid_apple swap_fn_leftctrl=1
  Ausführen: sudo update-initramfs -u
  Neustarten, Tasten physisch tauschen - fertig

# Autohotkey um Delete-Taste zu simulieren

## TODO
   PPA sichern 
   Helligkeitskontrolltasten wieder herstellen 
   gesture: 2-finger back and forth in browser
  
