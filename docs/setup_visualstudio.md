---
layout: spec
title: Setup Visual Studio (Windows)
excludeFromSitemap: true
---

Setting up Visual Studio
========================
{: .primer-spec-toc-ignore }

[Visual Studio](https://www.visualstudio.com/) is a feature-rich integrated debugging environment (IDE) that runs on Windows.

<div class="primer-spec-callout info" markdown="1">
If you already have Visual Studio installed, skip to the [Create a project](#create-a-project) section.
</div>

## Prerequisites
There are no prerequisites for Visual Studio.

## Restart
To start clean, first quit Visual Studio.  Back up your files, and then delete your project directory.  Your project directory might be different.
```console
$ pwd
/Users/awdeorio/src/eecs280
$ cp -a p1-stats p1-stats.bak  # Backup
$ rm -rf p1-stats              # Delete
```

Visual Studio has a *lot* of settings.  You can reset the entire user interface to the default settings by selecting "Tools" -> "Import" and Export Settings" -> "Reset all settings".  This is optional.


## Install
Install Visual Studio Community edition from [Microsoft's website](https://www.visualstudio.com/vs/community/).

Select "Desktop Development with C++" and make sure that the "Windows 10 SDK", "Windows 11 SDK", and "C++ AddressSanitizer" are selected.  Click "Continue".  Reboot your computer when the installer prompts you.

<img src="images/visualstudio010.png" width="768px" />

Start Visual Studio.  The first time, you'll be asked about a few preferences.

The screenshots in the tutorial were created with Visual Studio Community 2022.  Your version might be different.

## Create a project
A Visual Studio project contains the files and information to build your software.  In EECS 280, you'll eventually create one Visual Studio project for each EECS 280 project.

Start Visual Studio.  Create a new project.  You can also use "File" > "New" > "Project".

<img src="images/visualstudio040.png" width="512px" />

Select "Console App".  Click "Next".

<img src="images/visualstudio050.png" width="512x" />

Set a project name, we'll call our example project `p1-stats`.  Pick a location where you'll store your projects.  Check "Place solution and project in the same directory.  We recommend this in EECS 280 so that everyone has a one-level structure (`p1-stats/main.cpp`) instead of a two-level structure (`p1-stats/p1-stats/main.cpp`).  Click "Next".

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Avoid paths that contain spaces.  Spaces causes problems with some command line tools.

| Bad Example     | Good Example   |
|-----------------|----------------|
| `EECS 280/` | `eecs280/` |
| `Project 1 Stats/` | `p1-stats/` |

</div>

<img src="images/visualstudio060.png" width="512px" />

You can see the files created by Visual Studio in the File Explorer.  Right-click `p1-stats`, select "Show in Finder".

| <img src="images/visualstudio070.png" height="512px" /> | <img src="images/visualstudio071.png" height="512px" /> |

<img src="images/visualstudio072.png" width="768px" />

You can also see the files created by Visual Studio from the WSL command line.  Notice that Visual Studio created `.sln`, and `vcxproj` files.

```console
$ tree
.
├── p1-stats.cpp
├── p1-stats.sln
├── p1-stats.vcxproj
├── p1-stats.vcxproj.filters
└── p1-stats.vcxproj.user
```

### Rename main file
{: .primer-spec-toc-ignore }
Rename the default `p1-stats.cpp` to `main.cpp`.  Your main filename may be different.  Use Visual Studio to rename a file, not the command line or File Explorer.

Right-click `p1-stats.cpp` and select "Rename".  Change the file name.

| <img src="images/visualstudio075.png" width="256px" /> | <img src="images/visualstudio076.png" width="256px" /> | <img src="images/visualstudio077.png" width="256px" /> |

### Add new files
<div class="primer-spec-callout info" markdown="1">
Visual Studio created `main.cpp` by default.  Skip this subsection your first time through the tutorial.  You can come back to it.
</div>

Open your project folder by selecting `File` > `Open` > `Project/Solution`, navigate to your project directory (`p1-stats` in this example) and open `p1-stats.sln`.

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** Here's a quick way to open Visual Studio to a specific project folder from the command line.  FIXME FIXME FIXME
```console
$ wslview p1-stats
```
{: data-variant="no-line-numbers" }
</div>

Right-click "Source Files", then select "Add" > "New Item".

<img src="images/visualstudio080.png" width="768px" />

Select "Visual C++" and "C++ File".  Name your file, we'll use `stats.cpp` for this example.  Click "Add".

<img src="images/visualstudio090.png" width="600px" />

You should see your new file under "Source Files".

<img src="images/visualstudio100.png" width="768px" />

### Add existing files
If you have starter files, add them to your project directory.  This example is from EECS 280 Project 1.  Your URL or files might be different.

We'll use the WSL (Ubuntu) terminal to download, unpack, and move starter files into the `src` subdirectory.  Your URL or folder might be different.  **Pro-tip:** [copy/paste instructions for WSL](setup_wsl.html#how-do-i-copy-and-paste).

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Make sure you're in the subdirectory containing your source code.
```console
$ ls
main.cpp
p1-stats.sln
...
```
</div>

```console
$ wget https://eecs280staff.github.io/p1-stats/starter-files.tar.gz
$ tar -xvzf starter-files.tar.gz
$ mv starter-files/* .
$ rm -rf starter-files starter-files.tar.gz
```

You should see your new files in the `src` subdirectory.
```console
$ tree
.
├── Makefile
├── main.cpp
├── main_test.in
├── main_test.out.correct
├── main_test_data.tsv
├── p1-stats.sln
├── p1-stats.vcxproj
├── p1-stats.vcxproj.filters
├── p1-stats.vcxproj.user
├── p1_library.cpp
├── p1_library.h
├── stats.h
├── stats_public_test.cpp
└── stats_tests.cpp.starter
```

Right-click "Source Files", then select "Add" > "Existing Item".

<img src="images/visualstudio105.png" width="768px" />
 
Navigate to your project directory.  Select your files by holding <kbd>Control</kbd> and clicking each one.  Do *not* select any `.sln` or `.vcxproj` files.  Click "Add".

<img src="images/visualstudio106.png" width="512px" />

You will now see your files in the sidebar in the sidebar.

<img src="images/visualstudio107.png" width="768px" />

#### Rename files
If you need to rename any files, use Visual Studio, not the command line or File Explorer.  In EECS 280, you'll need to rename any files that end in `.starter`.

Right-click a file and select "Rename".  Change the file name.

| <img src="images/visualstudio108.png" width="256px" /> | <img src="images/visualstudio076.png" width="256px" /> | <img src="images/visualstudio109.png" width="256px" /> |

## Compile and Run
A Visual Studio Build compiles one executable.

We need to avoid multiple `main()` functions in the same build.  Exclude files not needed to compile the  main program.  This often means excluding unit tests from the build.

In this example from EECS 280 Project 1, we need to exclude our unit tests from the build because they each contain a `main()` function.

Right-click a file in the solution explorer (sidebar) and select "Properties".  Set "Excluded From Build" to "Yes".

<img src="images/visualstudio110.png" width="512px" />

You can see a red symbol next to each excluded file.

<img src="images/visualstudio120.png" width="768px" />

"Build" -> "Build Solution".  See that the build output was successful.

<img src="images/visualstudio130.png" width="768px" />

Click the "Local Windows Debugger" button.  You should see your program run.

<img src="images/visualstudio140.png" width="768px" />

### Sanitizers
Visual Studio provides an address sanitizer with bounds checking automatically with every debug build. You don’t need to do anything different!

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** When Visual Studio says "Press any key to continue", press a key, do *not* close the window by clicking the X.  Some memory checks run after you press any key.

<img src="images/visualstudio055.png" width="512px" />

</div>

### Input redirection
<div class="primer-spec-callout info" markdown="1">
Skip this subsection your first time through the tutorial.  You can come back to it.
</div>

You can use input redirection to avoid typing program input each time you run a program.  Here's an example program.
```c++
#include <iostream>
#include <string>
using namespace std;

int main() {
  cout << "What's your name?" << endl;
  string name;
  cin >> name;
  cout << "Hello " << name << "!\n";
}
```

Without input redirection, the user types input at the command line (highlighted).
```console
$ make main.exe
$ ./main.exe
What's your name?
Drew
Hello Drew!
```
{: data-highlight="4" }

Automate user input by putting it in a file.
```
Drew
```
{: data-title="main_test.in" data-highlight="1" }

Redirect file `main_test.in` to stdin of `main.exe`.
```console
$ ./main.exe < main_test.in
What's your name?
Hello Drew!
```
{: data-highlight="1" }

Without input redirection, here's how to type input in the Visual Studio command line.  Notice that when we run, a window pops up that accepts user input.

<img src="images/visualstudio145.png" width="768px" />

To configure input redirection, right-click the project in the Solution Explorer (`p1-stats` in this example).  Select "Properties".

<img src="images/visualstudio148.png" width="320px" />

Edit "Command Arguments" and click "OK".

<img src="images/visualstudio146.png" width="768px" />

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Set a breakpoint on the last line of `main` to keep the output window open.

With input redirection configured, Visual Studio will automatically close the terminal window, and you'll miss seeing the output.
</div>


### Arguments and options
<div class="primer-spec-callout info" markdown="1">
Skip this subsection for EECS 280 project 1.
</div>

Arguments and options are inputs to a program typed at the command line.  Here's an example from EECS 280 Project 5:
```console
$ ./main.exe train_small.csv test_small.csv --debug
```
{: data-variant="no-line-numbers" data-highlight="1" }

- `main.exe` is the name of the program
- `train_small.csv` and `test_small.csv` are arguments
- `--debug` is an option

Right-click the project in the Solution Explorer (`p1-stats` in this example).  Select "Properties".

<img src="images/visualstudio148.png" width="320px" />

Edit "Command Arguments" and click "OK".

<img src="images/visualstudio149.png" width="768px" />

## Debug
In this section, we'll set a breakpoint, which pauses the debugger.  Then, we'll cover some of the options to continue execution.

<img src="images/visualstudio_icon_step_over.png" style="vertical-align: text-top; height: 1.25em;" /> **Step Over**
Run one line of code, stepping _over_ any function calls by running the whole function in one step.

<img src="images/visualstudio_icon_step_in.png" style="vertical-align: text-top; height: 1.25em;" /> **Step Into**
Run one line of code, stepping _into_ any function calls to execute them line-by-line.

<img src="images/visualstudio_icon_step_out.png" style="vertical-align: text-top; height: 1.25em;" /> **Step Out**
Run the program until it returns from the current function (or until the next breakpoint).

<img src="images/visualstudio_icon_continue.png" style="vertical-align: text-top; height: 1.25em;" /> **Continue**
Run the program until the next breakpoint.

### Example code
{: .primer-spec-toc-ignore }

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
FIXME Select the file you want to debug.  Set a breakpoint by clicking to the left of a line number.  A breakpoint tells the program to pause.

<img src="images/visualstudio150.png" width="768px" />

### Run
Run the debugger.  The program pauses at the breakpoint.  The FIXME indicator highlights the next line of code to be run.

<img src="images/visualstudio160.png" width="768px" />

### Step over
Click "Step Over" a few times until you reach the highlighted line of code

<img src="images/visualstudio170.png" width="768px" />

### Inspect
Hover over a variable to inspect its value.  You can also see values in the variables pane.  FIXME

<img src="images/visualstudio180.png" width="360px" />

### Step into
Click "Step Into".  The cursor enters the `sum()` function.

<img src="images/visualstudio190.png" width="768px" />

### Step out
Click "Step Out".  The `sum()` function completes, and the program pauses again.

<img src="images/visualstudio200.png" width="768px" />

### Continue
Press "Continue" to run the program to the next breakpoint, or the end, whichever comes first.

<img src="images/visualstudioXYZ.png" width="768px" />

## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
