---
layout: spec
title: Windows command line tools (WSL)
sitemapOrder: 15
---

Windows Command line tools (WSL)
================================
{: .primer-spec-toc-ignore }

The Windows Subsystem for Linux (WSL) runs an Ubuntu Linux guest virtual machine on your Windows host machine.

When you see `$` in this tutorial, you should type into your shell the command that comes after the `$`.

## Upgrade Windows
We recommend Windows 11.  Windows 10 version 2004 build 19041 and higher will also work.  Here's how to [check your Windows version](https://support.microsoft.com/en-us/help/4027391/windows-10-see-which-version-you-have).

Free Windows upgrades are available for UM students via [OnTheHub](https://its.umich.edu/computing/computers-software/software-services/onthehub). If you have an older Windows machine and are not able to upgrade, please reach out to course instructors for assistance.

## Install Windows Subsystem for Linux (WSL)
Start PowerShell and run it as administrator.  Search for PowerShell in the start menu, then right-click and select "Run as administrator".

<img src="images/wsl010.png" width="240px" />

Check if WSL was already installed.  If you see this, WSL is already installed.
```console
C:\WINDOWS\system32> wsl -l -v
  NAME      STATE       VERSION
* Ubuntu    Stopped     2
```
{: data-highlight="3"}

If you see the WSL help text, WSL is not installed.
```console
C:\WINDOWS\system32> wsl -l -v
Copyright (c) Microsoft Corporation. All rights reserved.
Usage: wsl.exe [Argument]
...
```

Install WSL.
```console
C:\WINDOWS\system32> wsl --install
```

Note: these instructions are based on the [instructions from Microsoft](https://docs.microsoft.com/en-us/windows/wsl/install).

Start an Ubuntu Bash shell (not a Windows PowerShell).

<img src="images/wsl020.png" width="240px" />

An Ubuntu Bash shell is a terminal that looks like this.

<img src="images/wsl030.png" width="480px" />

## Install CLI tools
Use the `apt` package manager to install a few command line programs.  Linux users will run this same command.
```console
$ sudo apt install g++ make rsync wget git ssh gdb python3 tree
```
{: data-variant="no-line-numbers" }

## Pro-tips

### Copy paste

Right click on the window pane header of your Ubuntu WSL terminal. You should see a drop down like the one below:
<img src="/images/wsl040.png" width=720px>

If your dropdown contains the option "Settings" (as seen above) follow the [Windows 11 H2 or newer](#windows-11-h2-or-newer) instructions. If your dropdown contains the option "Properties" follow the [Older than Windows 11 H2](#older-than-windows-11-h2)

#### Windows 11 H2 or newer:


#### Older than Windows 11 H2:

Click on the properties option in the dropdown.

<img src="https://defragged.org/wp-content/uploads/2020/10/Defragged-LinuxSubsytem-Properties.png" width=480px>


Enable "Use Ctrl+Shift+C/V Copy/Paste" option in the Console "Options" properties page 
- Copy: <kbd>Control</kbd> + <kbd>Shift</kbd> + <kbd>c</kbd>
- Paste: <kbd>Control</kbd> + <kbd>Shift</kbd> + <kbd>v</kbd>

<img src="https://devblogs.microsoft.com/wp-content/uploads/sites/33/2019/04/copy-paste.png" width=480px>

(Source: [Microsoft blog](https://devblogs.microsoft.com/commandline/copy-and-paste-arrives-for-linuxwsl-consoles/) and [Defragged](https://defragged.org/2020/10/29/how-to-copy-paste-in-windows-subsystem-for-linux-wsl/))

### Accessing Windows files
Windows files are accessible from Linux at `/mnt/c/`.  You have a separate Ubuntu home directory, e.g., `/home/awdeorio/`.
```console
$ cd /mnt/c/Users/awdeorio/Desktop    # Windows Desktop
$ cd /mnt/c/Users/awdeorio/Documents  # Windows Documents
```

### CLI open file
Simulate a double-click from the command line in the WSL Bash shell.
```console
$ cmd.exe /c start babychickens.jpg
```
{: data-variant="no-line-numbers" }


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
