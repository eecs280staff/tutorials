---
layout: spec
title: Leak Checking
---

Leaking Checking
========================
{: .primer-spec-toc-ignore }

This tutorial explains how to check for memory leaks in C/C++ programs on different platforms.

FIXME: which platform do you have?

## Prerequisites
This tutorial relies on command line tools.  If you haven't installed CLI tools on your machine yet, follow one of these tutorials first.

| [macOS](setup_macos.html)| [Windows](setup_wsl.html) | [Linux](setup_wsl.html#install-cli-tools)

We're also assuming you are familiar with a text editor or IDE.  If you haven't already done so, follow one of these tutorials.

| [VS Code (recommended)](https://eecs280staff.github.io/tutorials/setup_vscode.html)| [Visual Studio](https://eecs280staff.github.io/tutorials/setup_visualstudio.html) | [Xcode](https://eecs280staff.github.io/tutorials/setup_xcode.html) |

Finally, you'll need to be familiar with the command line interface (CLI).  If you haven't seen it yet, take a look at the [Command Line Tutorial](cli.html).

## Example program
We'll work in a directory called `leakcheck` just for this tutorial.
```console
$ mkdir leakcheck
```

Create a file called `main.cpp` and copy-paste this leaky Hello World program.
```c++
#include <iostream>
using namespace std;

int main() {
  new int;  // Memory leak!
  cout << "Hello Leaks!\n";
}
```
{: data-title="main.cpp" }

Create a file called `Makefile` and copy-paste this code.
```make
CXX ?= g++
CXXFLAGS ?= -Wall -Werror -pedantic -g --std=c++11 -Wno-sign-compare -Wno-comment

# Run regression test
test: main.exe
	./main.exe

# Compile the main executable
main.exe: main.cpp
	$(CXX) $(CXXFLAGS) main.cpp -o main.exe

# Remove automatically generated files
clean :
	rm -rvf *.exe *~ *.out *.dSYM *.stackdump
```
{: data-title="Makefile" }

Your directory should look like this:
```console
$ tree
.
├── Makefile
└── main.cpp
```

The program should compile and run.
```console
$ make main.exe
$ ./main.exe
Hello Leaks!
```

## macOS `arm64`
Also known as Apple Silicon, M1, M2, etc.

FIXME

## macOS `x86_64` 
Also known as Intel chip.

FIXME

## WSL

FIXME

## CAEN Linux

FIXME


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
