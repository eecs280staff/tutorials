---
layout: spec
title: Setup Make
sitemapOrder: 30
---

`make` and Makefiles
====================
{: .primer-spec-toc-ignore }

The `make` utility automates compiling a program.  It reads a file that's usually called `Makefile`.

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
```

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

Put the correct output in a file.  We'll use `main_test.out.correct`.  In EECS 280 and EECS 281, a few correct output files are typically provided.
```
Hello World!
```
{: data-title="main_test.out.correct" }

Run manually with [input redirection](cli.html#input-redirection-) and [output redirection](cli.html#output-redirection-).  Compare with [`diff`](cli.html#diff).  No output means the files match.
```console
$ ./main.exe < main_test.in > main_test.out
$ diff main_test.out main_test.out.correct
```

Run the regression test automatically with `make`.
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


## Makefile syntax
This section explains basic `Makefile` syntax.

A `Makefile` consists of *rules* that look like this:
```make
target: prerequisites
	recipe
```

This is a concrete example of a rule.
```make
main.exe: main.cpp
	g++ main.cpp -o main.exe
```

A *target* is usually the name of a file that is created by the rule, like `main.exe`.

A *prerequisite* is a file that is used as input to create the target, like `main.cpp`.  There could be multiple prerequisites.

A *recipe* is a command that `make` runs, like `g++ main.cpp -o main.exe`.  There might be more than one command.

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Every recipe line must start with a TAB character.
</div>

A *variable* is helpful for avoiding duplication, like the compiler flags.
```make
CXX ?= g++
# primer-spec-highlight-start
CXXFLAGS ?= -Wall -Werror -pedantic -g --std=c++11 -Wno-sign-compare -Wno-comment
# primer-spec-highlight-end

main.exe: main.cpp
# primer-spec-highlight-start
	$(CXX) $(CXXFLAGS) main.cpp -o main.exe
# primer-spec-highlight-end

unit_tests.exe: unit_tests.cpp unit.cpp
# primer-spec-highlight-start
	$(CXX) $(CXXFLAGS) unit_tests.cpp unit.cpp -o unit_tests.exe
# primer-spec-highlight-end
```


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
