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


# Quick Start
If you're a first time user, skip this section.

Compile your program with the `-g` flag and start LLDB.  EECS 281 students: remove optimization flags like `-O3`.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ make stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
$ lldb stats_tests.exe
```

| `b main` | breakpoint on main function |
| `b 29` | breakpoint on line 29 of current file |
| `b stats.cpp:16` | breakpoint on line 16 of stats.cpp |
| `r` | run or rerun |
| `s` | step into |
| `n` | step over (AKA next) |
| `p myvar` | print variable `myvar` |
| `q` | quit |


# Prerequisites
At this point, you should already have a folder for your project ([instructions](setup.html#create-a-folder)).  Your folder location might be different.  You should have downloaded and unpacked the starter files already ([instructions](setup.html#download-and-unpack-starter-files)).
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


# Install
This tutorial focuses on LLDB for macOS.  Your versions might be different.  You should already have `g++` and `lldb` installed from the main [macOS tutorial](setup.html#macos).
```console
$ g++ --version
Apple clang version 12.0.0 (clang-1200.0.32.28)
$ lldb --version
lldb-1200.0.44.2
```


# Compile
LLDB debugs a compiled executable.  Compile the executable you plan to debug.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ make clean
rm -rvf *.exe *~ *.out *.dSYM *.stackdump
$ make stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
```

<div class="primer-spec-callout warning icon-warning" markdown="1">
**PITFALL:** LLDB debugging will be very hard to understand if there are no debugging symbols.  Double check the output of `make` and verify that you see `-g` being used in the commands.  The EECS 280 defaults include `-g`.
</div>

Optionally run your executable at the command line before debugging.  With EECS 280 project 1 starter files and functions stubs in `stats_tests.cpp`, we get an assertion failure. This is expected.
```console
$ ./stats_tests.exe
test_sum_small_data_set
Assertion failed: (false), function sum, file stats.cpp, line 16.
Abort trap: 6
```


# Run
LLDB is run like any other command line program, with its name (`lldb`). Add the name of your executable as an argument.  You now see the LLDB prompt.  LLDB's interface is similar to your command line shell, where you enter commands and press Enter (Return).
```console
$ lldb stats_tests.exe
(lldb) target create "stats_tests.exe"
Current executable set to '/Users/awdeorio/src/eecs280/p1-stats/stats_tests.exe' (x86_64).
(lldb)
```

The `r` command runs the program.
```
(lldb) r
Process 24226 launched: '/Users/awdeorio/src/eecs280/p1-stats-awdeorio/stats_tests.exe' (x86_64)
test_sum_small_data_set
Assertion failed: (false), function sum, file stats.cpp, line 16.
Process 24226 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = hit program assert
    frame #4: 0x000000010000629e stats_tests.exe`sum(v=<unavailable>) at stats.cpp:16:3
   13    }
   14    
   15    double sum(vector<double> v) {
-> 16      assert(false);
   17    }
   18    
   19    double mean(vector<double> v) {
Target 0: (stats_tests.exe) stopped.
```

This output tells us that the program failed, as well as the filename and line number (`stats.cpp`, line 16).  Use the back trace command (`bt`) to see the call stack at the time of failure.  You can ignore all the standard library functions, just pay attention to functions that you wrote.  This is a great way to get a quick look at the root cause of a segfault.
```
(lldb) bt
* thread #1, queue = 'com.apple.main-thread', stop reason = hit program assert
    frame #0: ...
    frame #1: ...
    frame #2: ...
    frame #3: ...
  * frame #4: 0x000000010000629e stats_tests.exe`sum(v=<unavailable>) at stats.cpp:16:3
    frame #5: 0x0000000100003a7c stats_tests.exe`test_sum_small_data_set() at stats_tests.cpp:45:3
    frame #6: 0x00000001000039b4 stats_tests.exe`main at stats_tests.cpp:30:3
    frame #7: ...
    frame #8: ...
```

Quit with `q`.  **Pro-tip:** `Control-D` will also quit at any time.
```
(lldb) q
Quitting LLDB will kill one or more processes. Do you really want to proceed: [Y/n] y
```


## Input redirection
Skip this subsection on your first time through the tutorial.  Without input redirection, you can type input into LLDB after running your program.
```
$ lldb main.exe
...
(lldb) r
Starting program: /vagrant/src/eecs280/p1-stats-awdeorio/main.exe
enter a filename
main_test_data.tsv
enter a column name
B
...
```

Run with input redirection.  Make sure to add the name of your input file (`main_test.in` in this example).
```
$ lldb main.exe
...
(lldb) process launch -i main_test.in
...
```

## Arguments and options
Skip this subsection for EECS 280 project 1.  You'll need it for project 2 and beyond.

*Arguments* and *options* are inputs to a program typed at the command line.  Arguments are often required.  Options (AKA *flags* or *switches*) start with a hyphen (`-`), and are typically optional.

**Arguments example** from project 2:  `resize.exe` is the name of the program, and the arguments are `horses.ppm`,  `horses_400x250.ppm`, `400`, and `250`.
```console
$ ./resize.exe horses.ppm horses_400x250.ppm 400 250
$ lldb resize.exe
(lldb) r horses.ppm horses_400x250.ppm 400 250
```

**Options example** from project 5:  `main.exe` is the name of the program.  `train_small.csv` and  `test_small.csv` are arguments.  `--debug` is an option.
```console
$ ./main.exe train_small.csv test_small.csv --debug
$ lldb main.exe
(lldb) r train_small.csv test_small.csv --debug
```


# Debug
Start LLDB with your executable.
```console
$ lldb stats_tests.exe
(lldb) target create "stats_tests.exe"
Current executable set to '/Users/awdeorio/src/eecs280/p1-stats/build/public/stats_tests.exe' (x86_64).
```

Set a breakpoint on the main function (`b main`) and run the program (`r`). LLDB pauses execution on the line where we set the breakpoint and waits for another command.
```
(lldb) b main
Breakpoint 1: where = stats_tests.exe`main + 15 at stats_tests.cpp:30:3, address = 0x00000001000039af
(lldb) r
Process 24576 launched: '/Users/awdeorio/src/eecs280/p1-stats-awdeorio/stats_tests.exe' (x86_64)
Process 24576 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 1.1
    frame #0: 0x00000001000039af stats_tests.exe`main at stats_tests.cpp:30:3
   27    
   28    int main()
   29    {
-> 30      test_sum_small_data_set();
   31      // Call your test functions here
   32    
   33      return 0;
Target 0: (stats_tests.exe) stopped.
```

**Pro-tip**: Other ways to set breakpoints:

| `b main` | breakpoint on main function |
| `b 29` | breakpoint on line 29 of current file |
| `b stats.cpp:16` | breakpoint on line 16 of stats.cpp |

Step over to the next line of code with `n`.  Our test fails because we haven't implemented `sum()` yet.
```
(lldb) n
test_sum_small_data_set
Assertion failed: (false), function sum, file stats.cpp, line 16.
Process 24576 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = hit program assert
    frame #4: 0x000000010000629e stats_tests.exe`sum(v=<unavailable>) at stats.cpp:16:3
   13    }
   14    
   15    double sum(vector<double> v) {
-> 16      assert(false);
   17    }
   18    
   19    double mean(vector<double> v) {
Target 0: (stats_tests.exe) stopped.
(lldb) 
```

Rerun the program.
```
(lldb) r
There is a running process, kill it and restart?: [Y/n] y
Process 24576 exited with status = 9 (0x00000009)
Process 24584 launched: '/Users/awdeorio/src/eecs280/p1-stats-awdeorio/stats_tests.exe' (x86_64)
Process 24584 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 1.1
    frame #0: 0x00000001000039af stats_tests.exe`main at stats_tests.cpp:30:3
   27    
   28    int main()
   29    {
-> 30      test_sum_small_data_set();
   31      // Call your test functions here
   32    
   33      return 0;
Target 0: (stats_tests.exe) stopped.
```

Step into the function with `s`.
```
(lldb) s
Process 24584 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = step in
    frame #0: 0x00000001000039cb stats_tests.exe`test_sum_small_data_set() at stats_tests.cpp:38:8
   35    
   36    void test_sum_small_data_set()
   37    {
-> 38      cout << "test_sum_small_data_set" << endl;
   39    
   40      vector<double> data;
   41      data.push_back(1);
Target 0: (stats_tests.exe) stopped.
```

Step over to the next line of code (`n`) a couple times until you're on this line of code.  **Pro-tip:** Hit Return to repeat your previous command.
```
(lldb) n
...
(lldb) n
Process 24608 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = step over
    frame #0: 0x0000000100003a69 stats_tests.exe`test_sum_small_data_set() at stats_tests.cpp:45:3
   42      data.push_back(2);
   43      data.push_back(3);
   44    
-> 45      assert(sum(data) == 6);
   46    
   47      cout << "PASS!" << endl;
   48    }
Target 0: (stats_tests.exe) stopped.
```

Print the value of a variable with `p`.
```
(lldb) p data
(std::vector<double, std::allocator<double> >) $1 = size=3 {
  [0] = 1
  [1] = 2
  [2] = 3
```

Quit LLDB.  **Pro-tip:** `Control-D` will quit. 
```
(lldb) q
Quitting LLDB will kill one or more processes. Do you really want to proceed: [Y/n] y
```


# Pro-tips
Use the up and down arrow keys to cycle through previous commands similar to your command line.

Use TAB completion to automatically complete the name of a command or a variable.

Use [Emacs](setup_emacs.html) keyboard shortcuts to enter and edit your current command.


# Next steps
[Return to the main set up tutorial.](setup.html#text-editor-and-debugger)


# Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
