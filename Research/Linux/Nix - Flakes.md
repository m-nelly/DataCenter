---
Author(s): M-Nelly
Source: Internal
Subject: Linux
Topic: NixOS
Description: Overview of multi-system configuration with Nix Flakes.
Comments: 
tags:
  - linux
---
## To-Do
- [ ] 
## References
- [Zero-To-Nix Flakes](https://zero-to-nix.com/concepts/flakes)
- [Official Flake Manual](https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake.html#flake-references)
- [Nix Wiki: Flakes](https://nixos.wiki/index.php?title=Flakes)
## Notes
### Enabling Flakes
Before we get started with Flakes, we have to enable them in `/etc/nixos/configuration.nix`. If you have been following this series of notes, you probably enabled flakes during [[Nix - Modules]] in your `system.nix` file. Otherwise they can be enabled by adding the following line anywhere in your NixOS configuration:
```r
# configuration.nix
{ config, pkgs, ... }:
{
  nix.settings.experimental-features = ["nix-command" "flakes"];
>>>
```
Now that we have the configuration to enable flakes, we need to rebuild the system with `sudo nixos-rebuild switch`. This will allow us to start constructing a configuration using flakes. 
### Flakes
The next step is either to build a flake with `nix flake init -t templates#trivial` or copy an existing one in. Let's start building a configuration from the trivial template. 
```r
# flake.nix
{
  description = "A very basic flake";

  outputs = { self, nixpkgs }: {

    packages.x86_64-linux.hello = nixpkgs.legacyPackages.x86_64-linux.hello;

    packages.x86_64-linux.default = self.packages.x86_64-linux.hello;

  };
}
```
This template is missing an inputs section which will be needed to define our package channel and add home manager when it comes time to do so. For now, let's add inputs for `nixpkgs` and `home-manager`, change the description, and remove the two outputs (we'll come back to those). 
```r
# flake.nix
{
	description = "First Flake!";
	
	inputs = {
		#nixpkgs.url = "github:nixos/nixpkgs/nixos-23.11"; #stable 
		nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable"; #unstable
		
		home-manger = {
			url = "github:nix-community/home-manager";
			inputs.nixpgs.follows = "nixpkgs";
		};
	};
	
	outputs = { self, nixpkgs, ... }@inputs: {
		
	};
}
```
>[!Note]
Now that we have the home-manager input declared in our flake, we have to change how we import it in `modules/home-manager.nix`. If we attempt to build with that import still in place, we will get a build failure. 
>
>To fix this, change `<home-manager/nixos>` to `inputs.home-manager.nixosModules.default` and add inputs to the attribute set: `{ lib, config, pkgs, inputs, ... }:`
>
>```r
># home-manager.nix
>{ lib, config, pkgs, inputs, ... }:
>let
>	cfg = config.homeManager;
>in
>{
>	imports = [ inputs.home-manager.nixosModules.default ];
>>>>
>```


We can now move our `configuration.nix` and `hardware-configuration.nix` to the directory `hosts/nixos`. Make sure to update your module paths to go back two directories in the imports section of `configuration.nix`. The final `configuration.nix` imports should look something like:
```r
# configuration.nix
{ config, pkgs, ... }
{
	imports = [
	./hardware-configuration.nix
	../../modules/desktop.nix
	../../modules/locale.nix
	../../modules/system.nix
	];
>>>
```

There are many applications of flakes that you can read about here: [Nix Wiki: Flakes](https://nixos.wiki/index.php?title=Flakes). For my use case, I focus on the `nixosConfigurations` output to configure multiple hosts. Let's configure our example host `nixos` and do a test build. Your final flake should look like this: 
```r
# flake.nix
{
	description = "New Flake!";
	
	inputs = {
		nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
		home-manager = {
		    url = "github:nix-community/home-manager";
		    inputs.nixpkgs.follows = "nixpkgs";
		};
	};
	
	outputs = { self, nixpkgs, ... }@inputs: {
		nixosConfigurations = {
			nixos = nixpkgs.lib.nixosSystem {
				specialArgs = {inherit inputs;};
			    modules = [
					./hosts/nixos/configuration.nix
					inputs.home-manager.nixosModules.default
				];
			};
	    };
	};
}
```

>[!IMPORTANT]
We have made a lot of changes to prep for the transition over to flakes. It is probably wise to build your system with the command `sudo nixos-rebuild dry-activate` to get a preview of changes and errors before you commit to building the system. Once you are building without errors, switch to your new configuration with `sudo nixos-rebuild switch`.
### Build From Anywhere
Flakes allow us to build our configuration from anywhere in the file system, so let's start by creating a configuration directory in the home folder: `mkdir ~/nixos`. Now you can copy the existing `configuration.nix` along with any other modules into that directory with: 
```sh
sudo cp -R /etc/nixos/* ~/nixos
```

Now, you can rebuild your system with `sudo nixos-rebuild switch --flake ~/nixos`. Right now this doesn't seem like a significant improvement. In fact, it is more complicated than it was before. I'll give you three scenarios to consider: 
1. When you build a new system, you can drop your configuration into a location you control with write permissions on all of your configuration files. 
2. When managing your configuration with a git repository, you can clone it to your home directory and work with it there. 
3. If something goes wrong with your configuration, resetting to the base config is as easy as running `sudo nixos-rebuild switch` because the only configuration change needed on the base config is to add support for flakes. 

>[!TIP]
>NixOS will build the flake that corresponds to your hostname if you do not specify a flake to use. Right now, you should only have one which is declared with `nixosConfigurations.nixos`. You can add more with other names in this section to configure multiple machines from your existing modules. If you want to build a configuration for another hostname, you can do so with the command `sudo nixos-rebuild switch --flake ~/nixos#hostname`. 

Now that we have modules, home manager, and flakes in place, it's time to finish configuring the system to your liking. This can be done at your own pace, but I would recommend taking a look at the configuration tour in [[Nix - Final]] while you gather ideas. 