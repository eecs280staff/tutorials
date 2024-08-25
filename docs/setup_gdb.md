---
layout: spec
title: Setup GDB
excludeFromSitemap: true
---

GDB Tutorial
============
{: .primer-spec-toc-ignore }

[GDB](https://www.gnu.org/software/gdb/) is a command line debugger.  It is a good choice on Linux or WSL.  On macOS, use LLDB instead ([LLDB Tutorial](setup_lldb.html)).

GDB is harder to learn compared to most visual debuggers.  However, you might want to use GDB in situations like these:
- Zero setup compared to Visual Studio or VS Code.  Fast and easy debugging once you've learned.
- Command line only, work on a remote server without a GUI
- Support for threads
- Support for connecting to a remote target like an operating system that has crashed so badly you can't use the keyboard or monitor
- Integration with Vim and Emacs

GDB uses commands.  Here's a handy [reference card](http://users.ece.utexas.edu/~adnan/gdb-refcard.pdf).


## Quick Start
Compile your program with the `-g` flag and start GDB in text user interface (TUI) mode.  EECS 280 project Makefiles include `-g` by default.
```console
$ g++ -g --std=c++17 main.cpp -o main.exe
$ make main.exe  # If you have a Makefile
$ gdb -tui main.exe
```

| `b main` | breakpoint on main function |
| `b 13` | breakpoint on line 13 of current file |
| `b main.cpp:13` | breakpoint on line 13 of main.cpp |
| `r` | run or rerun |
| `l` | list a few lines of code |
| `n` | step over (next) |
| `s` | step into |
| `up` | step out (up) |
| `c` | continue |
| `p myvar` | print variable `myvar` |
| `refresh` | refresh TUI view |
| `bt` | backtrace, useful for segfaults |
| `q` | quit |


## Install
You should already have `g++` and `gdb` installed from the [WSL tutorial](setup_wsl.html).  Your versions might be different.
```console
$ g++ --version
g++ (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
$ gdb --version
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04) 9.2
```

## Compile and Run
GDB uses an executable you build at the command line.

First, compile and run your executable at the command line.
```console
$ g++ -g --std=c++17 main.cpp -o main.exe
$ make main.exe  # If you have a Makefile
$ ./main.exe
Hello World!
```

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** GDB debugging will be very hard to understand if there are no debugging symbols.  If you're using a Makefile, double check the output of `make` and verify that you see `-g`.
</div>

Run with GDB.  You now see the GDB prompt.  GDB's interface is similar to your command line shell, where you enter commands and press Enter (Return).
```
$ gdb -tui main.exe
Reading symbols from main.exe...
(gdb)
```

The `r` command runs the program.  You can ignore any warnings about "separate debuginfos".
```
(gdb) r
Starting program: /n/higgins/z/awdeorio/gdbeg/main.exe
Hello World!
[Inferior 1 (process 95933) exited normally]
```

Quit with `q`.
```
(gdb) q
```

### Sanitizers
We recommend enabling the address sanitizer and undefined behavior sanitizer. These will help you find memory errors like going off the end of an array or vector.

First, edit your `Makefile` and add the `CXXFLAGS` recommended by the [ASAN Quick Start](setup_asan.html#quick-start).

### Command-Line Arguments
Inputs to a program may be provided when it is initailly run via command-line arguments. Here's an example from EECS 280 Project 1:

```console
$ ./two_sample.exe HCMST_ver_3.04.tsv q24_met_online 1 0 ppage
```

- `./two_sample.exe` is used to run the program
- Each of `HCMST_ver_3.04.tsv`, `q24_met_online`, `1`, `0`, `ppage` are passed to it as arguments

The arguments above specify the name of a data file, coulumns, and filter values for the program to use.

To run a program with options or arguments in GDB, include them after `r`.
```console
$ gdb main.exe
(gdb) r HCMST_ver_3.04.tsv q24_met_online 1 0 ppage
```
{: data-highlight="2" }

### Input redirection
If you're unfamiliar with input redirection, first read the CLI tutorial section on [input redirection](cli.html#input-redirection-).

Run with input redirection.  Make sure to add the name of your input file (`main_test.in` in this example).
```
$ gdb main.exe
...
(gdb) r < main_test.in
...
```

## Debug
In this section, we'll set a breakpoint, which pauses the debugger.  Then, we'll cover some of the options to continue execution.

`n` **Next / Step Over:**
Run one line of code, stepping _over_ any function calls by running the whole function in one step.

`s` **Step / Step Into:**
Run one line of code, stepping _into_ any function calls to execute them line-by-line.

`up` **Up / Step Out:**
Run the program until it returns from the current function (or until the next breakpoint).

`c` **Continue:**
Run the program until the next breakpoint.

`q` **Quit:**
Quit GDB.

### Example code

To get started, copy this example `main.cpp` into your editor.
```c++
#include <iostream>
#include <vector>
using namespace std;

double sum (const vector<double> &data) {
  double total = 0;
  for (size_t i = 0; i < data.size(); ++i) {
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

### Start GDB
Start GDB.  The `-tui` option makes it easier to see your code (optional).

```console
$ gdb -tui main.exe
```

```
   ┌──main.cpp─────────────────────────────────────────────────────────────────┐
   │13      int main() {                                                       │
   │14        vector<double> data;                                             │
   │15        data.push_back(10);                                              │
   │16        data.push_back(20);                                              │
   │17        data.push_back(30);                                              │
   │18        cout << "sum(data) = " << sum(data) << endl;                     │
   │19      }                                                                  │
   │20                                                                         │
   │21                                                                         │
   │22                                                                         │
   │23                                                                         │
   │24                                                                         │
   │25                                                                         │
   └───────────────────────────────────────────────────────────────────────────┘
exec No process In:                                                L??   PC: ?? 

(gdb) 
```
{: data-variant="no-line-numbers" }

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** If the user interface looks messed up, try
```
(gdb) refresh
```
</div>

### Breakpoint
Set a breakpoint on the main function.

```
   ┌──main.cpp─────────────────────────────────────────────────────────────────┐
   │13      int main() {                                                       │
b+ │14        vector<double> data;                                             │
   │15        data.push_back(10);                                              │
   │16        data.push_back(20);                                              │
   │17        data.push_back(30);                                              │
   │18        cout << "sum(data) = " << sum(data) << endl;                     │
   │19      }                                                                  │
   │20                                                                         │
   │21                                                                         │
   │22                                                                         │
   │23                                                                         │
   │24                                                                         │
   │25                                                                         │
   └───────────────────────────────────────────────────────────────────────────┘
exec No process In:                                                L??   PC: ?? 

(gdb) b	main
Breakpoint 1 at 0x400c66: file main.cpp, line 14.
```
{: data-highlight="3,18" }

<div class="primer-spec-callout info" markdown="1">
**Pro-tip**: There are several ways to set breakpoints.

| `b main` | breakpoint on main function |
| `b 13` | breakpoint on line 13 of current file |
| `b main.cpp:13` | breakpoint on line 13 of main.cpp |

</div>

### Run
Run the program being debugged.  The program pauses at the breakpoint.  Ignore any error about "Missing separate debuginfos".

```
   ┌──main.cpp─────────────────────────────────────────────────────────────────┐
   │13      int main() {                                                       │
B+>│14        vector<double> data;                                             │
   │15        data.push_back(10);                                              │
   │16        data.push_back(20);                                              │
   │17        data.push_back(30);                                              │
   │18        cout << "sum(data) = " << sum(data) << endl;                     │
   │19      }                                                                  │
   │20                                                                         │
   │21                                                                         │
   │22                                                                         │
   │23                                                                         │
   │24                                                                         │
   │25                                                                         │
   └───────────────────────────────────────────────────────────────────────────┘
native process 95465 In: main                                L14   PC: 0x400c66 
Breakpoint 1 at 0x400c66: file main.cpp, line 14.
(gdb) r
Starting program: /n/higgins/z/awdeorio/gdbeg/main.exe
Breakpoint 1, main () at main.cpp:14
```
{: data-highlight="3,18" }

<div class="primer-spec-callout info" markdown="1">
**Pro-tip**: `start` is a shortcut for `b main` followed by `r`.
```
(gdb) start
```
</div>

### List
List a few lines of surrounding code with `l` (that's a lowercase `L`).  This command is only useful when you're not using TUI mode.
```
(gdb) l
```

### Step over
Enter `n` (Next / Step Over) a few times until you reach the highlighted line of code.

```
(gdb) n
   ┌──main.cpp─────────────────────────────────────────────────────────────────┐
   │10        return total;                                                    │
   │11      }                                                                  │
   │12                                                                         │
   │13      int main() {                                                       │
B+ │14        vector<double> data;                                             │
   │15        data.push_back(10);                                              │
   │16        data.push_back(20);                                              │
   │17        data.push_back(30);                                              │
  >│18        cout << "sum(data) = " << sum(data) << endl;                     │
   │19      }                                                                  │
   │20                                                                         │
   │21                                                                         │
   │22                                                                         │
   └───────────────────────────────────────────────────────────────────────────┘
native process 95465 In: main                                L18   PC: 0x400cd2 

(gdb) n
(gdb) n
(gdb) n
(gdb) n
```
{: data-highlight="11,19-22" }

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** Hit <kbd>Enter</kbd> to repeat your previous command.
</div>

### Inspect
Print the value of a variable with `p`.  If you have trouble viewing the contents of a `vector`, see the [Pretty Printing STL Containers with `gdb`](#pretty-printing-stl-containers-with-gdb) section.  (Here's an explanation of why the [capacity](https://cplusplus.com/reference/vector/vector/capacity/) is 4 when the length is 3.)

```
(gdb) p data
$1 = std::vector of length 3, capacity 4 = {10,	20, 30}
```
{: data-highlight="1" }

### Step into
Enter `s` (Step / Step Into).  The cursor enters the `sum()` function.

```
   ┌──main.cpp─────────────────────────────────────────────────────────────────┐
   │1       #include <iostream>                                                │
   │2       #include <vector>                                                  │
   │3       using namespace std;                                               │
   │4                                                                          │
   │5       double sum (const vector<double> &data) {                          │
  >│6         double total = 0;                                                │
   │7         for (size_t i = 0; i < data.size(); ++i) {                       │
   │8           total += data[i];                                              │
   │9         }                                                                │
   │10        return total;                                                    │
   │11      }                                                                  │
   │12                                                                         │
   │13      int main() {                                                       │
   └───────────────────────────────────────────────────────────────────────────┘
native process 95465 In: sum                                 L6    PC: 0x400c02 

(gdb) s
```
{: data-highlight="7,18" }

### Step out
Enter `up` (Step Out).  The `sum()` function completes, and the program pauses again.

```
   ┌──main.cpp─────────────────────────────────────────────────────────────────┐
   │13      int main() {                                                       │
B+ │14        vector<double> data;                                             │
   │15        data.push_back(10);                                              │
   │16        data.push_back(20);                                              │
   │17        data.push_back(30);                                              │
  >│18        cout << "sum(data) = " << sum(data) << endl;                     │
   │19      }                                                                  │
   │20                                                                         │
   │21                                                                         │
   │22                                                                         │
   │23                                                                         │
   │24                                                                         │
   │25                                                                         │
   └───────────────────────────────────────────────────────────────────────────┘
native process 95465 In: main                                L18   PC: 0x400cf0 

(gdb) up
```
{: data-highlight="7,18" }

### Continue
Enter `c` (Continue) to run the program to the next breakpoint, or the end, whichever comes first.

```
(gdb) c
Continuing.
sum(data) = 60
[Inferior 1 (process 95465) exited normally]
```
{: data-highlight="1" }

### Quit
Quit GDB.  **Pro-tip:** `Control-D` will quit.

```
(gdb) q
```
{: data-highlight="1" }


## Pro-tips
Use the up arrow key to cycle through previously used commands.

Use <kbd>TAB</kbd> completion to automatically complete the name of a command or a variable.

Use [Emacs](setup_emacs.html) keyboard shortcuts to enter and edit your current command.


## Pretty-printing STL Containers with `gdb`
Some installations of `gdb` don't ship with pretty printing support for STL containers like `vector`.

If you have this problem, you'll see something like this when you try to view the contents of a `vector` while debugging in GDB or VS Code (some VS Code configurations use GDB under the hood).

<img src="images/vscode_wsl_140.png" width="480px" />


Install Subversion and Python.

Get the GDB STL pretty printers. These are written in Python.
```console
$ mkdir ~/.gdb.d/
$ svn co svn://gcc.gnu.org/svn/gcc/trunk/libstdc++-v3/python ~/.gdb.d/python
```

Use this command to create a `.gdbinit` file in your home directory.
```console
$ echo -e 'python\nimport os\nimport sys\nsys.path.insert(0, os.path.expanduser("~/.gdb.d/python"))\nfrom libstdcxx.v6.printers import register_libstdcxx_printers\nregister_libstdcxx_printers (None)\nend' > ~/.gdbinit
```
{: data-variant="no-line-numbers" }

Double check everything:
```console
$ cat ~/.gdbinit
python
import os
import sys
sys.path.insert(0, os.path.expanduser("~/.gdb.d/python"))
from libstdcxx.v6.printers import register_libstdcxx_printers
register_libstdcxx_printers (None)
end
$ ls -R ~/.gdb.d/
/Users/awdeorio/.gdb.d/:
python

/Users/awdeorio/.gdb.d/python:
Makefile.am  Makefile.in  hook.in  libstdcxx

/Users/awdeorio/.gdb.d/python/libstdcxx:
__init__.py  v6

/Users/awdeorio/.gdb.d/python/libstdcxx/v6:
__init__.py  printers.py  xmethods.py
```

Finally, restart GDB or VS Code and try to check the contents of vector
again.


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
