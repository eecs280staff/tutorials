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
IDENTIFIER  = copy_me_from_project_spec
# ...
PROJECTFILE = main.cpp
# ...
EXECUTABLE = main
```
{: data-title="Makefile" }

**Shortcut:** You can skip updating `PROJECTFILE` if your `main()` function is in one of these filenames: `main.cpp`, `project0.cpp`, `project1.cpp`, `project2.cpp`, `project3.cpp`, `project4.cpp`, `EXECUTABLE.cpp`.

If your project has additional dependencies, update the dependencies section at the bottom of the `Makefile`.

You should be able to compile and run your main function.
```console
$ make
$ ./main
hello world!
```

## Parsing command line arguments and options
Edit your main program (e.g., `main.cpp`) to parse command line options and print them.  Copy this sample code.

```c++
#include <iostream>
#include <string>
#include <getopt.h> // TODO: Visual Studio users change to "xgetopt.h"
using namespace std;


int main(int argc, char * argv[]) {
  // TODO: Create a variable to store each option
  int verbose = 0;
  string output;

  // TODO: Update these options
  option long_options[] = {
    {"verbose", no_argument, nullptr, 'v'},
    {"output", required_argument, nullptr, 'o'},
    { nullptr, 0, nullptr, '\0' }
  };

  // Parse options (starts with '-')
  while (1) {
    // getopt_long stores the option index here
    int option_index = 0;

    // TODO: Modify "o:v" to include each of the chars from long_options above.
    // An option with a required argument is followed by ":".
    int c = getopt_long (argc, argv, "o:v", long_options, &option_index);

    // End of options
    if (c == -1) break;

    // TODO: Add a case for each option
    switch (c) {
    case 'o':
      output = optarg;
      break;
    case 'v':
      verbose++;
      break;
    default:
      cerr << "Error: TODO: Update this help string" << endl;
      return 1;
    }
  }

  // TODO: Use the options
  cout << "verbose = " << verbose << endl;
  cout << "output = " << output << endl;
}
```
{: data-title="main.cpp" }

Compile and run.
```console
$ make main
$ ./main --verbose --output output.txt
verbose = 1
output = output.txt
$ ./main -v -o output.txt
verbose = 1
output = output.txt
$ ./main
verbose = 0
output = 
```

<div class="primer-spec-callout warning" markdown="1">
**Visual Studio** does not ship with a `getopt` library, which processes command line arguments.

Download [`xgetopt.h`](xgetopt.h) and place it in your project source code directory.  It's cross-platform and will work on both Windows and Linux.

Remove the `<getopt.h>` include.  Add the `"xgetopt.h"` include.
```c++
#include <getopt.h> // REMOVE
#include "xgetopt.h"
```
{: data-title="main.cpp" }

**Pitfall:** Make sure `xgetopt.h` is in the same folder as your source code.
```console
$ ls
Makefile  main.cpp  xgetopt.h
```
</div>


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
