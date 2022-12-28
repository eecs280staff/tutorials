---
layout: spec
title: Setup Make
sitemapOrder: 30
---

`make` and Makefiles
====================
{: .primer-spec-toc-ignore }

This tutorial will show you how to compile source code at the command line without typing long commands.  Instead, we'll write the commands in a file called `Makefile` and use shortcuts.


## Prerequisites
This tutorial relies on command line tools.  If you haven't installed CLI tools on your machine yet, follow one of these tutorials first.

| [macOS](setup_macos.html)| [Windows](setup_wsl.html) | [Linux](setup_wsl.html#install-cli-tools)

We're also assuming you are familiar with a text editor or IDE.  If you haven't already done so, follow one of these tutorials.

| [VS Code (recommended)](https://eecs280staff.github.io/tutorials/setup_vscode.html)| [Visual Studio](https://eecs280staff.github.io/tutorials/setup_visualstudio.html) | [Xcode](https://eecs280staff.github.io/tutorials/setup_xcode.html) |

Finally, you'll need to be familiar with the command line interface (CLI).  If you haven't seen it yet, take a look at the [Command Line Tutorial](cli.html).


## Example program
We'll work in a directory called `make-example` just for this tutorial.
```console
$ mkdir make-example
```

Create a file called `main.cpp` and copy-paste this Hello World program.
```c++
#include <iostream>
using namespace std;

int main() {
  cout << "Hello World!\n";
}
```
{: data-title="main.cpp" }

Create a file called `Makefile` and copy-paste this code.
```make
CXX ?= g++
CXXFLAGS ?= -Wall -Werror -pedantic -g --std=c++11 -Wno-sign-compare -Wno-comment

# Run regression test
test: main.exe
	./main.exe < main_test.in > main_test.out
	diff main_test.out main_test.out.correct
	echo PASS

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


## Compile
Compile manually, then remove the executable.
```console
$ g++ -Wall -Werror -pedantic -g --std=c++11 -Wno-sign-compare -Wno-comment main.cpp -o main.exe
$ ./main.exe
Hello World!
$ rm main.exe
```

Compile using `make`.  Notice that it automated the long compile command.
```console
$ make main.exe
g++ -Wall -Werror -pedantic -g --std=c++11 -Wno-sign-compare -Wno-comment main.cpp -o main.exe
```

Run the executable created by the previous `make` command.
```console
$ ./main.exe
Hello World!


Clean up using `make`.  Notice that it automated the `rm` command.
```console
$ make clean
rm -rvf *.exe *~ *.out *.dSYM *.stackdump
removed 'main.exe'
```


## Test
Makefiles are useful for automating tests, sometimes called a *regression test*.

Put input (stdin) in a file.  We'll use `main_test.in` in this example.
```
this program doesn't read stdin,
so this text doesn't matter
```
{: data-title="main_test.in" }

Put the correct output in a file.  We'll use `main_test.out.correct`.  In EECS 280 and EECS 281, these are typically provided.
```
Hello World!
```
{: data-title="main_test.out.correct" }

Run manually with [input redirection](cli.html#input-redirection-) and [output redirection](cli.html#output-redirection-).  Compare with [`diff`](cli.html#diff).
```console
$ ./main.exe < main_test.in > main_test.out
$ diff main_test.out main_test.out.correct
```

Run regression test automatically using the Makefile.
```console
$ make test
./main.exe < main_test.in > main_test.out
diff main_test.out main_test.out.correct
```

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** Run commands in parallel with `make -j`.
```console
$ make -j4 test
```
{: data-variant="no-line-numbers" }
</div>


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
