---
Author(s): M-Nelly
Source: Internal
Subject: Linux
Topic: NixOS
Description: Configuration tour of my NixOS configurations.
Comments: Configuration will change over time. This is a snapshot of current state.
tags:
  - linux
---
## To-Do
- [ ] 
## References
- [Vimjoyer](https://www.youtube.com/watch?v=a67Sv4Mbxmc)
- [LibrePhoenix](https://www.youtube.com/watch?v=ACybVzRvDhs)
- [Pentesting Tools Tracking Issue](https://github.com/NixOS/nixpkgs/issues/81418#issue-573452170)
- [Libre Phoenix Modularity](https://www.youtube.com/watch?v=bV3hfalcSKs)
- [Official Module Deep Dive](https://nix.dev/tutorials/module-system/module-system)
- [Official Documentation](https://nix-community.github.io/home-manager/index.xhtml#sec-install-nixos-module)
- [Libre Phoenix Custom Options Blog Post](https://librephoenix.com/2023-12-26-nixos-conditional-config-and-custom-options)
- [Zero-To-Nix Flakes](https://zero-to-nix.com/concepts/flakes)
- [Official Flake Manual](https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake.html#flake-references)
- [Nix Wiki: Flakes](https://nixos.wiki/index.php?title=Flakes)
- Reddit, user forums, Discord, etc.
## Notes
### Folder Structure 
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
### Modules
#### Home-Manager 
- Creates User Account
- Sources `.zshrc`
- Configures `git` 
- Configures `nvim`
- Installs command line tools
```r
# modules/home-manager.nix
{ lib, config, pkgs, inputs, ... }:
let
  cfg = config.homeManager;
in
{
  imports = [ inputs.home-manager.nixosModules.default ];

  options.homeManager = {
    userName = lib.mkOption {
      type = lib.types.str;
      default = "mnelly";
      description = "username";
    };

    gitUser = lib.mkOption {
      type = lib.types.str;
      default = "m-nelly";
      description = "git username";
    };

    gitEmail = lib.mkOption {
      type = lib.types.str;
      default = "mnelly.sec@gmail.com";
      description = "git email";
    };
  };
 config = {
    users.users.${cfg.userName} = {
      isNormalUser = true;
      description = "hunter2";
      extraGroups = [ "networkmanager" "wheel" ];
      shell = pkgs.zsh;
    };

    programs.zsh.enable = true;

    home-manager = {
      useGlobalPkgs = true;
      users.${cfg.userName} = {
        home.file = {
          ".zshrc".source = ../dotfiles/zshrc;
        };

        programs.zsh.enable = true;

        programs.git = {
          enable = true;
          userName = "${cfg.gitUser}";
          userEmail = "${cfg.gitEmail}";
        };

        programs.neovim = {
          enable = true;
          plugins = with pkgs.vimPlugins; [ gruvbox nvim-treesitter ];
          extraConfig = builtins.concatStringsSep "\n" [ (lib.strings.fileContents ../dotfiles/init.vim) ];
        };

        home.packages = with pkgs; [
          bat btop eza file fzf neofetch ripgrep toybox tmux
          zsh-autosuggestions
          zsh-fzf-history-search
          zsh-fzf-tab
          zsh-syntax-highlighting
        ];

        home.sessionVariables = { EDITOR = "nvim"; };
        home.stateVersion = "23.11";
      };
    };
  };
}
```
#### System
- Configures locale settings
- Configures grub
- Enables networking
```r
# modules/system.nix
{ config, pkgs, ... }:
{
  nix.settings.experimental-features = ["nix-command" "flakes"];
  nixpkgs.config.allowUnfree = true;
  boot.loader.grub = {
    enable = true;
    device = "/dev/sda";
    useOSProber = true;
  };
  
  networking.networkmanager.enable = true;

  time.timeZone = "America/Chicago";
  i18n.defaultLocale = "en_US.UTF-8";
  i18n.extraLocaleSettings = {
    LC_ADDRESS = "en_US.UTF-8";
    LC_IDENTIFICATION = "en_US.UTF-8";
    LC_MEASUREMENT = "en_US.UTF-8";
    LC_MONETARY = "en_US.UTF-8";
    LC_NAME = "en_US.UTF-8";
    LC_NUMERIC = "en_US.UTF-8";
    LC_PAPER = "en_US.UTF-8";
    LC_TELEPHONE = "en_US.UTF-8";
    LC_TIME = "en_US.UTF-8";
  };
  services.xserver.xkb = {
    layout = "us";
    variant = "";
  };
}
```
#### Desktop
- Installs GDM, GNOME, and GNOME-Tweaks
- Installs web browser
- Installs terminal emulator
```r
# modules/desktop.nix
{ lib, config, pkgs, inputs, ... }:
{
  config = {
    services.xserver.enable = true;

    services.xserver.displayManager.gdm.enable = true;
    services.xserver.desktopManager.gnome = {
      enable = true;
      extraGSettingsOverridePackages = [ pkgs.gnome.mutter ];
      extraGSettingsOverrides = ''
        [org.gnome.mutter]
        experimental-features=['scale-monitor-framebuffer']
      '';
    };

    environment.systemPackages = with pkgs; [
      alacritty
      gnome.gnome-tweaks
      vivaldi
    ];
  };
}
```
### Configuration
- Imports modules
- Defines host-specific options
- Defines hostname and username
```r
# hosts/homelab/configuration.nix
{ config, pkgs, ... }:
{
  imports = [
      ./hardware-configuration.nix
      ../../modules/system.nix
      ../../modules/home-manager.nix
      ../../modules/desktop.nix
    ];

  homeManager.userName = "mnelly";
  networking.hostName = "homelab";
  services.openssh.enable = true;
  system.stateVersion = "23.11";
}
```
### Dot Files
#### Alacritty
```toml
live_config_reload = true

[window]
padding = { x = 10, y=10 }

decorations = "None"

#opacity = 0.95
blur = true

[cursor]
blink_interval = 0

[colors.primary]
background = '#282828'
foreground = '#ebdbb2'

# Normal colors
[colors.normal]
black   = '#282828'
red     = '#cc241d'
green   = '#98971a'
yellow  = '#d79921'
blue    = '#458588'
magenta = '#b16286'
cyan    = '#689d6a'
white   = '#a89984'

# Bright colors
[colors.bright]
black   = '#928374'
red     = '#fb4934'
green   = '#b8bb26'
yellow  = '#fabd2f'
blue    = '#83a598'
magenta = '#d3869b'
cyan    = '#8ec07c'
white   = '#ebdbb2'
```
#### Vim
```yml
"basic options"
set nocompatible
set number
set showmatch
set showcmd
set ignorecase
set mouse=a
set hlsearch
set incsearch
set tabstop=4
set shiftwidth=4
set autoindent
syntax on
set ttyfast
set clipboard=unnamedplus
set wrap linebreak nolist
set fillchars=eob:\
set noshowmode

"theme changes"
highlight LineNr ctermfg=DarkGrey
```
#### Tmux
```ini
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# mouse options
set -g mouse on

# clipboard
#set -as terminal-features ',rxvt-unicode-256color:clipboard'
set -g default-shell /run/current-system/sw/bin/zsh
set -g default-terminal "alacritty"
set -g set-clipboard on

# disable auto-rename
#set-option -g allow-rename off

# visual changes
set -g visual-activity off
set -g visual-bell off
set -g visual-silence off
setw -g monitor-activity off
set -g bell-action none

## modes
set -g clock-mode-colour color5
set -g mode-style 'fg=yellow bg=black bold'

## windows
set -g base-index 1
set -g renumber-windows on
bind c new-window -c "#{pane_current_path}"
bind -n C-Tab last-window

## panes
set -g pane-border-style 'fg=black'
set -g pane-active-border-style 'fg=white'
set -g pane-base-index 1

## statusbar
#{?client_prefix,#[bg=color2],}
set -g status-position bottom
set -g status-justify left
set -g status-style 'bg=#1d2021 fg=white'
set -g status-left ''
set -g status-right '#{?client_prefix,#[reverse]<prefix>#[noreverse],}%a %m/%d/%Y %I:%M %p'
```
#### ZSH
```sh
# zshrc
source ~/.nix-profile/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source ~/.nix-profile/share/fzf-tab/fzf-tab.zsh
source ~/.nix-profile/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source ~/.nix-profile/share/zsh-fzf-history-search/zsh-fzf-history-search.zsh
# Prompt Version Control Indicator
setopt prompt_subst
function git_branch(){
    remote=$(git remote -v 2> /dev/null | head -n1 | awk -F'[.@]' '{print $2}')
    branch=$(git branch 2> /dev/null | cut -d ' ' -f 2)
    if [[ $remote == "github" ]]; then
        echo "<U+F09B> $branch"
    elif [[ $remote == "gitlab" ]] then
        echo "<U+E65C> $branch"
    elif [[ $remote != "" ]] then
        echo "<U+F02A2> $branch"
    else
        :
    fi
}

# Prompt
PROMPT=$'%F{cyan}%B┌[%F{blue}%D{%I:%M%p}%F{cyan}]-[%F{magenta}%n@%m:%F{blue}%(6~.%-1~/…/%4~.%5~)%f%F{cyan}]\n└─%F{blue}>>>%F{reset}%b '
RPROMPT=$'%B%F{#665c54}$(git_branch)%F{reset}%b'

# PATH
if [ -d "/usr/local/go/bin" ] ; then
    PATH="$PATH:/usr/local/go/bin"
fi

if [ -d "$HOME/go/bin" ] ; then
    PATH="$PATH:$HOME/go/bin"
fi

if [ -d "$HOME/.bin" ] ; then
    PATH="$PATH:$HOME/.bin"
fi

if [ -d "$HOME/.local/bin" ] ; then
    PATH="$PATH:$HOME/.local/bin"
fi

# Key Binding
bindkey '^[[1;5C' forward-word                    # ctrl + ->
bindkey '^[[1;5D' backward-word                   # ctrl + <-

# Auto Run
neofetch --disable gpu packages resolution kernel uptime --speed_shorthand on --cpu_cores off --distro_shorthand on --disk_percent on --colors 6 6 7 7 6 6 --bold on --ascii_colors 8 6

# Aliases
alias grep='grep --color=auto'
alias diff='diff --color=auto'
alias ip='ip --color=auto'

alias notes="cd ~/notes"

alias vim="nvim"
alias config="cd ~/nixos/"
alias update="sudo nixos-rebuild switch --flake ~/nixos/"
alias upgrade="cd ~/nixos && nix flake update && cd -"

alias ls='eza --group-directories-first'
alias lt="ls -T"
alias cat="bat -p"
alias vgrep="grep -B1 -A1 --color=auto"

### VPN
alias htb="sudo openvpn --daemon --config ~/.config/htb.ovpn"
```
## Conclusion
There is so much more that can be done with NixOS than what I have showcased in this example configuration. I highly recommend reading through the reference material I have linked and working out a structure for your configuration that meets your needs. This configuration can be found here: https://github.com/mnelly/NixOS