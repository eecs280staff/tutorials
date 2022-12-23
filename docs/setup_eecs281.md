---
layout: spec
title: EECS 281 Project Setup
---


EECS 281 Project Setup
======================

This tutorial will help you set up an EECS 281 project using the EECS 280 tutorials.

## Visual debugger
Set up your project in your visual debugger.  We'll use the project name `p1-stats` in this example, but your project name may be different.  If your project has starter files, you'll need the link from the project spec.

| [VS Code Tutorial](https://eecs280staff.github.io/tutorials/setup_vscode.html)| [Visual Studio Tutorial](https://eecs280staff.github.io/tutorials/setup_visualstudio.html) | [Xcode Tutorial](https://eecs280staff.github.io/tutorials/setup_xcode.html) |

<div class="primer-spec-callout warning" markdown="1">
Some EECS 281 professors are unfamiliar with VS Code.  VS Code works great for EECS 281 projects.  Just ask one of the IAs or GSIs who use VS Code if you need help.
</div>

After you're done, you should have a folder with a main file.  Your files may be different.
```console
$ tree p1-stats/
p1-stats/
├── main.cpp
    ...
```

## Makefile
Next, we'll get all our code to compile with the [EECS 281 Makefile](https://gitlab.umich.edu/eecs281/makefile).
```console
$ pwd
/Users/awdeorio/src/eecs281/p1-stats
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

## Arguments and options
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
/Users/awdeorio/src/eecs281/p1-stats
$ git status
On branch main
Your branch is up-to-date with 'origin/main'.

nothing to commit, working tree clean
$ git remote -v
origin	https://gitlab.eecs.umich.edu/awdeorio/p1-stats.git (fetch)
origin	https://gitlab.eecs.umich.edu/awdeorio/p1-stats.git (push)
```

You should have a `.gitignore` file ([instructions](https://eecs280staff.github.io/p1-stats/setup_git.html#create-a-local-repository)).
```console
$ pwd
/Users/awdeorio/src/eecs281/p1-stats
$ head .gitignore
# This is a sample .gitignore file that's useful for C++ projects.
...
```
