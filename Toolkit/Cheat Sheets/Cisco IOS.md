General: Ctrl+Shift+6 - Break sequence

Modes: User Exec - `>` Privileged Exec - `#` enable Global Config - `(config)\#` configure terminal Interface Config - `(config-if)\#` interface

Show Commands: `#` Privileged Exec
- `show version` - Shows IOS information
- `show flash` - Shows contents of flash memory
- `show ip interface brief` - Shows an overview of ports and interfaces
- `show ip protocols` - Shows routing protocol information
- `show ip route` - Shows routing table
- `show cdp neighbors` - Shows information about Cisco Discovery Protocol neighbors
- `show running-config` - Shows the running configuration file
- `show startup-config` - Shows startup configuration file
Saving/Deleting Config Files: `#` Privileged Exec
- `copy running-config startup-config` - Writes the current configuration to the startup configuration file
- `copy running-config tftp` - Copies the running config to a TFTP server
- `copy tftp: running-config` - Writes a file from a TFTP server to the running configuration file
- `write erase` - erases the startup configuration file
Interfaces: `(config-if)#` Interface Config
- `ip address x.x.x.x y.y.y.y` - Set the interface IP address and subnet mask
- `clock rate 6400` - Manually set interface clock rate
- `no shutdown` - Turns the interface on
SSH: (config)# Global Config & (config-line)#
- `username $user algorithm-type scrypt secret $pass` - Sets a username and secure password
- `login local` - Requires login on the TTY using local authentication table
- `transport input ssh` - Disables telnet on the TTY line
Routing (Static): (config)# Global Config
- `ip route x.x.x.x y.y.y.y z.z.z.z` - Configures a route to network x with subnet mask y via address z
- `ip route x.x.x.x y.y.y.y s0/0/0` - Configures a route to network x with subnet mask y via interface s0/0/0
- `ip route 0.0.0.0 0.0.0.0 z.z.z.z` - Configures a default route via address z
Routing (OSPF): `(config)#` Global Config, `(config-router)#` Routing Config, and `(config-if)#` Interface Config
- `router ospf 10` - Enable OSPF with process ID 10
- `network x.x.x.x y.y.y.y area 0` - Set OSPF to advertise network x with reverse mask y in area 0
- `area 0 authentication` - Enables simple auth for area 0
- `area 0 authentication message-digest` - Enables auth using your message-digest key in area 0
- `area 0 message-digest-key 10 md5 $pass` - Sets a PSK for OSPF hashed with MD5
- `ip ospf authentication-key $pass` - Sets a password on the OSPF interface
DNS: (config)# Global Config
- `ip domain-lookup` - enables DNS
- `name-server x.x.x.x` - Sets DNS server address
- `ip domain-name example.com` - Sets a domain name
- `ip domain-list example2.com` - Sets an additional domain name
- `ip dns server` - Set device to advertise DNS entries
Network Address Translation (NAT): `(config)#` Global Config & `(config-if)#` Interface Config
- `ip nat inside source static x.x.x.x y.y.y.y` - Set static NAT translation from address x to y
- `ip nat source list1 interface fa 1/1 overload` - Set PAT to translate the network in list1 to the address of fa 1/1
- `ip nat inside` - Marks an interface as a network to be translated
- `ip nat outside` - Marks an interface as a network to translate addresses to
Authentication (Local): `(config-line)#` TTY Config
- `password $pass` - Set TTY password
- `login` - Require login on TTY line
Password Encryption: `(config)#` Global Config
- `service password-encryption` - Encrypt passwords with basic encryption
- `security passwords min-length` - Set minimum password length
Authentication (LDAP): `(config)#`
- `ldap-server host x.x.x.x` - Specify the address of the LDAP server
- `ldap-server base-dn test.local` - Specify the distinguished name of the LDAP directory
- `ldap-server admin-dn admin.local` - Specify the admin user distinguished name
- `ldap-server admin-password $pass` - Specify the admin password
- `aaa new-model` - Enable the AAA authentication module
- `aaa authentication login default group ldap local` - Use LDAP to authenticate with the local database as a backup if LDAP fails
- `aaa authentication enable default group ldap enable` - Sets the enable prompt login to use LDAP
Access Control Lists: `(config)#` Global Config
- `access-list 101 deny tcp x.x.x.x y.y.y.y any eq 80` - Creates an access list to block HTTP traffic to network x
- `access-list 101 permit ip any any` - Adds a rule to access list 101 to permit any traffic that does not match the first rule
- `ip access-group 101 in` - Configured in interface config to apply access list 101 inbound