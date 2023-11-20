## Overview
During initial reconnaissance, you want to establish a baseline as quickly as possible. Identify business logic, systems/technologies in use, accessible endpoints, and any other information that could be used to inform testing. 
## Tools/Commands
### Scanners Network/Service Enumeration
#### Network
##### SMB
- `smbmap -H 127.0.0.1 -u $user -p $password -R --depth 3 --exclude IPC$`
##### DNS
Request short list of domains on the nameserver:
- `dig +short ns nameserver.dns`
Request all DNS records on the name server (TXT, MX, A, etc.):
- `dig @nameserver domain.local -t ANY`
Request DNS name for IP address:
- `dig 127.0.0.1 @nameserver`
Attempt DNS zone transfer:
- `dig axfr nameserver.dns @domain.to.transfer`
Sometimes, nameservers can be modified without credentialed access:
```r
nsupdate 
> server 127.0.0.1 53 
> update delete example.com A  
> send 
> update add example.com 86400 A 127.0.0.1 
> send
```
#### Cloud
- `cloudenum -k company_name -k company_domain -l company_cloudenum.log`
#### API
https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/web-api-pentesting
Find WordPress users API
- `inurl:"/wp-json/wp/v2/users"`
Find exposed API keys
- `intitle:"index.of" intext:"api.txt"`
Find interesting API directories
- `inurl:"/api/v1" intext:"index of /"`
Sites with XenAPI SQLi vulnerabilities
- `ext:php inurl:"api.php?action="`
Finds potentially exposed API keys
- `intitle:"index of" api_key OR "api key" OR apiKey -pool`
Search GitHub issues and previous versions
- `site:github.com inurl:api`
Use Amass for api enumeration
- `amass enum -active -d target-name.com | grep api`
- `amass enum -active -brute -w /usr/share/wordlists/API_superlist -d domain.local -dir [directory name]`
Use Kiterunner to enumerate API endpoints
- `kr scan HTTP://127.0.0.1 -w ~/api/wordlists/data/kiterunner/routes-large.kite`
#### Web-App Analysis
- Wappalyzer - Identify technologies used. 
- Entry Points - Input, uploads, buttons, URL parameters, etc.  
- Guest Accounts - Authenticated session without credentials?
- User Enumeration - Login errors, IDOR, etc. 
- `sslyze https://example.com` - Checks for SSL/TLS issues. 
#### Vulnerability Scanners
- `nuclei -l hosts.txt -o nuclei.txt`
### Wordlist Generation
Create wordlist based on target site +2 directories deep with a minimum word size of 5 characters
- `cewl http://example.com -w out.txt -d 2 -m 5`
### SMB Testing
Get accessible shares from list of hosts
- `crackmapexec smb scope.txt -u $user -p $password --shares
Map readable files
- `smbmap -H $rhost -u $user -p $password -R --depth 3 --exclude IPC$`
- `crackmapexec smb $rhost -u $user -p $password -M spider_plus`
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
- `hydra 127.0.0.1 ssh -l $user -P /wordlist.txt -s 22 -vV`



