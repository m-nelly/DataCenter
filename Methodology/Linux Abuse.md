Check for `sudo no password` commands
- `sudo -l`
- [GTFOBins](https://gtfobins.github.io)
- [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)
Find files created by a user
- `find / -user $USER ! -path "*proc*" ! -path "*var*" ! -path "*dev*" 2>/dev/null`
- [ ] `sudo` version
- [ ] `ssh` version
- [ ] Kernel Exploits
- [ ] pspy