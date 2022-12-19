---
layout: spec
title: Main Setup Tutorial
sitemapOrder: 10
---

EECS 280 Setup Tutorial
=======================
{: .primer-spec-toc-ignore }

This how-to will walk you through setting up your computer for EECS 280.  At the end, you'll have the starter files for a project compiling on your own computer.  You'll have both command line tools and a visual debugger ready to go.  We'll also walk you through a few other C++ tools.

## Command line tools
The command line interface (CLI) lets us interact with the computer using the keyboard instead of the mouse.  Select your operating system to install CLI tools.

| [macOS](setup_macos.html)| [Windows](setup_wsl.html) | [Linux](setup_wsl.html#install-cli-tools)

After you've installed CLI tools, you should have all these command line programs installed.  Your versions might be different.
```console
$ g++ --version
g++ (GCC) 6.4.0
$ make --version
GNU Make 4.2.1
$ gdb --version   # Windows and Linux only
GNU gdb (Ubuntu 8.1-0ubuntu3) 8.1.0.20180409-git
$ lldb --version  # macOS only
lldb-900.0.64
$ rsync --version
rsync  version 3.1.2  protocol version 31
$ wget --version
GNU Wget 1.19.4 built on linux-gnu.
$ git --version
git version 2.15.1
$ ssh -V
OpenSSH_7.6p1, OpenSSL 1.0.2m  2 Nov 2017
$ tree --version
tree v1.7.0 (c) 1996 - 2014 by Steve Baker, Thomas Moore, Francesc Rocher, Florian Sesser, Kyosuke Tokoro 
```

## Visual debugger
We recommend VS Code because it's easier to use.  Many people use Visual Studio (Windows) or XCode (macOS).  If you're already a whiz with your visual debugger and want a challenge, consider learning Emacs and GDB or LLDB.

| [VS Code Tutorial (recommended)](setup_vscode.html)| [Visual Studio Tutorial](setup_visualstudio.html) | [Xcode Tutorial](setup_xcode.html) | [Emacs (advanced)](setup_emacs.html) |

## Makefiles
Makefiles automate building and testing an application.  They provide shortcuts for long commands.

[Makefile tutorial](setup_make.html)

After you're done, you should be comfortable using the `make` command to clean up and run a regression test.  Remember, your regression test will fail at this point because you haven't finished the project yet.
```console
$ make clean
rm -rvf *.exe *~ *.out *.dSYM *.stackdump
$ make test
g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_public_test.cpp stats.cpp p1_library.cpp -o stats_public_test.exe
g++ -Wall -Werror -pedantic -g --std=c++11 main.cpp stats.cpp p1_library.cpp -o main.exe
./stats_public_test.exe
stats_public_test.exe: stats.cpp:12: int count(std::vector<double>): Assertion `false' failed.
make: *** [test] Aborted
```


## CAEN Linux
This how-to will help you log in to a Linux computer operated by the College of Engineering using `ssh` at the command line.  We'll make sure our code works on a computer that is a lot like the autograder.

[CAEN Linux set up](setup_caen.html)

After you're done, you should be able to copy files from your local computer to a CAEN Linux machine using `rsync`.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ rsync -rtv --exclude '.git*' ../p1-stats/ awdeorio@login.engin.umich.edu:p1-stats-copy/
```

Then you should be able to connect to a CAEN Linux machine with SSH and run your tests.  Remember, your regression test will fail at this point because you haven't finished the project yet.
```console
$ ssh awdeorio@login.engin.umich.edu
Success. Logging you in...
...
$ ls
p1-stats-copy
$ cd p1-stats-copy
$ make clean
rm -rvf *.exe *~ *.out *.dSYM *.stackdump
$ make test
g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_public_test.cpp stats.cpp p1_library.cpp -o stats_public_test.exe
g++ -Wall -Werror -pedantic -g --std=c++11 main.cpp stats.cpp p1_library.cpp -o main.exe
./stats_public_test.exe
stats_public_test.exe: stats.cpp:12: int count(std::vector<double>): Assertion `false' failed.
make: *** [test] Aborted
```

## Acknowledgments
Original document written by Andrew DeOrio <awdeorio@umich.edu>, fall 2017.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
