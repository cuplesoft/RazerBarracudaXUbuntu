# Razer BarracudaX 2022 support on Ubuntu 22
Supports Razer Barracuda X 2022 on Ubuntu 22 with microphone as default input after connection

Steps:
1. Check card name and profile name 
```sh
pactl list
```
It should be like Name: bluez_card.<bluetooth_address> in Card section and handsfree_head_unit in Profiles section
2. Create script:
```sh
#!/bin/bash
sleep 2 # wait for the headset to fully connect
sudo -u '#1000' XDG_RUNTIME_DIR=/run/user/1000 \
    pactl set-card-profile <card_name> handsfree_head_unit
logger "Switched BarracudaX 2022 headset to HFP profile"
```
2. Create rules file
```sh
sudo nano /etc/udev/rules.d/52-barracuda-x-headset.rules
```
3. Put this line with proper bluetooth_address and user_name
```sh
ACTION=="add", SUBSYSTEM=="input", ATTR{phys}=="<bluetooth_address>", ATTR{id/vendor}=="0000", ATTR{id/product}=="0000", RUN+="/home/<user_name>/.config/auto-pactl.sh"
```
4. Reload rules without reboot
```sh
udevadm control --reload-rules && udevadm trigger
```
