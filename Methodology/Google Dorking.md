## Storage & Leaked Data Enumeration
https://www.exploit-db.com/google-hacking-database
Search Pastebin
- `site:pastebin.com intext:"CompanyName"`
Search GitHub
- `site:github.com intext:"CompanyName"`
Search Docker Hub
- `site:hub.docker.com intext:"CompanyName"`
Search for S3 buckets
- `site:amazonaws.com inurl:s3 intext:"CompanyName"`
Search for Azure blobs
- `site:blob.core.windows.net intext:"CompanyName"`
Find email addresses in text
- `grep -E -o "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b"`
## API Discovery
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