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

Next, follow the instructions from Microsoft: [https://docs.microsoft.com/en-us/windows/wsl/install](https://msdn.microsoft.com/en-us/commandline/wsl/install-win10).  Be sure to select "Ubuntu Linux".

### Open terminal
Start an Ubuntu Bash shell (not a Windows PowerShell).

<img src="images/wsl020.png" width="240px" />

An Ubuntu Bash shell is a terminal that looks like this.

<img src="images/wsl030.png" width="480px" />

## Install CLI tools
Use the `apt-get` package manager to install a few command line programs.
```console
$ sudo apt-get install g++ make rsync wget git ssh gdb python3 tree
```
{: data-variant="no-line-numbers" }

## Pro-tips

### Copy paste
Enable "Use Ctrl+Shift+C/V Copy/Paste" option in the Console "Options" properties page ([Source: Microsoft blog](https://devblogs.microsoft.com/commandline/copy-and-paste-arrives-for-linuxwsl-consoles/)).
- Copy: <kbd>Control + Shift + C</kbd>
- Paste: <kbd>Control + Shift + V</kbd>

<img src="https://devblogs.microsoft.com/wp-content/uploads/sites/33/2019/04/copy-paste.png" width=480px>

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
