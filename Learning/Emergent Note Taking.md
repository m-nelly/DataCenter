## To-Do
- [ ] 
## References
https://zettelkasten.de/posts/overview/
## Notes
### Emergent Learning
Briefly describe emergent learning:
> [!ai]+ AI
>
> Emergent learning is a process where learning occurs naturally and spontaneously as a result of an individual's interactions and experiences with their environment, rather than through formal instruction. It often involves self-directed exploration, discovery, and problem-solving, which can lead to new skills, knowledge or behaviors that were not explicitly taught. This concept is often applied in contexts such as early childhood education, adult learning and artificial intelligence.

The goal of emergent learning in the scope of this document is to create a system of mapping concepts across a vault to create natural pathways through the vault that might not be explicitly defined. Think of the vault as a city, notes as landmarks, links as roads, and thoughts as cars. Imagine a system where a thought starts in one note and wants to arrive at another. The thought needs a neural pathway to travel to get there, but that pathway might not yet exist in your brain. With the right set of links to create roads through the vault, you might discover a pathway that causes some learning to occur simply from linking concepts and filling in the gaps. Think of notes that emerge from traveling these paths as gas stations along the route. These atomic pieces that fill in the gaps between larger concepts will become the backbone of your transport system without the need for grand efforts at tying it all together. 

This creates a new problem. If your notes are atomic in that they are small and hyper-focused at a specific gap between nodes, you will eventually end up in a cluttered environment where there is no clear identification for an atom or a node. Sure, you could create a confusing system of folders that contains atoms at each level, but I have a better proposal. Create a staging folder where all of your atoms will go until it makes sense to collect them or integrate them into existing nodes. Obsidian has an incredible feature with the graph view in that you can make decisions based on the associated gravity of your atoms in relation to a node. Let's define some of these pieces before we get carried away with terminology. 
#### Definitions
- **Atom** - A single note that is fixated on a particular topic. These are active learning notes that exist as part of a single note taking session. 
- **Node** - A gravity well that is put in place to attract atoms. These can be existing notes covering larger topics or theoretical notes that might be created later. 

Now let's introduce a new weapon into the arsenal: the back link. A back link is a link in another note to the current note. Obsidian has the ability to identify locations for potential back links automatically, so don't worry about being super on top of linking just yet. The muscle memory will come in time. The important thing to note is that, if you are linking based on concepts to larger nodes, there is the potential to link nodes that don't yet exist. Within the graph view that node will grow as more links are added which creates an environment where nodes that need to be created will "emerge" from the web. 
### Tagging
Now that we have an emergent web of thought established with the atoms/nodes, we need to talk about tags. There are two schools of thought on tags: 
1. Create tags that are useful and subject-focused to create additional intermediary gravity-points for your vault. 
2. Create tags that are generic enough to create mega-structures for your vault. 
Camp 1 would have you create tags like \#ActiveDirectory or \#SMB and camp 2 would have you create tags like \#Security or \#Operations. While it is possible to generate a set of tags entirely on your own, I have a better idea.

Create a system of tags based on Zettelkasten best practices for a notes vault focused on cyber security and topics:
> [!ai]+ AI
>
> - \#CyberSecurity
> - \#InformationSecurity
> - \#NetworkSecurity
> - \#ApplicationSecurity
> - \#EndpointSecurity
> - \#DataPrivacy
> - \#Cryptography
> - \#IdentityandAccessManagement (IAM)
> - \#IntrusionDetectionSystem (IDS)
> - \#Firewalls
> - \#MalwareAnalysis
> - \#PhishingAttacks
> - \#RansomwareAttacks
> - \#EncryptionAlgorithms
> - \#TwoFactorAuthentication (2FA)
> - \#VirtualPrivateNetworks (VPN)
> - \#CloudSecurity
> - \#InternetOfThings (IoT) Security
> - \#BlockchainAndCybersecurity
> - \#CybersecurityPolicies
> - \#EthicalHacking
> - \#CyberThreatIntelligence
> - \#IncidentResponse
> - \#RiskAssessment
> - \#ComplianceAndRegulation
> - \#SocialEngineeringAttacks
> - \#SecureCodingPractices
> - \#DDoSAttacks
> - \#ZeroDayVulnerabilities
> - \#PenetrationTesting
> 
> Please note that these tags are not exhaustive, and can be modified or extended depending upon the specific needs and focus areas within the field of cybersecurity.

Let's pick some tags from this list and extrapolate a bit. \#ApplicationSecurity looks interesting, but let's shorten that to \#AppSec. In a similar fashion we'll pick up \#NetSec and \#CloudSec. Do these tags solve any problems for us? Does this structure give us flexibility? Could these tags create emergence opportunities? 

While these tags could be useful, I think they would introduce more friction into the vault than they're worth and wouldn't give much flexibility to extend the system with new topics. Alright, what about tags like \#IoT, \#Linux, \#IAM, \#Cryptography? This system gives us some obvious points for mega nodes to be formed like \#Linux, but it also gives us plenty of flexibility to create nodes that might not collect as many atoms, but would certainly create useful gravity. Other tags that immediately pop out as useful would be \#Compliance, \#IncidentResponse, and \#OSINT. Some tags will be dissolved into documents and some will become mega structures. The point is not to achieve perfect organization, but to give yourself opportunities to discover additional ideas/paths through your vault. 

So what separates a tag from a node and how will you know when to create one or the other? The simplest answer is to create tags for concepts that you don't believe will get their own note. For everything else, create a link to a non-existing note. For instance, I'm not intending on creating a note to attempt to cover all of IR, OSINT, IAM, IoT, Linux, or Cryptography. That might change as I learn and that's perfectly fine, but for now I am designing my vault with a few base assumptions for the sake of organization. 

Other tags that look interesting as identification points would be \#RAT, \#Ransomware, and \#DDoS. These would allow me to search my vault for specific vulnerability types. The only way to truly know if a tag is useful within the context of your notes is to use them. The most important thing to remember here is that you need to identify the types of things you want to tag early to save yourself the time of going back and correcting all of the missed tags later. 

### Folder Structure
With a pretty good handle on the types of notes we'll be taking and a system for linking those notes together it might seem like we could just throw all of the notes into a single folder and let the metadata within them guide us around. While that is similar to how your brain works, it is still beneficial to folder your data into what I will call planetary structures. I have created a folder structure that looks like this:
```
Admin
	Attachments
	Tasks
	Templates
Archive
	CIS Benchmarks
	Manuals
	KnowledgeBases
Personal
	Events
	Journal
	Writing
Research
	Notes
	Resources
	Writeups
Security
	Cheatsheets
	Methodology
	Theory
Staging
README.md
```
These folders split things up based on type of content. I'll give brief descriptions to clear it up a bit: 
- Admin - Folders that don't need interaction. This stays closed 99% of the time.
- Archive - Files I reference on occasion that mostly don't belong to me.
- Personal - Journals and notes that are tied to creative projects or events I attend
- Research - Unorganized node sized documents
- Security - The main knowledge base that will be shared later
- Staging - Atoms folder (Named Staging for sorting purposes)
The folder structure doesn't matter much so long as you have these components: 
1. A garbage collector for incomplete notes
2. Rough separation between theory and practice
3. Space for your personal life, hobbies, and side projects
Some general guidelines I follow: 
1. Group in threes wherever you can
2. Separate personal/work notes and your knowledge base
3. Keep incomplete notes away from your reference material
Depending on use case these might not apply or might need to be modified. Don't fear making changes to this template and making the vault your own. The links will automatically update as things are moved and the tags will always interact the same. 
### Other Considerations
- Make a formatting guide to help introduce some consistency until you have a system dedicated to memory. 
- Add a metadata section to get even more queryable data in your notes. 
- Use plugins like dataview to create a map of your vault in a README folder. 
- Review other vaults for organization ideas
- Familiarize with the more nuanced concepts within frameworks like Zettelkasten. 
### Vector Learning
> _A vector is an object that has both a magnitude and a direction_.

When taking notes in an attempt to learn new things, we can think of each path as a vector with the following definitions:
- Direction - An orientation from what we know in the direction of what we don't
- Magnitude - An estimation of the body of work required to complete learning
Ideally, you want to work in small vectors from existing nodes until you build up a system of nodes/atoms that can be used to start approximating your destination. For example, if I had a starting point of Windows and wanted to learn Linux, I might start with an atom that lists similarities between the two operating systems, move to a node covering the Windows Subsystem for Linux (WSL), work tangentially on an atom covering Virtual Box VM creation, and then converge on a node covering Ubuntu. Notice that I'm defining nodes and atoms ahead of time. This gives me a system of sprints and marathons to work from. Even if the nodes and atoms change to tags or even folders, I have given myself a set of vector definitions to work with. 
## Conclusion
While this method remains untested by me, it is something that tons of other people have reported success with. Only time will tell, but I can tell already that my approach to note taking is changing and becoming easier to contend with. Each evolution will be documented forward from this point. 