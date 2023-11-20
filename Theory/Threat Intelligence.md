## Overview
Threat intelligence is an important part of a successful security program. Intelligence sources should be evaluated for relevance and actively monitored to insure currency of the tactics employed by the security team. 
## Core
### Intelligence Sources: 
OSINT  (Open-Source Intelligence) - Information that is publicly available on the internet.  
-   Public facing website   
-   Social media accounts for businesses and employees  
-   Voter registration records 
-   Prior data breach databases   
-   ISAC (Information Sharing and Analytics Center) - Infrastructure, Government, Healthcare, Finance, Aviation 
CSINT (Closed-Source Intelligence) - Information that is not publicly available. 
-   Vendor threat intel groups/channels 
-   Government threat intel groups/channels 
-   Private corporate data on previous breaches
### Confidence Levels: 
#### MISP: 
- A  (Reliable) -  No doubt of authenticity. Historically trustworthy. 
- B (Usually Reliable) - Minor doubt of authenticity. Mostly historically trustworthy. 
- C (Fairly Reliable) - Some doubt of authenticity. Has provided valid information before. 
- D (Not Usually Reliable) - Significant doubt of authenticity. Has provided valid information before. 
- E (Unreliable) - Completely lacking authenticity. History of invalid information. 
- F (Cannot Be Judged) - No data exists to evaluate authenticity. 
#### Percentage: 
- 100% - Information is known to be valid. 
- 90% - Information is most probably valid, but has not been confirmed. 
- 75% - Information is probably valid but more information is probably needed. 
- 50% - Information may or may not be valid and more information is probably needed. 
- 25% - Information is probably invalid but more information is probably needed. 
- 0% - Information is known to be invalid.  
#### Three-Tier: 
- High - Information is definitely valid and can be acted upon. 
- Medium - Information may be valid but needs verification. 
- Low - Information is probably invalid and should be ignored.
### Intelligence Cycle: 
- Requirements: Decide what, where, and how to collect intelligence. 
- Collection: Collect data in a central repository to be processed later.  
- Analysis: Automated processing of data into useful resources. 
- Dissemination: Publishing your analysis results to the people who need it. 
- Feedback: Provide feedback on the process to improve it.
### Attack Frameworks: 
#### MITRE ATT&CK: 
[https://mitre-attack.github.io/attack-navigator/](https://mitre-attack.github.io/attack-navigator/) 
There are three separate but related attack frameworks: 
-   Enterprise 
-   Mobile  
-   ICS   
Each one offers detailed information about several tactics attackers use and techniques for implementing those tactics.  Across the top are your tactics. These flow from left to right in rough chronological order meaning the further right you are in the matrix the later you are in the attack chain. Below each tactic several techniques are listed. These will be the individual things you can alert on.  
#### The Diamond Model of Intrusion Analysis: 
The diamond model provides a framework for mapping out all of the information you have on an attack. This information can be collected into a threat intelligence report. 
##### Adversary: 
-   Has infrastructure  
-   Uses infrastructure to develop capabilities   
-   Targets the victim   
##### Victim: 
-   Has infrastructure leveraged against them    
-   Has capabilities developed in their systems  
-   Is targeted by the adversary   
##### Capabilities: 
-   Vulnerabilities, exploits, and techniques used by the adversary     
##### Infrastructure:  
-   Physical infrastructure owned by the adversary    
-   Ex. Botnets, Command & Control (C2) Servers, etc. 
![[Pasted image 20231013165935.png]]
#### Lockheed Martin Cyber Kill Chain: 
Seven step process for mapping the events of an attack. 
1.  Recon - Attacker gathers information about the victim to find vulnerabilities. 
2.  Weaponization - Pairing a vulnerability with an exploit and creating a payload 
3.  Delivery - Deliver payload to victim to prepare to exploit the vulnerability.  
4.  Exploitation - Execution of payload from step 2 to exploit the vulnerability. 
5.  Installation - Executed payload opens a shell or downloads second stage malware.   
6.  Command & Control (C2) - Channel is opened for remote access into the machine.    
7.  Actions on Objectives - Attacker completes their objectives for the attack.
### Indicator Management: 
#### IoC Common Formats: 
- STIX (Structured Threat Information Expression) - Common format for sharing threat intelligence. 
- OpenIoC - Mandiant's framework for sending IOC data. XML format. 
- Sigma - Signature format for detecting IOCs in log files. 
- YARA - Pattern matching format for detecting IoCs in files.  
- TAXII (Trusted Automated Exchange of Indicator Information) - Protocol for sharing IoC data. 
- Download STAXX from [Anomali](https://www.anomali.com) to play around with STIX/TAXII
### Threat Classification: 
- Known - Threats that can be identified using signatures or detection techniques that are known. 
- Unknown - Threats that cannot be detected either because it is unknown or has evaded detection.  
- Zero-Day - An attack that has not been seen before and has no known detections or mitigations. 
- APT (Advanced Persistent Threat) - Sophisticated groups of attackers that are hard to detect.
### Threat Actors: 
**Nation-State** - State sponsored groups with unlimited budgets and loads of resources. 
-   APT28/Fancy Bear - Russian GRU Unit   
-   APT38/Lazarus Group - North Korean Group 
-   Equation Group - NSA Group 
-   Sandworm - Russian GRU Unit 
**Organized Crime** - Financially motivated groups with better capabilities than the common criminal. 
-   Carbanak - Organized crime group that targets banks. 
Hacktivist: Morally motivated hackers or hacking groups. 
-   Anonymous - Hacktivist group with no leader or specific target.    
**Insider Treat** - Anyone inside the organization that causes a security incident.
### Commodity Malware: 
Malware that can be easily bought and distributed:  
-   Mimikatz 
-   Powersploit 
-   Ursnif 
-   Metasploit 
-   Cobalt Strike
### Threat Research: 
**Reputation** - Research the reputation of the IP addresses and FQDNs (Fully Qualified Domain Names) of suspected threats. 
**Behavior** - Characterize threats based on their behavior to align with previous attack profiles. 
**IoC (Indicator of Compromise)** - Develop indicators to detect threats earlier. 
#### CVSS (Common Vulnerability Scoring System) 
-   AV (Attack Vector)  
    -   Network ( N ) - Device is "remotely exploitable".  
    -   Adjacent ( A ) - Device is exploitable from the physical or logical network it is on.  
    -   Local ( L ) - Device is exploitable only with physical or shell access to the system.  
    -   Physical ( P ) - Device is only physically exploitable. 
-   Qualitative Severity Rating 
    -   None - [ 0.0 ]  
    -   Low - [ 0.1 - 3.9 ] 
    -   Medium - [ 4.0 - 6.9 ] 
    -   High - [ 7.0 - 8.9 ]  
    -   Critical - [ 9.0 - 10.0 ]
### Threat Modeling Methodologies: 
- Capability - Model based on the demonstrated capabilities of the adversary. 
- Attack Surface - Model based on your total attack surface.  
- Attack Vector - Model based on a specific attack vector. 
- Impact - Model based on impact to the organization. 
- Likelihood - Model based on the likelihood of an event.  
All of these are likely to feed into a comprehensive threat model mapped into a hybrid of attack frameworks.
### Threat Intelligence Sharing: 
- Risk Management - Identifies risk and seeks out ways to mitigate it. 
- Vulnerability Management - Identifies vulnerabilities and prioritizes them based on impact. 
- Incident Response - Manages response to an attack in progress or one that has already happened.  
- Security Engineering - Designs secure systems and ensures vulnerabilities are properly mitigated/patched. 
- Detection and Monitoring - Analyzes logs and network traffic for IoCs.