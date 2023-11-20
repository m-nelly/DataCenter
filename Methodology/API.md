https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/web-api-pentesting
Find WordPress users API
- `inurl:"/wp-json/wp/v2/users"`
Find exposed API keys
- `intitle:"index.of" intext:"api.txt"`
Find interesting API directories
- `inurl:"/api/v1" intext:"index of /"`
Sites with XenAPI SQLi vulnerabilities
- `ext:php inurl:"api.php?action="`
Finds potentially exposed API keys
- `intitle:"index of" api_key OR "api key" OR apiKey -pool`
Search GitHub issues and previous versions
- `site:github.com inurl:api`
Use Amass for API enumeration
- `amass enum -active -d target-name.com | grep api`
- `amass enum -active -brute -w /usr/share/wordlists/API_superlist -d [target domain] -dir [directory name]`
Use Kiterunner to enumerate API endpoints
- `kr scan HTTP://127.0.0.1 -w ~/api/wordlists/data/kiterunner/routes-large.kite`