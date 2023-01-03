---
layout: spec
title: Setup Address Sanitizer
excludeFromSitemap: true
---

Address Sanitizer
=================
{: .primer-spec-toc-ignore }

This tutorial will compile your code with an Address Sanitizer enabled.  The Address Sanitizer is very good at finding memory errors, including going off the end of an array or vector.

If you're interested, here is more information about the [Address Sanitizer](https://github.com/google/sanitizers/wiki/AddressSanitizer) and [detailed examples](https://developers.redhat.com/blog/2021/05/05/memory-error-checking-in-c-and-c-comparing-sanitizers-and-valgrind) of what the Address Sanitizer can do.


## Quick Start
To enable the address sanitizer, add these compiler flags to add to your `Makefile`.

**WSL or Linux:** Add compiler flags `-fsanitize=address`, `-fsanitize=undefined`, and `-D_GLIBCXX_DEBUG`.  For example:
```make
CXXFLAGS = --std=c++11 -Wall -Werror -pedantic -g -fsanitize=address -fsanitize=undefined -D_GLIBCXX_DEBUG
```
{: data-variant="no-line-numbers" }

**macOS:**  Add compiler flags `-fsanitize=address` and `-fsanitize=undefined`.  For example:
```make
CXXFLAGS = --std=c++11 -Wall -Werror -pedantic -g -fsanitize=address -fsanitize=undefined
```
{: data-variant="no-line-numbers" }

**Visual Studio:** Address sanitizer is enabled by default.

**Xcode:** Enable the address sanitizer and undefined behavior sanitizer with this ([tutorial](setup_xcode.html#sanitizers)).


## Example without Address Sanitizer
Let's do an example without an address sanitizer.  We'll make a mistake on purpose in `asan.cpp`, going off the end of a `vector`.

```c++
#include <iostream>
#include <vector>
using namespace std;

int main() {
  cout << "hello from main!\n";
  vector<int> v = {0, 0};  // v contains 2 items
  // primer-spec-highlight-start
  cout << v[2] << "\n";    // off the end of v
  // primer-spec-highlight-end
}
```
{: data-title="asan.cpp" }

Clean, build and run.  Notice that the output of `v[2]` is `0`, however, this operation is undefined because it's off the end of the vector!  Because the output is undefined, your output might be different.
```console
$ g++ --std=c++11 -Wall -Werror -pedantic -g asan.cpp -o asan.exe
$ ./asan.exe
hello from main!
0
```
{: data-highlight="4" }


## Example with Address Sanitizer
We'll now recompile with the address sanitizer enabled.  If you're using GCC, you'll need version 4.9 or higher.

<div class="primer-spec-callout warning icon-warning" markdown="1">
**WARNING:** The address sanitizer will not work on heavily resource limited system such as CAEN Linux.
</div>

Compile with the address sanitizer flags.
```console
$ g++ --std=c++11 -Wall -Werror -pedantic -g -fsanitize=address -fsanitize=undefined asan.cpp -o asan.exe
```

Run.  The sanitizer detects the problem and prints a stack trace.
```console
$ ./asan.exe
hello from main!
=================================================================
==34112==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x6020000000d8 at pc 0x000105c1639c bp 0x7ffee9fe9f90 sp 0x7ffee9fe9f88
READ of size 4 at 0x6020000000d8 thread T0
    #0 0x105c1639b in main asan.cpp:8
    #1 0x7fff206a4f3c in start+0x0 (libdyld.dylib:x86_64+0x15f3c)
...
```
{: data-highlight="6" }


## Visual Debuggers

### Visual Studio
Visual Studio provides an address sanitizer with bounds checking automatically with every debug build.  You don't need to do anything different!

### Xcode
If you're using Xcode, you can enable the address sanitizer and undefined behavior sanitizer when you run or debug within Xcode ([tutorial](setup_xcode.html#sanitizers)).

### VS Code
If you're using VS Code, you can enable the address sanitizer by editing your `launch.json` ([tutorial](setup_vscode.html#sanitizers)).

### Emacs, VIM, et al.
Update your `Makefile` using the instructions in the [Quick Start](#quick-start).  Then, build and run in the terminal.

## Pro-tips
Here's a way to automatically add the correct compiler flags on macOS or Windows/WSL/Linux.  Edit the top of your `Makefile` with the following code, which will figure out the OS and append the correct compiler flags.  
```make
CXX ?= g++
CXXFLAGS ?= --std=c++11 -Wall -Werror -pedantic -g

# Add sanitizer flags for identifying undefined behavior.  The flags are
# different on macOS (AKA Darwin) and Windows/WSL/Linux.
UNAME := $(shell uname -s)
ifeq "$(UNAME)" "Darwin"
	CXXFLAGS += -fsanitize=address -fsanitize=undefined
else
	CXXFLAGS += -fsanitize=address -fsanitize=undefined -D_GLIBCXX_DEBUG
endif
```


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
