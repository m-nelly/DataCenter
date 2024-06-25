---
Author(s): M-Nelly
Source: Internal
Subject: Linux
Topic: NixOS
Description: An overview of basic NixOS configuration.
Comments: 
tags:
  - linux
---
## To-Do
- [ ] 
## References
- [Vimjoyer](https://www.youtube.com/watch?v=a67Sv4Mbxmc)
- [LibrePhoenix](https://www.youtube.com/watch?v=ACybVzRvDhs)
- [Pentesting Tools Tracking Issue](https://github.com/NixOS/nixpkgs/issues/81418#issue-573452170)
## Notes
### Installation
NixOS installs similarly to any other Linux distribution with the exception of the optional enable non-free software check. If you are planning to use flakes (recommended), this setting does not matter during installation. When finished with the installer, reboot your system into the finished [[Nix]] environment. 
### Initial Configuration
Configuration is done initially within the `/etc/nixos/configuration.nix` file. You will notice that there is also a `hardware-configuration.nix` file which contains auto-generated configuration options based on a hardware detection script. It is important to note that the hardware configuration is usually not modified or moved from system to system. 
>[!Warning]
>You can build a system with a broken hardware-configuration.nix file. Most of the time this will completely ruin the configuration and you will have to reboot and revert to a previous build. 

Within the `configuration.nix` file, most of the settings are system settings we will not modify; however, there are some sections that are immediately relevant to us. 
- The `users.users.$username` section will control your user's groups and user packages.  
- To install packages system-wide, use the `environment.systemPackages` section. 
- To enable services like [[SSH]], set `services.openssh.enable = true;`. 
	- This should open port 22 automatically, but if not set `networking.firewall.allowedTCPPorts = [22];`
This covers the majority of the things we might want to do with our system configuration. Packages can be quickly searched here: [Nix Package Search](https://search.nixos.org/packages) and package options can be found here: [Nix Package Option Search](https://search.nixos.org/options). Start by installing some packages for your user, 
```r
>>>
users.users.mnelly = {
	isNormalUser = true;
	description = "hunter2";
	extraGroups = [ "networkmanager" "wheel" ];
	shell = pkgs.zsh;
	packages = with pkgs; [
		# terminal
		alacritty
		bat btop eza fzf neovim tmux toybox 
		# browsers
		firefox vivaldi
		# programming
		python3 vscode
	];
};
```
You might also want to change your shell. I use [[zsh]] with the following configuration:
```r
>>>
programs.zsh = {
	enable = true;
};
```
>[!NOTE] 
>The `shell = pkgs.zsh;` line in the example user config sets my user's default shell. If you want to stick to [[bash]] with [[zsh]] as an option, you can omit that line. 
### Additional Configuration
While installing packages, you might discover some needed additions to other parts of your configuration to support what you are wanting. An example is the configuration changes I had to make to `nixpkgs.config` in order to get [[Vivaldi]] and [[Obsidian]] working.
```r
>>>
nixpkgs.config = {
	allowUnfree = true;
	vivaldi = {
		proprietaryCodecs = true;
		enableWideVine = true;
	};
	permittedInsecurePackages = [ "electron-25.9.0" ];
};
```
You might also want to change the owner of the `/etc/nixos` folder and its contents to your current user so you can make changes without needing to use `sudo`. You can do this with `sudo chown -R $username /etc/nixos/` and `sudo chown $username /etc/nixos/*.nix`. 

From here, you can start experimenting with things like [[Nix - Modules]], [[Nix - Home Manager]], and [[Nix - Flakes]]. I would recommend tackling them in that order from here. 