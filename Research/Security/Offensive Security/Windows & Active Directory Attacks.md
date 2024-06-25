---
Author(s): M-Nelly
Source: Internal
Subject: Penetration Testing
Topic: Windows & Active Directory
Description: Overview of Windows and Active Directory Testing
Comments: 
tags:
  - pentesting
  - security
---
## To-Do
- [ ] 
## Notes
The vast majority of organizations use Active Directory to manage identities and permissions within their internal network. While it is becoming more common to see cloud-native EntraID deployments, it is still incredibly common to see hybrid-cloud deployments that leverage on-prem AD. This document will only cover testing of on-prem Active Directory; however, a lot of the same principles apply when testing EntraID
### Network Enumeration
If you have access to the local network, identifying domain controllers should be one of your top priorities. Generally speaking, domain controllers will have port 445 and 53 open for SMB and DNS. These services are required for operation; however, that does not mean that every server with those ports open is guaranteed to be a domain controller. You should also be checking for port 22, 135/139, 389, 3389, and 5985 to identify other Windows/Active Directory related services that can be exploited. 
#### File Shares
NFS and SMB shares that can be mounted should always be searched for sensitive files. Tools like [NetExec](https://github.com/Pennyw0rth/NetExec), [Snaffler](https://github.com/SnaffCon/Snaffler), and [SMBMap](https://github.com/ShawnDEvans/smbmap) can be used for SMB shares. For NFS, you can mount the drive to a Linux file system and explore it with commands like `find` or `grep -r` to find files or strings.
```sh
showmount -e 127.0.0.1
```

Note that the SYSVOL share is readable without authentication and very often contains sensitive files. NETLOGON can also be read although it is less likely to contain sensitive information. When you conduct your searches, look for file extensions that are generally human-readable:
```
.cfg .conf .config .db .doc* .md .one .pdf .ppt* .txt .vhd .vmdk .xls* 
```
### Network Exploitation
#### LDAP & SNMP
In addition to scanning for open ports, you should be keeping an eye out for specific device types. Generally, any IoT device offers some opportunity for abuse. Specifically, devices connected to [[Active Directory]] via LDAP can offer opportunities for [passback attacks](https://notes.benheater.com/books/active-directory/page/passback-attacks-internalexternal). Additionally, devices with SNMP can be [walked](https://linux.die.net/man/1/snmpwalk) for credentials and other sensitive configuration information if not secured properly. 
#### Windows Multicast Traffic
Windows devices can be coerced in a number of ways to send NTLM hashes to a rogue device. Examples of tools to abuse this functionality would be [responder](https://github.com/lgandx/Responder),[mitm6](https://github.com/dirkjanm/mitm6), [ntlmrelayx](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py), etc. Note that some tools prep you for exploitation better than others. NTLMRelayX will give you active sessions to use while MITM6 and Responder will only give you hashes to brute force offline. 
#### Windows Credential Stores
Windows machines have two credential caches that can be accessed remotely using [[NetExec]] or [[Impacket]]. [SAM and LSA secrets](https://www.thehacker.recipes/ad/movement/credentials/dumping/sam-and-lsa-secrets) are one of the first things you should check for if you are able to authenticate to a machine as an administrator. 
#### Kerberos Attacks
###### ASREPRoasting
Whether you have valid credentials or not changes the type of attacks available to you. Generally, you want to be [discovering accounts](https://www.thehacker.recipes/ad/movement/kerberos/pre-auth-bruteforce) and doing [ASREProasting](https://www.thehacker.recipes/ad/movement/kerberos/asreproast) if you don't have credentials you can leverage. 
```sh
netexec ldap $TARGETS -u $USER -p $PASSWORD --asreproast ASREProastables.txt --KdcHost $KeyDistributionCenter
```
You can also use this as an opportunity to run tools like [[Responder]] to collect hashes to brute force or attempt some password spraying if you have a list of valid user accounts. 
>[!Note]
I recommend using [[NetExec]] for everything you can even if there are other tools that need to be leveraged on occasion. Learning one tool inside and out is immensely valuable when dealing with something with as much functionality as NetExec (formerly CrackMapExec). That being said, you at least need to be vaguely familiar with the [[Impacket]] suite as well. 
###### LLMNR/NBT-NS Poisoining
NTLM Relay and LLMNR/NBT-NS poisoning attacks are incredibly common and can be used to collect usernames, hostnames, and NTLM hashes. You can also leverage IPv6 to perform similar MITM attacks. 
- Responder
- MITM6
- NTLMRelayX
Note that responder can also be used to exploit mDNS and WPAD to get credentials.  
###### Kerberoasting
Kerberoasting targets user accounts with non-empty [Service Principal Names](https://learn.microsoft.com/en-us/windows/win32/ad/service-principal-names). This indicates that the service issues tickets that are encrypted with a password that can potentially be recovered using brute force methods. This attack requires valid credentials to perform; however, it does not require any special permissions. 

The easiest method of recovering these hashes is with [[Impacket]] from a Linux machine on the local network:
```PowerShell
GetUserSPNs.py -request -dc-ip $ip example.com/johndoe -outputfile hashes.txt
```
#### Password Spraying
There are several things you need to be aware of when password spraying that will make a huge difference in the level of success you are able to achieve:
- EntraID will automatically start falsely reporting account lockouts after certain thresholds for authentication attempts are reached.
- Microsoft maintains a list of banned passwords that you can reasonably assume contains the entire DeHashed leaked credential database. 
- The EntraID default password policy enforces 8 character passwords with at least three character types (lower, upper, number, symbol). 
	- You can reasonably assume that most companies are enforcing 12-14 character passwords due to compliance requirements. 
- [[NetExec]] can retrieve the password policy for a domain if you have a valid user account. 
If you decide to perform password spraying against a domain, I would recommend the following process for generating a password list: 
- Check the company's about page and anything you can dig up with Google for phrases that might be repeated internally. 
	- Slogans, values, etc. 
- Check local sports teams near office locations. 
- Don't be afraid to include long passwords. 
#### Active Directory Certificate Services 
ADCS is a commonly used digital certificate service used in Active Directory environments to manage identity and authentication. This service has become a prime target for threat actors in the last few years. Misconfigured certificate templates very often expose privilege escalation paths that can lead to domain admin. It is very important in any network to check for ADCS and potentially misconfigured certificate templates. 

To do this, you will need to have valid user credentials and network access. Once you have those two things, you can use `certipy` to check for potentially vulnerable templates. I would highly recommend reading through [this](https://www.blackhillsinfosec.com/abusing-active-directory-certificate-services-part-one/#resources) series of articles from BHIS to familiarize yourself with ADCS abuse. 

I will include the conditions needed for common types of exploitation and the basic command you will need for enumeration. 
###### ESC1
- Client Authentication: True
- Enabled: True
- Enrollee Supplies Subject: True
- Requires Management Approval: False
- Authorized Signatures Required: 0
###### ESC2
- Low Privilege Users Granted Enrollment Rights 
- Signatures Required: 0 
- Enabled: True 
- Requires Management Approval: False 
- Any Purpose: True **OR** Extended Key Usage: False
###### ESC4
- Owner
- WriteOwnerPrincipals
- WriteDaclPrincipals
- WritePropertyPrincipals
###### ESC8
- Web Enrollment: Enabled 
- Request Disposition: Issue

Use [certipy](https://github.com/ly4k/Certipy) or [certify.exe](https://github.com/GhostPack/Certify) to identify vulnerable certificate templates. Either tool should work; however, depending on configurations, network context, etc. one or the other might not produce results. I'd also recommend keeping an eye out for new tools in case either of these become unmaintained.
```
certipy find -u 'user@example.com' -p $password -dc-ip $ip -vulnerable -enabled
```
### Local Enumeration & Exploitation
When you land on a local machine, you want to immediately check the running process list for AV/EDR that might cause issues as you enumerate the machine and attempt to identify opportunities to pivot/escalate. As much as possible, you should be using [living-off-the-land binaries](https://lolbas-project.github.io/#) to avoid detection. You should also be aware of [PowerShell activity that will get you caught](https://netsec.expert/posts/getting-detected-by-edrs/) and commands that are generally associated with attacks that are likely to cause alerts like `Invoke-WebRequest` or `whoami`. You should also be aware of [PowerShell Constrained Language Mode](https://devblogs.microsoft.com/powershell/powershell-constrained-language-mode/). Note that while my notes are focused around penetration testing rather than red teaming, I do want to highlight some things to be aware of in the event that you as the reader want to dig deeper into red team tradecraft.  

Generally speaking, you want to be moving around the network like an end user would. Don't go mapping every file in every available file share or enumerating all of the privileges that a user on the system has unless you are sure you can get some results from that. These actions are better saved for after you have gained some form of persistence and can pivot around to a couple different machines. 

One thing you do want to do is enumerate local files. Look through downloads, documents, program files, etc. to get an idea of the kinds of things the user is doing and start to paint a picture of what access they might have to other sensitive systems. You can also attempt to look at the print queues using `Get-Printer` and `Get-PrintJob`. Any time you are looking through files, you should be keeping an eye out for potentially sensitive information that doesn't further your objectives as well. Things like access to financial or legal documents should be documented for your report.  

Once you have exhausted the host for information at the user level where you can reasonably assume you aren't being detected, you can start attempting to gain local admin. This can be done with a number of techniques that are always changing (given your target machine is up to date). I'd recommend googling around for Windows local privilege escalation and paying attention to when the results are from. There is usually a fairly easy exploit that either isn't patched or is a result of a misconfiguration that can be abused indefinitely. See [[Windows Privilege Escalation]] for more details. 

dumping local credential caches with something like [Mimikatz](https://github.com/ParrotSec/mimikatz) or run SharpHound to collect information about the domain. If you are concerned with stealth and have a SharpHound binary that can get passed the client's EDR, you can try collecting with the `--DCOnly`, `--Throttle`, and `--Jitter` options set for enhanced stealth. Once this is done, you can analyze your findings with [[BloodHound]]. 
>[!NOTE]
You can use the `runas /netonly` command to start a session belonging to a domain user from a Windows machine you control even if it is off domain so long as it is on the network with the domain controller. 

If you are able to get local credential caches and run SharpHound without being detected, it is probably a good idea to start looking for other things on the local host that can be of use later like local users and scheduled tasks. You can also look for vulnerable software on the machine that might be deployed across the network and attempt some exploits if you don't mind burning the host. 
## References
### External
- [ADCS Abuse Part 1-4](https://www.blackhillsinfosec.com/abusing-active-directory-certificate-services-part-one/)
- [ADCS ESC13 Abuse Technique](https://posts.specterops.io/adcs-esc13-abuse-technique-fda4272fbd53)
- [Certified_Pre-Owned.pdf](https://specterops.io/wp-content/uploads/sites/3/2022/06/Certified_Pre-Owned.pdf)
### Internal
- [[Initial Recon]]
- [[Windows Privilege Escalation]]