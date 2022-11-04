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
First, we'll install a shell (AKA command line).  The command line interface (CLI) lets us interact with the computer using the keyboard instead of the mouse.  Once you get used to it, it's very powerful and very fast.

When you see `$` in this tutorial, you should type into your shell the command that comes after the `$`.

#### macOS
macOS has a built-in shell.  Open the "Terminal" application.

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

Use Homebrew to install a few command line programs that we'll use later.  Your versions might be different.
```console
$ brew install wget git tree
```
{: data-variant="no-line-numbers" }

#### Windows
On Windows, we recommend Ubuntu running on the Windows Subsystem for Linux (WSL).  WSL runs native Linux command-line tools directly on Windows and includes: 

  * The command line interface programs you need
  * Easy installation of other CLI programs
  * SSH multiplexing to avoid repeated 2FA

You can follow the [WSL Tutorial here](setup_wsl.html).

#### Linux
These instructions work for Ubuntu systems.  Linux systems come with a built-in shell.  Open the "Terminal" application.
```console
$ sudo apt-get install g++ make rsync wget git ssh gdb tree
```
{: data-variant="no-line-numbers" }

#### Check your tools
After you've installed a command line interface, you should have all these command line programs installed.  Your versions might be different.
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


## Starter files
In this section, we'll download the starter files and get our project to compile by adding function stubs.

Open your shell for this step.  On macOS and Linux, that's the Terminal application.  On Windows, it's "Bash on Ubuntu on Windows".

#### Create a folder
Decide where to store your EECS 280 projects.  For reference, here are some common locations.  You might want to use your Desktop, Documents or Dropbox.

| System          | Desktop     | Documents  |
| --------------- | ----------- | ---------- |
| macOS | `/Users/awdeorio/Desktop/` | `/Users/awdeorio/Documents/` |
| Windows/WSL | `/mnt/c/Users/awdeorio/Desktop/` | `/mnt/c/Users/awdeorio/Documents/` |
| Linux | `/home/awdeorio/Desktop/` | `/home/awdeorio/Documents/` |

Navigate to a directory where you will store your EECS 280 projects.  (`awdeorio` likes to use a directory called `src` in his home directory.)
```console
$ cd /Users/awdeorio/src/
```
{: data-variant="no-line-numbers" }

<div class="primer-spec-callout info icon-info" markdown="1">
  A note about file paths that contain spaces.  There are two options:

```console
$ cd /Users/awdeorio/Desktop/EECS\ 280/   # escape with a backslash
$ cd "/Users/awdeorio/Desktop/EECS 280/"  # enclose with quotes
```
</div>


List what's in this directory.  It looks like `awdeorio` has some old files from EECS 485 laying around.
```console
$ ls
eecs485
```

You can create a directory like this.
```console
$ mkdir eecs280
$ ls
eecs280  eecs485
```

Change directory and create another directory for this project.
```console
$ cd eecs280
$ mkdir p1-stats
$ cd p1-stats
```

If you ever get lost at the command line, you can ask "where am I?"  That's called the Present Working Directory, or `pwd`.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
```

#### Download and unpack starter files
Download the starter files.  **Pro-tip:** [copy/paste instructions for WSL](setup_wsl.html#how-do-i-copy-and-paste).
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ wget https://eecs280staff.github.io/p1-stats/starter-files.tar.gz
$ ls
starter-files.tar.gz
```

Unpack the starter files
```console
$ tar -xvzf starter-files.tar.gz
starter-files/
starter-files/Makefile
starter-files/main_test.in
starter-files/main_test.out.correct
starter-files/main_test_data.tsv
starter-files/p1_library.cpp
starter-files/p1_library.h
starter-files/stats.h
starter-files/stats_public_test.cpp
starter-files/stats_tests.cpp.starter
$ ls
starter-files  starter-files.tar.gz
```

Move the starter files from `starter-files/` to your present working directory (denoted by the dot `.`).  Note that the asterisk in `starter-files/*` means "all the files inside the `starter-files/` directory".
```console
$ mv starter-files/* .
$ ls
Makefile               p1_library.cpp        stats.h
main_test.in           p1_library.h          stats_public_test.cpp
main_test.out.correct  starter-files         stats_tests.cpp.starter
main_test_data.tsv     starter-files.tar.gz
```

Now that we've moved the files, we can remove the directory and the tarball.
```console
$ rm -rf starter-files/
$ rm starter-files.tar.gz
$ ls
Makefile               main_test_data.tsv  stats.h
main_test.in           p1_library.cpp      stats_public_test.cpp
main_test.out.correct  p1_library.h        stats_tests.cpp.starter
```

Rename any `.starter` files.
```console
$ mv stats_tests.cpp.starter stats_tests.cpp
$ ls
Makefile               main_test_data.tsv  stats.h
main_test.in           p1_library.cpp      stats_public_test.cpp
main_test.out.correct  p1_library.h        stats_tests.cpp
```

## Visual debugger
We'll walk you through setting up a visual debugger on your local machine (your laptop).

We recommend VS Code because it's easier to setup and easier to use.  Many people use Visual Studio (Windows) or XCode (macOS), so we have tutorials for those too.  If you're already a whiz with your visual debugger and want a challenge, consider learning Emacs and GDB or LLDB.

| [VS Code Tutorial (recommended)](setup_vscode.html)| [Visual Studio Tutorial](setup_visualstudio.html) | [Xcode Tutorial](setup_xcode.html) | [Emacs (advanced)](setup_emacs.html) |

After finishing the visual debugger instructions, you should have added two new files, `stats.cpp` and `main.cpp`.  Double check that you have these files.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ ls
Makefile      main_test.out.correct  p1_library.h  stats_public_test.cpp
main.cpp      main_test_data.tsv     stats.cpp     stats_tests.cpp
main_test.in  p1_library.cpp         stats.h
```

You can view the content of a file at the command line using the `cat` command.
```console
$ cat main.cpp
// main.cpp
// Project UID 5366c7e2b77742d5b2142097e51561a5

#include "stats.h"
#include "p1_library.h"
#include <iostream>
#include <cmath>
using namespace std;

int main() {
  cout << "hello from main!\n";
}
```

`main.cpp` should look like this:
```c++
// main.cpp
// Project UID 5366c7e2b77742d5b2142097e51561a5

#include "stats.h"
#include "p1_library.h"
#include <iostream>
using namespace std;

int main() {
  cout << "hello from main!\n";
}
```
{: data-title="main.cpp" }

`stats.cpp` should look like this:
```c++
// stats.cpp
// Project UID 5366c7e2b77742d5b2142097e51561a5

#include "stats.h"
#include <cassert>
#include <vector>
using namespace std;

vector<vector<double> > summarize(vector<double> v) {
  assert(false);
}

int count(vector<double> v) {
  assert(false);
}

double sum(vector<double> v) {
  assert(false);
}

double mean(vector<double> v) {
  assert(false);
}

double median(vector<double> v) {
  assert(false);
}

double mode(vector<double> v) {
  assert(false);
}

double min(vector<double> v) {
  assert(false);
}

double max(vector<double> v) {
  assert(false);
}

double stdev(vector<double> v) {
  assert(false);
}

double percentile(vector<double> v, double p) {
  assert(false);
}
```
{: data-title="stats.cpp" }


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
