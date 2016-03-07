# Writing error-resistant Python code #
I was taught an unyielding approach to anticipating and checking for errors in my code. The Pythonic way is less rigid. You should only perform error checking when there is a reasonable possibility for errors. This file is a place for me to record what I learn about making my code as bulletproof as practical.

## Parameter validation ##
When taking parameters for your functions, think about where the data is coming from, and validate appropriately.

### Static type checking ###
Sometimes it is important to know exactly what type of object you're getting. You can verify this with a simple type assertion:

```python
def name_cat(last_name, first_name):
    """
    Concatenates a first and last name into a single string with a space
    separating the two.
    """
    assert type(last_name) == str, 'Only a string can be used for a name.'
    assert isinstance(first_name, str), 'Only a string can be used for a name.'

    full_name = first_name + ' ' + last_name

    return full_name
```

I used two different ways to do static type checking in the example, the `type` function, and the `isinstance` function. For a simple example checking for a string, either of these will work just fine. In practice, `isinstance` is vastly preferable for two reasons:

1.  It returns true if the object is the specified type, or if it is a 
    subclass of that type.
2.  It can take a tuple of class names and will return true if the object is 
    an instance of any of them.

### Duck typing ###
Duck typing is the programming lingo for dynamic type checking that verifies that an object implements certain methods, instead of checking its literal type. If it walks like a duck, etc.

One way to do simple duck typing is to `try` a call to a method and catch any `AttributeError` exceptions, raising them up the stack as `TypeError` exceptions.





