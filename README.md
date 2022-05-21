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

## 2. Deal with root user

### 3.1 Disable automatic login.
When SLAX initiates, I don't want it to login automatically. Comment the below line in `/etc/systemd/system/xorg.service`
```
[Service]
# ExecStart=/bin/su --login -c "/usr/bin/startx -- :0 vt7 -ac -nolisten tcp"
```

### 3.2 Create new user - Optional
In the terminal type the below to create a new user:
```
useradd -m username
passwd username
```

Or if you're happy with the already Slax existent "Guest" user.

### 3.3 Change root password
Thought about removing the root account altogether, but it's simpler to just change the password.

### 3.5 Login as new user
I'll using guest user (password in slax is "guest").
```
su guest
```
Don't forget to change guest's password.
```
passwd
````

## 4. Customize fluxbox

### 4.1 Copy some files
```
cp /root/.Xresources /home/guest
cp -R /root/.fluxbox/ /home/guest
```

## Start fluxbox
To start fluxbox:
```
startx
```

