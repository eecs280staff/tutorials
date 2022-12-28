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

## Run one unit test
To run one unit test, e.g., `stats_public_test.exe`, first compile it.
```console
$ make stats_public_test.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_public_test.cpp stats.cpp p1_library.cpp -o stats_public_test.exe
```

Then, run the unit test.
```console
$ ./stats_public_test.exe
Assertion failed: (false), function count, file stats.cpp, line 12.
countAbort trap: 6
```


## Run one system test
To run one system test, compile `main.exe`, run and save the output, then compare the output to the correct answer.

Compile
```console
$ make main.exe
g++ -Wall -Werror -pedantic -g --std=c++11 main.cpp stats.cpp p1_library.cpp -o main.exe
```

Run.  Redirect input from the file `main_test.in` and redirect output to the file `main_test.out`.
```console
$ ./main.exe < main_test.in > main_test.out
```

Compare your output in `main_test.out` to the correct output in `main_test.out.correct`.  What you see are the differences.
```console
$ diff main_test.out main_test.out.correct
1c1,23
< hello from main!
---
> enter a filename
> enter a column name
> reading column B from main_test_data.tsv
> Summary (value: frequency)
> 6: 1
> 7: 1
> 8: 1
> 9: 1
> 10: 1
> 
> count = 5
> sum = 40
> mean = 8
> stdev = 1.58114
> median = 8
> mode = 6
> min = 6
> max = 10
>   0th percentile = 6
>  25th percentile = 7
>  50th percentile = 8
>  75th percentile = 9
> 100th percentile = 10
```


## Run regression test
The `Makefile` contains a regression test.  It will compile all unit tests, your main application, and then run all unit tests and all system tests.

Notice that the regression test is a list of the commands we entered while running unit and system tests.
```make
# This appears in the Makefile
test: main.exe stats_tests.exe stats_public_test.exe
	./stats_public_test.exe
	./stats_tests.exe
	./main.exe < main_test.in > main_test.out
	diff main_test.out main_test.out.correct
```

Run regression test.  It will stop at the first error.
```console
$ make test
g++ -Wall -Werror -pedantic -g --std=c++11 main.cpp stats.cpp p1_library.cpp -o main.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_public_test.cpp stats.cpp p1_library.cpp -o stats_public_test.exe
./stats_public_test.exe
Assertion failed: (false), function count, file stats.cpp, line 12.
countmake: *** [test] Abort trap: 6
```


## Pro-tips
Speed up compilation by running multiple commands simultaneously.
```console
$ make -j4 test
```
{: data-variant="no-line-numbers" }


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
