## Windows
- Install Software
	- [VSCode](https://code.visualstudio.com/download)
	- [Vivaldi](https://vivaldi.com/download/)
	- [Firefox](https://www.mozilla.org/en-US/firefox/new/)
	- [VisualStudio](https://visualstudio.microsoft.com/downloads/)
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
- `sudo apt update && sudo apt -y upgrade`
- `sudo apt install -y open-vm-tools-desktop` 
- `sudo shutdown -r now`
- `sudo apt install -y amass ansible-core cewl certipy-ad cloud-enum crackmapexec curl db-util docker.io docker-compose enum4linux evil-winrm feroxbuster fierce golang-go hashcat hashid hydra ike-scan impacket-scripts john john-data masscan nbtscan netdiscover nikto nmap nuclei pip3 proxychains4 python3 recon-ng responder smbclient smbmap socat sslyze seclists vim wfuzz whois wordlists xrdp`
https://www.kali.org/docs/arm/x86-on-arm/
- `sudo apt install binfmt-support qemu-user-static`
- `sudo dpkg --add-architecture amd64`
- `sudo apt update`
- `sudo apt install libc6:amd64 libfuse2:amd64 libfontconfig:amd64 libx11-dev:amd64 libharfbuzz-dev:amd64 libfribidi-dev:amd64`
- `pip3 install git-dumper`
- `mkdir ~/wd 
- Update `.zshrc`
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
alias wd='cd ~/wd'

## end additions ##

```
- Update QTerminal Config
	- Font Size: 12
	- Scrollbar Position: No Scrollbar
	- Hide tab bar with only one tab: False
	- Application Transparency: 0%