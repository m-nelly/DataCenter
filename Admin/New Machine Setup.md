## Windows
- Install Software
	- [VSCode](https://code.visualstudio.com/download)
	- [VisualStudio](https://visualstudio.microsoft.com/downloads/)
	- [Vivaldi](https://vivaldi.com/download/)
	- [Neo4j](https://neo4j.com)
	- [Bloodhound](https://github.com/BloodHoundAD/BloodHound/releases)
	- [Obsidian](https://obsidian.md/download)
		- [Ayu Theme](https://github.com/ayu-theme/ayu-colors)- Reference
		- Style Settings:
```json
{
  "minimal-advanced@@font-ui-small": 14,
  "minimal-advanced@@font-ui-smaller": 12,
  "minimal-advanced@@cursor": "default",
  "minimal-style@@h4-variant": "normal",
  "minimal-style@@h5-variant": "normal",
  "minimal-style@@h6-variant": "normal",
  "minimal-style@@bold-color@@dark": "#73D0FF",
  "minimal-style@@italic-color@@dark": "#FFD173"
}
```
## Kali
Update
- `sudo apt update && sudo apt -y upgrade`
Install VMWare/VirtualBox Tools
- `sudo apt install -y open-vm-tools-desktop` 
Restart
- `sudo shutdown -r now`
Install Software
- `sudo apt install -y amass ansible-core cewl certipy-ad cloud-enum code-oss crackmapexec curl db-util docker.io docker-compose enum4linux evil-winrm feroxbuster fierce golang-go hashcat hashid hydra ike-scan impacket-scripts john john-data libu2f-udev masscan metasploit-framework nbtscan netdiscover nikto nmap nuclei proxychains4 python3 recon-ng responder smbclient smbmap socat sslyze seclists vim wfuzz whois wireshark wordlists`
- [Vivaldi](https://vivaldi.com/download/)
	- `cd ~/Downloads/ && sudo dpkg -i vivaldi`
	- Add bookmarks/sign in
	- Add web panels
	- Disable Password Saving
- `sudo apt remove firefox-esr`
- Install Burpsuite Community/Pro
	- Community: `sudo apt install burpsuite`
	- Pro: Same process as Vivaldi
	- Add Burp CA
		- http://burpsuite
		- chrome://settings/certificates
- Postman
	- Same process as Vivaldi
- Wireshark
	- Same process as Vivaldi
- `pip3 install git-dumper`
Remove XFCE Default Folders
- `rmdir Desktop Documents Music Pictures Public Templates Videos`
Remove Bash Config Files
- `rm .bash*`
Remove Firefox Folder
- `sudo rm -r .mozilla`
Make Useful Folders
- `mkdir ~/Work && mkdir ~/Transfer`
- Add nmap static binary, mimikatz, sharphound, PEASS-NG, etc. to transfer folder
Update `.zshrc`
```shell
## additions ##
# set PATH so it includes go bin if it exists
if [ -d "/usr/local/go/bin" ] ; then
    PATH="$PATH:/usr/local/go/bin"
fi

# set PATH so it includes user's go bin if it exists
if [ -d "$HOME/go/bin" ] ; then
    PATH="$PATH:$HOME/go/bin"
fi

# set PATH so it includes .local/bin if it exists
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$PATH:$HOME/.local/bin"
fi

# docker aliases
alias rustscan='docker run -it --rm --name rustscan rustscan/rustscan:latest'

# vpn aliases
alias thm='sudo openvpn --config /home/mnelly/Documents/thm.ovpn --daemon'
alias htb='sudo openvpn --config /home/mnelly/Documents/htb.ovpn --daemon'

# tool aliases
alias cme='crackmapexec'
alias wfuzz='wfuzz -c'
alias update='sudo apt update && sudo apt -y upgrade'
alias amd64='qemu-amd64-static'

## navigation aliases
alias hosts='sudo vim /etc/hosts'
alias wd='cd ~/Work'

## end additions ##
```
Update QTerminal Config
- Font Size: 12
- Scrollbar Position: No Scrollbar
- Show the menu bar: False
- Hide tab bar with only one tab: False
- Application Transparency: 0%
Update Panel
- Size 36
- Move to bottom of the screen
- Font where possible is set to Sans Regular 10
Update VPN IP Script
`/usr/share/kali-themes/xfce4-panel-genmon-vpnip.sh`
```sh
#!/bin/sh
vpn_interface="$(ip -f inet addr show tun0 | grep inet | sed -En -e 's/.*inet ([0-9.]+).*/\1/p')"
local_interface="$(ip -f inet addr show eth0 | grep inet | sed -En -e 's/.*inet ([0-9.]+).*/\1/p')"

if [ "${vpn_interface}" != "" ]; then
  printf "<txt>${vpn_interface}</txt>"
  printf "<tool>VPN IP</tool>"
else
  printf "<txt>${local_interface}</txt>"
  printf "<tool>LOCAL IP</tool>"
fi
```
Items from left to right:
- Transparent Separator
- Whisker Menu
- Separator
- Launchers
	- Thunar
	- Vivaldi
	- Terminal
	- Burpsuite
	- Postman
	- Wireshark
	- VS Code
- Separator
- Window Buttons (Timestamp, Labels)
- Transparent Separator Expanded
- System Load Monitor (CPU, MEM, NET)
- Free Space Checker (Name: DSK, Display Meter)
- Separator
- Status Tray Plugin (Network)
- Generic Monitor (`/usr/share/kali-themes/xfce4-panel-genmon-vpnip.sh`)
- Action Buttons (Logout...)
- Clock (Style Like Windows 10/11)
- Transparent Separator
Turn off Display Power Management
Add autologin user to `/etc/lightdm/lightdm.conf`
### ARM Specific
https://www.kali.org/docs/arm/x86-on-arm/
- `sudo apt install binfmt-support qemu-user-static`
- `sudo dpkg --add-architecture amd64`
- `sudo apt update`
- `sudo apt install libc6:amd64 libfuse2:amd64 libfontconfig:amd64 libx11-dev:amd64 libharfbuzz-dev:amd64 libfribidi-dev:amd64`