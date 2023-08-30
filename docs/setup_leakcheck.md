---
layout: spec
title: Leak Checking
---

Leaking Checking
========================
{: .primer-spec-toc-ignore }

This tutorial explains how to check for memory leaks in C/C++ programs.  We'll use the [address sanitizer](setup_asan.html) on WSL and Linux and the `leaks` tool on macOS.

## Quick start
**WSL or Linux:** Add the compiler flag `-fsanitize=address` in the `Makefile`.  Compile and run.
```make
CXXFLAGS ?= -Wall -Werror -pedantic -g --std=c++17 -Wno-sign-compare -Wno-comment
# primer-spec-highlight-start
CXXFLAGS += -fsanitize=address
# primer-spec-highlight-end
```
{: data-title="Makefile" }

```console
$ make main.exe
$ ./main.exe
```

**macOS:** Compile and run with the `leaks` tool.  Not compatible with the address sanitizer (remove compiler flag `-fsanitize=address`).
```console
$ make main.exe
$ MallocStackLogging=1 leaks -quiet -atExit -- ./main.exe
```

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
CXXFLAGS ?= -Wall -Werror -pedantic -g --std=c++17 -Wno-sign-compare -Wno-comment

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

## macOS
Verify that your Makefile does *not* enable the address sanitizer.
```make
CXXFLAGS ?= -Wall -Werror -pedantic -g --std=c++17 -Wno-sign-compare -Wno-comment
# primer-spec-highlight-start
# Should *not* see -fsanitize=address
# primer-spec-highlight-end
```
{: data-title="Makefile" }

Compile and run with the `leaks` program.
```console
$ make main.exe
$ MallocStackLogging=1 leaks -quiet -atExit -- ./main.exe
main.exe(14025) MallocStackLogging: could not tag MSL-related memory as no_footprint, so those pages will be included in process footprint - (null)
main.exe(14025) MallocStackLogging: recording malloc and VM allocation stacks using lite mode
Hello Leaks!
Process 14025 is not debuggable. Due to security restrictions, leaks can only show or save contents of readonly memory of restricted processes.

leaks Report Version: 4.0, multi-line stacks
Process 14025: 226 nodes malloced for 17 KB
Process 14025: 1 leak for 16 total leaked bytes.

STACK OF 1 INSTANCE OF 'ROOT LEAK: <malloc in main>':
3   dyld                                  0x1aea9be50 start + 2544
2   main.exe                              0x1001bf220 main + 16  main.cpp:0
1   libc++abi.dylib                       0x1aed808b0 operator new(unsigned long) + 32
0   libsystem_malloc.dylib                0x1aec2ec20 _malloc_zone_malloc_instrumented_or_legacy + 128 
====
    1 (16 bytes) ROOT LEAK: <malloc in main 0x12e7040f0> [16]
```
{: data-highlight="10" }

## WSL and Linux
Edit your Makefile and enable the address sanitizer by adding `-fsanitize=address`.
```make
CXXFLAGS ?= -Wall -Werror -pedantic -g --std=c++17 -Wno-sign-compare -Wno-comment
# primer-spec-highlight-start
CXXFLAGS += -fsanitize=address
# primer-spec-highlight-end
```
{: data-title="Makefile" }

Compile and run.
```console
$ make main.exe
$ ./main.exe
Hello Leaks!

=================================================================
==1905936==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 4 byte(s) in 1 object(s) allocated from:
    #0 0x7f54a90ae7b0 in operator new(unsigned long) (/lib64/libasan.so.5+0xf17b0)
    #1 0x400a73 in main /home/awdeorio/leakcheck/main.cpp:6
    #2 0x7f54a8303cf2 in __libc_start_main (/lib64/libc.so.6+0x3acf2)

SUMMARY: AddressSanitizer: 4 byte(s) leaked in 1 allocation(s).
```
{: data-highlight="13" }

<div class="primer-spec-callout warning" markdown="1">
**CAEN Linux Pitfall** On Michigan Engineering CAEN Linux, you may need to load a full-featured GCC with the `module` tool.  You'll need to do this each time you log in.
```console
$ module load gcc/9
```
</div>

## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
