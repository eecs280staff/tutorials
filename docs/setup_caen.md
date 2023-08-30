---
layout: spec
title: Setup CAEN
sitemapOrder: 40
---

CAEN Linux
==========
{: .primer-spec-toc-ignore }

This tutorial will show you how to copy source code from your Laptop to CAEN Linux.  CAEN Linux is a server in the University data center that runs the Linux operating system.  It's useful for making sure that your code works on a computer that is a lot like the autograder.

<img src="images/caen005.excalidraw.png" width="768px" class="invert-colors-in-dark-mode" />

Why use CAEN Linux?  One reason is that it has the same compiler used by the autograder, which is useful for making sure you'll get the same results as on your laptop.  For example, on an Apple laptop, `g++` is actually an alias for the Clang compiler supplied by Apple.  Similar things can happen on Windows.
```console
$ g++ --version
Apple clang version 12.0.0 (clang-1200.0.32.29)
```

After logging in to CAEN Linux, you can use the same compiler used by the autograder.
```console
$ g++
g++ (GCC) 4.8.5 20150623 (Red Hat 4.8.5-44)
```

## Prerequisites
We're assuming that you already have a folder with starter source code in it, e.g., `p1-stats/`.  You've created new files and added function stubs to make the code base compile, e.g., `stats.cpp` and `main.cpp`.  We expect that your unit tests will fail because many functions are stubs.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ ls
Makefile      main_test.out.correct  p1_library.h  stats_public_test.cpp
main.cpp      main_test_data.tsv     stats.cpp     stats_tests.cpp
main_test.in  p1_library.cpp         stats.h
$ make clean
rm -rvf *.exe *~ *.out *.dSYM *.stackdump
$ make test
g++ -Wall -Werror -pedantic -g --std=c++17 main.cpp stats.cpp p1_library.cpp -o main.exe
g++ -Wall -Werror -pedantic -g --std=c++17 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++17 stats_public_test.cpp stats.cpp p1_library.cpp -o stats_public_test.exe
./stats_public_test.exe
Assertion failed: (false), function count, file stats.cpp, line 12.
countmake: *** [test] Abort trap: 6
```

You have installed `ssh` and `rsync`.  Your versions might be different.
```console
$ ssh -V
OpenSSH_7.4p1, LibreSSL 2.5.0
$ rsync --version
rsync  version 2.6.9  protocol version 29
```

## Test log in
CAEN is the information technology (IT) services department for the University of Michigan (U-M) College of Engineering.  Everyone who registers for an EECS class (like EECS 280) should receive a CAEN account automatically by the first day class.  If you register after the first day of class, you should get your account within 24 hours of registration.

You'll need a two factor authentication app set up on your mobile device.  Make sure that you have the Duo Mobile app installed and configured according the [ITCS documentation](http://documentation.its.umich.edu/2fa/enroll-smartphone-or-tablet-duo).

<img src="images/caen010.png" width="480px" />

Test if you have access to CAEN Linux computers.  Be sure to change `awdeorio` to your own uniqname.  If this is the first time you're logging in, you'll need to confirm that you want to continue connecting.

```console
$ ssh awdeorio@login.engin.umich.edu
The authenticity of host 'login.engin.umich.edu (141.213.74.65)' can't be established.
ECDSA key fingerprint is SHA256:LL0GPTtaVGa6gvv2kVpGq4ZULA1l5pw2wXC4dK3ymIk.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'login.engin.umich.edu,141.213.74.65' (ECDSA) to the list of known hosts.
Password: 
Duo two-factor login for awdeorio

Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-3142
 2. Phone call to XXX-XXX-3142
 3. SMS passcodes to XXX-XXX-3142

Passcode or option (1-3): 1
Success. Logging you in...
...
-bash-4.2$ 
```

<div class="primer-spec-callout info icon-info" markdown="1">
If you see an error like `Could not chdir to home directory /home/awdeorio: No such file or directory`, a common reason is that you don't have a home directory set up yet.  Request one at the [IFS Storage Request page](https://ifsprovisioning.its.umich.edu/ifs_storage/request). Note that it may take a few hours before your home directory is available across all CAEN systems. If you already have one, you should see "You already have IFS Storage!".

If you're still having trouble accessing your account, see the [CAEN Help Desk](https://caen.engin.umich.edu/contact/).
</div>

Exit as soon as your test is successful.
```console
$ exit
Connection to login.engin.umich.edu closed.
```


## Copy files with `rsync`
Next, we will copy our source code to CAEN Linux using the `rsync` command line program.

We don't want to copy any compiled binary files, so clean up first.
```console
$ make clean
rm -rvf *.exe *~ *.out *.dSYM *.stackdump
removed 'main.exe'
removed 'stats_tests.exe'
removed 'stats_public_test.exe'
```

Next, copy files using `rsync`.  Remember to change `awdeorio` to your username.
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ rsync -rtv --exclude '.git*' ../p1-stats/ awdeorio@login.engin.umich.edu:p1-stats-copy/
building file list ... done
created directory p1-stats-copy
./
Makefile
main.cpp
main_test.in
main_test.out.correct
main_test_data.tsv
p1_library.cpp
p1_library.h
stats.cpp
stats.h
stats_public_test.cpp
stats_tests.cpp

sent 9557 bytes  received 268 bytes  19650.00 bytes/sec
total size is 8818  speedup is 0.90
```

## Connect with `ssh`
Now connect to CAEN Linux.  We're going to get a shell on a remote computer.  Don't forget to change `awdeorio` to your own uniqname.
```console
$ pwd                                  # folder on awdeorio's laptop
/Users/awdeorio/src/eecs280/p1-stats
$ hostname                             # name of awdeorio's laptop
manzana.local
$ ssh awdeorio@login.engin.umich.edu   # connect to CAEN
$ pwd                                  # folder on CAEN computer
/home/awdeorio
$ hostname                             # name of a CAEN computer
caen-vnc-vm16.engin.umich.edu
```

Notice that the folder we copied is there on the CAEN Linux machine.
```console
$ ls
p1-stats-copy
```

Change directory into the copied folder and double-check that all binary files are cleaned up.
```console
$ cd p1-stats-copy
$ make clean
```

Compile and run main, just like we did before using the shell on our local machine.
```console
$ make main.exe
$ ./main.exe
hello from main!
```

Run the regression test.  It fails on the assertion we added, just like it did on our local machine.
```console
$ make test
g++ -Wall -Werror -pedantic -g --std=c++17 main.cpp stats.cpp p1_library.cpp -o main.exe
g++ -Wall -Werror -pedantic -g --std=c++17 stats_tests.cpp stats.cpp p1_library.cpp -o stats_tests.exe
g++ -Wall -Werror -pedantic -g --std=c++17 stats_public_test.cpp stats.cpp p1_library.cpp -o stats_public_test.exe
./stats_public_test.exe
stats_public_test.exe: stats.cpp:12: int count(std::vector<double>): Assertion `false' failed.
make: *** [test] Aborted
```

Log out.
```console
$ hostname
caen-vnc-vm16.engin.umich.edu
$ exit
$ hostname
manzana.local
```


## Avoiding repeated 2FA
You've noticed that each time we use `ssh` or `rsync`, CAEN requires us to re-authenticate with 2FA (two factor authentication, using our phone).  You can configure SSH to share one connection and only authenticate once.  Note that `rsync` uses SSH under the hood.

Add some lines to the SSH config file, which lives in `~/.ssh/config`.  Alternatively, you can use a text editor to make the changes.
```console
$ echo -e '# SSH multiplexing\nHost *\n  ControlMaster auto\n  ControlPersist yes\n   ControlPath ~/.ssh/socket-%C\n  ServerAliveInterval 60\n  ServerAliveCountMax 5' >> ~/.ssh/config
$ chmod 600 ~/.ssh/config
```

Let's double-check and make sure you see this chunk in your `~/.ssh/config` file.
```console
$ cat ~/.ssh/config
# SSH multiplexing
Host *
  ControlMaster auto
  ControlPersist yes
  ControlPath ~/.ssh/socket-%C
  ServerAliveInterval 60
  ServerAliveCountMax 5
```

SSH into CAEN Linux.  You'll need to use 2FA.
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

Now, open a second terminal.  We're going to use SSH again, this time via `rsync`.  Notice that no authentication is required.  Cool!
```console
$ pwd
/Users/awdeorio/src/eecs280/p1-stats
$ rsync -rtv --exclude '.git*' ../p1-stats/ awdeorio@login.engin.umich.edu:p1-stats-copy/
building file list ... done

sent 273 bytes  received 20 bytes  586.00 bytes/sec
total size is 13015  speedup is 44.42
```

## Version control on CAEN Linux
If you're working on EECS 280 Project 1, you can skip this section.

We can also check out a copy of our committed code on CAEN Linux.

SSH to a CAEN Linux machine and see the copy we made earlier using `rsync`.
```console
$ ssh awdeorio@login.engin.umich.edu
$ ls
p1-stats-copy  # this is from our rsync'ed copy earlier
```

Notice that the copy is *not* under version control.
```console
$ cd p1-stats-copy
$ git status
fatal: Not a git repository (or any of the parent directories): .git
```

Change directory and `git clone` your repo.
```console
$ cd ~  # This will move to your home directory
$ git clone https://github.com/awdeorio/p1-stats.git
$ ls
p1-stats p1-stats-copy
```

## Pro-tips

### Synchronizing deleted files
Tell `rsync` to synchronize deleted files.  In other words, if it's gone on your laptop, delete it on CAEN.
```console
$ rsync -rtv --delete --exclude '.git*' ../p1-stats/ awdeorio@login.engin.umich.edu:p1-stats-copy/
```
{: data-variant="no-line-numbers" }

### Don't synchronize Git-ignored files
Tell `rsync` not to synchronize files ignored by Git.  You can also combine this option with `--delete`.
```console
$ rsync -rtv --exclude '.git*' --filter=':- .gitignore' ../p1-stats/ awdeorio@login.engin.umich.edu:p1-stats-copy/
```
{: data-variant="no-line-numbers" }

### `make sync` shortcut
Avoid typing a long `rsync` command by adding a Makefile target.  Add these lines to the end of your `Makefile`.  Notice that's a TAB character before `rsync`.
```make
# Copy files to CAEN Linux
sync :
	rsync \
  -rtv \
  --delete \
  --exclude '.git*' \
  --filter=':- .gitignore' \
  ../p1-stats/ \
  awdeorio@login.engin.umich.edu:p1-stats-copy/
```

Now you can type `make sync` as a short cut.
```console
$ make sync
rsync \
  -rtv \
  --delete \
  --exclude '.git*' \
  --filter=':- .gitignore' \
  ../p1-stats/ \
  awdeorio@login.engin.umich.edu:p1-stats-copy/
building file list ... done
./
Makefile

sent 446 bytes  received 60 bytes  1012.00 bytes/sec
total size is 8939  speedup is 17.67
```

<div class="primer-spec-callout warning icon-warning" markdown="1">
**PITFALL** Some IDEs (Xcode, for example) will automatically insert several spaces instead of a TAB.  Makefiles *must* have a TAB just before a command (`rsync` in this case).  On macOS, Option-Tab will insert a literal TAB.  Here's an example of the problem:
```console
$ make sync
make: Nothing to be done for `sync'.
```
</div>


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
