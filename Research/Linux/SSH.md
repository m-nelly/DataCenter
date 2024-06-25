---
Author(s): M-Nelly
Source: Internal
Subject: 
Topic: 
Description: 
Comments: 
tags:
  - linux
---
## To-Do
- [ ] Double tunneling
- [ ] Known hosts
- [ ] Key-Only Authentication
## References
- [SSH Manual](https://man7.org/linux/man-pages/man1/ssh.1.html)
- [SCP Manual](https://man7.org/linux/man-pages/man1/scp.1.html)
- [SSH Port Forwarding](https://phoenixnap.com/kb/ssh-port-forwarding )
## Notes
### Getting Started
In order to set up SSH on most systems, a key will need to be generated. The command to do so is `ssh-keygen`. This will start a series of prompts that will help you generate a key. If stored in the default location, the keys can be found in `~/.ssh/`. 

>[!NOTE]
Note that SSH will use keys found in `~/.ssh` unless a key is specified with the `-i` option. 

The `id_rsa` file stores your private key and should not be shared. The `id_rsa.pub` file stores your public key which can be shared. This is the file you would typically upload in order to set up a trust between your system and a service like GitHub. 
### Remote Access
Connect to a remote host using a hostname:
`ssh user@host.local`
Connect to a remote host using an IP address:
`ssh user@127.0.0.1` 
### Secure Copy
Use SCP to copy a file from a remote host: 
`scp user@host.local:/file/request/path /local/path/`
Use SCP to send a file to a remote host:
`scp ./file.txt user@host.local:/where/to/place/`
Use SCP to send a file from one remote host to another:
`scp user@host1.local:/file/to/send user@host2.local:where/to/place`
### Basic Tunnels
Use SSH to serve a remote host port on a local port:
`ssh -L 4443:203.0.113.10:443 user@203.0.113.10`
- Local traffic sent to port 4443 will be forwarded to 203.0.113.10:443. 

>[!IMPORTANT]
>Note that only TCP traffic will be passed over the tunnel, so tools relying on UDP/ICMP will not work. 

Set up SOCKS proxy via SSH:
`ssh -D 3988 user@203.0.113.10
Run a command through the SOCKS proxy:
- Add the IP/Port to `/etc/proxychains4.conf` 
	- ex. `socks4 203.0.113.10 3988`
- Run the command with `proxychains` in front
	- ex. `proxychains crackmapexec dc1.local --shares`

