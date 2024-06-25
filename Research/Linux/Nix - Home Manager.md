---
Author(s): M-Nelly
Source: Internal
Subject: Linux
Topic: NixOS
Description: Overview of user configuration with home manager.
Comments: 
tags:
  - linux
---
## To-Do
- [ ] 
## References
- [Official Documentation](https://nix-community.github.io/home-manager/index.xhtml#sec-install-nixos-module)
- [Libre Phoenix Custom Options Blog Post](https://librephoenix.com/2023-12-26-nixos-conditional-config-and-custom-options)
## Notes
### Installing Home Manager
Continuing from the state in [[Nix - Modules]], we have a modular configuration that has all but removed everything from the main `configuration.nix` file. What remains is the user configuration and a small handful of system settings that might change depending on use case. I want to get the configuration down to just the `system.stateVersion` and a list of imports. 

Eventually we will manage home manager with [[Nix - Flakes]], but for now we will add it manually with the following command:
```sh
sudo nix-channel --add https://github.com/nix-community/home-manager/archive/release-23.11.tar.gz home-manager && sudo nix-channel --update
```

Now we can create a module called `home-manager.nix` to import the commands we need for user-level configuration management. At a minimum we need to define a state version and import the home manager module:
```r
# home-manager.nix
{ lib, config, pkgs, ... }:
{
	imports = [ <home-manager/nixos> ];
    home.stateVersion = "23.11";
  };
}
```
### Configuring Home Manager
Now we can follow that same logic to declare a username option we can access in our `configuration.nix` file and move the relevant user configurations from the existing file. 
```r
# home-manager.nix
{ lib, config, pkgs, ... }:
let 
	cfg = config.homeManager;
in
{
  imports = [ <home-manager/nixos> ];
  
  options.homeManager = {
    userName = lib.mkOption {
	    type = lib.types = str;
        default = "mnelly";
        description = "username";
    };
  };

  config = {
    users.users.${cfg.userName} = {
      isNormalUser = true;
      description = "hunter2";
      extraGroups = [ "networkmanager" "wheel" ];
      shell = pkgs.zsh;
    };
	
    services.getty.autologinUser = "${cfg.userName}";
    programs.zsh = {
      enable = true;
      autosuggestions.enable = true;
      syntaxHighlighting.enable = true;
    };
	
    home-manager.useGlobalPkgs = true;
    home-manager.users.${cfg.userName} = {pkgs, ... }: {
      home.packages = with pkgs; [
        # terminal
        bat btop eza fzf neovim tmux toybox
        # programming
        python3
        ];
	  home.stateVersion = "23.11";
    };
  };
}
```

>[!NOTE]
System configuration like `programs.zsh` doesn't belong in this file; however, being the only user on this system I have included it here to reduce the amount of configuration in my `configuration.nix` file while I work on other modules leading up to [[Nix - Final]]. 

Before moving on to the core of home manager, I want to stress that this module declares the option `homeManager.userName` and then reads it using `${config.homeManager.userName}`. The let binding shortens `config.homeManager` to `cfg` in `${cfg.userName}`. I define the option in my `configuration.nix` file with `homeManager.userName = mnelly;`
###  Managing Dot Files
Home manager allows us to keep our dot files in the configuration directory and then link them to other places in the file system. Let's start by creating a `dotfiles` directory in `/etc/nixos/` and a simple `.zshrc`. See [[Nix - Final]] for [[zsh]] plugin configurations. 
```bash
# zshrc
PROMPT=$'%F{blue}[%B%n@%m:%2~]\n%F{yellow}>>>%F{reset}%b '

alias vim='nvim'
alias cat='bat'
alias ls='eza'
```

We can now use home manager to source that file by adding to the `home-manager.nix` module:
```r
# home-manager.nix
{ lib, config, pkgs, ... }:
{  
	config = {
		home-manager.users.${cfg.userName} = {pkgs, ... }: {
			home.file = {
				".zshrc".source = ../dotfiles/zshrc;
			};
		};
	};
}
```

This configuration will place the file stored at `./dotfiles/zshrc` into our home directory `~/`. If you needed to place a file in `~/.config/`, you just need to specify the path before `.source` like:
```r
".config/alacritty/alacritty.toml".source = ../dotfiles/alacritty.toml;
```

>[!IMPORTANT]
>Home manager will fail to build the configuration if files specified by `home.file` already exist in your home directory. 

### Other Configurations
Some things like `neovim` might need additional considerations around importing the configuration files. In my configuration, I have chosen to let Nix handle plugin management, but I still want to use my own `init.vim` for everything else. As a result, my `neovim` configuration in home manager looks like this:
```r
# home-manager.nix
{
	config = {
		programs.neovim = {
			enable = true;
			plugins = with pkgs.vimPlugins; [
				gruvbox
				nvim-treesitter
				vim-pencil
			];
			extraConfig = builtins.concatStringsSep "\n" [
				(lib.strings.fileContents ../dotfiles/init.vim)
			];
		};
		home.sessionVariables = {
			EDITOR = "nvim";
		};
	};
}
```

You can also specify options for `git` so you don't have to go through the process of manually setting your username and email address: 
```r
# home-manager.nix
{
	config = {
		programs.git = {
			enable = true;
			userName = "m-nelly";
			userEmail = "mnelly.sec@gmail.com";	
		};
	};
}
```

The next step in getting the configuration set up is to manage everything with [[Nix - Flakes]]. This will allow us to mange multiple machine configurations independently using modules, options, and separate `configuration.nix` files to define those options.  