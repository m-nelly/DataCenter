SSH forward remote port 8000 to localhost:1234
- `ssh -L 1234:localhost:8000 user@$rhost`
SSH Dynamic SOCKS Proxy
- `ssh -D 9001 user@203.0.113.10`
Add SOCKS proxy
- `socks4 203.0.113.10 9001`
Use SOCKS proxy to run a command: 
- `proxychains4 crackmapexec smb $target --shares`