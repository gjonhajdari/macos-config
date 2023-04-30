# macos-config
Configuration for personal and University development environment for Mac.

Last updated on: *30/04/2023*.
Changes may occur in the future.

## Table of contents
1. [Homebrew Instalation](#homebrew-instalation)
	- [Available commands](#homebrew-available-commands)
	- [Apps to install with Homwbrew](#apps-to-install-with-homebrew)
2. [iTerm 2 Configuration](#iterm-2-configuration)
3. [JavaFX Development Environment](#javafx-development-environment)
4. [Linux Configuration](#linux-configuration)
	- [Set up VMBox and Ubuntu](#setting-up-utm-and-ubuntu)
		- [Themes and libraries](#themes-and-libraries)
	- [Compiling the Linux Kernel](#compiling-linux-kernel)
		- [Create new system call](#create-new-system-call)


## Homebrew instalation
1. Install Homebrew with the following command
```bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. Add Homebrew to PATH
```bash
$ echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
$ eval "$(/opt/homebrew/bin/brew shellenv)"
```

### Available commands
- Install packages use `brew install --cask / --formula [package-name]`
- Uninstall packages with `brew uninstall [package-name]`
- Uninstall while ignoring dependencies with `brew uninstall --ignore-dependencies [package-name]`
- Update Homebrew with `brew update`
- Find out what packages need updates with `brew outdated`
- Update installed packages with `brew upgrade`
- Update singlular package with `brew install [formula-name] && brew cleanup [formula-name]`
- List installed packages with `brew list`
- Search for packages with `brew search --cask / --formula [package-name]`
- View packages with a dependency tree `brew deps --tree --installed`

### Apps to install with Homebrew
Download all apps with the following command
```bash
$ brew install --cask git wget python iterm2 visual-studio-code google-chrome figma slack github microsoft-word latest rocket mission-control-plus middle appcleaner maccy hiddenbar
```


## iTerm 2 Configuration
1. Install using Homebrew (if not already installed)
```	bash
$ brew install --cask iterm2
```

2. Install Oh My ZSH
```bash
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

3. Download custom fonts [here](https://github.com/Falkor/dotfiles/blob/master/fonts/SourceCodePro%2BPowerline%2BAwesome%2BRegular.ttf) and put them in font book **[OPTIONAL]**

4. Download powerlevel10k
```bash
$ git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```

5. Set up color scheme and font
	- Download scheme `git clone https://github.com/dracula/iterm.git`
	- iTerm2 > Preferences > Profiles > Colors
	- Color presets > import... and choose Dracula.itermcolors
	- Go to Text > Font and choose SourceCodePro **[OPTIONAL]**


6. Set theme on *.zshrc* `ZSH_THEME="powerlevel10k/powerlevel10k"`, open iTerm 2 and follow the configuration steps
	- If you don't like the configuration change it again with `pk10 configure`

7. Configure VSCode terminal
	- Open Code > Preferences > Settings
	- Type settings.json, click JSON and Edit in settings.json
	- Add this to the json file `"terminal.integrated.fontFamily": "MesloLGS NF",`

## JavaFX Development Environment
1. Download JDK via Homebrew or from [this link](https://www.oracle.com/cis/java/technologies/downloads/#jdk19-mac) (current version 19.0.2)
```bash
$ brew install --cask oracle-jdk
```

2. Download IntelliJ IDEA via Homebrew or from  [this link](https://www.jetbrains.com/idea/download/#section=mac)
```bash
$ brew install --cask intellij-idea
```

3. Create a new JavaFX project
	- File > New > Project > JavaFX
	- Go to src > main > java right click and create new package Ushtrimet_KNK
	- Create subpackages with classes as you go
	- Edit src > main > java module-info.java and add `exports Ushtrimet_KNK.[subpackage] to javafx.graphics;`

4. Download SceneBuilder via Homebew or [this link](https://gluonhq.com/products/scene-builder/#download)
```bash
$ brew install --cask scenebuilder
```

## Linux Configuration

### Setting up UTM and Ubuntu
1. Download the software
	- UTM via the App Store or from [this link](https://mac.getutm.app/)
	- Ubuntu Dekstop for ARM [here](https://cdimage.ubuntu.com/jammy/daily-live/current/)

2. Create a new virtual machine
	- Create > Virtualize > Linux > Browse and choose iso image
	- VM Specs: 4GB RAM, 4 CPU cores, 80 GB storage
	- Enable OpenGL hardware acceleration and Downloads shared directory
	- Right click VM > Settings > Display and enable Retina Mode

3. Start VM, log in with *ubuntu*, follow instalation steps and power off

4. Click on Drive image options at the rop right and eject the ISO image

5. Start VM again and install support for VM guest additions and reboot
```bash
$ sudo apt install spice-vdagent spice-webdavd
$ reboot
```

To view files on shared directory go to https://localhost:9843.

#### Themes and libraries

1. Use Dracula theme for terminal **[OPTIONAL]**
```bash
$ sudo apt-get install dconf-cli

$ git clone https://github.com/dracula/gnome-terminal
$ cd gnome-terminal

$ ./install.sh
```

2. Libraries to install
```bash
$ sudo apt install gcc
```

### Compiling Linux Kernel
1. Install the latest kernel version [here](https://www.kernel.org) or with command
```bash
$ wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-[version].tar.xz
```

3. Extract files from kernel zip
```bash
$ tar xvf linux-[version].tar.xz
```

3. Install requierments
```bash
$ sudo apt-get install git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison
```

4. Copy the configuration file
```bash
$ cd linux-[version]
$ cp -v /boot/config-$(uname -r) .config
```


5. Disable the security certificates
```bash
$ scripts/config --disable SYSTEM_TRUSTED_KEYS
$ scripts/config --disable SYSTEM_REVOCATION_KEYS
```

7. Save config and build
```bash
$ make menuconfig
$ make -j 4
```

7. Install modules and finally install the kernel
```bash
$ sudo make modules_install
$ sudo make install
```

8. Update the bootloader **[OPTIONAL]**
```bash
$ sudo update-initramfs -c -k 6.0.7
$ sudo update-grub
```

9. Reboot and verify version
```bash
$ uname -mrs
```

#### Create new system call
1. Switch user to root, go to linux folder and create a new folder containing the C file
```bash
$ su
$ cd /usr/src/linux-[version]/
$ mkdir hello
$ cd hello
$ vim hello.c
```

2. Paste the following code
```c
#include <linux/kernel.h>

asmlinkage long sys_hello(void)
{
	printk("Hello world!\n");
	return 0;
}
```

3. Create a file named *Makefile* which contains `obj-y := hello.o`

4. Go back to the linux directory, open *Makefile* and add your file name at the end of this line
```makefile
core-y += kernel/ mm/ fs/ ipc/ security/ crypto/ block/ [filename]/
```

5. Go to the system calls table directory and add your function to the *syscall_64.tbl* file
```bash
$ cd /usr/src/linux[version]/arch/x86/entry/syscalls
$ vim syscall_65.tbl

# Ending line should look like this
-> 548		64		hello		sys_hello
```

6. Add new system call at the end of the system calls heaader file
```c
$ cd /usr/src/linux[version]/include/linux
$ vim syscalls.h

asmlinkage long sys_helo(void);
#endif
```

7. Recompile, update the kernel and reboot the system
```bash
$ cd /usr/src/linux[version]
$ make bzimage -j 4

$ make modules_install
$ sudo make install

$ reboot
```

8. Create a new program and use the system call
```c
$ vim test.c

#include <stdio.h>
#include <linux/kernel.h>
#include <sys/syscall.h>
#include <unistd.h>

int main()
{
	long int test = syscall(548); // ID of system call (sys_hello)
	printf("The system call returns: %ld\n", test);
	return 0;
}
```

9. Compile and show the message sent by the kernel
```bash
$ gcc -o test test.c
$ ./test
-> The system call returns: 0

$ dmseg
-> [ value] Hello World!
```