## Overview
Access control is anything that prevents someone from doing something they aren't supposed to do or seeing something they aren't supposed to see. There are a few forms of access control that you need to know. Each describes a model for assigning access to appropriate personnel based on different characteristics of the individual or group getting access. 
### Types:
DAC - Rights are assigned arbitrarily at the discretion of a rights manager. 
RBAC - Rights are assigned based on role, group, or function. 
MAC - Rights are tiered and assigned to groups.
NAC - Any form of access control taking place at the network level.
## Core
### Discretionary Access Control (DAC):
Discretionary access control assigns rights to individuals based on the discretion of the person managing the group. An example would be a manager deciding whether their employees should have access to an Adobe Professional license or not. There are two main problems with this type of access control.
1.  Managing groups this way can get complicated and confusing because different users will potentially have different reasons for requiring access and access requests/approvals could potentially come from multiple sources.
2.  Because access is only evaluated as necessary at the time of provisioning. This type of access control requires regular audits of the members of the access group to determine if access is appropriate.
Let’s walk through an example of how these two problems manifest:

> User A is in marketing and needs access to Adobe Pro to design marketing materials for the company. User A gets approval from their manager and has the help desk add them to the group. A ticket is used to track the access approval and is closed.
>
> The help desk moves from Kaseya to ZenDesk for ticket management and stores the old tickets in a cold storage database.
>
> *User A* is moved from marketing to sales and no longer needs their Adobe Pro license. Because of the nature of DAC, this will not be caught until an audit or access review is performed.
>
> *User A*’s manager gets a bill for an Adobe Pro license and requests that the help desk provide the access request ticket to sort out who approved the license/when. Now the help desk has to retrieve the ticket from cold storage in order to ultimately determine that the access is inappropriate.

In most scenarios, the manager would request that access be removed from the user without asking to see the approval ticket or the user would be questioned to determine if the access is necessary; however, in the case that an MSP (Managed Service Provider) is used to provide help desk services, this would not be entirely unexpected. In this scenario, inappropriate access cost the company some money and put extra tickets in the help desk queue. If this were a admin access on a compromised account, the consequences could be much worse.
### Role-Based Access Control (RBAC):
Role-based access control uses group membership to determine access based on job role. To go back to the Adobe example, an RBAC solution would check to see if *User A* is in the marketing group and provisions a license dynamically to that user. When *User A* moves to sales, the RBAC controls will automatically remove *User A* from the Adobe group. Properly implemented RBAC is tricky to design; however, once it is in place, it offers a much more efficient way to manage permissions.
### Mandatory Access Control (MAC):
Mandatory access control uses group membership and a tiered data labeling system to determine access. This system is most often associated with government entities and the concept of sensitive compartmentalized information or SCI. With this system, you will assign an access level to each user and structure your permissions around that access level. This system is fairly cumbersome, but if implemented correctly can be the most secure of the three. To use our Adobe example once again:
> Our company assigns Adobe Professional licenses to all users; however, document signing is disabled for users who are not in *access tier two* or higher. Users in *access tier four* can sign using the CEOs signature.
### Network Access Control (NAC):
Network access control is unique in that it assigns access based on device properties as well as account properties. Here are a few examples of what NAC can do:
1.  Automatically put IoT devices on a specific network based on MAC address.
2.  Only allow registered computers to connect to the corporate network based on MAC address and user authentication.
3.  Only allow devices that comply with corporate update and antivirus policy to connect to the network.
4.  Limit what traffic can be sent to and from an endpoint (Firewalls)
## References
[Microsoft - What is Access Control](https://www.microsoft.com/en-us/security/business/security-101/what-is-access-control#:~:text=Access%20control%20is%20an%20essential,control%20policies%20protect%20digital%20spaces.)