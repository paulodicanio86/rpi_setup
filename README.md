# RaspberryPi Setup
Collection of Raspberry Pi commands and instructions that I use frequently. 


## Headless mode
### SSH and WiFi
With a freshly erased SD card do the following:
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
(you can also add a 3rd, etc.). When the RPi is up and running you can always modify these settings with:
```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```
###Deactivate Audio Message
In headless mode and with a screen connected an audio message will always play. To stop this do:
```
sudo rm /etc/xdg/autostart/piwiz.desktop
```

## General

### Change password
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

###Install emacs & python3
```
sudo apt install emacs
sudo apt install python3-pip
```

## PiHole
Some PiHole commands. More detail to be added
```
sudo curl -sSL https://install.pi-hole.net | bash

pihole enable – Enable PiHole
pihole disable – Disable PiHole permanently
pihole -a -p – Change WebUI password
```