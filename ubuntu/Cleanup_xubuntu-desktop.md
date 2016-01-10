
credit:
http://askubuntu.com/questions/550300/how-to-properly-delete-xubuntu-desktop-with-its-loginmanager

# purge xubuntu-desktop and configures
$ sudo apt-get remove --purge abiword abiword-common abiword-plugin-grammar abiword-plugin-mathview alacarte bison blueman brltty-x11 catfish espeak exo-utils flex fonts-droid fonts-lyx gigolo gmusicbrowser gnome-system-tools gnome-time-admin gstreamer0.10-gnomevfs gthumb gthumb-data gtk2-engines-pixbuf indicator-application-gtk2 indicator-sound-gtk2 leafpad libabiword-3.0 libbison-dev libdigest-crc-perl libexo-1-0 libexo-common libexo-helpers libfl-dev libgarcon-1-0 libgarcon-common libgdome2-0 libgdome2-cpp-smart0c2a libglade2-0 libgnomevfs2-0 libgnomevfs2-common libgnomevfs2-extra libgsf-1-114 libgsf-1-common libgstreamer-perl libgtk2-notify-perl libgtk2-trayicon-perl libgtkmathview0c2a libgtkspell0 libido-0.1-0 libindicate-gtk3 libintl-perl libjpeg-progs libjpeg-turbo-progs libkeybinder0 liblink-grammar4 libloudmouth1-0 libnet-dbus-perl liboobs-1-5 libotr5 libots0 librarian0 libsexy2 libtagc0 libthunarx-2-0 libtidy-0.99-0 libtie-ixhash-perl libtumbler-1-0 libunique-1.0-0 libvte-common libvte9 libwv-1.2-4 libxfce4ui-1-0 libxfce4ui-utils libxfce4util-bin libxfce4util-common libxfce4util6 libxfcegui4-4 libxfconf-0-2 libxml-parser-perl libxml-twig-perl libxml-xpath-perl lightdm-gtk-greeter link-grammar-dictionaries-en m4 orage parole pastebinit pavucontrol pidgin pidgin-data pidgin-libnotify pidgin-microblog pidgin-otr plymouth-theme-xubuntu-logo plymouth-theme-xubuntu-text python-configobj rarian-compat ristretto screensaver-default-images scrollkeeper shimmer-themes system-tools-backends tcl8.5 thunar thunar-archive-plugin thunar-data thunar-media-tags-plugin thunar-volman tumbler tumbler-common xbrlapi xchat xchat-common xfburn xfce-keyboard-shortcuts xfce4-appfinder xfce4-cpugraph-plugin xfce4-dict xfce4-indicator-plugin xfce4-mailwatch-plugin xfce4-netload-plugin xfce4-notes xfce4-notes-plugin xfce4-notifyd xfce4-panel xfce4-places-plugin xfce4-power-manager xfce4-power-manager-data xfce4-quicklauncher-plugin xfce4-screenshooter xfce4-session xfce4-settings xfce4-systemload-plugin xfce4-taskmanager xfce4-terminal xfce4-verve-plugin xfce4-volumed xfce4-weather-plugin xfce4-xkb-plugin xfconf xfdesktop4 xfdesktop4-data xfwm4 xscreensaver xscreensaver-data xscreensaver-gl xubuntu-artwork xubuntu-default-settings xubuntu-desktop xubuntu-docs xubuntu-icon-theme xubuntu-wallpapers && sudo apt-get autoremove

-----------
# remove for re-install ubuntu-desktop
$ sudo apt-get remove --purge ubuntu-desktop && sudo apt-get autoremove

$ sudo reboot
then go into tty1   # Ctrl+Alt+F1

# install ubuntu-desktop
$ sudo apt-get update && sudo apt-get install ubuntu-desktop

$ sudo reboot
