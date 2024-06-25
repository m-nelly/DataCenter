---
Author(s): 
Source: Internal
Subject: 
Topic: 
Description: 
Comments: 
tags:
---
## To-Do
- [ ] 
## Notes
### OSINT
Open source intelligence is a time consuming endeavor that rarely yields exactly the results you're hoping for. That being said, it is an important part of the enumeration process. A quick look at [OSINT4ALL](https://start.me/p/L1rEYQ/osint4all) will tell you this is not a subject that is easy to cover in one all-inclusive document. That being said, I'm going to split OSINT into two categories:
- [Forensic OSINT](https://github.com/tracelabs/tofm/blob/main/tofm.md) (People, Places, Things, etc.)
- Technical OSINT (IP Addresses, Domain Names, Ports, etc.)
In the context of [[Offensive Security]], we are primarily concerned with the technical side of OSINT; however, there are a few cases (like building phishing campaigns) that might cross into the territory of forensic OSINT. For the purposes of this document, I will be focusing on the technical side of things. 
#### Google
- Use AROUND(#) to do correlation between sets of keywords
	- This is much more accurate and predictable than other filters
#### IP and Domain Registrations
- TLS certificates will occasionally have information on other domains registered to the same certificate. 
- Reverse DNS lookup each domain to see what IP it resolves to and then resolve that IP with a DNS query for all records on that name server.
- [DNS Dumpster](https://dnsdumpster.com/) can help you find name servers, hosts, and subdomains.
- [Whois](https://who.is/) and [Reverse Whois](https://reversewhois.domaintools.com/) can be used to track down domain names. 
- [DNSTwist](https://github.com/elceef/dnstwist) coupled with [CeWL](https://github.com/digininja/CeWL) can be used to find additional domains. 
#### Host Enumeration
Tools like [Shodan.io](https://www.shodan.io/) and [CloudEnum](https://github.com/initstring/cloud_enum) can be used to identify public-facing resources belonging to a company. It is important to do your research on the company before you start looking for hosts to improve your searches and their outcomes. You should know things like, reserved IP ranges, domain names, office locations, etc. Most of the time, you will notice that your target company is multi-cloud whether they know it or not, so don't filter out any cloud providers to speed up your searches. Public S3 buckets belonging to "Azure only" shops are particularly good finds.  
### NMAP
The most common tool to use for enumeration of network ports is [[NMAP]]. Basic operation is pretty simple; however, [[NMAP]] is an incredibly powerful tool with quite a bit of nuance in terms of performance tuning based on use case. Here are couple of example scans I like to use: 
- `nmap scope.txt -sn | grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" > hosts.txt`
- `nmap -iL hosts.txt -p 20-8443 -oA nmap.script-commonext`
Once I have a pretty good idea of what ports are open on the hosts I can reasonably get to, I'll start scanning interesting ports on those hosts with either `-sC` if I don't know what the service is or something like `--script=*smb*` if I know what the service is. I will also kick off a scan of hosts that are a reverse match of `hosts.txt` which can be retrieved with the following commands:
- `nmap -Pn -sn -iL scope.txt | grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" > all-hosts.txt`
- `git diff all-hosts.txt hosts.txt --unified=0 | grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" > noping.txt`
- `nmap --open -iL noping.txt -Pn --top-ports 100 -oA nmap.noping-top100`
This should give you an idea of the types of endpoints that are not responding to pings. Note that it is also generally a good idea to do a no ping for common Windows ports:
- `20,21,22,53,80,135,139,389,443,445,636,3389,5985,5986,9389`
I included a few other common ports in the list that you'll almost always want to search for. It is important to note that some of these services can be abused like `135/139/445` indicating that a device is potentially vulnerable to `SMB/LLMNR/NBNS/mDNS/WPAD` attacks or `5985` indicating that a device might be vulnerable to Evil WinRM.

Following up on your NMAP scans, you should identify hosts that have web servers running and run them through a screenshot utility like [gowitness](https://github.com/sensepost/gowitness) in order to get an overview of the types of applications that are out there. 
## References
### External
- [Atypical OSINT Guide](https://osintteam.blog/the-atypical-osint-guide-2023-276a8d00959)
- [OSINT Framework](https://osintframework.com)
### Internal
- 