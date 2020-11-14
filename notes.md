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
-   [String formatting](#string-formatting)

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
## Preliminaries ##
Python is at once an easy language to pick up and an incredibly deep one to use.
It consistently tries to be both readable and clever, which causes a fair amount
of dissonance. 

### Versions ###
Python has been split for quite a while into two release branches, 2.7.x and 3.x.
Python 3 was created as a bit of a refactoring to better handle some things
about how the language worked. Bu by the time they got around to making 3, there
was an enormous amount of code written against the "old-style" Python. Adoption
has been slow going, partly because there are so many little packages out there 
don't support 3.x for one reason or another. 

I've written this guide with 3.x in mind, but in places I'll point out something
that varies between the the versions. You can often get the newer version
even while using 2.7.x by using `from __future__ import [something]`. For 
example, you might call `from __future__ import division` to enable real 
division (int / int = float).

### Style ###
Readability is a big part of Python. The style for Python code is defined in [PEP (Python enhancement proposal) 8](https://www.python.org/dev/peps/pep-0008/).

## Operators ##

### Mathematical ###

| Operation          | Operator | Notes                                   |
| ------------------ | -------- | --------------------------------------- |
| Addition           | `+`      | Also used for concatenation of strings. |
| Subtraction        | `-`      |                                         |
| Multiplication     | `*`      |                                         |
| Division           | `/`      | Resolves to a `float`.                  |
| Floor division     | `//`     | Resolves to `int`. No remainder given.  |
| Remainder division | `%`      | Resolves to modulo.                     |
| Raise to power     | `**`     | `x ** y` is x to the power of y.        |

Python also provides built-ins for some math functions. These are called like
functions, but share a lot in common with operators.

| Operation           | Built-in        | Notes                                      |
| ------------------- | --------------- | ------------------------------------------ |
| Absolute value      | `abs(x)`        | Returns the absolute value of `x`.         |
| Divide with modulus | `divmod(x, y)`  | Returns a tuple: `(x / y, x % y)`.         |
| Raise to power      | `pow(x, y[,z])` | Equivalent to `(x ** y)[% z]`, but faster. |

### Bitwise ###

Bitwise operations are only valid with `int` data.

| Operation | Operator         | Example<sup>1</sup>                    |
| --------- | ---------------- | -------------------------------------- |
| And       | `&`              | `0b00110011 & 0b01010101 == 00010001`  |
| Or        | `\|`             | `0b00110011 \| 0b01010101 == 01110111` |
| ExOr      | `^`              | `0b00110011 ^ 0b01010101 == 01100110`  |
| Invert    | `~`              | `~0b00110011 == 11001100`              |
| L shift   | `<<`<sup>2</sup> | `0b1100 << 2 == 0b0011 (3 << 2 == 12)` |
| R shift   | `>>`<sup>3</sup> | `0b0011 >> 1 == 0b0110 (12 >> 1 == 6)` |

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->

<sup>1</sup> Examples use 8-bit values for readability. Python integers are
complicated in their implementation, with no practical maximum value, but always
much larger than 8 bits.

<sup>2</sup> Through the magic of Python, you can shift integers up to basically
infinite values without affecting the sign. Don't worry about it, just let it be.

<sup>3</sup> Remember that shifting down drops bits off the end, so `3 >> 1` is
equivalent to `3 // 2` not `3 / 2`.

But, really, what are you doing bitwise operations in Python for in
the first place?!

### Relational ###

| Operation           | Operator | Example   | x = 10, y = 4 | x = 4, y = 10 | x = "Z", y = "Z" |
| ------------------- | -------- | --------- | ------------- | ------------- | ---------------- |
| Greater than        | `>`      | `x > y`   | `True`        | `False`       | `False`          |
| Less than           | `<`      | `x < y`   | `False`       | `True`        | `False`          |
| Equal to            | `==`     | `x == y`  | `False`       | `False`       | `True`           |
| Not equal to        | `!=`     | `x != y`  | `True`        | `True`        | `False`          |
| Greater or equal to | `>=`     | `x >= y`  | `True`        | `False`       | `True`           |
| Lees or equal to    | `<=`     | `x <= y`  | `False`       | `True`        | `True`           |


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

## Control structures ##

### Branching ###

#### if statement ####
The `if` statement, along with its permutations, is the only branching control
supported in Python. The lack of a switch statement is one of the tragedies of
Python, IMO. Here's the if:

```python
if x == y:
    # do a thing
    pass
elif x > y:
    # do another thing
    pass
else:
    # do the default thing
    pass
```

Note that `pass` is a keyword that you can use if you want to set up a code
branch that doesn't do anything yet. Using it as above lets you run your
code without errors, because normally, Python requires there to be code in a
defined block.

#### Inline if ####

You can use `if` as an expression instead of a statement, which ends up being 
like the ternary operator in C and other languages:

```python
# Let's say there's a variable that might be populated, but might not.
returnCode = None # Later this might get set, depending on async stuff.
do_some_stuff("...")
print('Code: {}.'.format(returnCode) if returnCode else "No return code yet.")
```

As with ternary operators in all languages, only use it when it avoids code
clutter to a degree that outweighs its potential to confuse.

#### Value or ####

The inline `if` example in the previous section is pretty verbose. For complex
apllications, it adds a lot of text to a statement. To simplify one of the most
common uses, Python overloads the `or` operator. When you use `or` in cases
where on or both operands are not Boolean values, the expression resolves to
the left value, if that value is non zero, or to the right value if it is. Some
examples:

```python
empty_int = 0
empty_str = ""
empty_list = []
unreferenced_var = None

my_number = empty_int or 33
my_number == 33

my_string = empty_str or "Hubbub"
my_string == "Hubbub"

my_list = empty_list or [4, 6, 8, 10, 12, 20]
my_list == [4, 6, 8, 10, 12, 20]

my_ref = unreferenced_var or my_number
my_ref is my_number == True
```

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
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
metro = []
metro.extend(regional)

metro.remove(360)
metro.remove(253)
```


Strangely, `metro = list().extend(regional)` doesn't cause an error, but results
in `metro` being created and set to `None`. Not even to an empty list?

Here's a handy trick:

```python
my_big_list = [0] * 100
```

The multiplication operator is overloaded to, in this case, create a big list 
that is 100 items matching the item listed.

#### List enumeration ####

The default behavior of the `for` loop in Python is designed to enumerate a list
or other iterable object.

```python
for thing in things:
    print(thing)
```

This works great and is very readable when what you need is just the value of
each element as you iterate through the list. Sometimes, though, you need to
access the index so that you can do offset math to access other elements in
the list. The simplest way to do that is to iterate through a range set to the
length of the list:

```python
for i in range(len(things)):
    if i != 0:
        if things[i] > things[i - 1]:
            print("{} is greater than {}.".format(things[i], things[i - 1]))
        elif things[i] < things[i - 1]:
            print("{} is less than {}.".format(things[i], things[i - 1]))
        else:
            print("{} is equal to {}.".format(things[i], things[i - 1]))
```

That approach is a bit less readable that ideal. So Python includes the
built-in function `enumerate`, which returns a list of two-element tuples, the
first element of which contains the index, and the second contains the element
of the original list.

```python
for i, thing in enumerate(things):
    if i != 0:
        if thing > things[i - 1]:
            print("{} is greater than {}.".format(thing, things[i - 1]))
        if thing < things[i - 1]:
            print("{} is less than {}.".format(thing, things[i - 1]))
        else:
            print("{} is equal to {}.".format(thing, things[i - 1]))
```

In this example, it's maybe not significantly easier to follow, but every little
bit counts!

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->



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

And you can do other tricky things besides finding sublists inside lists:

#### Create a list from a template and an index number

Sometimes you want to name a series of things with a name and an incremental
number, like 'file-1', 'file-2', ... 'file-n'. List comprehensions make this
ridiculously easy:

```python
files1to10 = ['somefilename-{}.txt'.format(i) for i in range(1, 11)]
```

#### Create matrices

Here's a problem: you want to make a matrix (a 2-d array). How would you do it?

You can try to take advantage of the handy listifying overload of the asterisk,
like this:

```Python
tictacboard = [[0] * 3] * 3
```

That should give you a list of eight lists, each with eight zeros. If you look
at the string representation, it looks like everything worked.

```
>>> tictacboard
[[0, 0, 0], [0, 0, 0], [0, 0, 0]]
```
But if your try to set a single position, you get some trouble.

```Python
>>> tictacboard[1][1] = 'x'
>>> tictacboard
[[0, 'x', 0], [0, 'x', 0], [0, 'x', 0]]
```

What the heck? Let's think about how Python variables work in order to puzzle this out. We know that `[0] * 3` gives us a three-element list populated with zeros. But when you try to use * to make copies of a list it doesn't work. This is because [0] is a list containing a numeric literal, while [[0] *  n] 


```Python
chessboard = [[0] * 8 for i in range(8)]
```

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
### String formatting ###
In addition to easy formatting:

```python
theAnswer = 42
theStatement = '{} is the answer to life, the universe, and everything'.format(theAnswer)

# OR

foods = ['Spam', 'egg', 'bacon']
menuItem = '{0}, {1}, {0}, {0}, {2}, and {0}'.format(food[0], food[1], food[2])
```
Note&nbsp;&nbsp;When using an identifier with string formatting, it needn't be
confined to an index as it is in the example above. You can provide any valid
identifier, and then specify them in the args to `format()` just like finction
params. `'Welcome {username}! Your session id is {session}.',format(username='sdoo', session='42')`
You can use [dictionary unpacking here](#dictionary-unpacking) here, too!

Python supports a more complicated string formatting system, based on C-style 
string formatting (ala `sprintf`)

```python
theAnswer = 42
theStatement = '%d is the answer to life, the universe, and everything' % (theAnswer)
```

You can use this formatting to bulk format and make a list:

```python
someNums = range(33, 51)
myFilenames = ['jel-log%d.txt' % num for num in someNums]
```

Notice the similarity in form to list comprehension (I bet this is considered
simple list comprehension, though it seems like an advanced edge case to me).

<!--
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
