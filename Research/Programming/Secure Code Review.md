---
Author(s): M-Nelly
Source: Internal
Subject: Programming
Topic: Secure Code review
Description: An overview of vulnerabilities and how to spot them.
Comments: 
tags:
  - programming
  - security
  - devops
---
## To-Do
- [ ] 
## References

## Notes
### Getting Started
You will need to make sure you have access to any repositories, architecture diagrams, road maps, tickets, etc. associated with the project. I recommend scheduling a 15-30 minute meeting with the project lead or a senior developer to walk through the project. In that meeting you should cover the following: 
- Project Goals (Problems/Solutions)
- IT Infrastructure (Local/Cloud/Hybrid)
- Authentication/Authorization/Availability
- Expected Users/User Input
- Output/Storage
### Initial review
Once you have a decent understanding of what the application is doing, it is probably a good idea to map some of that understanding to the code base. If you aren't comfortable deciphering the functionality yourself, ask a developer to help you identify the specific parts of the code base that handle your points of concern. The goal at this stage is to understand the project at a high level. Ideally, this will be done as the functionality is being built so that there is less work being repeated over time. 

All applications can be broken down into three main components: input, processing, and output. Understanding these components should be sufficient to start, but you will eventually want to add in auxiliary components like authentication and error handling. 
### Code Review
OWASP publishes a [code review guide](https://owasp.org/www-pdf-archive/OWASP_Code_Review_Guide_v2.pdf) that can be referenced during the process of performing a code review. Generally, you want to perform as much of this as you can without engaging the developers first. Once you have a set of questions and points of concern, schedule a meeting with the development team to address them. Some highlights from the OWASP document:

**Determining the Scale of a Secure Source Code Review**: The scale of a secure source code review depends on factors such as the software's business or regulatory needs, the size of the software development organization, and the skills of the personnel involved.
**We Can’t Hack Ourselves Secure**: Penetration testing is a crucial part of security but is not enough on its own. Code should be automatically tested with each new release or build to detect any potential regressions. 
**Sensitive Data**: During the review, sensitive data like account numbers and passwords should be noted. Categorizing data based on sensitivity can help determine the impact of potential data loss.
**User roles and access rights**: Understanding user types and their access rights is crucial for assessing threats. Internal and external users pose different risks, and understanding these can help identify potential security violations or privilege escalation attacks. 
**Application type**: The type of application (browser-based, desktop, web-service, mobile, hybrid) influences the kinds of security threats it faces. Understanding this can help reviewers identify specific security flaws and necessary controls.
**Code**: The programming language used in an application has its own security features and issues that need to be considered during review. Best practices for security and performance should also be taken into account. 
**Design**: The design framework of an application affects its security. Aspects like code layout, storage of configuration parameters, identification of business classes for features/URLs, processing requests, and rendering views all need to be examined.
**Company Standards and Guidelines**: Company standards and guidelines dictate what levels of security are applied to various functions. Reviewers should understand these guidelines to apply them during code reviews effectively.
### Finding Vulnerabilities
#### Dependencies
The number one place you will find bugs and vulnerabilities in an application is in implementations of third-party libraries. Usually, these will be imported at the top of a file. An example in Python would be something like this:
```Python
import os
import math
```
If your company uses mostly the same languages, it would be a good idea to establish a baseline of known dependencies and core modules. As an example, the two modules above are core Python modules, so as long as the Python binary running the application is up to date you can assume that these dependencies are safe. 

There are several tools that will analyze source code for dependencies and keep them up to date. [Dependabot](https://github.com/dependabot) is built into GitHub, but can be used on other platforms as well. [Snyk](https://snyk.io) is another potential solution. 
#### Static Application Security Testing
One thing that can save a ton of time in the review process is leveraging a SAST tool. Snyk (mentioned above) offers one of the better SAST solutions. [Semgrep](https://github.com/semgrep/semgrep) is an open source solution that can be leveraged locally if using other solutions is not an option. 
#### Dynamic Application Security Testing
This portion of the security assessment can be performed in house or by a third party. I would recommend both to get comprehensive coverage. This stage of testing is likely to happen after the application is merged with prod and continue periodically through the life cycle of the application. 
#### OWASP Top 10
[These vulnerabilities](https://owasp.org/www-project-top-ten/) are constantly evolving and shouldn't be viewed as an end-all-be-all; however, these are guaranteed to be what threat actors are going after for the most part in any given year. 
- [Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) 
- [Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/) 
- [Injection](https://owasp.org/Top10/A03_2021-Injection/) 
- [Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/) 
- [Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) 
- [Vulnerable and Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/) 
- [Identification and Authentication Failures](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/) 
- [Software and Data Integrity Failures](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/) 
- [Security Logging and Monitoring Failures](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/) 
- [Server-Side Request Forgery](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/) 
In general, we can break these down into three major categories: **Input Handling**, **IAM**, and **Misconfigurations**. Checking for these vulnerabilities usually involves some kind of DAST (Dynamic Application Security Testing). 

The injection omnibus can be used to check for improper input handling: 
```
dddd",'|&$;:`({{@<\%=ddd
```
Generally speaking, inserting this string into any field available for input will cause an error if there is an injection vulnerability present. Additionally, DAST tools like [BurpSuite](https://portswigger.net/burp) or [OWASP Zap](https://www.zaproxy.org) have scanners and automated tools for testing injections. Generally, these will be better than any short-term human effort at finding these types of issues. Fuzzing can also be useful at this stage. Tools like [FFUF](https://github.com/ffuf/ffuf), [WFUZZ](https://github.com/xmendez/wfuzz), or most DAST tools can be used for input fuzzing. 

IAM (Identity Access Management) vulnerabilities are generally a bit more difficult to track down. The [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/) can provide guidance on assessing IAM flows for potential issues. A cursory look for surface level issues is likely sufficient unless there are significant concerns regarding authentication or one of the SAST tools has identified a potential issue. I would recommend ensuring that vendor documentation was followed when setting up IAM for the application as a first step in tracking potential issues beyond initial testing. Additionally, making sure logging and monitoring is in place to detect anomalies is a good step toward detecting any potential IAM related issues as they are discovered by threat actors. 

Misconfigurations can exist all the way through the application's technology stack and include everything from vulnerable/outdated dependencies to error messages with sensitive information being returned to the user. This also includes things like missing patches or unsecured exposed services on the hosting server. At this stage, you are looking at the application from an external visibility perspective. What can you see and what can you get it to tell you. Use the rest of your testing, the architecture diagrams, etc. as a reference point to inform your testing from here on out. 



