---
layout: spec
title: Setup VSCode
sitemapOrder: 20
---

Setup up VS Code for C/C++ on Windows
=====================================
{: .primer-spec-toc-ignore }

[Visual Studio Code](https://code.visualstudio.com/) is a lightweight, easy-to-use, source code editor with debugging support.  It runs on macOS, Windows, and Linux (including CAEN Linux).  Visual Studio Code is not the same program as Visual Studio.

This tutorial is specific to Windows.  Looking for the [macOS version](setup_vscode_macos.html)?


<div class="primer-spec-callout info" markdown="1">
If you already have VS Code installed with the C/C++ extensions, skip to the [Create a project](#create-a-project) section.
</div>

## Prerequisites

Complete the [WSL tutorial](setup_wsl.html) to ensure your Windows and WSL installations are up-to-date and you have CLI tools installed.

Review our [Command Line Interface (CLI)](cli.html) tutorial.

Make sure you have a compiler and a debugger [installed](setup_wsl.html#install-cli-tools).  Your version might be different.

```console
$ g++ --version
g++ (Ubuntu 13.2.0-23ubuntu4) 13.2.0
$ gdb --version
GNU gdb (GDB)
```

## Install

Ensure the prerequisites above are satisfied, then install VS Code from the web [https://code.visualstudio.com/](https://code.visualstudio.com/).

Select each of the options below during installation.

<img src="images/vscode_wsl_005.png" width="480px" />

Open VS Code. You can skip the welcome screen.

<img src="images/vscode_wsl_007.png" width="768px" />


### WSL Extension

Install the WSL Extension, which allows the VS Code backend to run in WSL where the C++ compiler lives.

1. Open the extensions panel from the left sidebar.
2. Search for WSL.
3. Click "Install".

<img src="images/vscode_wsl_008.png" width="768px" />

Next, connect to WSL:
1. Click the button in the bottom left corner.
2. Select "Connect to WSL" from the menu.

<img src="images/vscode_wsl_012.png" width="768px" />

Now, the button in the bottom left should say "WSL: Ubuntu". For any C++ development, make sure VS code is always connected to WSL.

<img src="images/vscode_wsl_013.png" width="768px" />


### Microsoft C++ Extension

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Make sure you're connected to WSL before installing the C++ Extension. Check the button in the bottom left.

<img src="images/vscode_wsl_013a.png" width="200px" />

</div>

Install the C++ Extension to run in WSL.

1. Open the extensions panel from the left sidebar.
2. Search for C++.
3. Click "Install".

<img src="images/vscode_wsl_014.png" width="768px" />

Note that you need the "C/C++" extension. You do *not* need the "C/C++ Extension Pack".

Clear out the search bar in the extensions panel. You should see:
- WSL installed locally
- C/C++ installed in WSL: Ubuntu

<img src="images/vscode_wsl_015.png" width="768px" />

## Create a project
To create a VS Code project, create a folder (directory).  There are many ways to create folders: File Explorer, VS Code interface, VS Code integrated terminal, and the system terminal.  We'll use the system terminal and call our example project `p1-stats`.

Open the Terminal ([Ubuntu Bash Shell](cli.html#open-terminal-windows)).

Create a new folder for your project.

```console
$ mkdir -p ~/eecs280/p1-stats
$ cd ~/eecs280/p1-stats
```

This `mkdir -p` command creates a new `p1-stats` folder within an `eecs280` folder (creating that too, if it doesn't exist), within your Ubuntu home directory (`~`). The `cd` command changes your working directory to the new `p1-stats` directory.

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Avoid paths that contain spaces.  Spaces causes problems with some command line tools.

| Bad Example     | Good Example   |
|-----------------|----------------|
| `EECS 280/` | `eecs280/` |
| `Project 1 Stats/` | `p1-stats/` |

</div>
  
<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Linux (Ubuntu) has a separate home directory.  Storing code in your Windows home directory can cause slowdowns.

```console
$ pwd
/home/awdeorio ...         # Good, Linux home
/c/mnt/Users/awdeorio ...  # Bad, Windows home
```

Here's how to [access your Linux files from Windows](setup_wsl.html#access-via-file-explorer).
</div>

Now, verify you're in the project directory with `pwd` and launch VS code with `code .`:
```console
$ pwd
/home/awdeorio/eecs280/p1-stats
$ code .
```

The `.` in `code .` means "current working directory". It would also be fine to use an absolute path, for example, `code ~/eecs280/p1-stats`.

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** If you've just installed VS Code, you may need to restart your terminal before the `code` command will work.
</div>

You can also open your project folder through the VS Code interface. Go to `View` > `Command Palette` (or press <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>p</kbd>). Search for and select `WSL: Open Folder in WSL`, then find your project folder.

You should see your project open in VS Code, with "WSL: Ubuntu" in the lower left corner.

Finally, open the integrated terminal within VS Code. Go to `Terminal` > `New Terminal` (or press <kbd>ctrl</kbd> + <kbd>`</kbd>). The integrated terminal will automatically use your project directory as its working directory, and using it is easier than switching back and forth to your separate terminal window.

<div id="pitfall-wsl-mode" class="primer-spec-callout warning" markdown="1">
**Pitfall:** Make sure you're in WSL mode. Double check that the lower left corner says "WSL: Ubuntu".

<img src="images/vscode_wsl_069.png" width="768px">

If you accidentally open VS Code in Windows mode, you won't see "WSL: Ubuntu" in the lower left corner, your integrated terminal may default to powershell, and compiling/running C++ code won't work correctly.

<img src="images/vscode_wsl_070.png" width="768">

Open the Command Palette with `View` > `Command Palette` (<kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>p</kbd>). Search for and select `Reopen Folder in WSL` (or `Open Folder in WSL` if you hadn't opened anything yet).

<img src="images/vscode_wsl_071.png" width="768">
</div>

### Add existing files
If you have starter files, add them to your project directory. We'll use [EECS 280 Project 1](https://eecs280staff.github.io/p1-stats/) as an example, downloading an archive of starter files from the provided URL.

We'll run several commands to get the starter file (see below). First, verify you're in the correct project directory (`pwd`). Use the terminal to download (`wget`), unpack (`tar -xvzf`), and move (`mv`) the starter files into our project directory. Finally, clean up the downloaded archive (`rm`). Your URL or folder might be different.

```console
$ pwd
/home/awdeorio/eecs280/p1-stats
$ wget https://eecs280staff.github.io/p1-stats/starter-files.tar.gz
$ tar -xvzf starter-files.tar.gz
$ mv starter-files/* .
$ rm -rf starter-files starter-files.tar.gz
```

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** If copy/paste isn't working in the terminal, see our [copy/paste instructions for WSL](setup_wsl.html#copypaste-in-terminal) for WSL.
</div>

You should see your new files in your project directory.
```console
$ tree
.
├── Makefile
├── cats.csv
├── cats.out.correct
├── main.cpp
├── p1_library.cpp
├── p1_library.hpp
├── stats.hpp
├── stats_public_tests.cpp
├── stats_tests.cpp.starter
└── two_sample.cpp.starter
```

You should see your new files appear in VS Code. Your specific files may not match the image below.

<img src="images/vscode_wsl_026.png" width="768px" />

#### Rename files
If you need to rename any files, you can do this in VS Code by right clicking a file and selecting "rename".

In EECS 280, you'll do this to any file that ends in `.starter`.

| <img src="images/vscode_wsl_027.png" height="512px" /> | <img src="images/vscode_wsl_028.png" height="512px" /> |

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** You can also rename files the command line, for example:
```console
$ mv stats_tests.cpp.starter stats_tests.cpp
```
</div>

### Add new files

We'll use [EECS 280 Project 1](https://eecs280staff.github.io/p1-stats/) as an example.

Select the add file icon and give it a name, for example, `stats.cpp`.

<img src="images/vscode_wsl_023.png" width="768px" />

Alternatively, create your `stats.cpp` file from the command line using [`touch`](cli.html#touch). 

```console
$ touch stats.cpp
```

## Compile and Run
VS Code does not use an internal build system for C++. That means you'll be compiling your code from the terminal. In EECS 280 projects, you compile with the `make` utility and a `Makefile` provided with each project.

Identify your desired compilation target, for example, `stats_public_tests.exe` from EECS 280 project 1. (Note that this may not compile/run successfully if you haven't started the project yet.) Compile with:

```console
$ make stats_public_tests.exe
```

And run with:

```console
$ ./stats_public_tests.exe
```

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** If something doesn't seem to be working, you can always check your current directory with `pwd` or `ls` to verify your terminal is in the correct place with the correct files.
```console
$ pwd
/home/awdeorio/eecs280/p1-stats
$ ls
Makefile  cats.out.correct  p1_library.hpp  stats.hpp               stats_tests.cpp
cats.csv  p1_library.cpp    stats.cpp       stats_public_tests.cpp  two_sample.cpp
```
</div>

## Debug

You can also run your compiled executable through the VS Code visual debugger to diagnose runtime errors, inspect the state of your program at a breakpoint, or step line-by-line through your code.

### Create `launch.json`

Running and debugging code through VS Code requires a `launch.json` configuration file. The examples below assume a source file `main.cpp` compiled to `main.exe`. Your filenames may be different.

Select the file you would like to run. Navigate to the debugging pane.

<img src="images/vscode_wsl_030.png" width="768px" />

Click "create a launch.json file".

<img src="images/vscode_wsl_031.png" width="768px" />

If you are prompted to select a debugger, select C++ (GDB/LLDB).

<img src="images/vscode_wsl_031a.png" width="768px" />

Click "Add Configuration".  If the button does not appear in the bottom-right corner, select "Run" from the top menu, then select "Add Configuration".

<img src="images/vscode_wsl_032.png" width="768px" />

Select the "C/C++ (gdb) Launch" configuration.  This will create a default `launch.json` ([Microsoft Reference](https://code.visualstudio.com/docs/cpp/launch-json-reference)).

<img src="images/vscode_wsl_033.png" width="768px" />

Edit the `program` and `cwd` fields in `launch.json`.  Save the updated file.  Your `program` name might be different.

<img src="images/vscode_wsl_034.png" width="768px" />


### Run
Click the triangle to run.  You'll see your program's output in the terminal window at the bottom.

<img src="images/vscode_wsl_035.png" width="768px" />

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Remember to build your executable at the command line first.
```console
$ make main.exe
```
</div>

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** If you're having trouble running your program, delete your `launch.json` and try the [compile and run](#compile-and-run) section again.

<img src="images/vscode_wsl_160.png" width="768px" />
</div>

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** If you encounter an error like the one below, it is likely that VS Code is in Windows mode rather than WSL. Follow the instructions in [WSL Mode Pitfall](#pitfall-wsl-mode) to reopen the folder in WSL mode.

<img src="images/vscode_wsl_170.png" width="600px" />
</div>

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** If you see output like the following in the terminal:

<img src="images/vscode_wsl_180.png" width="600px" />

And an error like this in the debug console:

<img src="images/vscode_wsl_181.png" width="600px" />

The likely cause is that you have WSL 1 rather than WSL 2. Follow the instructions in the [WSL tutorial](setup_wsl.html#install-wsl) to upgrade to WSL 2, install the [CLI tools](setup_wsl.html#install-cli-tools), and then restart this tutorial.

</div>

### Configure Debugger

Depending on your program, its inputs, and how you want to debug it, you'll need to adjust `launch.json`.

#### Edit `launch.json` program

If you already have a working `launch.json` and want to debug a different program, edit the `program` field `launch.json`.  Your `program` name might be different.  Make sure `cwd` is set to `"${workspaceFolder}"`.
```json
{
    "program": "${workspaceFolder}/main.exe",
    ...
    "cwd": "${workspaceFolder}",
}
```
{: data-highlight="2,4" }

#### Sanitizers
We recommend enabling the address sanitizer and undefined behavior sanitizer. These will help you find memory errors like going off the end of an array or vector.

First, edit your `Makefile` and add the `CXXFLAGS` recommended by the [ASAN Quick Start](setup_asan.html#quick-start).

Edit the `"environment"` property in your `launch.json`.  If there's already an empty `"environment": []`, replace it.  If there isn't one, add it after the `"args"` property.

```json
  "environment": [
    {
      "name": "ASAN_OPTIONS",
      "value": "abort_on_error=1:detect_leaks=0"
    }
  ],
```
{: data-highlight="3-4" }

#### Arguments and options

Arguments and options are inputs to a program typed at the command line.  Here's an example from EECS 280 Project 4:
```console
$ ./main.exe train_small.csv test_small.csv --debug
```
{: data-variant="no-line-numbers" data-highlight="1" }

- `main.exe` is the name of the program
- `train_small.csv` and `test_small.csv` are arguments
- `--debug` is an option

To run a program with options or arguments in VS Code, edit `launch.json`.  Each option or argument should goes in a separate comma-separated string.
```json
{
    "configurations": [
        {
            ...
            "program": "${workspaceFolder}/main.exe",
            "args": ["train_small.csv", "test_small.csv", "--debug"],
            ...
        }
    ]
}
```
{: data-title="launch.json" data-highlight="6" }

#### Input redirection
<div class="primer-spec-callout info" markdown="1">
Skip this subsection for EECS 280 project 1.
</div>

If you're unfamiliar with input redirection, first read the CLI tutorial section on [input redirection](cli.html#input-redirection-).

To configure input redirection, edit `launch.json`.  These changes are for the Microsoft [C/C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).
```json
{
    "configurations": [
        {
            ...
            "program": "${workspaceFolder}/main.exe",
            "args": ["<", "main_test.in"],
            ...
        }
    ]
}
```
{: data-title="launch.json" data-highlight="6" }

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Make sure you're using the Microsoft C++ extension.  You should see `cppdbg` in your `launch.json`.  If not, delete your `launch.json` and try the [compile and run](#compile-and-run) section again.

```json
{
    "configurations": [
        {
            "type": "cppdbg",
            ...
```
{: data-title="launch.json" data-highlight="4" }

</div>



### Run Debugger

In this section, we'll set a breakpoint, which pauses the debugger.  Then, we'll cover some of the options to continue execution.

<img src="images/vscode_icon_step_over.png" style="vertical-align: text-top; height: 1.25em;" /> **Step Over**
Run one line of code, stepping _over_ any function calls by running the whole function in one step.

<img src="images/vscode_icon_step_in.png" style="vertical-align: text-top; height: 1.25em;" /> **Step Into**
Run one line of code, stepping _into_ any function calls to execute them line-by-line.

<img src="images/vscode_icon_step_out.png" style="vertical-align: text-top; height: 1.25em;" /> **Step Out**
Run the program until it returns from the current function (or until the next breakpoint).

<img src="images/vscode_icon_continue.png" style="vertical-align: text-top; height: 1.25em;" /> **Continue**
Run the program until the next breakpoint.

#### Example code

The examples below presume you have the sample `main.cpp` and `Makefile` files below.

Download them from your terminal with:

```console
$ pwd
/home/awdeorio/eecs280/p1-stats
$ wget -nc https://eecs280staff.github.io/tutorials/main.cpp
$ wget -nc https://eecs280staff.github.io/tutorials/Makefile
```

These commands will not overwrite existing files. If you already have a Makefile for your project and it doesn't have a target for `main.exe`, copy the `main.exe` target below into it.

```c++
// Sample main.cpp for EECS 280 Setup Tutorials

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

```make
# Sample makefile for EECS 280 Setup Tutorials

CXX ?= g++
CXXFLAGS ?= -Wall -Werror -pedantic -g --std=c++17 -Wno-sign-compare -Wno-comment

# Compile the main executable
main.exe: main.cpp
	$(CXX) $(CXXFLAGS) main.cpp -o main.exe


# Remove automatically generated files
clean :
	rm -rvf *.exe *~ *.out *.dSYM *.stackdump

# Disable built-in rules
.SUFFIXES:
```
{: data-title="Makefile" }

#### Set Breakpoints
Select the file you want to debug.  Set a breakpoint by clicking to the left of a line number.  A breakpoint tells the program to pause.

<img src="images/vscode_wsl_080.png" width="768px" />


Select the debugging pane, then run the debugger.  The program pauses at the breakpoint.  The yellow indicator highlights the next line of code to be run.

<img src="images/vscode_wsl_090.png" width="768px" />

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Don't forget to compile!
```console
$ make main.exe                # With a Makefile
$ g++ -g main.cpp -o main.exe  # Without a Makefile
```
</div>

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** VS Code debugging will fail if there are no debugging symbols.  Double check your compilation command or the output of `make` and verify that you see `-g` being used in the commands.  The EECS 280 defaults include `-g`.
```console
$ make main.exe
g++ ... -g main.cpp ...
```
</div>

#### Step over
Click "Step Over" a few times until you reach the highlighted line of code

<img src="images/vscode_wsl_100.png" width="768px" />

#### Inspect
Hover over a variable to inspect its value.  You can also see values in the VARIABLES pane.

<img src="images/vscode_wsl_110.png" width="768px" />

<div class="primer-spec-callout warning" markdown="1">
If you have trouble viewing the contents of a container like this screenshot, see [Pretty Printing STL Containers with `gdb`](setup_gdb.html#pretty-printing-stl-containers-with-gdb).

<img src="images/vscode_wsl_140.png" width="480px" />
</div>

#### Step into
Click "Step Into".  The cursor enters the `sum()` function.

<img src="images/vscode_wsl_115.png" width="768px" />

<img src="images/vscode_wsl_120.png" width="768px" />

#### Step out
Click "Step Out".  The `sum()` function completes, and the program pauses again.

<img src="images/vscode_wsl_125.png" width="768px" />

<img src="images/vscode_wsl_130.png" width="768px" />

#### Continue
Press "Continue" to run the program to the next breakpoint, or the end, whichever comes first.

<img src="images/vscode_wsl_135.png" width="768px" />

## Troubleshooting
This section is for common problems and solutions.

### Reset
To reset VS Code project settings and starter files, first quit VS Code.  Make a backup copy of your files, and then delete your project directory.  Your project directory might be different.

```console
$ pwd
/Users/awdeorio/src/eecs280
$ cp -a p1-stats p1-stats.bak  # Backup
$ rm -rf p1-stats              # Delete
```

VS Code has a *lot* of settings and extensions.   You can reset the entire user interface and remove all extensions using these commands (Based on [Microsoft instructions](https://code.visualstudio.com/docs/setup/uninstall#_clean-uninstall)).  This is optional.

Replace `awdeorio` with your Windows username.  List the usernames with `ls /mnt/c/Users/`.
```console
$ rm -rf ~/.vscode
$ rm -rf "/mnt/c/Users/awdeorio/.vscode"
$ rm -rf "/mnt/c/Users/awdeorio/AppData/Roaming/Code"
```

Then, return to the [Create a project](#create-a-project) section.

### Intellisense C++ Standard
Intellisense is the feature that indicates compiler errors with red squiggly lines and suggests code completions.  If the C++ standard is out-of-date, you'll see squiggles where you shouldn't.

First, you should already have the `C/C++` extension installed ([Instructions](#extensions)).

Next, open VS Code's Command Palette with `View` > `Command Palette` (<kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>p</kbd>).  Search for and select `C/C++: Edit Configurations (JSON)`.  This will open the file `c_cpp_properties.json`.

<img src="images/vscode_wsl_150.png" width="480px" />

Modify the `cStandard` and `cppStandard` settings in `c_cpp_properties.json`.  Don't change any other settings.  Save the file.
```json
{
    "configurations": [
        {
            ...
            "cStandard": "c17",
            "cppStandard": "c++17",
            ...
        }
    ],
    ...
}
```
{: data-title="c_cpp_properties.json" data-highlight="5-6" }

### C/C++ extension alternatives
There are multiple options for C/C++ extensions.

[Microsoft C/C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) provides debugging support and intellisense on Windows, Linux and macOS.  At the time of this writing (January 2023) debug support has a [bug on macOS](https://github.com/microsoft/vscode-cpptools/issues/7240).

[CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) provides debugging support for those using the LLVM compiler.  Apple's compiler on macOS is based on LLVM.

[clangd](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd) provides intellisense and requires the `clangd` language server, which is related to the LLVM compiler.  We do not recommend installing the `clangd` extension with the Microsoft C/C++ extension because multiple intellisense providers can produce confusing results.

[WSL](https://code.visualstudio.com/docs/remote/wsl) lets us develop with Linux-based utilities like the `g++` compiler.

## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
