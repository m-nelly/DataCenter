---
Author(s): M-Nelly
Source: Internal
Subject: 
Topic: 
Description: 
Comments: 
tags:
  - security
  - pentesting
---
## To-Do
- [ ] 
## Notes
### Disclaimer
The landscape of offensive security is constantly changing and this document should be considered outdated as of today. Offensive security operations should only be performed with written permission of the asset owner and relevant stakeholders. 
### Enumeration
In any engagement, enumeration is the process of identifying and mapping entry points and/or endpoints which you can interact with. This could be targets for a phishing engagement, open USB ports, open network ports, a login page, etc. The important part with all of these things is that there is some potential for interaction on the part of the user. Other things to look for would include open wireless networks or bluetooth devices that can be connected without authentication. See [[Initial Recon]] and [[CTF Methodology]] for a guide on enumerating your targets.  
### Exploitation
Generally speaking, exploitation will involve either a technical vulnerability or a misconfiguration. In the case of technical vulnerabilities, it is important to track version numbers and fingerprint applications wherever possible. Most of the time, this will be your ticket to finding previously disclosed vulnerabilities and their PoCs. For misconfigurations, exploitation is usually fairly well documented. Resources like [HackTricks](https://book.hacktricks.xyz/) and the [Rapid7 Vulnerability Database](https://www.rapid7.com/db/) can be leveraged to find proof of concept code and exploitation information. I want to make it clear that the common techniques used during this phase of an engagement are constantly changing and learning to do your own research on this front will prove invaluable. That being said, I have collected notes on the methodology that I have learned in the following documents:
- [[Web Application Attacks]]
- [[Windows & Active Directory Attacks]]
	- [[Windows Privilege Escalation]]

## References
### External
- [OWASP WSTG](https://owasp.org/www-project-web-security-testing-guide/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security/all-topics)
- [ADCS Abuse Part 1-4](https://www.blackhillsinfosec.com/abusing-active-directory-certificate-services-part-one/)
### Internal
- [[NMAP]]