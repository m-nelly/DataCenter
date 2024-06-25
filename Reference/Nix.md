---
Author(s): M-Nelly
Source: Internal
Subject: Linux
Topic: NixOS
Description: MOC For NixOS Notes
Comments: https://github.com/m-nelly/NixOS-Notes-Companion
tags:
  - linux
---
## Overview
[[Nix]] is a Linux distribution that allows users to configure their system in a declarative configuration file. [[Nix - Modules]] allow configurations to be split up into several files which makes management of large configurations much easier and assists with portability. 

The addition of [[Nix - Home Manager]] allows for declarative configuration of user-space configuration files, environment variables, and user packages. [[Nix - NixOS]] also allows users to roll back configurations to any point in the system's configuration history, so if something breaks you are only ever one reboot away from a working system. 

The addition of [[Nix - Flakes]] ensures consistency of package versions if the configuration is moved and allows for multiple configurations to be managed simultaneously. 
## Nix From Scratch
- Start with [[Nix - NixOS]]
- Split your config into [[Nix - Modules]]
- Manage user config with [[Nix - Home Manager]]
- Manage multiple configs with [[Nix - Flakes]]
- Review configuration with [[Nix - Final]]

## Links
```dataview
TABLE Description
FROM "Research"
WHERE Topic = "NixOS"
```