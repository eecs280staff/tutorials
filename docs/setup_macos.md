---
layout: spec
title: macOS Command Line Tools
---

macOS command line tools
========================
{: .primer-spec-toc-ignore }

The command line interface (CLI) lets us interact with the computer using the keyboard instead of the mouse.  Once you get used to it, it's very powerful and very fast.

When you see `$` in this tutorial, you should type into your shell the command that comes after the `$`.

macOS has a built-in shell.  Open the "Terminal" application.

## Compiler
Install a compiler.
```console
$ xcode-select --install
```
{: data-variant="no-line-numbers" }

Notice that this compiler is really Apple LLVM pretending to be `g++`.
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
{: data-variant="no-line-numbers" }

<div class="primer-spec-callout warning" markdown="1">
Apple Silicon users ("M1" or "M2") only - Homebrew installs to a non-standard location, `/opt/homebrew/`. You'll need to run the following to add Homebrew to your path:

```console
$ echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
$ eval "$(/opt/homebrew/bin/brew shellenv)"
```
{: data-variant="no-line-numbers" }
</div>

## CLI tools
Use Homebrew to install a few command line programs that we'll use later.  Your versions might be different.
```console
$ brew install wget git tree
```
{: data-variant="no-line-numbers" }
