---
layout: spec
title: Debugging Guide
sitemapOrder: 30
---

Debugging Guide
==========================
{: .primer-spec-toc-ignore }

TODO intro

TODO disclaimer that some code is blurred, functions shown are not necessarily implemented well

TODO specific examples from projects are used, but you don't need to be familiar with the project to understand the example

## Inspect Variables


<div class="primer-spec-callout info icon-info" markdown="1">
Use the **variables panel** to inspect the values of variables while your debugger is paused.
</div>

**Example, Project 3 - Euchre**  
There's a lot to keep track of in a game of Euchre. I'd like to check that everything seems in order after I've dealt out the cards and the players have finished making.

When the debugger pauses at my breakpoint, I can see what's in my local variables:

<img src="images/debug_feature_variables_0.png" width="700px" />

You can see some variables directly. For example, `trump` is currently `"Hearts"`.

Variables may contain "memory junk". `i` is currently `-11632`. That's expected in this case, because we just haven't run the line `int i=0;` quite yet. In other cases, bogus values might indicate a bug!

You can click on more complex variables like pointers, arrays, or classes to inspect their contents. Below, I've expanded the `leader` variable to see the `Player` it is pointing to, then also expanded the `cards` array to take a look at their current hand.

<img src="images/debug_feature_variables_1.png" width="700px" />

<div class="primer-spec-callout warning" markdown="1">

**Heads up!** You may need to configure your debugger to ensure full information is shown for polymorphic objects through pointers. In the example above, the `leader` variable is a `Player*` pointing to a `SimplePlayer` object. By default, the debugger may only show the base class `Player` data.

To fix this in VS Code, run `-exec set print object on` from the debug console, or add the following to the `setupCommands` in your `launch.json` configuration:

```json
  "setupCommands": [
      ...
      {
        "description": "Print polymorphic objects according to dynamic type",
        "text": "set print object on"
      }
      ...
  ]
```
{: data-variant="legacy" }
</div>

In **member functions**, you can also open up the `this` pointer. In our Euchre example, we'd see member variables from the current `Game` class instance:

<img src="images/debug_feature_variables_2.png" width="700px" />

## Evaluate Expressions

<div class="primer-spec-callout info icon-info" markdown="1">
Use the **debug console** to evaluate expressions while your debugger is paused.
</div>

Let's say I'm working on project 2 and I get a segfault from dereferencing `ptr` in my `Matrix_max()` function below. My debugger pauses when the sefault occurs.

I can see `ptr` holds the address `0x55555586e000` and `size` is `25` in the variables window.

But, I don't know how far `ptr` is from the beginning of the array. For this, I enter `ptr - mat->data` at the debug console.

<img src="images/debug_feature_console_0.png" width="700px" />

The debugger evaluates that pointer subtraction for me, which confirms that the pointer is far out of bounds.

<img src="images/debug_feature_console_1.png" width="400px" />

That suggests the loop is effectively infinite, and only stops when the pointer travels far enough to segfault. Looking back at my code (see above), I find that the loop condition `ptr < ptr + size` is incorrect.

You can enter almost any valid C++ expression at the debug console - even function calls! Take a look below - we've called `Matrix_print()` from the debug console and the result shows at the terminal.

<img src="images/debug_feature_console_2.png" width="800px" />


## Diagnosing Crashes

You can use a debugger to assess the cause of a crash or runtime error, including:

- Failed debugging assertions
- Undefined behavior detected by sanitizer tools
- Segmentation faults
- Unhandled exceptions
- And more!

Basically, if your code is doing something bad:
1. Run through the debugger. No breakpoints needed.
2. The debugger pauses when the error occurs.
3. You look at local variables, the call stack, etc. to diagnose the issue

Here's several examples:

**Basic Example**  
I'm working on project 3 and run my own `Player_tests.exe` via the terminal:

```console
$ ./Player_tests.exe
Running test: test_get_name
Segmentation fault
```

My code crashed with a segmentation fault, but I don't know much else.

Let's run in a debugger and let it crash there. No need to set any breakpoints.

<img src="images/debug_crash_0.png" width="800px" />

TODO, maybe another example where you need to look at the calling context via the call stack

**Crash in Library Code**  
TODO

**Crash in Implicitly Defined Functions**  
There are some implicitly-defined functions that compiler provides for you, for example an implicitly-defined constructor or assignment operator for a class.

What happens if a crash occurs in these functions? Because these functions aren't present in your source code, the compiler just highlights the top line of the corresponding class definition:

<img src="images/debug_crash_3.png" width="800px" />

In this case, check the call stack. First, observe that the segfault did occur in an implicitly-defined function, the built-in `Card::operator=` assignment operator for copying a `Card`.

<img src="images/debug_crash_4.png" width="800px" />

Now, click the next stack frame below to see the where that operator was used. It was in our `Pack` constructor. That code doesn't seem right...let's just assume "my partner wrote that".

<img src="images/debug_crash_5.png" width="800px" />

In all seriousness, taking a look at the _calling_ stack frame(s) allows me to see where the problem originated and is generally sufficient to figure out what part of _my_ code was responsible for causing the implicitly-defined function to crash.





## DEBUG Print Statements

<div class="primer-spec-callout info icon-info" markdown="1">
In _some_ situations, adding **DEBUG print statements** may be more efficient than manually stepping line-by-line with a debugger. For example, debugging a loop across many iterations or generating verbose output for quick inspection.
</div>


<div class="primer-spec-callout info icon-info" markdown="1">

**Tip:** When adding print statements for debugging:  
- Use a prefix like `"DEBUG: "` to clearly distinguish from other output.
- Take some time to format them nicely so it's easier to see what's going on.
- Use `endl` for newlines (don't use `\n`). This guarantees you see the output, even if your code crashes.
- Comment out your debug outputs once you're done (rather than deleting them). You might use them again later!
</div>

**Example, Project 3 - Euchre**  
I'm getting the wrong output in my project 3 Euchre program on a public test.

Let's look at my output vs. the sample correct output in the VS Code diff checker. The players are playing the right cards, but the wrong person takes the trick.

<img src="images/debug_print_0.png" width="800px" />

I've also identified the relevant portion of my code:

<img src="images/debug_print_1.png" width="500px" />

I might be tempted to dive in with my debugger, but it will take a while to step to the right place (especially if the first mismatch was even farther into the game). I could also get a bit lost trying to manually walk through the card comparisons one-at-a-time.

We could get the same information quickly with some debug printouts:

<img src="images/debug_print_2.png" width="800px" />

Then, scan the output for the relevant portion:

<img src="images/debug_print_3.png" width="600px" />

The _Queen of Clubs_ is chosen to beat out the _Ten of Diamonds_, which is wrong. The _Ten of Diamonds_ is better because _diamonds_ is the led suit. Ah ha - we may notice that the wrong version of `Card_less()` is used in the code above, which doesn't consider the led suit.






