# Python quick notes #

## Operators ##

### Mathematical ##

These are as you'd expect, including the C-style modulo (`%`).

Except that 'raise to the power of' is `**` not `^`.

There's also 'interger divide' (`//`) which returns an integer without 
remainder. This is always needed to get such a result in 3.x, but is the default
behavior for `/` in 2.x. However, you can get 3.x-style behavior in 2.x by 
using:

```python
from __future__ import division
```
### Relational ###

```python
x > y
x < y
x == y
X != y
```

### Logical ###

```python
logicand = x and y
logicor  = x or y
logicneg = x not y
```
## Control structures ##

### if, etc. ###

```python
if x == y:
    # do a thing
elif x > y:
    # do another thing
else:
    # do the default thing
```

Note that `pass` is a keyword that you can use if you want to set up a code
branch that doesn't do anything yet. The example above, with only comments in 
each branch, would cause errors.

Sadly, there is no case statement. Just use multiple `elif` clauses to do what
you need to.

### Loops ###

```python
while x > y:
    x -= 1
```

and

```python
for var in iterable:
    # do something with var
```

You can also use an `else` clause at the end of any loop to catch instances 
where the condition is not met even once.

A very common for loop is to do something a number of times, where the number of
iterations is not based on the number of items in a list:

```python
times = int(input('Enter a number of time to print spam: ')

for n in range(times):
    print('spam')
```

Note that the `range` function essentially returns a list of integers from 0 to
one less than the max value you pass to it. That's really important if you want 
to do something with the iteration variable.

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
## Sets ##

A set is a special kind of collection that you can use for set comparison 
operations. Python supports both `set` and `frozenset`, the the latter of which 
is identical to the first except it is immutable, and doesn't support the 
dynamic methods.

```python
topfivegames = {'Monsterhearts', 'Apocalypse World', 'Dungeon World', 
                'Kagematsu', 'Danger Patrol Alpha'}
                
pbta = {'Monsterhearts', 'Apocalypse World', 'Urban Shadows', 
        'Sagas of the Icelanders', 'Dungeon World'}
        
# And
topfivegames & pbta # {'Monsterhearts', 'Apocalypse World', 'Dungeon World'}

# Or
topfivegames | pbta # {'Kagematsu', 'Danger Patrol Alpha', 'Urban Shadows', 
                    #  'Monsterhearts', 'Apocalypse World',
                    #  'Sagas of the Icelanders', 'Danger Patrol Alpha'}
                    
# XOr
topfivegames ^ pbta # {'Kagematsu', 'Danger Patrol Alpha',
                    #  'Sagas of the Icelanders', 'Danger Patrol Alpha'}
                    
# In A, but not in B
topfivegames - pbta # {'Kagematsu', 'Danger Patrol Alpha'}
```

Membership testing (`in` and `not in`) is the primary boolean operation on sets.

**Gotcha** You can't use empty brackets to make an empty set the way you can 
with other collectioon types. That's becuase `{}` is interpreted as an empty 
dictionary. Instead, you create an empty set with the `set` constructor:

```python
newset = set()
```

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
## Multiple assignment ##

It isn't always how you might think it will work.

```python
x = 2
y = 4

x, y = y, x + y

x == 4
y == 6
```

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

-   `myString` is the string to search.

## argparse ##
The argparse module is an abstraction of C-style command line arguments for Python

### What does it do? ###

The argparse module provides functionality that enables you to use command-line
arguments with your Python scripts. As part of its processing, argparse gives you
two other things for free: automatically generated help strings (including the 
automatic inclusion of -h and --help args) and argument validation and error 
handling. If a user enters a problematic argument, argparse automatically aborts 
your script and displays its usage to the user.

### The very basics ###

#### Step 1: Make sure to import the module! ####
To use argparse, you must `import argparse`.

#### Step 2: Create your argument parser ####
You then instantiate the parser and give your script a description that will be
displayed when the user sees help text.

```python
parser = argparse.ArgumentParser(description="Does something " +
          "interesting")
```
#### Step 3: Define your arguments ####
You can define arguments in one of two ways: positional and optional.

Positional arguments are required and must be specified in the order defined. 
All you need to define them is an identifier name so that you can access them in
the parsed arguments object.

Optional arguments are, well, optional. They are also defined by the options, or
switches, that you use to specify them.

Here's what it looks like to invoke a script with two positional and two 
optional arguments:

```bash
$ myscript.py positional1 -o optional1 positional2 -t optional2
```
In both cases, you use `add_argument` to define an argument.

```python
parser.add_argument('apositionalarg', help='This positional argument is required')
parser.add_argument('-o', '--output', dest='output', default='regular',
                    help='Specifies the kind of output to use.')
parser.add_argument('-v', '--verbose', action='store_true', dest='verbose',
                    help='If set, causes the script to print detailed status.')
```
There are many other options for defining arguments, but these basic ones should
be enough to get you going in most simple cases.

#### Step 4: Parse the arguments ####
Because Python is interpreted, the arguments have been stored away waiting while
you've been describing them. Now you need to actually parse the arg string into 
an arguments object.

```python
args = parser.parse_args()
```
You can specify a list of args to parse, but under most circumstances you'll 
want to parse them all. Provided nothing bad happened, you should now have an 
object containing the values associated with all the arguments you defined.

#### Step 5: Use the arguments in your script! ####
The arguments object you got in step 4 has members for every parsed argument. 
The name of each member is either the name of the argument, or the value you 
used for `dest` in your definition.

```python
someVariable = args.apositionalarg
if output != True:
    doSomethingImportant()
```

## List comprehensions ##
List comprehensions are essentially queries into lists. It's a syntaxt that 
equates to a new list contianing the item from an existing list (or lists).

A really simple use is to make a list out of one item in each of a list of 
tuples:

```python
# Here's a list of tuples, each containing a string, an int, and a float.
myList = [('something', 35, 65.3), 
          ('another thing', 98, 103.9), 
          ('yet another thing', 61, 341.0)]

# Let's use list comprehensions to make a list of just the ints.
ints_in_myList = [entry[1] for entry in myList]
```
But you can do much more complicated things with them!

Let's say I want to make a list of all of the names in one list that are not in 
another list:

```python
# Here's the first list of names
names1 = ['John', 'William', 'Francis', 'Robert']

# And the second
names2 = ['Francis', 'Robert', 'Harold', 'Egbert']

# Now let's find every name in the second list that's not in the first!
names2exclusive = [name for name in names2 if name not in names1]
```

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
