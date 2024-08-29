---
layout: spec
title: macOS Command Line Tools
---

macOS command line tools
========================
{: .primer-spec-toc-ignore }

macOS comes with a Terminal and can run UNIX command-line tools directly.

The macOS terminal command prompt ends with a `$`. To follow the steps below, type commands that appear after the `$` and hit enter.

## Open terminal
Open the Terminal application which comes with macOS.

<img src="images/macos010.png" width="480px" />

## Install compiler
Install a compiler.
```console
$ xcode-select --install
```

Notice that this compiler is really Apple LLVM pretending to be `g++`.  Your version might be different.
```console
$ g++ --version
Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
Apple LLVM version 9.0.0 (clang-900.0.38)
Target: x86_64-apple-darwin16.7.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin
```

## Homebrew package manager
Install the [Homebrew package manager](https://brew.sh/).
```console
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

<div class="primer-spec-callout warning" markdown="1">
Apple Silicon users ("M1", "M2", etc.) only - Homebrew installs to a non-standard location, `/opt/homebrew/`. You'll need to run the following to add Homebrew to your path:

```console
$ echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
```

Close your terminal and reopen your terminal.
</div>

Check your install.  Your version might be different.
```console
$ brew --version
Homebrew 4.3.18
```

## Install CLI tools
Use the Homebrew package manager to install a few command line programs.
```console
$ brew install wget git tree
```

## Home Directory
Run `cd ~`. This will take you to your Ubuntu home directory. Running `pwd` afterward confirms the location. (Your username will be different.)

```console
$ cd ~
$ pwd
/Users/jjuett
```

Create an EECS 280 folder by running `mkdir ~/eecs280`. Running `ls` afterward confirms the folder has been created.

```console
$ mkdir ~/eecs280
$ ls
Desktop		Downloads	Movies		Pictures	eecs280
Documents	Library		Music		Public
```

We highly recommend you store all coding work for EECS 280 projects and labs here.

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Avoid paths names that contain spaces. Spaces cause problems with some command line tools.

| Bad Example     | Good Example   |
|-----------------|----------------|
| `EECS 280/` | `eecs280/` |
| `Project 1 Stats/` | `p1-stats/` |

</div>

## Use CLI tools
Now would be a great time to take a look at our [CLI Tutorial](cli.html).

## Pro-tips

### CLI open file
Opens a file or directory with the default application, like a double click.  See the [`open` command](cli.html#open--wslview) in the CLI tutorial.


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
