# RaspberryPi Setup
Collection of Raspberry Pi commands and instructions that I use frequently. 


## Headless mode
### SSH and WiFi
With a fresh SD card do the following:
- Create an empty `ssh` file in the boot directory to enable headless mode with ssh activated
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

## NautiluX Screensaver
Required for using the RPi as a [screensaver](https://github.com/paulodicanio86/photo-manager/tree/master/screensaver). 
Install the dependencies:
```
sudo apt install libexif12 qt5-default
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

## PiHole
Some PiHole commands. More detail to be added
```
sudo curl -sSL https://install.pi-hole.net | bash

pihole enable – Enable PiHole
pihole disable – Disable PiHole permanently
pihole -a -p – Change WebUI password
```

