## About
- This is just a tutorial on how to install Debian with XFCE4 on Termux.
- Don't trust all of this without researching.

- Original Blog: [https://ivonblog.com/en-us/posts/termux-proot-distro-debian/]

## Installation
- Install Termux.
- Setup Termux X11.
- Install virglrenderer.

### Install Termux X11
- Termux X11: [https://github.com/termux/termux-x11/releases/download/nightly/app-universal-debug.apk]

### Install virglrenderer-Android
- Install virglrenderer-android
```sh
pkg install virglrenderer-android
```
- You can start virgl server by executing this command
```sh
virgl_test_server_android &
```

### In Termux
```sh
pkg install x11-repo termux-x11-nightly virglrenderer-android pulseaudio nano
nano ~/.profile
proot-distro login debian --user root --shared-tmp
```

### In Debian
```sh
apt install sudo vim firefox-esr

passwd

groupadd storage
groupadd wheel
groupadd video

useradd -m -g users -G wheel,audio,video,storage -s /bin/bash user
passwd user

visudo
find something like root ALL=(ALL:ALL) ALL. Add this in the next line of it.
user ALL=(ALL:ALL) ALL

su user
cd

apt install xfce4 xfce4-goodies dbus-x11
ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime
apt install locales fonts-noto-cjk
```
```sh
nano /etc/locale.gen
Uncomment (remove “#”) your langauge.
```
```sh
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

## Desktop Environment
There's 2 ways to do this, depend on your system.

### Start desktop environment (manually)
- Relaunch Termux
- Launch Termux X11 app, keep it opening in the background. Go back to Termux and type the following commands to run Termux X11.

```sh
pulseaudio --start --exit-idle-time=-1
pacmd load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1

export DISPLAY=:0
termux-x11 :0 &

virgl_test_server_android &

proot-distro login debian --user user --shared-tmp
```

- Start desktop environment
```sh
export DISPLAY=:0
PULSE_SERVER=tcp:127.0.0.1
dbus-launch --exit-with-session startxfce4 &
```

- You shall see XFCE4 desktop showing at Termux X11. Tap floating window and revoke permission to make it go full screen.

### Start desktop environment in one-click
- Use Termux Widget to start everything automatically
    + Install Termux API and Termux Widget
      
      Termux API: [https://github.com/termux/termux-api/releases/download/v0.53.0/termux-api-app_v0.53.0+github.debug.apk]
      Termux Widget: [https://github.com/termux/termux-widget/releases/download/v0.15.0/termux-widget-app_v0.15.0+github.debug.apk]

    + Go to system settings → all apps, turn on “Permit Drawing Over Other Apps” for Termux.
    + Restart Termux. Create a shortcut in Termux (not in proot-distro)
    ```sh
    mkdir .shortcuts
    nano .shortcuts/startproot_debian.sh
    ```
    + Type these
    ```sh
    #!/bin/bash

    killall -9 termux-x11 pulseaudio virgl_test_server_android termux-wake-lock

    termux-toast "Starting X11"
    am start --user 0 -n com.termux.x11/com.termux.x11.MainActivity
    XDG_RUNTIME_DIR=${TMPDIR}
    termux-x11 :0 -ac &
    sleep 3

    pulseaudio --start --exit-idle-time=-1
    pacmd load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1

    virgl_test_server_android &

    proot-distro login debian --user user --shared-tmp -- bash -c "export DISPLAY=:0 PULSE_SERVER=tcp:127.0.0.1; dbus-launch --exit-with-session startxfce4"
    ```
    + Make it executable.
    ```sh
    chmod +x .shortcuts/startproot_debian.sh
    ```
    + Go to your home screen, long press and add widgets → select “Termux Widget”. You would see the shortcut we made is on the list.
    + Click “startproot_debian.sh” then the XFCE desktop would start automatically.
    + Swipe down the notification bar, click Preferences of Termux X11. Then you can switch touch screen mode to simulating touchpad.
    + If the fonts are too small in Termux X11, click Settings Manager at top-left → Appearance and change font size or select 2x window scaling.
    + To stop the XFCE session, press CTRL+C in Termux. Then logout of proot Debian.
    ```sh
    exit
    ```

## Temp Commands
```sh
wget -qO- https://raw.githubusercontent.com/Botspot/pi-apps/master/install | bash
```
[https://dl.llaun.ch/legacy/bootstrap]
```sh
sudo apt install openjdk-17-jdk
```

## Conclusion
- That is it currently, I have no plan to add anything soon.
