# Python quick notes #

## Regular expressions ##

All functionality is included in:

```python
import re
```

Perhaps the simplest way to use a regular expression is to find all instances of
a thing and replace them with another thing.

The following snippet compresses all of the whitespace characters in a string 
into single spaces. This is a good way to condense things, or to remove 
unexpectedly malicious mystery characters.

```python
myString = complicatedStringWithWhitespacesGalore

myString = re.sub(r'\s+', ' ', myString)
```

Let's break that down:

-   `r'\s+'` is the regular expression. In this case it means find any group of 
    at least one whitespace character. We need the `r` because backslashes mean
    different things in regexes than in Python, and that notation tells the 
    interpreter to treat everything in the quotes as literal characters.
    
-   `' '` is just the single space to substitute for what we find.

=   `myString` is the string to search.

## argparse ##
The argparse module is an abstraction of C-style command line arguments for Python

### What does it do? ###

The argparse module provides functionality that enables you to use command-line arguments with your Python scripts. As part of its processing, argparse gives you two other things for free: automatically generated help strings (including the automatic inclusion of -h and --help args) and argument validation and error handling. If a user enters a problematic argument, argparse automatically aborts your script and displays its usage to the user.

### The very basics ###

To use argparse, you must `import argparse`.

You then instantiate the parser and give your script a description that will be displayed when the user sees help text.

```python
parser = argparse.ArgumentParser(description="Does something " +
          "interesting")
```
<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
