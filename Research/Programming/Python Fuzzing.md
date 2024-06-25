## Introduction
During the process of reverse engineering and exploit development, 
a little bit of Python knowledge goes a long way. Python can be used 
to automate payload generation, input, or network communication.
## Core
Python can be used to generate a list characters in hex. This list 
is useful when attempting to identify bad characters:
[Script](badchars.py)
### Socket
---
Socket can be used as part of your exploit script to send packets into 
an application. If your application will take generic TCP packets, the 
script below will work. This particular setup was created to connect 
to Vulnserver.
```python
import socket
ip = '10.10.100.162' # change this
port = 9999
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) 
# creates an object to take our variables as parameters so we can open the connection. 

# See <https://docs.python.org/3/library/socket.html>

# AF_INET is a constant that takes our port and ip variables.
# SOCK_STREAM is a constant that tells the program what our socket type is. 
# The stream type is for TCP connections. 
# More info can be found here <https://docs.oracle.com/cd/E19455-01/806-1017/sockets-4/index.htmls.connect((ip,port)>)
```
If your application is expecting something like FTP, you will have to 
manually format the packet to look like an FTP packet. The more 
complicated the network protocol is, the harder it will be to script; 
however, you should be able to find pre-built functions to connect to 
common services.

