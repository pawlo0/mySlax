# Customize Slax
Wrote this just to remember how I customized SLAX Linux. Done this for version 11.3.0.

## 1. Download and copy iso
Grab the iso from https://www.slax.org/ Follow instructions to make it bootable.
Note, if in windows just execute bootinst.bat. If in linux, copy the iso files to USB flash drive as root (or using sudo), otherwise when running bootinst.sh it will fail. Running bootinst.sh will also require root user / sudo.

## 2. Setup network access and updates
This can be as simple as running Slax for the first time as normal and set the network using the GUI. Then log off and that will take you to the command line. Login as root. Passwork can be seen on screen. Then update:
`apt update && apt upgrade``

## 3. Disable automatic login.
When SLAX initiates, I don't want it to login automatically. This is doen by commenting the below line in /etc/systemd/system/xorg.service

```
[Service]
# ExecStart=/bin/su --login -c "/usr/bin/startx -- :0 vt7 -ac -nolisten tcp"
```

Then I'd change the password for the root user:  `passwd`

Before logging out, you can create a new user:
```
useradd -m username
```
And set password for it:
```
passwd username
```

Or if you're happy with the already existent Guest user, just ignore the creation of new user steps and logout: `exit`
And login back again either with your new user or with Guest user. Don't forget to change the Guest password in this case.

To start fluxbox:
`startx`

