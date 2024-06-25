---
Author(s): M-Nelly
Source: Internal
Subject: 
Topic: 
Description: 
Comments: 
tags:
  - linux
---
## To-Do
- [ ] Add configuration for [[Nix]]
## References
- https://wiki.archlinux.org/title/Universal_2nd_Factor
- https://www.redhat.com/sysadmin/mfa-linux
- https://developers.yubico.com/yubico-pam/
- https://linux.die.net/man/5/pam.d
- https://www.youtube.com/watch?v=INi-xKpYjbE
## Notes
### Dependencies
You will need `libpam-u2f`/`pam-u2f` and the PAM module for Yubikey: https://developers.yubico.com/yubico-pam/. Check your distribution's repositories for an up to date version of these libraries before you install manually. 
### Adding Your Key
Create a folder at `~/.config/Yubico` and run:
```bash
pamu2fcfg > ~/.config/Yubico/u2f_keys
```
You will likely have to touch your key to get it to finish reading. If you have previously set a PIN, you will be prompted for it at this time. 

Run `cat ~/.config/Yubico/u2f_keys` to confirm that your key data is in the file. 
### PAM Setup
#### Configuration Files
Files at `/etc/pam.d/` can be configured to use `u2f` for authentication. Some examples of files you might want to consider are `/etc/pam.d/sudo` to configure your Yubikey for `sudo` permissions, `/etc/pam.d/{sddm,lightdm,gdm,etc.}` to configure your Yubikey for session manager login, and `/etc/pam.d/sshd` to configure your Yubikey for `ssh` login. 

>[!CAUTION]
>**It is highly recommended to test configuration with `sudo` or [[SSH]] before you implement changes elsewhere.**
#### Authentication Configuration
If you would like to set up your Yubikey to be usable for authentication as an alternative to your password, include `auth sufficient pam_u2f.so` before the  `auth include system-local-login` or `auth include system-auth` line. If you want to add your Yubikey as a second factor, add `auth required pam_u2f.so` after the `auth include system-local-login` or `auth include system-auth` line. 
#### Adding Backup Authenticator
Backup MFA can be added using an OTP generator such as Google or Microsoft Authenticator. I opted to use `libpam-google-authenticator` and the Microsoft Authenticator app for my implementation of local MFA. In order to get the QR code to generate, I had to also install `libqrencode`. Once you have the codes set up, you can add the required configuration to your `/etc/pam.d/` files. 
### Example Configuration:
This is my PAM configuration for `/etc/pam.d/gdm-password`. This configuration requires a password with `auth include system-local-login` and requires either Yubikey or OTP with `auth sufficient pam_u2f.so` and `pam_google_authenticator.so` respectively. 
```
#%PAM-1.0

auth        include         system-local-login
auth        optional        pam_gnome_keyring.so
auth        sufficient      pam_u2f.so      
auth        sufficient      pam_google_authenticator.so

account     include         system-local-login

password    include         system-local-login
password    optional        pam_gnome_keyring.so use_authtok

session     include         system-local-login
session     optional        pam_gnome_keyring.so auto_start
```

> [!NOTE]
>The order of these items is significant because, if the system does not detect a Yubikey, it will bypass that authentication mechanism and fallback to the OTP. If the two sufficient lines were swapped, it would always prompt for the OTP since that module is always available. 