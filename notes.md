# Python quick notes #

## Contents ##

-   [Operators](#operators)
    -    [Mathematical](#mathematical)
    -    [Relational](#relational)
    -    [Logical](#logical)
-   [Basic I/O](#basic-i-o)
-   [Control structures](#control-structures)
    -   [if, etc.](#if-etc-)
    -   [Loops](#loops)
-   [Collection types](#collection-types)
    -   [Tuples](#tuples)
    -   [Sets](#sets)
    -   [Lists](#lists)
    -   [Dictionaries](#dictionaries)

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
x != y
x >= y
x <= y
```

### Logical ###

```python
logicand = x and y
logicor  = x or y
logicneg = x not y
```
## Basic I/O ##

When writing console applications, you can use `input()` and `print()` for input
and output.

The `input` function permits you to include a prompt string as its parameter:

```python
try:
    user_int = int(input('Enter an integer: '))
except ValueError:
    print('That wasn\'t an integer!')
```

The `print` function is pretty straightforward&mdash;it prints a string to
the console. By default, `print` ends whatever string you specify with a newline
character so that each print call starts its output on a new line. You can 
specify a different set of characters to end the output with y using the `end`
parameter:

```python
for i in range(10):
    print(i, end=', ')
else:
    print(10)
```

Which produces this output:

```
0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
>>>
```

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
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

## Collection types ##
One of Python's strengths is its support for a lot of collection types.

### Tuples ###

The faster type of arbitrary collection in Python, tuples are immutable, 
ordered, iterable lists of arbitrary objects.

```python
coords = (23, 42)

coords[1] == 42 # True

coords[1] = 56 # Raises a TypeError
```

Even though they are immutable, tuples can contain mutable values:

```python
color_scheme = ([0, 0, 120], [255, 255, 0], [100, 180, 76])

color_scheme[0][0] = 255 # Works fine!

color_scheme[0] = [255, 0, 120] # TypeError
```

### Sets ###

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
### Lists ###

More or less the default collection type in Python. Mutable, ordered, but slower
than a tuple.

```python
mylist = []
mylist.append('thing1')
mylist.append('thing2')
mylist.extend(['thing3', 'thing4', 'thing5'])

# Or we could create the list with everything in it:
mylist = ['thing1', 'thing2', 'thing3', 'thing4', 'thing5']
```

For reasons that are somewhat opaque, assigning a list to another list is by
reference. This can be annoying if you forget:

```python
animals = ['ant', 'wolverine', 'bat', 'crocodile', 'penguin', 'cat']

mammals = animals
mammals.remove('ant')
mammals.remove('crocodile')
mammals.remove('penguin')

print(animals)
# ['wolverine', 'bat', 'cat'] uh oh! We changed the parent list!
```

There are a couple of ways around this:

```python
regional = [206, 425, 253, 360]

metro = regional[:]
# OR
metro.extend(regional)

metro.remove(360)
metro.remove(253)
```

Here's a handy trick:

```python
my_big_list = [0] * 100
```

The multiplication operator is overloaded to, in this case, create a big list 
that is 100 items matching the item listed.

### Dictionaries ###

Unordered key-value pairs

```python
colors = {'red': (255, 0, 0), 'green': (0, 255, 0), 'blue': (0, 0, 255)}

# Get an entry by key name:
print(colors['red'])

# Add a new entry:
colors['cyan'] = (0, 255, 255)
colors['lime'] = (128, 255, 0)

# Delete individual entries:
del(colors['lime'])

# Get a list of the keys in the dictionary:
print colors.keys()

# Looping through all is easy:
for color in colors:
    print('{}: {}'.format(color, colors[color]))

# Membership checking:
if 'lime' not in colors:
    print('Oh, yeah, we deleted that!')
```

Trying to access a key that doesn't exist results in a `KeyError` exception. One way to avoid this is to check membership, but you can also use the `get()` method:

```python
magentaRGB = colors.get('magenta')

# Now magentaRGB == none, or you can shortcut further:

oliveRGB = colors.get('olive', 'color undefined')

# Now magentaRGB == 'color undefined'
```

Even though dictionaries are unsorted, you can get a list of the keys sorted
alphabetically with `sorted()`:

```python
print(sorted(colors))
```

Keys don't have to be strings, but they need to be immutable objects. However, 
Python expects string keys&mdash;so much so that you can omit quotes for key 
names if they conform to variable name conventions:

```python
colors = {red = (255, 0, 0), green = (0, 255, 0), blue = (0, 0, 255)}
```

You can also use the `dict()` constructor with a list of tuples to create your
dictionary (but this is generally considered less pythonic):

```python
colors = dict([('red', (255, 0, 0)), ('green', (0, 255, 0)), ('blue', (0, 0, 255))])
```

#### Dictionary unpacking ####
Python has a magical mechanism in place to assemble the arguments for a function
in a dictionary:

First, you make a dictionary with its labels named the same as the arguments of 
the function you're collecting them for:

```python
argDict = {'first':'Scoobert', 'last':'Doo', 'species':'canis lupus familiaris'}
```

Then you call the function, passing the dictionary's name as the argument, 
preceded by the special `**` operator:

```python
def printCritter(first, last, species):
    print('First name: {}'.format(first))
    print('Last name: {}'.format(last))
    print('Species: {}'.format(species))
    
...

printCritter(**argDict)
```

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
## Using wildcards in filenames (glob) ##

The `glob` library supports wildcard expansion for filenames:

```python
import glob

my_path = 'C:/*.sys'

# Let's print all the .sys files in the root directory.
for filename inglob.glob(my_path):
    print(filename)
```

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
