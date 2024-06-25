---
Author(s): M-Nelly
Source: Internal
Subject: Penetration Testing
Topic: Web Application Security
Description: 
Comments: 
tags:
  - security
  - pentesting
---
## To-Do

## Notes
### Setup Process
Regardless of your proxy of choice, there are some things you'll want to get set up before you start working on mapping a web app. You will need a means of managing your local proxy and a few browser extensions to help do things like manipulate cookies/user agent strings at a minimum. I'm not going to walk you through the installation process (there are plenty of guides out there for that). Instead, I'm going to walk through some post install steps and browser configuration. 

My browser recommendation for testing is going to be Google Chrome. Note that a Chromium based browser is not recommended. Use a site like [amiunique.org](https://amiunique.org/fingerprint) to determine if traffic from your browser is likely to get picked up by a blue team. Generally speaking, they will be able to ID the UserAgent, Platform, and Region information fairly quickly. You should also be cognizant of your IP address and/or the ASN that you are connecting from and the fact that Tor traffic is also generally easy to identify. If possible, use residential proxies in the US and common user agent strings at a minimum to avoid detection (if needed). 

I generally try to use Chrome with most of the default settings in place. I want to look as boring as possible. The only things I'd recommend changing is enabling third party cookies and disabling safe browsing. You don't want anything getting in the way of you getting all of the content a web page is serving. 

The list of extensions I'd recommend assessing:
- Edit This Cookie
- Proxy SwitchyOmega
- Wappalyzer
- UserAgent-Switcher For Chrome
- Single File

In Proxy SwitchyOmega, you'll want to set up a proxy that points at 127.0.0.1:8080 browse to that address to grab the CA certificate. Once you have the certificate, go to your security settings in Chrome to import it. You should now be ready to start testing.  

One thing to be cognizant of that may or may not matter depending on the application's WAF is your JA3/4 fingerprint. [[JA3 Fingerprinting]] is used to identify a browser based on the contents of its TCP hello packet which includes the supported cipher suites. This is possible to get around, in fact, Chrome already [randomizes its JA3](https://docs.google.com/document/d/13hbMJDFU8kZ_qtLukNYoWZOEnr0wRKeP6XBmY_TQ0B4/) and FireFox has added support for this behavior. [JA4 Fingerprints](https://github.com/FoxIO-LLC/ja4) don't have this issue and are now seeing more widespread adoption. If you are being blocked from accessing a website, it could be that the user agent string that your client is using or, more annoyingly, the JA3/4 has been blocked. 
### Capabilities
Most of BurpSuite's core utilities can be found elsewhere. There are relatively few places in which the BurpSuite equivalent of a tool is the best way to go about solving a problem. What it is really good for is manual testing and analysis. Most often, you will end up using the proxy to capture traffic and pipe it into repeater for some analysis/manual fuzzing. If you have Burp Professional, you can also use it for automated scanning and vulnerability discovery; however, I have had very limited success with that approach. Even with BurpSuite Pro, the rate at which it scans is not quite as fast as other tools and Burp's memory efficiency becomes an issue with larger data sets. For example, trying to load a large word list like RockYou.txt into intruder for password spraying will usually crash the program. 

Recognizing the limitations of BurpSuite as an all-in-one testing platform, I would recommend you view it as a platform for testing for very specific vulnerabilities in a semi-automated way. That being said, the burp scanner that is always running and identifying vulnerabilities passively is a good place to keep an eye on just in case it does pick up something valuable. The way I handle findings from automated scans is very similar to how meteorologists handle radar indicated tornadoes. I will let the client know that there is probably something there and that they should take action; however, I make it clear that it was identified by an automated process and not verified by a human. 

The only other piece of functionality that I want to mention here is the cookie jar. In Burp > Settings > Sessions, you can change the behavior Burp uses to manage cookies and view everything that is stored currently. 
### Testing Workflow
Testing should follow the [OWASP WSTG](https://owasp.org/www-project-web-security-testing-guide/). For additional information on how to identify and exploit common vulnerabilities, consult [[Offensive Security]], [[Research/Security/Offensive Security/Web Application Attacks]] and [HackTricks](https://book.hacktricks.xyz/pentesting-web/web-vulnerabilities-methodology). The general workflow you should follow while using BurpSuite to assist with testing is as follows:
1. Manually crawl the application. 
2. In the site map, select all requests and highlight/add notes.
3. Scan sites (pro only) 
	- Scan Type: 'Crawl'
	- No scan config
	- Add credentials 
	- Start scan
	- The Burp dashboard and logger will now show scan activity 
	- Mark additional endpoints with highlights/comments
4. Scan sites (external tools)
	- [FeroxBuster](https://github.com/epi052/feroxbuster) offers the `--burp` option to send data to the BurpSuite proxy. 
	- Most other tools will allow you to set `--proxy 127.0.0.1:8080` to get data into Burpsuite
	- If not, there are [other options](https://blog.ropnop.com/proxying-cli-tools/)
5. Highlight potential entry points
	- Filter to parameterized queries
	- Sort by request method to find POST requests
6. Send requests to repeater for [manual fuzzing](https://portswigger.net/burp/documentation/desktop/tools/repeater)
7. Send requests to intruder for [automated fuzzing](https://portswigger.net/burp/documentation/desktop/tools/intruder)
8. Send requests to sequencer for [pattern recognition](https://portswigger.net/burp/documentation/desktop/tools/sequencer)
Note that you can also use the [Organizer](https://portswigger.net/burp/documentation/desktop/tools/organizer) as part of your workflow to manage requests and notes. This will generally be more in depth than you will need to get unless you are on a dedicated web application penetration test or you are hunting a specific bug. 
### Burp Tips
- Target View - Switch view to tabs 
- Filter - Include 4XX responses
- Comments - Useful to mark source of information
- Highlights - Useful to mark things for follow-up or positive vulnerability ID
- Renaming Tabs - Double click to rename tabs
- Context Menu Encoding - Allows encoding on the fly (Available where request/response can be edited)
- Identifying Sitemap Changes - Highlight, comment, sort by time requested
- Managing default wordlists - Intruder > Configure to add predefined payload lists
- Exporting Intruder Data - Select and `Ctrl+C` or use Burp Logger CSV export
- Scanning Defined Insertion Points
	- More controlled scanning to augment manual testing
	- Helpful when working with non-standard payloads (XML)
	- Highlight an insertion point, right click the request, and select "Actively scan defined insertion points" from the context menu
		- Intruder is required for multiple insertion points
- Stripping TLS
	- Quickly test for protection against TLS stripping 
	- Clear cache/history to remove cached 301 redirects
	- Configure proxy to force TLS 
	- Modify responses to "Convert HTTPS links to HTTP"
	- Modify responses to "Remove secure flag from cookies"
	- Disable the use of HTTP/2 by default
	- Modify responses to strip Content Security Policy headers
### Other Tools
- [[NMAP]] - Port Scanner
- [[Nuclei]] - Vulnerability Scanner
- [SQLMap](https://sqlmap.org/) - SQLi Testing Suite
- [Hydra](https://github.com/vanhauser-thc/thc-hydra) - Login Brute Forcing Utility
- [Nikto](https://github.com/sullo/nikto) - Web Application Fingerprinting Utility
## References
### External
- 
### Internal
- 