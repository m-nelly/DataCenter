---
Author(s): M-Nelly
Source: Internal
Subject: Penetration Testing
Topic: Client Communications
Description: General guidelines for communicating with clients during a pentest.
Comments: 
tags:
---
## Introduction
This document is intended to serve as a valuable resource for both novice and experienced penetration testers, providing guidance on the essential principles, methodologies, and techniques involved in assessing the security of systems and networks.
### Legal and Ethical Considerations
Before embarking on any penetration test, it is crucial to emphasize the importance of conducting such activities within the boundaries of legality and ethics. Unauthorized or irresponsible testing can lead to legal consequences, damage to systems, and harm to your reputation.

**Authorization and Consent:** Always ensure that you have explicit authorization from the client or organization before conducting any penetration test. This includes defining the scope of the engagement, obtaining written consent, and adhering to any legal requirements and industry regulations.

**Ethical Conduct:** Penetration testers are expected to adhere to a strict code of ethics. This includes respecting privacy, not causing harm to systems, and reporting any discovered vulnerabilities responsibly.
### Document Scope
This document provides a structured framework for conducting penetration tests, it is important to note that the methods outlined here are not exhaustive. This document serves as a foundation and reference point; don't hesitate to adapt and tailor your approach to specific scenarios.
### Engagement Types
Clients will commission different test types based on their needs. The most common assessments include an external and an internal pentest. Larger organizations might commission a full red team engagement which will include several other engagement types as part of an adversary simulation. 
- [[OSINT]] - Passive reconnaissance on a target using publicly available sources.
- [[Social Engineering]] - Coerce information/access from the people at the target org.
- Physical - Attempt to gain access to secure areas of a physical location. 
- External - Attempt to gain entry to the internal network from the internet.
- Internal - Attempt to escalate privileges from within the target network.
- [[Research/Security/Offensive Security/Web Application Attacks]] - Attempt to exploit a web application to gain privileged access. 
- [[Mobile]] - Attempt to exploit a mobile application and/or the backend APIs. 
- [[Red Team]] - Full adversary simulation.
- [[Purple Team]] - Adversary simulation where the Blue Team cooperates with the Red Team to detect/remediate threats in real time. 
## Engagement Timeline
### Gathering Documentation
Make sure you have a statement of work (SoW), scoping information, any network documentation needed, credentials, contact information, and a list of systems that are in-scope but might be sensitive to testing. 
### Kickoff Call
Make sure to cover start and end times, dates, and any information in the initial scoping documentation. Address concerns with [[Reporting]], allowlisting, etc. during the kickoff call to save some time later. 
### Active Testing
During this phase of testing, clients should be informed of start and end times as well as potentially critical vulnerabilities that are found. Any time exploitation is performed or an account is compromised the client should be informed. Theoretically speaking, you should have a technical contact for any actions performed during this stage to let the blue team know alerts are not true incidents as soon as possible. Note that during a true red team engagement, this might not happen. 
### Vulnerability Scanning
Once active testing is finished, vulnerability scans should be performed to catch any vulnerabilities that were not identified during the engagement and provide an additional layer of reporting that the client can use to remediate issues. 
### Reporting
Reporting should begin as soon as possible once an engagement is started and finished within a week of the engagement ending. Good notes and a solid framework for automating report writing should make this easy. 
### Closeout Call
The closeout call will be scheduled as part of the final report delivery. Be prepared to go over the results of your test and answer any questions the client might have about remediation options. 
# Framework
## Pre-Engagement
### Risk Assessment
Before a pentest is performed, it is recommended that clients get a risk assessment. This way they can direct the pentest toward the crown jewels of the organization. This will usually lead to better scoping/rules of engagement and an easier closeout call. 
### Scope and Rules of Engagement
Pentest scope should be fully established and validated (especially on external assessments) prior to testing. Along with your scope you should receive rules of engagement. These will usually consist of test conditions that are explicitly allowed and a list of tests the client does not want performed. 
### Client Communication
Establish start/stop times and a general narrative of what testing will be performed. Additionally, establish conditions for actions that should be reported to the client. Most of the time they will not know what is normal/necessary in this area, so expect to provide some recommendations. In general, admin compromise, malware deployment, and any conditions that might impact performance should be communicated to the client. 
### Credentials and Access Handling
In some engagements the client will provide credentials to test or perform vulnerability scans with. In any case, what actions/communication should follow the collection of credentials should be well established. Most of the time, you will want to let your client know when you have compromised an account so they can monitor for alerts regarding that account and either ignore them or allow the actions you are attempting to perform. During a red team engagement, this will not be the case.   
### Data Handling
As part of the rules of engagement, actions allowed on sensitive data should be well established. If they are not and access is obtained, it is imperative that you get permission before exfiltrating or modifying anything. 