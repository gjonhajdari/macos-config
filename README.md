# macos-config
Configuration for personal and University development environment for Mac.

Last updated on: *17/03/2023*.
Changes may occur in the future.

## Table of contents
1. [Homebrew Instalation](#homebrew-instalation)
	- [Apps to install with Homwbrew](#apps-to-install-with-homebrew)
2. [iTerm 2 Configuration](#iterm-2-configuration)
3. [Linux Configuration](#homebrew-instalation)
	- [Downloading VMBox and Ubuntu](#downloading-vmbox-and-ubuntu)
	- [Compiling the Linux Kernel](#compiling-linux-kernel)


## Homebrew instalation
1. install Xcode command line tools with
```
xcode-select --install
```

2. Install home brew with the following command
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
- Install packages use `brew install --cask / --formula [package-name]`
- Uninstall packages with `brew uninstall [package-name]`
- Uninstall while ignoring dependencies with `brew uninstall --ignore-dependencies [package-name]`
- Update homebrew with `brew update`
- Find out what packages need updates with `brew outdated`
- Update installed packages with `brew upgrade`
- Update singlular package with `brew install [formula-name] && brew cleanup [formula-name]`
- List installed packages with `brew list`
- Search for packages with `brew search --cask / --formula [package-name]`
- View packages with a dependency tree `brew deps --tree --installed`

### Apps to install with Homebrew
Download all apps with the following command
```
brew install --cask git wget iterm2 visual-studio-code google-chrome figma slack
```


## iTerm 2 Configuration
1. Install using Homebrew (if not already installed)
```	
brew install --cask iterm2
```

2. Install Oh My ZSH
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

3. Download custom fonts [here](https://github.com/Falkor/dotfiles/blob/master/fonts/SourceCodePro%2BPowerline%2BAwesome%2BRegular.ttf) and put them in font book. **[OPTIONAL]**

4. Download powerlevel10k
```
git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```

5. Set up color scheme and font
	- Download scheme `git clone https://github.com/dracula/iterm.git`
	- iTerm2 > Preferences > Profiles > Colors
	- Color presets > import... and choose Dracula.itermcolors
	- Go to Text > Font and choose SourceCodePro


6. Set theme on *.zshrc* `ZSH_THEME="powerlevel10k/powerlevel10k"`, open iTerm 2 and follow the configuration steps.
	- If you don't like the configuration change it again with `pk10 configure`

7. Configure VSCode terminal
	- Open Code > Preferences > Settings
	- Type settings.json, click JSON and Edit in settings.json
	- Add this to the json file `"terminal.integrated.fontFamily": "MesloLGS NF",`

## Linux Configuration

### Setting up UTM and Ubuntu
1. Download the software
	- UTM via the App Store or from [this link](https://mac.getutm.app/).
	- Ubuntu Server for ARM [here](https://ubuntu.com/download/server/arm).

2. Create a new virtual machine
	- Create > Virtualize > Linux > Browse and choose iso image
	- VM Specs: 4GB RAM, 4 CPU cores, 80 GB storage
	- Enable directory share with Downloads folder **[OPTIONAL]**

3. Start VM, follow instalation steps and reboot

4. Right click VM > Edit > Drives and delete ISO image

5. Install Ubuntu Desktop
```
sudo apt install tasksel
sudo tasksel install ubuntu-desktop
sudo reboot
```

6. Enable Directory Sharing
```
sudo apt install spice-vdagent spice-webdavd
```

### Compiling Linux Kernel