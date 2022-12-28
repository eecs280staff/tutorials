---
layout: spec
title: Setup GDB
excludeFromSitemap: true
---

GDB Tutorial
============
{: .primer-spec-toc-ignore }

[GDB](https://www.gnu.org/software/gdb/) is a command line debugger.  It runs primarily on Linux.  On Windows, you can use GDB via WSL.  On macOS, GDB is [unreliable](https://sourceware.org/bugzilla/show_bug.cgi?id=24069), so we recommend using LLDB instead ([LLDB Tutorial](setup_lldb.html)).

GDB is harder to learn compared to most visual debuggers.  However, you might want to use GDB in situations like these:
- Zero setup compared to Visual Studio or VS Code.  Fast and easy debugging once you've learned.
- Integration with Vim and Emacs (used in EECS 280, optional)
- Command line only, work on a remote server without a GUI
- Support for threads (used in EECS 482 Operating Systems)
- Support for connecting to a remote target like an operating system that has crashed so badly you can't use the keyboard or monitor

GDB uses commands.  Here's a handy [reference card](http://users.ece.utexas.edu/~adnan/gdb-refcard.pdf).


# Quick Start
<div class="primer-spec-callout info icon-info" markdown="1">
If you're a first time user, skip this section.
</div>

Compile your program with the `-g` flag and start GDB in text user interface (TUI) mode.  EECS 281 students: remove optimization flags like `-O3`.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ make stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
$ gdb -tui stats_tests.exe
```

| `b main` | breakpoint on main function |
| `b 29` | breakpoint on line 29 of current file |
| `b stats.cpp:16` | breakpoint on line 16 of stats.cpp |
| `r` | run or rerun |
| `s` | step into |
| `n` | step over (AKA next) |
| `refresh` | refresh TUI view |
| `p myvar` | print variable `myvar` |
| `q` | quit |


# Prerequisites
This tutorial uses command line tools.  If you haven't installed CLI tools on your machine yet, follow one of these tutorials first.

| [macOS](setup_macos.html)| [Windows](setup_wsl.html) | [Linux](setup_wsl.html#install-cli-tools)

We presume you've created a folder for your project, downloaded and unpacked the starter files, and created any new files with appropriate function stubs.

```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ ls
Makefile     main_test.in           p1_library.cpp  stats.h
launch.json  main_test.out.correct  p1_library.h    stats_public_test.cpp
main.cpp     main_test_data.tsv     stats.cpp       stats_tests.cpp
```

You should have function stubs in `stats.h` and `main.cpp`.  If you simply want to download a copy with the stubs already in place, use these commands.
```console
$ wget --no-clobber https://eecs280staff.github.io/tutorials/stats.cpp -O stats.cpp
$ wget --no-clobber https://eecs280staff.github.io/tutorials/main.cpp -O main.cpp
```

If you're not sure how to do this, you might find one of our IDE tutorials helpful. Walk through the tutorial up through the "Create a project" section.

| [VS Code](setup_vscode.html) | [Visual Studio](setup_visualstudio.html) | [Xcode](setup_xcode.html) | [Emacs](setup_emacs.html) |



# Install
GDB works best on Linux, including WSL.  If you're running macOS, use [LLDB](setup_lldb.html) instead.  If you're on CAEN Linux, GDB and GCC are already installed.  Your versions might be different.
```console
$ sudo apt-get install g++ gdb
$ g++ --version
g++ (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
$ gdb --version
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04) 9.2
```


# Compile
GDB debugs a compiled executable.  Compile the executable you plan to debug.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ make clean
rm -rvf *.exe *~ *.out *.dSYM *.stackdump
$ make stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
```

<div class="primer-spec-callout warning icon-warning" markdown="1">
**PITFALL:** GDB debugging will be very hard to understand if there are no debugging symbols.  Double check the output of `make` and verify that you see `-g` being used in the commands.  The EECS 280 defaults include `-g`.
</div>

Optionally run your executable at the command line before debugging.  With EECS 280 project 1 starter files and functions stubs in `stats_tests.cpp`, we get an assertion failure.
```console
$ ./stats_tests.exe
test_sum_small_data_set
stats_tests.exe: stats.cpp:16: double sum(std::vector<double>): Assertion `false' failed.
Aborted (core dumped)
```


# Run
To run inside GDB, prefix your executable name with `gdb`.
```console
$ gdb stats_tests.exe
...
Reading symbols from stats_tests.exe...
(gdb)
```

The `r` command runs the program.  You can ignore any error about "raise.c: No such file or directory.".
```
(gdb) r
Starting program: /vagrant/src/eecs280/p1-stats-awdeorio/stats_tests.exe
test_sum_small_data_set
stats_tests.exe: stats.cpp:16: double sum(std::vector<double>): Assertion `false' failed.

Program received signal SIGABRT, Aborted.
```

Use the back trace command (`bt`) to see the stack at the time of failure.  You can ignore all the standard library functions.  Just pay attention to functions that you wrote.  This is a great way to get a quick look at the root cause of a segfault.
```
(gdb) bt
#0  ...
#1  ...
#2  ...
#3  ...
#4  0x0000555555559244 in sum (v=std::vector of length 3, capacity 3 = {...})
    at stats.cpp:16
#5  0x0000555555557a5c in test_sum_small_data_set () at stats_tests.cpp:47
#6  0x0000555555557996 in main () at stats_tests.cpp:30
```

Quit.  **Pro-tip:** `Control-D` will quit, and a second `Control-D` will confirm the `y or n` prompt.
```
(gdb) q
A debugging session is active.

  Inferior 1 [process 1090] will be killed.

Quit anyway? (y or n) y
```

## Input redirection
Skip this subsection on your first time through the tutorial.  You can use input redirection to avoid typing program input each time you run (for debugging) a program.

Without input redirection, you can type input into GDB after running your program.
```
$ gdb main.exe
...
(gdb) r
Starting program: /vagrant/src/eecs280/p1-stats-awdeorio/main.exe
enter a filename
main_test_data.tsv
enter a column name
B
...
```

With input redirection, use a run command like this `r < main_test.in`.
```
$ gdb main.exe
...
(gdb) r < main_test.in
```

## Arguments and options
Skip this subsection for EECS 280 project 1.  You'll need it for project 2 and beyond.

*Arguments* and *options* are inputs to a program typed at the command line.  Arguments are often required.  Options (AKA *flags* or *switches*) start with a hyphen (`-`), and are typically optional.

**Arguments example** from project 2:  `resize.exe` is the name of the program, and the arguments are `horses.ppm`,  `horses_400x250.ppm`, `400`, and `250`.
```console
$ ./resize.exe horses.ppm horses_400x250.ppm 400 250
$ gdb resize.exe
(gdb) r horses.ppm horses_400x250.ppm 400 250
```

**Options example** from project 5:  `main.exe` is the name of the program.  `train_small.csv` and  `test_small.csv` are arguments.  `--debug` is an option.
```console
$ ./main.exe train_small.csv test_small.csv --debug
$ gdb main.exe
(gdb) r train_small.csv test_small.csv --debug
```


# Debug
Start GDB with your executable.
```console
$ gdb stats_tests.exe
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04) 9.2
...
Reading symbols from stats_tests.exe...
(gdb)
```

Set a breakpoint on the main function (`b main`) and run the program (`r`).
```
(gdb) b main
Breakpoint 1 at 0x3989: file stats_tests.cpp, line 29.
(gdb) r
Starting program: /home/awdeorio/src/eecs280/p1-stats/stats_tests.exe

Breakpoint 1, main () at stats_tests.cpp:29
29  {
```

**Pro-tip**: `start` is a shortcut for `b main` followed by `r`.

**Pro-tip**: Other ways to set breakpoints:

| `b main` | breakpoint on main function |
| `b 29` | breakpoint on line 29 of current file |
| `b stats.cpp:16` | breakpoint on line 16 of stats.cpp |

Step over to the next line of code with `n`, do this twice.  Our test fails because we haven't implemented `sum()` yet.  You can ignore any error about "raise.c: No such file or directory.".
```
(gdb) n
30    test_sum_small_data_set();
(gdb) n
test_sum_small_data_set
stats_tests.exe: stats.cpp:16: double sum(std::vector<double>): Assertion `false' failed.

Program received signal SIGABRT, Aborted.
```

Rerun the program.
```
(gdb) r
The program being debugged has been started already.
Start it from the beginning? (y or n) y
Starting program: /home/awdeorio/src/eecs280/p1-stats/stats_tests.exe

Breakpoint 1, main () at stats_tests.cpp:29
29  {
```

Step over to the next line of code (`n`), then step into the function with `s`.
```
(gdb) n
30    test_sum_small_data_set();
(gdb) s
test_sum_small_data_set () at stats_tests.cpp:39
39  {
```

List a few lines of surrounding code with `l` (that's a lowercase `L`).
```
(gdb) l
34
35    return 0;
36  }
37
38  void test_sum_small_data_set()
39  {
40    cout << "test_sum_small_data_set\n";
41
42    vector<double> data;
43    data.push_back(1);
```

Step over to the next line of code (`n`) a couple times until you're on this line of code.  **Pro-tip:** just hit Enter to repeat your previous command.
```
(gdb) n
40    cout << "test_sum_small_data_set\n";
(gdb) n
test_sum_small_data_set
42    vector<double> data;
(gdb) n
43    data.push_back(1);
(gdb) n
44    data.push_back(2);
(gdb) n
45    data.push_back(3);
(gdb) n
47    assert(sum(data) == 6);
```

Print the value of a variable with `p`.  If you have trouble viewing the contents of the `vector` in the previous step, see the [Pretty Printing STL Containers with `gdb`](#pretty-printing-stl-containers-with-gdb) section.
```
(gdb) p data
$3 = std::vector of length 3, capacity 4 = {1, 2, 3}
```

Quit.  **Pro-tip:** `Control-D` will quit, and a second `Control-D` will confirm the `y or n` prompt.
```
(gdb) q
A debugging session is active.

  Inferior 1 [process 1090] will be killed.

Quit anyway? (y or n) y
```


# Debug with TUI mode
Start GDB in TUI mode with your executable and hit Enter a few times until you see the `(gdb)` prompt.  TUI mode shows your source code at the top with a GDB prompt at the bottom.  It uses the [ncurses](https://en.wikipedia.org/wiki/Ncurses) library for text-based user interfaces.
```
$ gdb -tui stats_tests.exe
┌──stats_tests.cpp─────────────────────────────────────────────────────────────┐
│   29          {                                                              │
│   30            test_sum_small_data_set();                                   │
│   31            test_sum_small_data_set();                                   │
│   32                                                                         │
│   33            // Call your test functions here                             │
│   34                                                                         │
│   35            return 0;                                                    │
│   36          }                                                              │
│   37                                                                         │
│   38          void test_sum_small_data_set()                                 │
│   39          {                                                              │
│   40            cout << "test_sum_small_data_set\n";                         │
│   41                                                                         │
└──────────────────────────────────────────────────────────────────────────────┘
exec No process In:                                                L??   PC: ??
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
--Type <RET> for more, q to quit, c to continue without paging--
Type "apropos word" to search for commands related to "word"...
Reading symbols from stats_tests.exe...
(gdb)
```

Set a breakpoint on the main function (`b main`) and run the program (`r`).
```
┌──stats_tests.cpp─────────────────────────────────────────────────────────────┐
│B+>29          {                                                              │
│   30            test_sum_small_data_set();                                   │
│   31            test_sum_small_data_set();                                   │
│   32                                                                         │
│   33            // Call your test functions here                             │
│   34                                                                         │
│   35            return 0;                                                    │
│   36          }                                                              │
│   37                                                                         │
│   38          void test_sum_small_data_set()                                 │
│   39          {                                                              │
│   40            cout << "test_sum_small_data_set\n";                         │
│   41                                                                         │
└──────────────────────────────────────────────────────────────────────────────┘
native process 1154 In: main                           L29   PC: 0x555555557989
Reading symbols from stats_tests.exe...
(gdb) b main
Breakpoint 1 at 0x3989: file stats_tests.cpp, line 29.
(gdb) r
Starting program: /vagrant/src/eecs280/p1-stats-awdeorio/stats_tests.exe

Breakpoint 1, main () at stats_tests.cpp:29
(gdb)
```

**Pro-tip**: `start` is a shortcut for `b main` followed by `r`.

**Pro-tip**: Other ways to set breakpoints:

| `b main` | breakpoint on main function |
| `b 29` | breakpoint on line 29 of current file |
| `b stats.cpp:16` | breakpoint on line 16 of stats.cpp |

Step over to the next line of code with `n` and notice the line cursor `>` advance.
```
┌──stats_tests.cpp─────────────────────────────────────────────────────────────┐
│B+ 29          {                                                              │
│  >30            test_sum_small_data_set();                                   │
│   31            test_sum_small_data_set();                                   │
│   32                                                                         │
│   33            // Call your test functions here                             │
│   34                                                                         │
│   35            return 0;                                                    │
│   36          }                                                              │
│   37                                                                         │
│   38          void test_sum_small_data_set()                                 │
│   39          {                                                              │
│   40            cout << "test_sum_small_data_set\n";                         │
│   41                                                                         │
└──────────────────────────────────────────────────────────────────────────────┘
native process 1161 In: main                           L30   PC: 0x555555557991
(gdb) b main
Breakpoint 1 at 0x3989: file stats_tests.cpp, line 29.
(gdb) r
Starting program: /vagrant/src/eecs280/p1-stats-awdeorio/stats_tests.exe

Breakpoint 1, main () at stats_tests.cpp:29
(gdb) n
(gdb)
```

Again, step over to the next line code with `n`.  Our test fails because we haven't implemented `sum()` yet.  You can ignore any error about "raise.c: No such file or directory.".
```
...
stats_tests.exe: stats.cpp:16: double sum(std::vector<double>): Assertion `false' failed.

Program received signal SIGABRT, Aborted.
```

Refresh the TUI interface and rerun the program.  Sometimes, a crash can cause random characters to appear in TUI mode, which is why we use `refresh`.
```
(gdb) refresh
(gdb) r
The program being debugged has been started already.
Start it from the beginning? (y or n) y
```

At this point, you should be back at the breakpoint on `main` that you set earlier.
```
┌──stats_tests.cpp─────────────────────────────────────────────────────────────┐
│B+>29          {                                                              │
│   30            test_sum_small_data_set();                                   │
│   31            test_sum_small_data_set();                                   │
│   32                                                                         │
│   33            // Call your test functions here                             │
│   34                                                                         │
│   35            return 0;                                                    │
│   36          }                                                              │
│   37                                                                         │
│   38          void test_sum_small_data_set()                                 │
│   39          {                                                              │
│   40            cout << "test_sum_small_data_set\n";                         │
│   41                                                                         │
└──────────────────────────────────────────────────────────────────────────────┘
native process 1209 In: main                           L29   PC: 0x555555557989
(gdb) refresh
(gdb) r
The program being debugged has been started already.
Start it from the beginning? (y or n) y
Starting program: /vagrant/src/eecs280/p1-stats-awdeorio/stats_tests.exe

Breakpoint 1, main () at stats_tests.cpp:29
(gdb)
```

Step over to the next line of code (`n`), then step into the function with `s`.
```
┌──stats_tests.cpp─────────────────────────────────────────────────────────────┐
│B+ 29          {                                                              │
│   30            test_sum_small_data_set();                                   │
│   31            test_sum_small_data_set();                                   │
│   32                                                                         │
│   33            // Call your test functions here                             │
│   34                                                                         │
│   35            return 0;                                                    │
│   36          }                                                              │
│   37                                                                         │
│   38          void test_sum_small_data_set()                                 │
│  >39          {                                                              │
│   40            cout << "test_sum_small_data_set\n";                         │
│   41                                                                         │
└──────────────────────────────────────────────────────────────────────────────┘
native process 1209 In: test_sum_small_data_set        L39   PC: 0x5555555579a2
Start it from the beginning? (y or n) y
Starting program: /vagrant/src/eecs280/p1-stats-awdeorio/stats_tests.exe

Breakpoint 1, main () at stats_tests.cpp:29
(gdb) n
(gdb) s
test_sum_small_data_set () at stats_tests.cpp:39
(gdb)
```

Step over to the next line of code (`n`) a couple times until you're on this line of code.  If the user interface ever starts to look messed up, just `refresh` it.  **Pro-tip:** just hit Enter to repeat your previous command.
```
┌──stats_tests.cpp─────────────────────────────────────────────────────────────┐
│   42            vector<double> data;                                         │
│   43            data.push_back(1);                                           │
│   44            data.push_back(2);                                           │
│   45            data.push_back(3);                                           │
│   46                                                                         │
│  >47            assert(sum(data) == 6);                                      │
│   48                                                                         │
│   49            cout << "PASS!\n";                                           │
│   50          }                                                              │
│   51                                                                         │
│   52          // Add the test function implementations here.                 │
│                                                                              │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
native process 1209 In: test_sum_small_data_set        L47   PC: 0x555555557a3d
(gdb) n
(gdb) n
(gdb) n
(gdb) n
(gdb) n
(gdb) n
(gdb) refresh
(gdb)
```

Print the value of a variable with `p`.  If you have trouble viewing the contents of the `vector` in the previous step, see the [Pretty Printing STL Containers with `gdb`](#pretty-printing-stl-containers-with-gdb) section.
```
(gdb) p data
$3 = std::vector of length 3, capacity 4 = {1, 2, 3}
```

Quit.  **Pro-tip:** `Control-D` will quit, and a second `Control-D` will confirm the `y or n` prompt.
```
(gdb) q
A debugging session is active.

  Inferior 1 [process 1090] will be killed.

Quit anyway? (y or n) y
```


# Pro-tips
Use the up arrow key to cycle through previously used commands.

Use TAB completion to automatically complete the name of a command or a variable.

Use Emacs keyboard shortcuts to enter and edit your current command.


# Pretty-printing STL Containers with `gdb`
Some installations of `gdb` don't ship with pretty printing support for STL containers like `vector`.

If you have this problem, you'll see something like this when you try to view the contents of a `vector` while debugging in GDB or VS Code (some VS Code configurations use GDB under the hood).

<img src="images/vscode140.png" width="480px" />


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


# Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
