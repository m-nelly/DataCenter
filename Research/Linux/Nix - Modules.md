---
Author(s): M-Nelly
Source: Internal
Subject: Linux
Topic: NixOS
Description: Overview of creation of NixOS modules.
Comments: 
tags:
  - linux
---
## To-Do
- [ ] Create Module Structure (TTY, DE, User)
- [ ] Modules without options
- [ ] Modules with options
## References
- [Libre Phoenix Modularity](https://www.youtube.com/watch?v=bV3hfalcSKs)
- [Official Module Deep Dive](https://nix.dev/tutorials/module-system/module-system)
## Notes
### First Module
Picking up the configuration from [[Nix - NixOS]], we can now create modules to break our configuration into logical sections across multiple files.  I'll start by making a modules directory in `/etc/nixos/`. In that folder, we will start with `system.nix` to handle locale settings and any settings we don't anticipate being different between systems.

At the top of our module, we need to include an attribute set to give us the functions we need for configuration. Don't worry too much about this yet; you can find more information if needed in the official documentation or in [[Nix Language]]. For the module we are creating, we are only including configuration options, so we'll start with the following template:
```r
# system.nix
{ config, ... }:
{

}
```
We can now start adding our configuration to the body of the module:
```r
# system.nix
{ config, ... }:
{
	# enable experimental features
	nix.settings.experimental-features = [ "nix-command" "flakes" ];
	
	# enable bootloader
	boot.loader.grub = {
		enable = true;
		device = "/dev/sda";
		useOSProber = true;
	};
	
	# enable networking
	networking.networkmanager.enable = true;\
	
	# locale settings
	time.timeZone = "America/Chicago"
	il8n.defaultLocale = "en_US.UTF-8"
>>>
```
We now need to add the module to our `configuration.nix` file's imports.
```r
# configuration.nix
{ config, pkgs, ... }:
{
	imports = [
		./hardware-configuration.nix
		./modules/system.nix
	];
>>>
```
>[!IMPORTANT]
>Don't forget to remove any lines you add to modules from the original file. Duplicate configurations will cause build errors. 
### Second Module
The next thing that needs to be moved is your desktop environment configuration. I would recommend naming the file with the same name as your desktop environment unless you don't anticipate trying new ones out (NixOS makes this really easy). Try creating this module on your own using the modules above as a template. Hint: Your attribute set will need to include `pkgs`. 

The last thing I will do at this stage is clean up the comments and formatting of the existing configuration files. My final `configuration.nix` file looks like this:
```r
# configuration.nix
{ config, pkgs, ... }:
{
	imports = [
		./hardware-configuration.nix
	    ./modules/locale.nix
	    ./modules/system.nix
	    ./modules/desktop.nix
	];

  users.users.mnelly = {
    isNormalUser = true;
    description = "mnelly";
    extraGroups = [ "networkmanager" "wheel" ];
    shell = pkgs.zsh;
    packages = with pkgs; [
		# terminal
	    bat btop eza fzf neovim tmux toybox
	    # programming
	    python3
    ];
  };

  programs.zsh = {
    enable = true;
    autosuggestions.enable = true;
    syntaxHighlighting.enable = true;
  };

  services.getty.autologinUser = "mnelly";
  networking.hostName = "nixos"; 
  services.openssh.enable = true;
  system.stateVersion = "23.11";
}
```

Next steps from here are either to set up user space configurations with [[Nix - Home Manager]] or multi-machine configurations with [[Nix - Flakes]]. Either option is valid since there is little overlap between the configuration changes, but before we move on, I need to cover options. 
### Declaring, Defining, and Reading Options
In order to manage the home username inside `configuration.nix`, we need to set the username as an option in the module. If options are defined in a module, NixOS no longer assumes the rest of the file is part of `config`. As a result, you have to specify `config {};` as part of your module. Here is an example module with imports, options, and configurations. 
```r
# test.nix
{ lib, config, pkgs, ... }
{
	imports = [ ./example/import.nix ];
	
	options.test = {
		hello = lib.mkOption.default = "hello"
	};
	
	config = {
		test.hello = "hello-world"
		environment.systemPackages = with pkgs; [
			${config.test.hello}
		];
	};
}
```
You can see a familiar structure with the attribute set at the top of the file and the body of the module below. The only difference is now we are specifying `options` as part of the module. When `options` are included, the `config` attribute set must also be included. It is no different than normal configuration other than it being wrapped in another set of brackets. In this module we declare the option `test.hello` with a default of "hello", then we define it with a value of "hello-world", and finally we read it with `${config.test.hello}`. 

Option declaration: `options = { option.homeManager.userName }` 
Option definition: `config = { homeManager.userName = "foo" }`  
Option read:  `${config.homeManager.userName}` returns `"foo"`
### Module Planning  
I do think it is a good time to start planning your folder structure. My final result will look something like this:
```json
flake.lock
flake.nix
dotfiles
	zshrc.zsh
	alacritty.toml
	tmux.conf
	init.vim
hosts
	anchor
		configuration.nix
		hardware-configuration.nix
	homelab
		configuration.nix
		hardware-configuration.nix
modules
	desktop.nix
	home-manager.nix
	system.nix
```