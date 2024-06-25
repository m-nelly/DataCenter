---
Author(s): M-Nelly
Source: Internal
Subject: Linux
Topic: Linux Network Configuration
Description: Overview of tools used to configure networks on Linux systems.
Comments: Non-Exhaustive & Subject to Change
tags:
  - linux
---
## To-Do
- [ ] Document NMCLI
- [ ] Document [[Nix]] Configuration
## References
- ChatGPT
- StackOverflow
- SuperUser

All of the research for this document was performed during normal troubleshooting/investigation on a lab machine. No one source was particularly useful as I was trying everything and documenting what worked. 
## Notes
### Debian Based Systems
#### The Network Interfaces File
Primary network configuration on [[Debian]] based systems can be done in the `/etc/network/interfaces` file. 

For typical laptop configurations where a wired and wireless adapter are available, we can assume that hot-plugging is safe/preferred and the wired network should be preferred over the wireless one. Example configuration:
```sh
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# Wired and wireless interfaces 
# Allow hot-plugging
allow-hotplug eth0
allow-hotplug wlan0

# Set address, ssid, & psk
iface eth0 inet dhcp
iface wlan0 inet dhcp
	wpa-ssid "Bird Reprogramming Station"
	wpa-psk 1fcca5f0...
iface wlan0 inet dhcp
	wpa-ssid TestNet
	wpa-psk 1facaa77...
```

This configuration will dynamically switch between wired and wireless networks. Additional known wireless networks can be added following the same format as the others. To add a PSK, you can include it as plain text or generate a secure key to include using `wpa_passphrase $ssid $psk`. It is important to note that this configuration/command works regardless of the WPA version in use. 

> I have not tested guest networks requiring captive portal authentication with this method. 
#### Temporary Wireless Connections
We can safely assume that our `/etc/network/interfaces` file will cover any and all wired connections; however, only known wireless networks will be connected to. If we want to connect to a wireless network temporarily (think coffee shop or hotel wifi), we will need to manually connect via the command line or add the network to our known networks list. 

Let's assume that we are connecting one time and do not want to retain the configuration after a reboot. Use the `iwconfig` command:
`iwconfig` 
- List available interfaces                         
`sudo iwconfig wlan0 up`
- Make the wireless interface available for connections 
`sudo iwlist wlan0 scan`
- Dynamically populate a list of available networks 
`sudo iwconfig wlan0 essid "TestNet" key s:TestNetPassphrase`
- Connect to the selected wireless network
- Process the passphrase (key) as a string
`sudo dhclient wlan0`
- Request an IP address using DHCP on the wireless interface
### Troubleshooting
In general, you will want to google around the specific issue you are having; however, for most common connection issues (like conflicting configurations from multiple sources) you can simply restart the networking service using `sudo systemctl restart networking`.

If this doesn't work a reboot might be your best bet; however, if your issues are related to rfkill you will need to resolve the issue manually. This can usually be done using the `rfkill` command. 
- `rfkill list all`
```
0: phy0: Wireless LAN
	Soft blocked: no
	Hard blocked: no
```
- `rfkill unblock all` or `rfkill unblock phy0`
If you do not have `rfkill` available, you can manually unblock devices by changing the values in `/sys/class/rfkill/rfkill#/soft` where the # is the device number that you are attempting to enable. Note that hard blocks are usually hardware level meaning a physical switch needs to be flipped or hardware needs to be replaced. 

I don't currently have a good way to query which device numbers are associated with which devices, but as a general rule they seem to follow the order you would expect with 0 being Ethernet, 1 being wireless, and 2 being Bluetooth in most cases. 
### Bluetooth
Bluetooth is a separate beast entirely, but it is not all that complicated to set up. 
#### Pair New Device
Make sure you have the proper drivers installed
- `sudo apt install pulseaudio-module-bluetooth` 
Reboot your system
- `sudo reboot`
Check the status of the Bluetooth service
- `systemctl status bluetooth`
Make your device discoverable and scan
- `bluetoothctl scan on
Once you see your device, `ctrl+c` to exit and pair
- `bluetoothctl`
- `pair $MAC_ADDRESS`
Trust the paired device
- `trust $MAC_ADDRESS`
- `connect $MAC_ADDRESS`
- `info`
- `exit`
#### Connect To Paired Device
List devices
- `bluetoothctl devices`
- `bluetoothctl connect $MAC_ADDRESS`
Note that trusted devices should connect automatically; however, devices that are paired to multiple computers might need you to manually connect if this was not the last device it connected to. 