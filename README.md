# Customize Slax
Wrote this just to remember how I customized SLAX Linux. Done this for version 11.3.0.

## 1. Download and copy iso
Grab the iso from https://www.slax.org/ Follow instructions to make it bootable.

Note, if in windows just execute bootinst.bat. If in linux, copy the iso files to USB flash drive as root (or using sudo), otherwise when running bootinst.sh it will fail. Running bootinst.sh will also require root user / sudo.

## 2. Setup network access and updates
This can be as simple as running Slax for the first time as usual and set the network using the GUI. Or find how to do it using connman if you want to do it in the terminal.

## 3. Correct changing time in dual boot
Because I use Slax in a windows laptop, when i boot back to Windows it is common to have the wrong time. To fix this issue I'll set slax to use the local time for the hardware clock (RTC). In terminal type: 
```
timedatectl set-local-rtc 1
```

## 4. Disable automatic login.
When SLAX initiates, I don't want it to login automatically. Comment the below line in `/etc/systemd/system/xorg.service`
```
[Service]
# ExecStart=/bin/su --login -c "/usr/bin/startx -- :0 vt7 -ac -nolisten tcp"
```

## 5. Create new user - Optional
In the terminal type the below to create a new user:
```
useradd -m username
passwd username
```

Or if you're happy with the already Slax existent "Guest" user.

## 6. Change root password
Thought about removing the root account altogether, but it's simpler to just change the password.

## 7. Install sudo
```
apt install sudo
usermod -aG sudo guest
```

## 8. Login as new user
I'll using guest user (password in slax is "guest").
```
su guest
```
Don't forget to change guest's password.
```
passwd
````

## 9. Start fluxbox
To start fluxbox:
```
startx
```

