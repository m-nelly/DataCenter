---
Author(s): 
Source: Internal
Subject: 
Topic: 
Description: 
Comments: 
tags:
  - security
---
## To-Do
- [ ] 
## References
- [Profiling And Detecting All Things SSL With JA3](https://www.youtube.com/watch?v=oprPu7UIEuk&t=2622s&pp=ygUPamEzIGZpbmdlcnByaW50)
## Notes
Hashes are calculated using the TCP client hello packet. Specifically, semi-unique identifiers within it like the `Extensions Length` and `Cipher Suites`. Looking at TrickBot vs MS Edge, the order of the cipher suites is significant. MS Edge orders from most to least secure whereas TrickBot orders in a semi-random order. The extensions themselves can also be used as part of the fingerprinting process. 