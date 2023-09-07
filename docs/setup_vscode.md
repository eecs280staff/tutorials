---
layout: spec
title: Setup VSCode
sitemapOrder: 20
---

Setup up VS Code for C/C++
==========================
{: .primer-spec-toc-ignore }

[Visual Studio Code](https://code.visualstudio.com/) is a lightweight, easy-to-use, source code editor with debugging support.  It runs on macOS, Windows, and Linux (including CAEN Linux).  Visual Studio Code is not the same program as Visual Studio.

## macOS
Follow the [VS Code on macOS ](setup_vscode_macos.html) tutorial.  You'll use some command line tools installed by Xcode, and some installed with Homebrew.

## Windows
Follow the [VS Code on Windows](setup_vscode_wsl.html) tutorial.  You'll use tools installed on the Windows Subsystem for Linux (WSL).

## Linux
Install VS Code using the [Microsoft documentation](https://code.visualstudio.com/docs/setup/linux).

Install CLI tools using the `apt` command from the [WSL tutorial](setup_wsl.html#install-cli-tools).  The `apt` command works on Ubuntu and other Debian-based systems.

Continue with the VS Code on Windows instructions, beginning with the [Extensions section](setup_vscode_wsl.html#extensions).

## CAEN Linux
VS Code is already installed on the CAEN Linux desktop environment.  Follow the [VS Code on Windows tutorial](setup_vscode_wsl.html), just skip the Install subsection.  You *will* need to install VS Code [extensions](setup_vscode_wsl.html#extensions).
