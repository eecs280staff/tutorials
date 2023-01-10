---
layout: spec
title: Setup VSCode
sitemapOrder: 20
---

Setup up VS Code for C/C++
==========================
{: .primer-spec-toc-ignore }

[Visual Studio Code](https://code.visualstudio.com/) is a lightweight, easy-to-use, source code editor with debugging support.  It runs on macOS, Windows, and Linux (including CAEN Linux).  Visual Studio Code is not the same program as Visual Studio.

<div class="primer-spec-callout info" markdown="1">
If you already have VS Code installed with the C/C++ extensions, skip to the [Create a project](#create-a-project) section.
</div>

## Prerequisites
VS Code relies on external command line tools.  If you haven't installed CLI tools on your machine yet, follow one of these tutorials first.

| [macOS](setup_macos.html)| [Windows](setup_wsl.html) | [Linux](setup_wsl.html#install-cli-tools)

Next, follow our [Command line interface (CLI)](cli.html) tutorial.

## Restart
To start clean, first quit VS Code.  Back up your files, and then delete your project directory.  Your project directory might be different.

```console
$ pwd
/Users/awdeorio/src/eecs280
$ cp -a p1-stats p1-stats.bak  # Backup
$ rm -rf p1-stats              # Delete
```

## Install
Choose your platform below. Also make sure to install extensions.

### Linux
Install the .deb package from the web [https://code.visualstudio.com/docs/setup/linux](https://code.visualstudio.com/docs/setup/linux).

### CAEN Linux
VS Code is already installed on CAEN Linux desktop environment.  You can use it while sitting at a CAEN Linux computer, or through a [VNC connection to CAEN Linux](https://teamdynamix.umich.edu/TDClient/76/Portal/KB/ArticleDet?ID=4999).

### macOS
Make sure you have macOS 11.1 or later.
```console
$ sw_vers
ProductName:	macOS
ProductVersion:	11.7
```

Use the homebrew package manager to install VS Code.  You can run this command from any directory.
```console
$ brew install --cask visual-studio-code
```

### Windows
Make sure you have updated Windows and WSL installations according to the [WSL tutorial](setup_wsl.html).

Then, Install VS Code from the web [https://code.visualstudio.com/](https://code.visualstudio.com/).

Select "Add to PATH".

<img src="images/vscode005.png" width="480px" />

Reboot.  Open a terminal again (WSL/Ubuntu) and verify your installation.  Your version might be different.

```console
$ code --version
1.74.1
1ad8d514439d5077d2b0b7ee64d2ce82a9308e5a
x64
```

### Extensions
Make sure VS Code is installed correctly by checking the version.  You need version 1.52.1 or higher.
```console
$ code --version
1.52.1
```

Install the Microsoft [C/C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).
```console
$ code --install-extension ms-vscode.cpptools
```

Verify that the extension is installed.  It's OK if you have other extensions installed.
```console
$ code --list-extensions
ms-vscode.cpptools
```

#### macOS
Install the [CodeLLDB extension](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb).
```console
$ code --install-extension vadimcn.vscode-lldb
```

Verify that the extension is installed.  It's OK if you have other extensions installed.
```console
$ code --list-extensions
vadimcn.vscode-lldb
```

#### Windows
Install the [WSL extension](https://code.visualstudio.com/docs/remote/wsl) to develop with Linux-based utilities like the `g++` compiler.
```console
$ code --install-extension ms-vscode-remote.remote-wsl
```

Quit VS Code and start it again.

Verify that the extension is installed.  It's OK if you have other extensions installed.
```console
$ code --list-extensions
ms-vscode-remote.remote-wsl
```

You'll know that VS Code is running in remote mode when you see the remote mode indicator in the bottom left corner.

<img src="images/vscode069.png" width="768px">

## Create a project
To create a VS Code project, create a folder (directory).  There are many ways to create folders: Finder AKA File Explorer, VS Code interface, VS Code integrated terminal, and the system terminal.  We'll use the system terminal and call our example project `p1-stats`.

**macOS:** Open the Terminal application.

**Windows/WSL:** Open the Ubuntu application.

Navigate to the directory where you store your projects, create a new directory, then move into the new directory. Your folder location might be different.  Here's some help with [`cd`](cli.html#cd), the [tilde `~`](cli.html#home-directory-), and [`mkdir`](cli.html#mkdir).
```console
$ mkdir ~/Desktop/eecs280
$ cd ~/Desktop/eecs280
$ mkdir p1-stats
$ cd p1-stats
```

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Avoid paths that contain spaces.  Spaces causes problems with some command line tools.

| Bad Example     | Good Example   |
|-----------------|----------------|
| `EECS 280/` | `eecs280/` |
| `Project 1 Stats/` | `p1-stats/` |

</div>

Start VS Code and open your project folder by selecting `File` > `Open Folder...` > navigate to the `p1-stats` folder.

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** Here's a quick way to open VS Code to a specific project folder from the command line.  First make sure you're in the directory that contains your source code.
```console
$ ls
main.cpp ...
$ code .
```
</div>

<img src="images/vscode020.png" width="768px" />

### Add new files
Open your project folder by selecting `File` > `Open Folder...` > navigate to the `p1-stats` folder.

Select the add file icon and give it a name, e.g., `main.cpp`.

<img src="images/vscode023.png" width="768px" />

Alternatively, create your `main.cpp` file from the command line using [`touch`](cli.html#touch). 

```console
$ touch main.cpp
```

Copy-paste this Hello World program into your `main.cpp`.

```c++
#include <iostream>
using namespace std;

int main() {
  cout << "Hello World!\n";
}
```
{: data-title="main.cpp" }

### Add existing files
If you have starter files, add them to your project directory.  This example is from [EECS 280 Project 1](https://eecs280staff.github.io/p1-stats/), but this tutorial doesn't require understanding the files.  Your URL or files might be different.

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Make sure you're in the directory containing your source code.
```console
$ ls
main.cpp
```
</div>

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** [Copy/paste instructions](setup_wsl.html#copy-paste) for WSL.
</div>

We'll use the terminal to download, unpack, and move the starter files into the directory that already contains `main.cpp`.  Your URL or folder might be different.
```console
$ wget https://eecs280staff.github.io/p1-stats/starter-files.tar.gz
$ tar -xvzf starter-files.tar.gz
$ mv starter-files/* .
$ rm -rf starter-files starter-files.tar.gz
```

You should see your new files in your project directory.
```console
$ tree
.
├── Makefile
├── main.cpp
├── main_test.in
├── main_test.out.correct
├── main_test_data.tsv
├── p1_library.cpp
├── p1_library.h
├── stats.h
├── stats_public_test.cpp
└── stats_tests.cpp.starter
```

You should see your new files appear in VS Code.

<img src="images/vscode026.png" width="768px" />

#### Rename files
If you need to rename any files, you can do this from VS Code or from the command line.  In EECS 280, you'll need to rename any files that end in `.starter`.

Right click a file and select "rename".  Change the file name.  In EECS 280, you'll do this to any file that ends in `.starter`.

| <img src="images/vscode027.png" height="512px" /> | <img src="images/vscode028.png" height="512px" /> |

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** You can also rename files the command line, for example:
```console
$ mv stats_tests.cpp.starter stats_tests.cpp
```
</div>

## Compile and Run
VS Code uses an executable you build at the command line.

### Compile
{: .primer-spec-toc-ignore }

Compile and run your executable at the command line.
```console
$ touch stats.cpp  # Needed for EECS 280 P1
$ make main.exe
$ ./main.exe
Hello World!
```

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Make sure you're in the directory containing your source code.
```console
$ ls
main.cpp ...
```
</div>

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** If you're in EECS 280 and get an error like this, [add a new file](#add-new-files) `stats.cpp`.  It's OK if the file is empty for now.
```console
$ make main.exe
make: *** No rule to make target `stats.cpp', needed by `main.exe'.  Stop.
```
</div>

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** VS Code debugging will fail if there are no debugging symbols.  Double check the output of `make` and verify that you see `-g` being used in the commands.  The EECS 280 defaults include `-g`.
```console
$ make main.exe
g++ ... -g main.cpp ...
```
</div>

<div class="primer-spec-callout warning" markdown="1">
If you don't have a `Makefile`, you can compile manually.  We don't recommend this for EECS 280 students.
```console
$ g++ -g main.cpp -o main.exe
$ ./main.exe
Hello World!
```
</div>

### Create `launch.json`
{: .primer-spec-toc-ignore }

<div class="primer-spec-callout warning" markdown="1">
**WSL Pitfall:** Make sure you're in WSL mode.

<img src="images/vscode069.png" width="768px">

If you accidentally open VS Code from Windows mode, click on the green icon in the lower left hand corner and then select "Reopen Folder in WSL".

<img src="images/vscode070.png" width="720">
</div>

Select the file you would like to run.  Navigate to the debugging pane.

<img src="images/vscode030.png" width="768px" />

Click "create a launch.json file".

<img src="images/vscode031.png" width="768px" />

**macOS:** Select LLDB.

<img src="images/vscode031b.png" width="768px" />

You may need to click "Add Configuration".

<img src="images/vscode032.png" width="768px" />

You may need to select a "Launch" configuration.  This will create a default `launch.json` ([Microsoft Reference](https://code.visualstudio.com/docs/cpp/launch-json-reference)).
- macOS: "C/C++ (lldb) Launch"
- WSL orLinux: "C/C++ (gdb) Launch"

<img src="images/vscode033.png" width="768px" />

#### Edit `launch.json` program
{: .primer-spec-toc-ignore }

Edit the `program` and `cwd` fields in `launch.json`.  Save the updated file.  Your `program` name might be different.
```json
{
    "name": "(lldb) Launch",
    ...
    "program": "${workspaceFolder}/main.exe",
    ...
    "cwd": "${workspaceFolder}",
    ...
}
```
{: data-highlight="4,6" }

Your `launch.json` might be different.

<img src="images/vscode034.png" width="768px" />

<div class="primer-spec-callout info" markdown="1">
**Pro-tip:** VS Code puts its configuration files in a hidden directory called `.vscode`.  You can see hidden files with `ls -A`.
```console
$ ls -A
.vscode
main.cpp
...
```
{: data-highlight="2" }
</div>

### Run
{: .primer-spec-toc-ignore }

Click the triangle to run.  You'll see your program's output in the debug console.

<img src="images/vscode035.png" width="768px" />

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Remember to build your executable at the command line first.
```console
$ make main.exe
```
</div>


### Sanitizers
We recommend enabling the address sanitizer and undefined behavior sanitizer. These will help you find memory errors like going off the end of an array or vector.

First, edit your `Makefile` and add the `CXXFLAGS` recommended by the [ASAN Quick Start](setup_asan.html#quick-start).

Then, edit the `"environment"` property in your `launch.json`.  If there's already an empty `"environment": []`, replace it.

```json
  "environment": [
    {
      "name": "ASAN_OPTIONS",
      "value": "abort_on_error=1:detect_leaks=0"
    }
  ]
```
{: data-highlight="3-4" }

When ASan detects an error, VSCode will stop so that you can see the stack trace and inspect the current state of the program.  This configuration also turns off leak-checking (LSan), which can't run simultaneously with the visual debugger. If you do want to check for leaks, just run from the terminal with sanitizers enabled.

If you're debugging something else in your program and don't want it to terminate on ASAN errors, you can change to `abort_on_error=0`.

### Input redirection
<div class="primer-spec-callout info" markdown="1">
Skip this subsection your first time through the tutorial.  You can come back to it.
</div>

If you're unfamiliar with input redirection, first read the CLI tutorial section on [input redirection](cli.html#input-redirection-).

#### Windows/WSL or Linux `launch.json` changes

To configure input redirection, edit `launch.json`.
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

#### macOS `launch.json` changes

To configure input redirection, edit `launch.json`.
```json
{
    "configurations": [
        {
            ...
            "program": "${workspaceFolder}/main.exe",
            ...
            "MIMode": "lldb",
            "setupCommands": [
                {
                    "text": "settings set target.input-path main_test.in"
                }
            ]
            ...
        }
    ]
}
```
{: data-title="launch.json" data-highlight="10" }

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

## Debug
In this section, we'll set a breakpoint, which pauses the debugger.  Then, we'll cover some of the options to continue execution.

<img src="images/vscode_icon_step_over.png" style="vertical-align: text-top; height: 1.25em;" /> **Step Over**
Run one line of code, stepping _over_ any function calls by running the whole function in one step.

<img src="images/vscode_icon_step_in.png" style="vertical-align: text-top; height: 1.25em;" /> **Step Into**
Run one line of code, stepping _into_ any function calls to execute them line-by-line.

<img src="images/vscode_icon_step_out.png" style="vertical-align: text-top; height: 1.25em;" /> **Step Out**
Run the program until it returns from the current function (or until the next breakpoint).

<img src="images/vscode_icon_continue.png" style="vertical-align: text-top; height: 1.25em;" /> **Continue**
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
Select the file you want to debug.  Set a breakpoint by clicking to the left of a line number.  A breakpoint tells the program to pause.

<img src="images/vscode080.png" width="768px" />

### Run
Select the debugging pane, then run the debugger.  The program pauses at the breakpoint.  The yellow indicator highlights the next line of code to be run.

<img src="images/vscode090.png" width="768px" />

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Don't forget to compile!
```console
$ make main.exe                # With a Makefile
$ g++ -g main.cpp -o main.exe  # Without a Makefile
```
</div>

### Step over
Click "Step Over" a few times until you reach the highlighted line of code

<img src="images/vscode100.png" width="768px" />

### Inspect
Hover over a variable to inspect its value.  You can also see values in the VARIABLES pane.

<img src="images/vscode110.png" width="768px" />

<div class="primer-spec-callout warning" markdown="1">
If you have trouble viewing the contents of a container like this screenshot, see [Pretty Printing STL Containers with `gdb`](setup_gdb.html#pretty-printing-stl-containers-with-gdb).

<img src="images/vscode140.png" width="480px" />
</div>

### Step into
Click "Step Into".  The cursor enters the `sum()` function.

<img src="images/vscode115.png" width="768px" />

<img src="images/vscode120.png" width="768px" />

### Step out
Click "Step Out".  The `sum()` function completes, and the program pauses again.

<img src="images/vscode125.png" width="768px" />

<img src="images/vscode130.png" width="768px" />

### Continue
Press "Continue" to run the program to the next breakpoint, or the end, whichever comes first.

<img src="images/vscode135.png" width="768px" />

## Troubleshooting
This section is for common problems and solutions.

### Compile and run
If you have trouble with the [compile and run](#compile-and-run) section, a good first step is to delete your `launch.json` and try the [compile and run](#compile-and-run) section again.

<img src="images/vscode160.png" width="768px" />

### Intellisense C++ Standard
Intellisense is the feature that indicates compiler errors with red squiggly lines and suggests code completions.  If the C++ standard is out-of-date, you'll see squiggles where you shouldn't.

First, you should already have the `C/C++` extension installed ([Instructions](#extensions)).

Next, open VS Code's Command Palette with `View > Command Palette` or with the keyboard shortcut `ctrl + shift + P` on Windows or `cmd + shift + P` on macOS.  Search for and select `C/C++: Edit Configurations (JSON)`.  This will open the file `c_cpp_properties.json`.

<img src="images/vscode150.png" width="480px" />

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


## Acknowledgments
Original document written by Andrew DeOrio awdeorio@umich.edu.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with this document.
