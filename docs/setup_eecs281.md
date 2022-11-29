---
layout: spec
title: EECS 281 Project Setup
---


EECS 281 Project Setup
======================

This tutorial will help you set up an EECS 281 project using the EECS 280 tutorials.

## TODO
- [ ] Code goes in a subdirectory with the same name
      ```
      p0-hello/p0-hello/main.cpp
      ```
- [ ] Configure visual debugger for CLI args and opts

## OS and tool installs
FIXME: if you have a new computer or you wiped your computer, install stuff.

## Project folder
Create a folder for this project ([instructions](https://eecs280staff.github.io/p1-stats/setup.html#create-a-folder)). Your folder location might be different.
```console
$ pwd
/Users/awdeorio/src/eecs281/p0-hello
```

## Starter files
Download the starter files and unpack them.  These instructions are adapted from the Setup Tutorial [Starter files](https://eecs280staff.github.io/p1-stats/setup.html#starter-files) section.
```console
$ pwd
/Users/awdeorio/src/eecs281/p0-hello
$ wget https://eecs280staff.github.io/p2-cv/starter-files.tar.gz
$ tar -xvzf starter-files.tar.gz
$ ls
starter-files  starter-files.tar.gz
```

Move the starter files from `starter-files/` to your present working directory (`.`) and clean up the old directory and tarball.
```console
$ mv starter-files/* .
$ rm -rf starter-files/ starter-files.tar.gz
$ ls
FIXME
```

Create any new files.
```console
$ touch main.cpp
```
{: data-variant="no-line-numbers" }

your project directory should look like this.
```console
$ ls
FIXME
```
{: data-variant="no-line-numbers" }

## Create a project in your IDE
Create a project in your IDE (visual debugger).  Later, we'll compile and debug.

### VS Code
<div class="primer-spec-callout warning" markdown="1">
Some EECS 281 professors are unfamiliar with VS Code.  VS Code will work great for EECS 281 projects.  Just ask one of the IAs or GSIs if you need help.  -- Andrew DeOrio
</div>

If you've used VS Code on your computer before, all you need to do is start VS Code from your project directory.
```console
$ pwd
/Users/awdeorio/src/eecs281/p0-hello
$ code .
```

If VS Code is new to you or new to your computer, start the tutorial at the [beginning](https://eecs280staff.github.io/p1-stats/setup_vscode.html).  Stop after you've completed the [Install the C/C++ extension](https://eecs280staff.github.io/p1-stats/setup_vscode.html#install-the-cc-extension) section.

### Visual Studio
If you've used Visual Studio on your computer before, start the tutorial at the [Create a project](https://eecs280staff.github.io/p1-stats/setup_visualstudio.html#create-a-project) section

If Visual Studio is new to you or new to your computer, start at the [beginning](https://eecs280staff.github.io/p1-stats/setup_visualstudio.html).  Stop after you've completed the [Add existing files](https://eecs280staff.github.io/p1-stats/setup_visualstudio.html#add-existing-files) section.

#### Getopt Setup

Next, we will make sure to add the `getopt` library to Visual Studio. `getopt` is a C/C++ library used to process command line arguments, and its the library that we will be using in EECS 281. While the other IDEs contain of version of `getopt` in their compiler, Visual Studio does not.

First, please download the `getopt.c` file <a href="/getopt.c" download>here</a> and the `getopt.h` file  <a href="/getopt.h" download>here</a>.

The `getopt.c` file is simple to use: merely put a copy of `getopt.c` in each project that needs it. When you turn in your project, do NOT make `getopt.c` part of your tarball (the Makefile that we give you will not include `getopt.c` in the tarballs).

Next, we will need to make sure that `getopt.h` is accessible to Visual Studio. There are two ways to do this:

1. Place `getopt.h` in Visual Studio include folder (**Preferred**)
    Put the `getopt.h` file inside one of the standard Visual Studio include folders. For Visual Studio 2022 Community, this folder is usually located in:
    ```
    C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include
    ```

    If you’re using a different version of VS (such as Community 2019), the location might be slightly different:
    ```
    C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Auxiliary\VS\include
    ```

    This location is usually locked and can only be accessed if you are “Administrator”.

    Open your file explorer and find the appropriate location shown above by starting at “This PC” (in the bottom left), and paste the file.

    You have successfully included `getopt.h` into the Visual Studio include folder, and you will simply need to `#include <getopt.h>` in every project that you do.

2. Place `getopt.h` inside Project Directory
    Put `getopt.h` inside your project folder, and `#include "getopt.h"` in every file where you use `getopt`.

    When you pack up your project to test on CAEN or submit, you have to change your include statement to <getopt.h>, as the Autograder/CAEN will be using the built in library, and not the files provided in the project directory. You should also NOT include getopt.h as part of your submission tarball.

    <div class="primer-spec-callout info" markdown="1">
    Note: The big problem with this solution is that if you forget to change from double quotes to angle quotes, your project will fail to compile when you submit, and waste time (and possibly a submit for the day).
    </div>

### Xcode
If you've used Xcode on your computer before, start the tutorial at the [Create a project](https://eecs280staff.github.io/p1-stats/setup_xcode.html#create-a-project) section.

If Xcode is new to you or new to your computer, start at the [beginning](https://eecs280staff.github.io/p1-stats/setup_xcode.html).  Stop after you've completed the [Add existing files](https://eecs280staff.github.io/p1-stats/setup_xcode.html#add-existing-files) section.

## Create a new file
FIXME: create `main.cpp` or whatever.  Link to IDE tutorials.

Edit `main.cpp` to print "hello world!".  Your filename might be different.
```c++
#include <iostream>
using namespace std;

int main() {
  cout << "hello world!\n";
}
```
{: data-title="main.cpp" }

## Makefile
Next, we'll get all our code to compile with the [EECS 281 Makefile](https://gitlab.umich.edu/eecs281/makefile).
```console
$ pwd
/Users/awdeorio/src/eecs281/p0-hello
$ wget https://gitlab.umich.edu/eecs281/makefile/-/raw/main/Makefile
$ make help
EECS281 Advanced Makefile Help
...
```

Edit these three lines in the Makefile.  Your values might be different, check the project spec for required file names.
```make
UNIQNAME = not_awdeorio
# ...
EXECUTABLE = main
# ...
PROJECTFILE = main.cpp
```
{: data-title="Makefile" }

You should be able to compile and run your main function.
```console
$ make
$ ./main
hello world!
```

## Parsing command line arguments and options
Edit your main program (e.g., `main.cpp`) to parse command line options and print them.

``Note: If you are using Visual Studio, please make sure that you have setup `getopt` [here](#getopt-setup)``

A sample copy of the code required for `getopt` to work can be found below:

```c++
// TODO: Finish this function, look for the individual TODO comments internally.
// Process the command line; the only thing we need to return is the mode
// when the user specifies the -m/--mode option.
string getMode(int argc, char * argv[]) {
  bool modeSpecified = false;
  string mode;
  // These are used with getopt_long()
  opterr = false; // Let us handle all error output for command line options
  int choice;
  int option_index = 0;
  option long_options[] = {
      {"required", required_argument, nullptr, 'r'},
      {"no_arg", no_argument, nullptr, 'n'},
      { nullptr, 0, nullptr, '\0' }
  };
  // TODO: Fill in the double quotes, to match the mode and help options.
  while ((choice = getopt_long(argc, argv, "r:n", long_options, &option_index)) != -1) {
    switch (choice) {
      case 'r':
        cout << "The required argument flag was passed in with argument: " << optarg;
        break;
      case 'o':
        cout << "The no argument flag was passed in.";
        break;
      default:
        cerr << "Error: invalid option" << endl;
        exit(1);
  } // switch
} // while
if (!modeSpecified) {
cerr << "Error: no mode specified" << endl;
exit(1);
} // if
return mode;
} // getMode()
```
{: data-title="main.cpp" }

After adding the code to you file, you can now compile and run the program. You should see the sample command line arguments. This might be a good time to change them to match your project spec.

```console
$ make main
$ ./main FIXME
```

## Debug
Configure your visual debugger to compile and run your main program.

### VS Code
Follow the   [VS Code Run instructions](https://eecs280staff.github.io/p1-stats/setup_vscode.html#run) and configure it to run `main`.  Don't forget to change the `program` in `launch.json`.  Your program name might be different.
```json
  "program": "${workspaceFolder}/main"
```

Finally, follow the [Debug instructions](https://eecs280staff.github.io/p1-stats/setup_vscode.html#debug) to run your program with a breakpoint.  During unit testing, add a breakpoint to the first line of one test.

### Visual Studio
First, make sure the correct files are included in the build.  You can figure out the right files by compiling at the command line.  In this example, you can see that we'll only need `main.cpp`.  All other files should be excluded from the build.
```console
$ make main
FIXME
```

Now you should be able to run your main program from inside Visual Studio ([instructions](https://eecs280staff.github.io/p1-stats/setup_visualstudio.html#run)).

Finally, follow the [Debug instructions](https://eecs280staff.github.io/p1-stats/setup_visualstudio.html#debug) to run your program with a breakpoint.  During unit testing, add a breakpoint to the first line of one test.

### Xcode
First, make sure the correct files are included in the Xcode compile sources.  You can figure out the right files by compiling at the command line.  In this example, you can see that we'll only need `main.cpp`.  All other files should be excluded from the build.
```console
$ make main
FIXME
```

Next, change the settings for running your program ([instructions](https://eecs280staff.github.io/p1-stats/setup_xcode.html#run)).

Finally, follow the [Debug instructions](https://eecs280staff.github.io/p1-stats/setup_xcode.html#debug) to run your program with a breakpoint.  During unit testing, add a breakpoint to the first line of one test.

## Configure Address Sanitizer
The Address Sanitizer is very good at finding memory errors, including going off the end of an array or vector, making a mistake with a pointer, or leaking memory.  Follow our [Address Sanitizer Quick Start](https://eecs280staff.github.io/p1-stats/setup_asan.html#quick-start).

<div class="primer-spec-callout warning" markdown="1">
The EECS 280 faculty no longer recommend Valgrind.  Here's why.
- ASAN is just as good at catching undefined behavior and leaks
- ASAN is better supported, notably on macOS
- ASAN is [faster](https://developers.redhat.com/blog/2021/05/05/memory-error-checking-in-c-and-c-comparing-sanitizers-and-valgrind#performance)
</div>

FIXME: enable leak check


## Version control
<div class="primer-spec-callout info" markdown="1">
When you're working alone, version control is optional.  Here are two reasons to consider using it.
- Code backup
- Easy to try and revert ideas
</div>

Set up version control using the [Version control tutorial](https://eecs280staff.github.io/p1-stats/setup_git.html).  If you've used version control before on your computer, you'll probably want to start with the [Create a local repository](https://eecs280staff.github.io/p1-stats/setup_git.html#create-a-local-repository) section.

After you're done, you should have a local repository with a "clean" status and your local repository should be connected to a remote GitLab repository.
```console
$ pwd
/Users/awdeorio/src/eecs281/p0-hello
$ git status
On branch main
Your branch is up-to-date with 'origin/main'.

nothing to commit, working tree clean
$ git remote -v
origin	https://gitlab.eecs.umich.edu/awdeorio/p0-hello.git (fetch)
origin	https://gitlab.eecs.umich.edu/awdeorio/p0-hello.git (push)
```

You should have a `.gitignore` file ([instructions](https://eecs280staff.github.io/p1-stats/setup_git.html#create-a-local-repository)).
```console
$ pwd
/Users/awdeorio/src/eecs281/p0-hello
$ head .gitignore
# This is a sample .gitignore file that's useful for C++ projects.
...
```
