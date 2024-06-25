---
Author(s): M-Nelly
Source: Internal
Subject: Programming
Topic: Automation
Description: Summary of Pragmatic Automation Talk
Comments: 
tags:
  - programming
  - automation
---
## To-Do
- [ ] 
## Notes
### Common Failure Points
- If the effort to automate greatly exceeds the payoff, evaluate the cost/benefit over time before you "automate everything"
- Looking at automation from a traditional waterfall perspective is an anti-pattern. You won't have 100% understanding when you start the project and that is ok. 
### Before You Write Any Code
- Figure out the process
- Automate for human parsers (checklists/documentation)
	- "If you can't explain how a person should do it, you have no chance of telling a computer how to do it."
- Self-Organizing checklists can be built by defining stakeholders and collaborators for each step of the process.
	- Which processes run in parallel, depending on one another?
	- Which processes are required before this one can run? 
	- [[Obsidian]] can be leveraged here with visualization of links between documents. 
- Figure out where cycles exist and how to break them
### Getting Started
- Once you have your processes defined, you need to begin the work of translating the human-readable instructions into machine-readable instructions. Do this, then that, but not before this other thing. 
- One you have defined the items that the automation will need to track as completion tasks and event triggers, it is time to build a stateful status tracker for them. One way to implement this is to have each step in the process with a pass/fail and mark whether it is automated or human-dependent. Time tracking becomes a player here in the event that a quick human task to check the system is more beneficial than taking the time to automate. 
- Now you are in a position to make incremental progress in a hybrid-automated workflow. 
## References
### External
- [Pragmatic Automation - Max Luebbe, Google SRE](https://www.usenix.org/conference/srecon19americas/presentation/luebbe)
- [Google SRE Books](https://sre.google/books/)
### Internal
- [[Automation]]
- [[Programming]]
- [[Toyota Production System.pdf|Toyota Production System]]