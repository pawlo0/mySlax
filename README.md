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

### 2.3 Change startfluxbox
Before leaving root need to change a file. Edit `/bin/startfluxbox` and edit the below line:
```
fluxdir="~/.fluxbox"
```

Then logout. Loging as guest, but don't startx just yet.

## 3 Prepare guest fluxbox
```
cp /root/.Xresources ~
cp /root/.blackbox* ~
cp /root/.xinitrc ~
cp -R /root/.config /home/guest
cp -R /root/.fluxbox /home/guest
```
Slax Fluxbox assumes you're root, hence need to modify some files

Type the below in order to modify .fluxbox/init and .fluxbox/startup:
```
sed -i 's/\root/~/g' /home/guest/.fluxbox/init
sed -i 's/\root/~/g' /home/guest/.fluxbox/startup
```

### 3.1 Automaticaly login as guest.
When SLAX initiates, I want it to login to guest user instead. Change the below line in `/lib/systemd/system/xorg.service`
```
[Service]
ExecStart=/bin/su - guest --login -c "/usr/bin/startx -- :0 vt7 -ac -nolisten tcp"
```



