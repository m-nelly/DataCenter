## Domain Discovery
Enumerate sub-domains
- `gobuster dns -d example.com -w /wordlists/domains.txt -i`
https://subscription.packtpub.com/book/security/9781785284588/2/ch02lvl1sec16/fierce
- `fierce -dns example.com -wordlist /wordlists/domains.txt`
## Directory Discovery
- `gobuster dir -u https://example.com -w /wordlists/test.txt -lk -o gobuster.txt` 
- `dirsearch -u http://example.com`
- `feroxbuster -u http://example.com -L 10 -t 10 -qn`
- `nikto -h https://example.com`
- `wpscan --url https://example.com -e -o wpscan.txt`
## API enumeration
- `amass enum -active -d target-name.com | grep api`
- `amass enum -active -brute -w /usr/share/wordlists/API_superlist -d [target domain] -dir [directory name]`