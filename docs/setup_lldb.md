---
layout: spec
title: Setup LLDB (MacOS)
excludeFromSitemap: true
---

LLDB Tutorial
=============
{: .primer-spec-toc-ignore }

[LLDB](https://lldb.llvm.org/) is a command line debugger.  It is the default debugger on macOS, and is used by Xcode behind the scenes.  The basic commands are almost identical to [GDB](https://lldb.llvm.org/use/map.html), and you might want to check out the [GDB Tutorial](setup_gdb.html) too.

Similar to GDB, LLDB is harder to learn than most visual debuggers.  However, you might want to use LLDB instead of GDB or Xcode for several reasons:
- On macOS, LLDB is more reliable than GDB
- Zero setup.  Fast and easy debugging once you've learned.
- Integration with Vim and Emacs (used in EECS 280, optional)

LLDB uses text-based commands, as opposed to the clickable user interface of a visual debugger.  Here's a handy list of LLDB and GDB [commands](https://developer.apple.com/library/archive/documentation/IDEs/Conceptual/gdb_to_lldb_transition_guide/document/lldb-command-examples.html).


## Quick Start
Compile your program with the `-g` flag and start LLDB.
```console
$ make main.exe
g++ -Wall -Werror -pedantic -g --std=c++11 main.cpp -o main.exe
$ lldb main.exe
```

| `b main` | breakpoint on main function |
| `b 13` | breakpoint on line 29 of current file |
| `b main.cpp:13` | breakpoint on line 16 of main.cpp |
| `r` | run or rerun |
| `s` | step into |
| `n` | step over (AKA next) |
| `p myvar` | print variable `myvar` |
| `q` | quit |


## Install
You should already have `g++` and `lldb` installed from the main [macOS tutorial](setup_macos.html).  Your versions might be different.
```console
$ g++ --version
Apple clang version 12.0.0 (clang-1200.0.32.28)
$ lldb --version
lldb-1200.0.44.2
```


## Compile and Run
LLDB uses an executable you build at the command line.

First, compile and run your executable at the command line.
```console
$ make main.exe
$ ./main.exe
Hello World!
```

<div class="primer-spec-callout warning icon-warning" markdown="1">
**Pitfall:** LLDB debugging will be very hard to understand if there are no debugging symbols.  Double check the output of `make` and verify that you see `-g`.
</div>

Run with LLDB.  You now see the LLDB prompt.  LLDB's interface is similar to your command line shell, where you enter commands and press Enter (Return).
```
$ lldb main.exe
(lldb) target create "./main.exe"
Current executable set to '/Users/awdeorio/src/eecs280/p1-stats/main.exe' (arm64).
```
{: data-highlight="1" }

The `r` command runs the program.
```
(lldb) r
Process 72801 launched: '/Users/awdeorio/src/eecs280/p1-stats-vscode/main.exe' (arm64)
Hello World!
Process 72801 exited with status = 0 (0x00000000)
```
{: data-highlight="1" }

Quit with `q`.  **Pro-tip:** `Control-D` will also quit at any time.
```
(lldb) q
```
{: data-highlight="1" }

FIXME `bt` example.

### Sanitizers
We recommend enabling the address sanitizer and undefined behavior sanitizer. These will help you find memory errors like going off the end of an array or vector.

First, edit your `Makefile` and add the `CXXFLAGS` recommended by the [ASAN Quick Start](setup_asan.html#quick-start).

### Input redirection
If you're unfamiliar with input redirection, first read the CLI tutorial section on [input redirection](cli.html#input-redirection-).

Run with input redirection.  Make sure to add the name of your input file (`main_test.in` in this example).
```
$ lldb main.exe
...
(lldb) process launch -i main_test.in
...
```

### Arguments and options
Arguments and options are inputs to a program typed at the command line.  Here's an example from EECS 280 Project 5:
```console
$ ./main.exe train_small.csv test_small.csv --debug
```
{: data-variant="no-line-numbers" data-highlight="1" }

- `main.exe` is the name of the program
- `train_small.csv` and `test_small.csv` are arguments
- `--debug` is an option

To run a program with options or arguments in LLDB, include them after `r`.
```console
$ lldb main.exe
(lldb) r train_small.csv test_small.csv --debug
```
{: data-highlight="2" }

## Debug
In this section, we'll set a breakpoint, which pauses the debugger.  Then, we'll cover some of the options to continue execution.

`n` **Next AKA Step Over**
Run one line of code, stepping _over_ any function calls by running the whole function in one step.

`s` **Step AKA Step Into**
Run one line of code, stepping _into_ any function calls to execute them line-by-line.

`up` **Up AKA Step Out**
Run the program until it returns from the current function (or until the next breakpoint).

`c` **Continue**
Run the program until the next breakpoint.

`q` **Quit**
Quit LLDB.

### Example code

To get started, copy this example `main.cpp` into your editor.
```c++
#include <iostream>
#include <vector>
using namespace std;

double sum (const vector<double> &data) {
  double total = 0;
  for (size_t i=0; i<data.size(); ++i) {
    total += data[i];
  }
  return total;
}

int main() {
  vector<double> data;
  data.push_back(10);
  data.push_back(20);
  data.push_back(30);
  cout << "sum(data) = " << sum(data) << endl;
}
```
{: data-title="main.cpp" }

### Breakpoint
Start LLDB.

```console
$ lldb main.exe
(lldb) target create "main.exe"
Current executable set to '/Users/awdeorio/src/eecs280/p1-stats/main.exe' (arm64).
```

Set a breakpoint on the main function.

```
(lldb) b main
Breakpoint 1: where = main.exe`main + 20 at main.cpp:14:18, address = 0x0000000100003e48
```
{: data-highlight="1" }

<div class="primer-spec-callout info" markdown="1">
**Pro-tip**: There are several ways to set breakpoints.

| `b main` | breakpoint on main function |
| `b 13` | breakpoint on line 13 of current file |
| `b main.cpp:13` | breakpoint on line 16 of stats.cpp |

</div>

### Run
Run the program being debugged.  The program pauses at the breakpoint.

```
(lldb) r
Process 72957 launched: '/Users/awdeorio/src/eecs280/p1-stats-vscode/main.exe' (arm64)
Process 72957 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 1.1
    frame #0: 0x0000000100003e48 main.exe`main at main.cpp:14:18
   11  	}
   12  	
   13  	int main() {
-> 14  	  vector<double> data;
   15  	  data.push_back(10);
   16  	  data.push_back(20);
   17  	  data.push_back(30);
Target 0: (main.exe) stopped.
```
{: data-highlight="1" }

### Step over
Enter `n` AKA "Next" AKA "Step Over" a few times until you reach the highlighted line of code.

```
(lldb) n
Process 72957 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = step over
    frame #0: 0x0000000100003ea4 main.exe`main at main.cpp:18:8
   15  	  data.push_back(10);
   16  	  data.push_back(20);
   17  	  data.push_back(30);
-> 18  	  cout << "sum(data) = " << sum(data) << endl;
   19  	}
```
{: data-highlight="1,8" }

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** Hit Return to repeat your previous command.
</div>

### Inspect
Print the value of a variable with `p`.

```
(lldb) p data
(std::vector<double, std::allocator<double> >) $0 = size=3 {
  [0] = 10
  [1] = 20
  [2] = 30
}
```
{: data-highlight="1" }

### Step into
Enter `s` AKA "Step" AKA "Step Into".  The cursor enters the `sum()` function.

```
(lldb) s
Process 72957 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = step in
    frame #0: 0x0000000100003d7c main.exe`sum(data=size=3) at main.cpp:6:10
   3   	using namespace std;
   4   	
   5   	double sum (const vector<double> &data) {
-> 6   	  double total = 0;
   7   	  for (size_t i=0; i<data.size(); ++i) {
   8   	    total += data[i];
   9   	  }
```
{: data-highlight="1,7-8" }

### Step out
Enter `up` AKA "Step Out".  The `sum()` function completes, and the program pauses again.

```
(lldb) up
frame #1: 0x0000000100003eb8 main.exe`main at main.cpp:18:29
   15  	  data.push_back(10);
   (lldb) up
frame #1: 0x0000000100003eb8 main.exe`main at main.cpp:18:29
   15  	  data.push_back(10);
   16  	  data.push_back(20);
   17  	  data.push_back(30);
-> 18  	  cout << "sum(data) = " << sum(data) << endl;
   19  	}
```
{: data-highlight="1,9" }

### Continue
Enter `c` AKA "Continue" to run the program to the next breakpoint, or the end, whichever comes first.

```
(lldb) c
Process 72957 resuming
sum(data) = 60
Process 72957 exited with status = 0 (0x00000000)
```
{: data-highlight="1" }

### Quit
Quit LLDB.  **Pro-tip:** `Control-D` will quit.

```
(lldb) q
```
{: data-highlight="1" }

## Pro-tips
Use the up and down arrow keys to cycle through previous commands similar to your command line.

Use TAB completion to automatically complete the name of a command or a variable.

Use [Emacs](setup_emacs.html) keyboard shortcuts to enter and edit your current command.


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
