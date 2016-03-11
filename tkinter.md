# GUI with tkinter #
There are several different GUI libraries available in Python, but tkinter is 
the one that ships with the standard library, and is extremely common in use.

## Widgets! ##
Every UI element supported by tkinter is a widget. Widgets have a formal hierarchy, with each having a parent widget and zero or more children.
The exception is the root widget, which is an application window.

Each widget has configuration parameters, which are stored in a dictionary. Let's take a look at a simple app (that doesn't really do anything):

```python
from tkinter import *
from tkinter import ttk # Themed versions of widgets to match the OS.

root = Tk()
button = ttk.Button(root, text='Click Me')
button.pack()
```

You can see that one of the parameters of the button widget is `text`, which we set when we created the button. You can also change configuration after creation, either by using the configuration parameter's name 

```python
print(button['text'])

button[text] = 'Press Me'

# Or use the config method

button.config(text='Push Me')

# Return all the configuration objects
print(button.config())
```

## Widget names ##
Widgets have tcl names that are assigned automatically by tkinter. Every widget is added to a hierarchy of widgets, so they aren't garbage collected until the root object goes out of scope.

You can get the tcl names for objects, which are randomly assigned, by casting the object as a string. The root is always named '.'.

## Geometry management ##
The issue of where widgets are placed relative to other widgets is a complicated one, and it is handled by the geometry manager.

Widgets have a master/slave relationship. Master widgets control the placement of their slaves.

Three geometry managers to choose from:

1.  Pack: simplest and most automated. You can specify an edge of the parent 
    widget on which to pack the widget.
2.  Grid: defines rows and columns for the parent widget, and arranges the 
    children on that grid.
3.  Place: lets you use relative or absolute coordinates to place each widget 
    within its parent.

You can use multiple geometry managers within a single application, but you must use a single manager for each parent object.

## User events ##
Like Windows programming, tkinter uses events that you listen for to process user input and other interactions.

You start your event handling with the `Tk.mainloop` function.

Keep handlers short and sweet to avoid UI lag.

Some widgets have command callbacks that you set as properties of their objects, while others (like labels) don't have an explicit command associated with them. Those widgets can still be handle events by using the `bind` method.

## Labels ##
Contains text or an image.

properties:
<table>
    <tr>
        <td>text</td>
        <td>the text to display</td>
    </tr>
    <tr>
        <td>wraplength</td>
        <td>the number of pixels at which to wrap the text</td>
    </tr>
    <tr>
        <td>justify</td>
        <td>[LEFT, RIGHT, CENTER]</td>
    </tr>
    <tr>
        <td>foreground</td>
        <td>text color, use hex value or name of common colors in a string</td>
    </tr>
    <tr>
        <td>background</td>
        <td>background color</td>
    </tr>
    <tr>
        <td>font</td>
        <td>`font = ('Courier', 18, 'bold')`</td>
    </tr>
    <tr>
        <td>image</td>
        <td>
            <p>
                takes a PhotoImage object containing the image you want to use
            </p>
            <p>
                You need to first create a PhotoImage object, by passing it the filename of the image to use: `my_image = PhotoImage(file='C;\\Path\\file.gif')`
            </p>
            <p>
                <strong>NOTE</strong>&nbsp;&nbsp;&nbsp;When you assign an image to a label, it does not make a copy or hold a reference to the image object. Instead, it uses the address. This means that when the PhotoImage variable goes out of scope, Python will discard the variable and the image won't work anymore. Be careful where you hold the reference to the image to keep everything working.
                A good practice is to store a reference to the image object in the label object, taking advantage of Python's flexible objects:
                <code>my_label.img = my_image</code>
            </p>
        </td>
    </tr>
    <tr>
        <td>compound</td>
        <td>
            <p>
                ['text', 'image', 'center', 'top', 'bottom', 'right', 'left']
            </p>
            <p>
                configures how to deal with labels with both text and image values, either showing just one of them, centering the text atop the image, or placing the text next to the image on the specified side.
            </p>
        </td>
    </tr>
</table>

## Buttons ##

```python
root = Tk()
button = ttk.Button(root, text='Click Me')
button.pack()

def callback():
    print('Clicked!')

button.config(command=callback)
```

You can simulate a button click by calling the `invoke()` method.

`button.state(['disabled'])`

`button.instate(['disabled'])` returns Boolean.

properties:
<table>
    <tr>
        <td>
            command
        </td>
        <td>
            The function that gets called whenever the button is clicked.
        </td>
    </tr>
    <tr>
        <td>
            
        </td>
        <td>
        </td>
    </tr>
    <tr>
        <td>
        </td>
        <td>
        </td>
    </tr>
    <tr>
        <td>
        </td>
        <td>
        </td>
    </tr>
    <tr>
        <td>
        </td>
        <td>
        </td>
    </tr>