## Resources
- [ ] [Exploit-DB](https://www.exploit-db.com)
- [ ] [CyberChef](https://gchq.github.io/CyberChef/)
- [ ] [RevShells](https://www.revshells.com)
- [ ] [HackTricks](https://book.hacktricks.xyz/welcome/readme)
- [ ] [[API]]
- [ ] [[WebApp]]
## Enumeration
### Scanning 
- [ ] [[DataCenter/Toolkit/Cheat Sheets/NMAP]] SecLists Common HTTP Ports
- [ ] GoWitness
- [ ] Directory Brute Force (FeroxBuster, GoBuster, etc.)
- [ ] Fuzz Host Headers
- [ ] Scan Known Content Management Systems (WPScan, JoomScan, Droopscan)
- [ ] Check TLS (SSLyze)
### Analysis
- [ ] Check Headers For Sensitive Information
- [ ] Identify Technologies (Wappalyzer, Nikto)
- [ ] Unlinked Content (sitemap.xml, robots.txt, certificates)
- [ ] Known Endpoints (wp-admin, phpmyadmin, admin)
- [ ] Happy Path (Intended Use)
- [ ] Identify Entry Points (Forms, Search Bars, Post Editors)
- [ ] Alt Content (Responsive Design, User Agent Switching, Disable JS, IE Support)
- [ ] Analyze Business Logic
- [ ] Check CORS Policy
- [ ] Identify Configuration Options
- [ ] Check Dependency Versions For Vulnerabilities
- [ ] Observe Sensitive Data Handling Mechanisms
- [ ] Observe Authentication & Session Management Logic
## Exploitation
### Authentication
- [ ] Default Credentials
- [ ] Review Error Logging For Overly Verbose Errors
- [ ] Test For Click Jacking Using Page Imbed
- [ ] Password Policy
- [ ] Password Recovery
	- [ ] Reusable Reset Link
	- [ ] Unencrypted Reset Password
	- [ ] Default/Common Reset Password
- [ ] Username Enumeration
- [ ] Brute Force Prevention
- [ ] Reauthentication For Sensitive Functions
- [ ] Reregister Existing User (Spaces, Non-Printable Characters)
### Session Management
- [ ] Predictable Session Tokens
- [ ] Temporary Credential Valid After Logout
- [ ] Session Expiration
- [ ] Cookie Flags (Secure, HttpOnly, SameSite)
### Authorization
- [ ] User Pivoting
	- [ ] IDOR
	- [ ] Session Hijacking
- [ ] Privilege Escalation
	- [ ] 403 Bypass
	- [ ] Header Manipulation
	- [ ] Token/Cookie Manipulation
### Injection Vulnerabilities
#### Client Side
- [ ] XSS
	- [ ] Reflected
	- [ ] Stored
	- [ ] DOM-Bases
- [ ] SSTI
- [ ] Open Redirect
#### Server Side
- [ ] SQLi
	- [ ] Classic
	- [ ] Union
	- [ ] Blind
- [ ] OSCI (Command Injection)
- [ ] Insecure Deserialization
- [ ] XML Injection
### File System
- [ ] Local/Remote File Inclusion (LFI/RFI)
- [ ] Directory Traversal
- [ ] File Upload
	- [ ] MIME Type Swapping
	- [ ] File Extension Obfuscation
	- [ ] File Name Trimming
### Denial of Service
- [ ] Endpoints With Long Response Times
- [ ] Inputs Increase Response Time
- [ ] Inputs Written To File System
- [ ] File Upload Limits/Size Requirements
- [ ] Lockout Mechanisms 
### Miscellaneous Issues
- [ ] Test Content Type Submissions
- [ ] CSRF
- [ ] JWT Abuse