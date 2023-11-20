## Overview
Web app testing is usually one of the first types of tests you will perform. This is because web applications are designed to be public facing entry points to the business. The main goal of a web app test is to gain access to backend systems via the front end application.  
## Tools/Commands
- Injection Omnibus: dddd",'|&$;:\`({{@<\%=ddd
- `gobuster dir -u https://example.com -w /wordlists/test.txt -lk -o gobuster.txt` 
- `dirsearch -u http://example.com`
- `feroxbuster -u http://example.com -L 10 -t 10 -qn`
- `nikto -h https://example.com`
- `wpscan --url https://example.com -e -o wpscan.txt`
WFUZZ Header Value With Burp Proxy
- `wfuzz -c -p 127.0.0.1:8080 -u http://example.com/ -w /wordlist -H "host:FUZZ.example.com"
SQLMap
- `sqlmap -r form.sqlmap --batch`
- `sqlmap -r form.sqlmap --batch --dbms sqlite --tables`
- `sqlmap -r form.sqlmap --batch --dbms sqlite -t $table --dump`
### Server-Side Testing
#### SQL Injection (SQLi)
- `valid_input'-- -`
- `valid_input'select * from information_schema.tables -- -`
#### File Upload Vulnerabilities
- Burp Upload Scanner
- Test MIME Type Swapping
- Test Nested File Extensions
- Test Alternate File Extensions
- Look For Uploads Directory
#### Content Security Policy (CSP) Testing
#### Server-Side Request Forgery (SSRF) Testing
#### Web Cache Poisoning
#### HTTP Host Header Attacks
#### HTTP Request Smuggling
#### OAuth and JWT Attacks
#### Prototype Pollution
#### Server-Side Template Injection (SSTi)
#### GraphQL Security Testing
### Client-Side Testing
#### Cross-Site Scripting (XSS)
#### Cross-Site Request Forgery (CSRF)
URL in URL
- `http://example.com/index.php?request=http://example2.com/image.png`
URL in Header
- `Host:http://example.com/endpoint/image.png`
#### Cross-Origin Resource Sharing (CORS)
#### Clickjacking
- [Burp Clickbandit](https://portswigger.net/burp/documentation/desktop/testing-workflow/testing-for-clickjacking)
- [Clickjacking Test Site](https://clickjacker.io)
#### DOM Vulnerabilities
#### WebSockets Security Testing
#### Client-Side Template Injection (CSTi)
