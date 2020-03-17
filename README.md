# wsl-setup

With the terminal feature in Visual Studio Code, it is possible to access WSL from VS Code. This enables you to create a setup where VS Code behaves like a Linux system for programming within a Windows environment. Because it is possible to edit Windows files from WSL by mounting the Windows file system, the key to this setup is to Symlink from WSL to the Windows file system (/mnt/c/dev in Linux is C:\dev in Windows).

An example setup for programming in Rust illustrates how this works:

* Install Rust in Windows and WSL [rust-lang install](https://www.rust-lang.org/tools/install)
* Customize WSL, install terminal editors or just use default vim.
* Install Visual Studio Code, add favorite plugins.
* Symlink WSL to Windows file system (see instructions below).
* VS Code terminal can open one or many shells to WSL, sort of like tmux or screen.
* VS Code file explorer works sort of like nerdtree.
* Changing colour themes in VS Code also works in the VS Code terminal.

Now you can choose to edit code in VS Code, or with emacs / vim in the VS Code terminal - or use both at the same time. You can also use GitHub for Windows desktop on the "Windows side" for git.

The steps below cover Installing and setting up Windows, installing and setting up WSL, and setting up vim 8.


### Resources

* [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

* [Writing on GitHub](https://help.github.com/categories/writing-on-github/)

* [jessfraz windows-for-linux-nerds](https://blog.jessfraz.com/post/windows-for-linux-nerds/)

* [getting-crazy-with-windows-subsystem-for-linux](https://brianketelsen.com/getting-crazy-with-windows-subsystem-for-linux/)

* [windows-10-fall-creators-update](https://blogs.msdn.microsoft.com/commandline/2017/10/11/whats-new-in-wsl-in-windows-10-fall-creators-update/)

* [windows-10-creators-update](https://blogs.msdn.microsoft.com/commandline/2017/04/11/windows-10-creators-update-whats-new-in-bashwsl-windows-console/)

* [jessfraz/dotfiles](https://github.com/jessfraz/dotfiles)

* [jessfraz/.vim](https://github.com/jessfraz/.vim)

* [bketelsen/dotfiles](https://github.com/bketelsen/dotfiles)


### Install Windows (Specfically for Lenovo T450s)

* backup data

* check access to google and microsoft accounts

* [windows10](https://www.microsoft.com/en-us/software-download/windows10)

* create usb install media and install

* turn off sounds

* install [chrome](https://www.google.com/chrome/)

* install [Lenovo System Update](https://support.lenovo.com/us/en/downloads/ds012808)

* update drivers

* install [git](https://git-scm.com/) (github for windows uses its own stripped down version of git.  Vscode will not use github for windows so it is helpful to install complete version of git.)

* install [github for windows](https://desktop.github.com/)

* [set git username](https://help.github.com/articles/setting-your-username-in-git/)

* [set git email](https://help.github.com/articles/setting-your-commit-email-address-in-git/)

* install [visual studio code](https://code.visualstudio.com/)

Powershell Commands
```
#--- CertUtil to generate hash checksums --#
CertUtil -hashfile path [MD2 MD4 MD5 SHA1 SHA256 SHA384 SHA512]
> CertUtil -hashfile C:\TEMP\MyDataFile.img MD5

#--- List all installed programs --#
> Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table -AutoSize

#--- List all store-installed programs --#
> Get-AppxPackage | Select-Object Name, PackageFullName, Version | Format-Table -AutoSize

#--- Remove package --#
> Get-AppxPackage [Name] | Remove-AppxPackage

# Autodesk
Get-AppxPackage *Autodesk* | Remove-AppxPackage

# BubbleWitch
Get-AppxPackage *BubbleWitch* | Remove-AppxPackage

# Candy Crush
Get-AppxPackage king.com.CandyCrush* | Remove-AppxPackage
```


### Enable WSL

* Windows search for `Windows Features`

* Check Windows Subsystem for Linux

* Reboot

* In Microsoft Store search `Linux`

* Choose Ubuntu or any other preferred distro.

* Run WSL

Install Location
```
C:\Users\[username]\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows...

# File system (Do not edit from windows!)
C:\Users\[username]\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows...\LocalState\rootfs
```

Edit Windows files from Linux
```
# in windows
C:\dev

# in linux
/mnt/c/dev
```


### Find Linux Distro Version

display release information
```
$ lsb_release -a
$ lsb_release -sc
```


### Add user to sudoers

```
-- create a file "custom-users" with visudo
$ sudo EDITOR=vim visudo -f /etc/sudoers.d/custom-users
-- add line to file, prevents password requirement for sudo command
[user] ALL=(ALL) NOPASSWD:ALL
```


### Install tree

To help visually list folder structures, install tree.

```
$ sudo apt-get update
$ sudo apt-get install tree
```


### Symlink to Windows file system

To enable editing in both windows text editor and WSL text editor (like vim), create a symbolic link from WSL to Windows file system.

```
-- create a local folder
$ mkdir projects

-- symlink from WSL to Windows file system
$ ln -s /mnt/c/Users/[user]/Documents/GitHub/[repo-name] /home/[user]/projects/

-- remove the symlink
$ cd projects
$ rm [repo-name]
```


### Install fish

[fishshell](https://fishshell.com/)

To improve shell experience, install fish.

```
-- install fish
$ sudo apt-get update
$ sudo apt-get install fish

-- check version
$ fish -v
fish, version x.x.x

-- temporarily change shell from bash to fish
$ fish

-- if you like fish set as default shell
$ which fish
$ chsh -s [path]

-- to change back to bash
$ which bash
$ chsh -s [path]
```

[Universal Variables in fishshell](https://fishshell.com/docs/current/tutorial.html#tut_universal)
```
 set -U EDITOR vim
```


### Setup vim 8

The Ubuntu distro from the Microsoft store already includes vim 8. To setup I have included a very basic .vimrc in this repo.

```
$ rm ~/.vimrc
$ cp ~/projects/learn-rust/.vimrc ~/.
```
