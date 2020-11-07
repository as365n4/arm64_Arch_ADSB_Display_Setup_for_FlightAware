# arm64 Arch ADSB Display Setup for FlightAware

#### 1.)  Follow the instructions in my repo `Arch_on_Rock64` or the instructions given on the Arch Wiki https://archlinuxarm.org/platforms/armv8/rockchip/rock64	A 16GB eMMC module is sufficient as the whole setup requires only 3.29GB in space when finished.

#### 2.)  Install XFCE4

`sudo pacman -S xfce4`  select → ALL (just press ENTER)

`sudo pacman -S xorg-server`

#### 3.)  Hide xfce panel for Firefox full screen

`startxfce4`

then click Applications → Settings → Panel, Tab → Display, Section → General, Automatically hide the panel → Always

Do the above for `Panel 1` and `Panel 2` and don’t forget the Display Power Management settings ;-)

Close xfce4 and return to concole.

#### 4.)  Install Firefox

`sudo pacman -S firefox firefox-extension-privacybadger firefox-ublock-origin`  select → 1 (GNU Fonts)

#### 5.)  Make Firefox full screen and autostart

Open Firefox and install `“Auto Fullscreen”` Add-on by `tazeat`

`mkdir .config/autostart`

`nano .config/autostart/FlightAware.desktop`

    [Desktop Entry]
    Name=FlightAware
    Exec=firefox http://192.168.X.XXX:8080
    StartupNotify=true
    Hidden=false
    Terminal=false
    Type=Application

`xxx` should match your FlightAware Feeder Address → see my repo `arm64 Arch - ADSB Receiver Setup (FlightAware)` for details

`sudo reboot`

#### 6.)  Console auto log-in and start XFCE at boot/log-in

`nano /home/fjb/.bashrc`  add the following to the END of file

    if systemctl -q is-active graphical.target && [[ ! $DISPLAY && $XDG_VTNR -eq 1 ]]; then
                startxfce4
    fi

`sudo nano /etc/systemd/logind.conf`  uncomment `#NAutoVTs=6` and set to `NAutoVTs=2`

`sudo mkdir /etc/systemd/system/getty@tty1.service.d`

`sudo nano /etc/systemd/system/getty@tty1.service.d/override.conf`

    [Service]
    ExecStart=
    ExecStart=-/usr/bin/agetty --autologin fjb --noclear %I $TERM
    Type=simple

`sudo systemctl enable getty@tty1`

`sudo reboot`

Done, enjoy your new ADSB display !
