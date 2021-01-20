# Terminal graphics with curses #

Python provides character-mode terminal graphics via the curses library, which
is a wrapper for the curses C library. This guide is intended to replace the
official documentation, which I find to be difficult to navigate and use.

## Operating system variations ##

The original curses library is a Unix implementation. You can use Python curses
on most modern operating systems, but the implementation varies. I'm writing
this using Windows 10 Command Prompt. Your mileage may vary.

An example of implementation differences:

When running a curses application from Windows 10 Command Prompt, I have noticed
that the original state of the terminal is restored even when the application
terminates on an exception and the code that restores the terminal settings in
curses isn't run. I expect that other shells handle this differently, and that I
should continue to the practice of reversing everything I change in the terminal
settings before my application exits.

Don't assume that things work exactly the same from one environment to the next,
and build your own applications to gracefully handle the differences. When
possible, test your applications on as many different environments as you can.

## Getting started ##

The curses module is large. It provides lots of advanced functionality that
enables you to make complex user interfaces. The easiest way to get started is
to focus just on the essentials. You can very easily get to a state where you
can put text where you want it on the screen.

<!--
         10        20        30        40        50        60        70        80
    \    |    \    |    \    |    \    |    \    |    \    |    \    |    \    |
-->
