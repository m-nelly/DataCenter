## Initial Recon: 
- Enumerate the open ports on the host and determine services accessible to you. 
- Determine if these services allow interaction pre-authentication. 
- Mark access points which require authentication for future review. 
## Brute Force Discovery:
- Web services (including APIs) can be fuzzed using directory and DNS brute forcing. If the request header has a `Host` parameter, this can be used to enumerate subdomains rather than relying on DNS. 
- Services like SMB, SSH, and FTP can be brute forced on occasion if there is no lockout policy in place. If there is a lockout policy, default credentials such as `root:root`, `root:toor`, and `admin:password` should still be attempted. 
	- Hydra, CrackMapExec, and NMAP can all be used for brute force credential guessing. 
## Error Fishing:
- Attempt to use common control characters to break out of intended execution. Specifically, `;`,`--`,`|`,`>`,`'`, and `"` should be attempted before and after some expected input.
	- Example: `admin'-- -` to attempt SQL injection.
- Switching protocols, request methods, connection options, etc. can bait out error messages with function names, software versions, etc. 
	- Example: PUT instead of POST for a login form. 
- Any time an entry point can be fuzzed using an automated tool (Ex. SQLMap), this approach should be considered; however, note that automatic fuzzers have a tendency to crash applications and/or servers if not restricted properly. 
- Timing attacks can also be used to get error messages. If input is multi-staged it can be useful to delay the second stage or submit multiple of the same stage before moving on. 
- Tokens/Session Cookies can be deleted/modified to invalidate a session and cause errors. This can be useful when searching for token impersonation or client-side attacks.
## Research: 
- Once a service is discovered, the first step is to identify that service and common misconfigurations. Version numbers make vulnerability discovery very easy. 
- If a service cannot be identified via normal channels, google port numbers, connect using `netcat` or a simple python listener, and research received output. 