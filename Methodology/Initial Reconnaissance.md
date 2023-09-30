## Overview
During initial reconnaissance, you want to establish a baseline as quickly as possible. Identify business logic, systems/technologies in use, accessible endpoints, and any other information that could be used to inform testing. 
## Theory
**OSINT (Open Source Intelligence)** - Information gathering using publicly available resources including social media, search engines, public records, and other common content discovery methods. 

**Port Scanning** - Tools like NMAP or RustScan can be used to enumerate the services running on a network. These scanners will usually only test for open TCP ports and can be inaccurate depending on security measures put in place. 

**Subdomain Discovery** - Subdomains are prefixes added onto the base domain such as **subdomain**.example.com. Subdomains can be enumerated using SSL certificates, websites such as DNS Dumpster, or tools such as GoBuster. 

**Directory Brute Forcing** - While noisier than the other methods, directory brute forcing on web applications can reveal sensitive data as well as potentially vulnerable endpoints such as legacy login portals. 
## Tools/Commands
### OSINT (Open Source Intelligence)
#### Google
https://www.exploit-db.com/google-hacking-database
Search Pastebin
- `site:pastebin.com intext:"CompanyName"`
Search GitHub
- `site:github.com intext:"CompanyName"`
Search Docker Hub
- `site:hub.docker.com intext:"CompanyName"`
Search for S3 buckets
- `site:amazonaws.com inurl:s3 intext:"CompanyName"`
Search for Azure blobs
- `site:blob.core.windows.net intext:"CompanyName"`
Find email addresses in text
- `grep -E -o "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b"`
#### LinkedIn
Create list of usernames
- `linkedin2username your_linkedin@email.com corp -n @corp.com -o ./li2u/corp/`
#### Websites
- Whois: https://whois.domaintools.com/
- Reverse Whois: https://reversewhois.domaintools.com/
- ICANN IP Lookup: https://lookup.icann.org/en/lookup- 
- Internet Connected Host Search: https://www.shodan.io
- DNS Archives: https://dnsdumpster.com
- Breach DB Search: https://www.dehashed.com
#### Brute-Force Domain Discovery
Enumerate sub-domains
- `gobuster dns -d example.com -w /wordlists/domains.txt -i`
https://subscription.packtpub.com/book/security/9781785284588/2/ch02lvl1sec16/fierce
- `fierce -dns example.com -wordlist /wordlists/domains.txt`
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
- 
#### WebApp
- `gobuster dir -u https://example.com -w /wordlists/test.txt -lk -o gobuster.txt` 
- `nikto -h https://example.com`
- `wpscan --url https://example.com -e -o wpscan.txt`
- `dirsearch -u http://example.com`
- `feroxbuster -u http://example.com -L 10 -t 10 -qn`
SQLMap
- `sqlmap -r form.sqlmap --batch`
- `sqlmap -r form.sqlmap --batch --dbms sqlite --tables`
- `sqlmap -r form.sqlmap --batch --dbms sqlite -t $table --dump`
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
- `amass enum -active -brute -w /usr/share/wordlists/API_superlist -d [target domain] -dir [directory name]`
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
- `cewl http://example.com -w out.txt -d 2 -m 5`\

