# RaspberryPi Setup
Collection of Raspberry Pi commands and instructions that I use frequently. 


## Headless mode
### SSH and WiFi
With a fresh SD card do the following:
- Create an empty `ssh` file in the boot directory to enable headless mode with ssh activated
- Create a `userconf.txt` file in the boot directory, containing **username:encrypted-password**
   - You can create an encrypted password in a terminal with `echo 'mypassword' | openssl passwd -stdin` 
- Create a `wpa_supplicant.conf` file in the boot directory with following content:
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=GB

network={
 ssid="SSID"
 psk="PASSWORD"
 priority=1
 id_str="home"
}

network={
 ssid="SSID"
 psk="PASSWORD"
 priority=2
 id_str="work"
}
```
Change the SSID, PASSWORD and country as required. The second wifi is used in case the first one does not work 
(you can also add more). When the RPi is up and running you can always modify these settings here:
```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```
### Deactivate Audio Message
In headless mode and with a screen connected an audio message will always play. To stop this do:
```
sudo rm /etc/xdg/autostart/piwiz.desktop
```

## General

### Change password
Recommended for safety:
```
passwd
```

### Update & Upgrade
```
sudo apt update
sudo apt upgrade
sudo apt clean
sudo reboot
```
Or as a chain:
```
sudo apt update && sudo apt upgrade && sudo apt clean && sudo reboot
```

### Install emacs and python3
```
sudo apt install emacs
sudo apt install python3-pip
```
## Passwordless Rsync into Synology
Only works if the RPi is in the same LAN as the Synology. Some preparation on the Synology is required before the below
works, see [here](https://github.com/paulodicanio86/backup_scripts#passwordless-rsync).

- on Synology: add 'user' to administrator group 
- on Synology: enable ssh
- on RPi: Generate SSH keys `ssh-keygen -t rsa` (Press 'Enter' to all prompts)
- on RPi: `ssh-copy-id [user]@[synology_ip]`
- on Synology: disable ssh on Synology
- on Synology: remove 'user' from administrator group

Now you can rsync from the RPi to the Synology without entering the password. This also works if the RPi is not in the
same LAN but the Synology can be reached via a DNS service or similar. 


## NautiluX Screensaver
Required for using the RPi as a [screensaver](https://github.com/paulodicanio86/photo-manager/tree/master/screensaver). 
Install the dependencies:
```
sudo apt install libexif12 qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools
```
Then download and unpack a stable release (in this case v0.9.13):
```
wget https://github.com/NautiluX/slide/releases/download/v0.9.13/slide_pi_0.9.13.tar.gz
tar xf slide_pi_0.9.13.tar.gz
sudo mv slide_0.9.13/slide /usr/local/bin/
rm slide_pi_0.9.13.tar.gz
```
To run the slide app automatically at boot up edit this file:
```
sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
```
And add:
```
lxpanel --profile LXDE-pi
@pcmanfm --desktop --profile LXDE-pi
@xscreensaver -no-splash

# added for screenserver app
@xset s noblank
@xset s off
@xset -dpms
@slide -t 15 -o 200 -p /home/pi/photo-manager/screensaver/photos/
```
The option `-t 15` determines how frequent the slideshow changes photos in seconds.

### Error fix Kernel panic
When running this on a RPi Zero I experienced some Kernel panic, which caused the RPi to crash. 
[This](https://forums.raspberrypi.com/viewtopic.php?p=1611724#p1611724) seems to have fixed it by adding the following
to the `/boot/config.txt` file:

```
over_voltage=6
force_turbo=1
```

## PiHole
Some PiHole commands. More detail to be added
```
sudo curl -sSL https://install.pi-hole.net | bash

pihole enable – Enable PiHole
pihole disable – Disable PiHole permanently
pihole -a -p – Change WebUI password
```

