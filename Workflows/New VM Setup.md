## Windows
- Install Software
	- VSCode
	- Firefox
	- VisualStudio
	- Neo4j
	- Bloodhound
## Kali
- `sudo apt update && sudo apt -y upgrade`
- `sudo apt install -y open-vm-tools-desktop` 
- `sudo shutdown -r now`
- `sudo apt install -y amass ansible-core cewl certipy-ad cloud-enum crackmapexec curl db-util docker.io docker-compose enum4linux evil-winrm fierce gobuster hashcat hashid hydra ike-scan impacket-scripts john john-data masscan nbtscan netdiscover nikto nmap nuclei proxychains4 recon-ng responder smbclient smbmap socat sslyze seclists vim wfuzz whois wordlists xrdp`
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
## end additions ##

```
- Update QTerminal Config
	- Font Size: 12
	- Scrollbar Position: No Scrollbar
	- Hide tab bar with only one tab: False
	- Application Transparency: 0%