---
layout: spec
title: Setup Style Checking
excludeFromSitemap: true
---

Style Checking
==============
{: .primer-spec-toc-ignore }

This tutorial will show you how to run style checking tools on your source code.


## Style criteria
First, take a look at the [EECS 280 C++ style guide](style_guide.html).

We will use several tools to check the style of your code.

`oclint` checks
- Lines are no longer than 90 characters.
- Functions are short.
- Code is not too deeply nested.
- Functions do not have too many parameters. (Structs or classes should be used for functions that require more inputs).

`cpd` checks for significant code duplication.

## Prerequisites
You can connect to CAEN Linux, where the tools are installed.  See the [CAEN Linux tutorial](setup_caen.html) for help.
```console
$ ssh awdeorio@login.engin.umich.edu
...
Success. Logging you in...
```

You have some source code you want to check.  Our example contains a long line.
```c++
int main() {
#include <iostream>
using namespace std;


int main() {
  cout << "hello world this is a veryyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy long line" << endl;
}
```
{: data-title="test.cpp" data-highlight="7" }

Make sure the code compiles before running style checks.  If you have a Makefile, you could just type `make`.
```console
$ g++ test.cpp -o test
```
{: data-variant="no-line-numbers" }

<div class="primer-spec-callout danger icon-danger" markdown="1">
We prohibit the phrase `oclint`, regardless of capitalization, from appearing in your source files, as it can be used to turn off `oclint` checks.
</div>


## Check style manually
Run `oclint` on `test.cpp`.  You'll need to load a module on CAEN, which adds a tool that isn't available in the standard environment.  There's a long line in our example, which results in error output.
```console
$ hostname
caen-vnc-vm16.engin.umich.edu
$ module load oclint
$ oclint \
    -no-analytics \
    -rule=LongLine \
    -rule=HighNcssMethod \
    -rule=DeepNestedBlock \
    -rule=TooManyParameters \
    -rc=LONG_LINE=90 \
    -rc=NCSS_METHOD=40 \
    -rc=NESTED_BLOCK_DEPTH=4 \
    -rc=TOO_MANY_PARAMETERS=4 \
    -max-priority-1 0 \
    -max-priority-2 0 \
    -max-priority-3 0 \
    test.cpp \
    -- -xc++ --std=c++11

OCLint Report

Summary: TotalFiles=1 FilesWithViolations=1 P1=0 P2=0 P3=1

/home/awdeorio/test.cpp:6:1: long line [size|P3] Line with 104 characters exceeds limit of 90

[OCLint (http://oclint.org) v0.13]

oclint: error: violations exceed threshold
P1=0[0] P2=0[0] P3=1[0]
```
{: data-highlight="24" }

Run `cpd` on `test.cpp`.  You'll need to load the `pmd` module, which contains `cpd`.  There isn't any duplicate code in our example, so there is no error output.
```console
$ hostname
caen-vnc-vm16.engin.umich.edu
$ module load pmd
$ cpd \
    --minimum-tokens 100 \
    --language cpp \
    --failOnViolation true \
    --files test.cpp
Added /afs/umich.edu/user/a/w/awdeorio/test.cpp
```


## Check style automatically
You can run the style checks using the Makefile on an EECS 280 project.

### Copy code to CAEN Linux
We're going to synchronize our code to CAEN Linux many times.  These shortcuts will let you synchronize quickly and easily.

Be sure that you can [avoid repeated two-factor authentication (2FA)](setup_caen.html#avoiding-repeated-2fa).  In a separate terminal, connect to CAEN Linux with SSH so you won't have to enter your password again.
```console
$ ssh awdeorio@login.engin.umich.edu
Password:
Duo two-factor login for awdeorio

Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-3142
 2. Phone call to XXX-XXX-3142
 3. SMS passcodes to XXX-XXX-3142

Passcode or option (1-3): 1
Success. Logging you in...

$
```

If you didn't already, [add a `make sync` shortcut](setup_caen.html#make-sync-shortcut) to your `Makefile`.

Clean up (from your laptop).
```console
$ hostname
manzana.local
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ make clean
```

Copy (from your laptop).
```console
$ hostname
manzana.local
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ make sync
rsync -rtv --delete --exclude '.git*' --filter=':- .gitignore' ./  awdeorio@login.engin.umich.edu:p1-stats-copy/
```

### Connect to CAEN Linux
Connect to CAEN Linux via `ssh`.  If you're already connected, you don't need to connect again.
```console
$ hostname
manzana.local
$ ssh awdeorio@login.engin.umich.edu
Success. Logging you in...
...
$ hostname
caen-vnc-vm16.engin.umich.edu
```

Change directory to the folder containing your source code.  In this tutorial, we're going to use the copy created by `rsync` via `make sync`.  Note that you might also have a `p1-stats` directory created during the [Version control on CAEN Linux](setup_caen.html#version-control-on-caen-linux) tutorial.
``` console
$ pwd
/home/awdeorio
$ ls
p1-stats p1-stats-copy
$ cd p1-stats-copy
$ pwd
/home/awdeorio/p1-stats-copy
```

### Run style checks
We've provided a `make style` Makefile target to run all the style checks for you.
```console
$ hostname
caen-vnc-vm16.engin.umich.edu
$ pwd
/home/awdeorio/p1-stats-copy
$ make style
...
EECS 280 style checks PASS
```


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
