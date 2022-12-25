---
layout: spec
title: Setup Make
sitemapOrder: 30
---

`make` and Makefiles
====================
{: .primer-spec-toc-ignore }

This tutorial will show you how to compile source code at the command line without typing long commands.  Instead, we'll write the commands in a file called `Makefile` and use shortcuts.


# Prerequisites
We're assuming that you already have a folder with starter source code in it, e.g., `p1-stats/`.  You've created new files and added function stubs to make the code base compile, e.g., `stats.cpp` and `main.cpp`.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ ls
Makefile      main_test.out.correct  p1_library.h  stats_public_test.cpp
main.cpp      main_test_data.tsv     stats.cpp     stats_tests.cpp
main_test.in  p1_library.cpp         stats.h
```


# Compile without `make`
Compile and run unit tests written by you (start test provided).  We expect a failure from our function stub.
```console
$ g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
$ ./stats_tests.exe
test_sum_small_data_set
Assertion failed: (false), function sum, file stats.cpp, line 16.
Abort trap: 6
```

Compile and run unit tests provided by the staff.  We expect a failure from our function stub.
```console
$ g++ -Wall -Werror -pedantic -g --std=c++11 stats_public_test.cpp stats.cpp p1_library.cpp -o stats_public_test.exe
$ ./stats_public_test.exe
Assertion failed: (false), function count, file stats.cpp, line 12.
countAbort trap: 6
```

Compile and run main.
```console
$ g++ -Wall -Werror -pedantic --std=c++11 -g main.cpp stats.cpp p1_library.cpp -o main.exe
$ ./main.exe
hello from main!
```


# Compile with `make`
Remove all automatically generated files
```console
$ make clean
rm -rvf *.exe *~ *.out *.dSYM *.stackdump
removed 'main.exe'
removed 'stats_tests.exe'
removed 'stats_public_test.exe'
```

The `make` command reads a file called `Makefile`, which is a plain text file.  Open it in your IDE or text editor and you'll see the exact command it executed to do the cleanup.
```make
# This appears in the middle of the Makefile
clean :
	rm -rvf *.exe *~ *.out *.dSYM *.stackdump
```

Without `make`, we had to type a long command to compile our unit tests:
```console
$ g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
```

Notice that the `Makefile` contains this exact command.  The *target* is `stats_tests.exe`, on the left side of the colon (`:`).
```make
# This appears in the middle of the Makefile
stats_tests.exe: stats_tests.cpp stats.cpp p1_library.cpp
	g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
```

With `make`, we can run the command saved in the `Makefile`.
```console
$ make stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++11 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
```

This produces an executable, which we can now run.  A failure is expected due to the function stub.
```console
$ ./stats_tests.exe
test_sum_small_data_set
Assertion failed: (false), function sum, file stats.cpp, line 16.
Abort trap: 6
```


# Run one unit test 
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


# Run one system test
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


# Run regression test
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


# Pro-tips
Speed up compilation by running multiple commands simultaneously.
```console
$ make -j4 test
```
{: data-variant="no-line-numbers" }

# Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
