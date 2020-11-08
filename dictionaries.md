# Dictionaries #

Dictionaries in Python are associative data structures or unordered collections
of key/value pairs. Keys are strings, values can be any data object, including
complex, nested collection types.

Use a dictionary if:
-   Every entry in your data set has a unique key.
-   The order of your records is unimportant.
-   The purpose of your data structure is primarily random access lookup.

## Dictionary basic syntax ##

### Definition ###

Dictionary literals are defined by putting the key/value pairs in curly braces.
Item names (keys) are string values. Item data is any valid data literal.

```python
# Prisoner record dictionary.
prisoner = {"last_name": "Valjean",
            "first_name": "Jean",
            "id": 24601,
            "crimes": ["theft", "vandalism", "escape"],
            "imprisonment": "19051796",
            "parole": "10101815"}
```

### Item access ###

You can access items in a dictionary by putting its name in square brackets
appended to the dictionary's identifier, just like you do with an index in list
offset notation.

```python
print("{}, {}".format(prisoner["last_name"], prisoner["first_name"]))
```

Or you can use the `get()` method:

```python
fugitive_id = prisoner.get("id")
```

## Tips and tricks ##

### Getting a random item ###

You can't use `random.choice()` directly to get an item, key, or value. But that
doesn't mean you can't use it! You have to first cast the view that you want as
a list:

```python
city_info = {"New York": {"State": "New York", "County": "New York"},
             "Los Angeles": {"State": "California", "County": "Los Angeles"},
             "Chicago": {"State": "Illinois", "County": "Cook"},
             "Houston": {"State": "Texas", "County": "Harris"},
             "Phoenix": {"State": "Arizona", "County": "Maricopa"}}

random_city = random.choice(list(city_info.keys()))
```
        




<!--     10        20        30        40        50        60        70        80
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
