# Customize Slax
Wrote this just to remember how I customized SLAX Linux. Done this for version 11.3.0.

## 1 Get Slax running
These would be things I would do even if I was using Slax without customization.

### 1.1 Download and copy iso
Grab the iso from https://www.slax.org/ Follow instructions to make it bootable.

Note, if in windows just execute bootinst.bat. If in linux, copy the iso files to USB flash drive as root (or using sudo), otherwise when running bootinst.sh it will fail. Running bootinst.sh will also require root user / sudo.

### 1.2 Setup network access and updates
This can be as simple as running Slax for the first time as usual and set the network using the GUI. Or find how to do it using connman if you want to do it in the terminal.

Always good to start with
```
apt update && apt upgrade
```

### 1.3 Fix keyboard
```
fbsetkb gb
```

### 1.4 Correct changing time in dual boot
Because I use Slax in a windows laptop, when I go back to Windows it is common to have the wrong time. To fix this issue I'll set slax to use the local time for the hardware clock (RTC). In terminal type: 
```
timedatectl set-local-rtc 1
```
And probably will need to fix the system and hardware time:
```
timedatectl set-timezone Europe/London
```

## 2. Login as guest user
Slax is set to automaticaly login as root. I prefer to log in as guest. But first we need to prepare fluxbox for guest user. 

### 2.1 Create new user - Optional
Optionally you can create new user if you don't want to be called Guest. In the terminal type the below to create a new user:
```
useradd -m username
passwd username
```
In this tutorial I'm happy with the already Slax existent "Guest" user.

### 2.2 Change root password
Might wandering why use guest user if Slax root pass is widely known. Thought about removing the root account altogether, but it's simpler to just change the password.
```
passwd
```
Don't forget to change guest's password as well
```
passwd guest
```

### 2.3 Prepare guest fluxbox
```
cp /root/.Xresources /home/guest
cp /root/.blackbox-menu /home/guest
cp /root/.blackboxrc /home/guest
cp -R /root/.config /home/guest
cp -R /root/.fluxbox /home/guest
```
Slax Fluxbox assumes you're root, hence need to modify some files

Edit `/home/guest/.fluxbox/init` in the below line
```
session.screen0.windowMenu:	/root/.fluxbox/windowmenu
```
to
```
session.screen0.windowMenu:	~/.fluxbox/windowmenu
```

Edit `/home/guest/.fluxbox/menu`. Delete (almost all) and replace by the below:
```
[begin] (Desktop menu)
   [exec] (Terminal) { xterm -ls }
   [exec] (File Manager) { pcmanfm }
   [exec] (Web Browser) { chrome }
   [exec] (Text Editor) { scite }
   [exec] (Calculator) { gnome-calculator }
   [exec] (Network Manager) { connman-gtk }
   [exec] (Run) { fbappselect }
   [separator]
   [workspaces] (Workspaces                ...)
   [submenu] (Screen resolution       ...) {}
      [include] (~/.fluxbox/menu_resolution)
   [end]
   [submenu] (Keyboard layout          ...) {}
      [exec] (English UK) { fbsetkb gb } </usr/share/icons/locolor/16x16/flags/flag_usa.png>
      [exec] (Portuguese) { fbsetkb pt } </usr/share/icons/locolor/16x16/flags/flag_portugal.png>
      [end]
   [end]
   [exec] (Exit / Logout) { fblogout }

[end]
```

Edit `

### 3.1 Automaticaly login as guest.
When SLAX initiates, I want it to login to guest user instead. Change the below line in `/lib/systemd/system/xorg.service`
```
[Service]
ExecStart=/bin/su - guest --login -c "/usr/bin/startx -- :0 vt7 -ac -nolisten tcp"
```

Edit `/home/guest/.fluxbox/startup`. Replace the below:
```
xmodmap "/root/.Xmodmap"
```
by
```
xmodmap "~/.Xmodmap"
```
And delete the below:
```
# Share common directories with guest user. This is necessary
# because some apps like chromium must be running under guest
for dir in Desktop Documents Downloads Music Pictures Public Templates Videos; do
   if ! mountpoint /root/$dir; then
      mount --bind /home/guest/$dir /root/$dir
   fi
done
```
And delete the below (won't need it):
```
# If we are running inside VirtualBox or vmware,
# switch to bigger screen size right away
OUTPUT=$(xrandr 2>/dev/null | grep -iv disconnected | grep -i 'connected' | head -n 1 | cut -d " " -f 1)
if [ "$(virt-what | egrep -i "virtualbox|vmware")" != "" ]; then
  xrandr --output $OUTPUT --mode 1280x800 -s 1280x800
fi
```



