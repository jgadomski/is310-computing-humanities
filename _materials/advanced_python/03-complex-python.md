---
title: "Advanced Python: Part 2"
permalink: /materials/advanced-python/03-complex-python
toc: true
---

So far we've learned how to store data in different data types (integers, strings, floats, etc...) and structures (lists and dictionaries). We've also learned how to use for loops to manipulate our data, conditionals to change the data flow, and functions to encapsulate our code and make it reusable and generalizable.

Now that we've started to learn the range of Python syntax, we can start putting it all together. One of the main ways we can do that is with `Classes`.

Let's take a look at code from the Advance Python Part 1.

```python
def make_tool_dict(name, value_2015):
    return {
        "2015":value_2015 ,
        "name":name,
    }

dh_tools= {
    "tool1": make_tool_dict("Python",9),
    "creator1": "Guido van Rossum"
}
```

This code snippet contains a **function** called make_tool_dict that returns a **dictionary** with the name and value from 2015 for a DH tool. We call this function inside of the dh_tools **variable**.

Right now this code works well if we just have one script, but what if we wanted to build upon this code to add more fields and information about the tools, and maybe even reuse this code in multiple files? Then we might want to consider rewriting the code into a **class**.

While dictionaries are great for describing complex data within a single object, a class is really useful to encapsulate both data and functions in a single cohesive unit.

A class is a particular type of **objects**. We can create ("instantiate") an object of a class and store it as a variable. A variable therefore contains (technically, contains a reference to) an *instance* of a class.

We've already used a lot of built-in classes. Strings, lists, dictionaries are all classes. We can have Python tell us what kind of class it is using the built-in function `type()`.

```sh
>>> a = [1,2,3]
>>> type(a)
<class 'list'>
>>> b = "123"
>>> type(b)
<class 'str'>
```

## Classes

Python classes are great for storing complex data structures, but they can also be simple.

Here's how you define a simple class that does nothing.

```python
# Define a class
class noop:
    pass  # Pass means "Move along, please. Nothing to see here."

# Create an instance of the class and invoke it
noop()
```

To first define a class, we need to use the keyword `class` followed by the name of the class (which can be anything!) and then a semi colon. All the logic of the class is indented (just like with functions, loops, and conditionals) and then we call the class similar to a function, using its name and parentheses to pass arguments to the class.

In this case, since the class has no actual logic in it, just the keyword `pass`, nothing actually happens. For any class, when you invoke it, it executes the `__init__` method, which is another built in method from Python (this one is called a **constructor** method). Thus, since our example above didn't define any logic for the built-in `__init__` method, nothing happened.

## Simple Class

Let's define a class that actually does something when it's initialized.

```python
class DHTool:
    def __init__(self, name):
        self.tool_name = name

a_tool = DHTool("Python")
a_tool.tool_name
```

So similar to our initial `make_dh_tool` function, we are creating a way to store data about DH tools. However, in this example we've created a class that has an initial method (which is really just a function!) that takes the argument we pass and assigns it to something called `self`.

Self references the class **instance** that is created when you call a class, which is why `self` is the first argument to ***any*** function defined in a class. You can read more about [class scope here](https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces).

```python
class DHTool:
    """Contains methods for maintaining data related to a dh tool

    Methods:
    --------
    add_authors
    add_year_popularity
    calculate_total_popularity
    """
    def __init__(self, name):
        self.tool_name = name
        self.authors = list()
        self.year_values = dict()
        self.total_value = 0


    def add_authors(self, authors):
        """Adds a list of authors of the DH Tool

        Method argument:
        -----------------
        authors(list) -- A list of people who built the dh tool
        """
        if isinstance(authors, list) is False:
            authors = [authors]

        self.authors.extend(authors)


    def add_year_popularity(self, year, value):
        """Adds the popularity value for each year of the DH tool

        Method argument:
        -----------------
        year(integer or string) -- A year 
        value(integer) - Popularity value
        """

        self.year_values[str(year)] = value


    def calculate_total_popularity(self):
        """Calculate the total popularity for a tool
        """

        self.total_popularity = sum(self.year_values.values())
        print(self.total_popularity)


a_tool = DHTool("Python")
a_tool.add_authors("Guido van Rossum")
a_tool.add_year_popularity(2015, 9)
a_tool.calculate_total_popularity()
print(a_tool.total_popularity)

print(a_tool.add_year_popularity.__doc__) # To view the docstring for the method
```

So what's going in this code block?

First we're taking our `DHTool` class and expanding it so that it can contain more information about each tool. We still only initially create the class with one argument - the tool's name. But we've also added additional attributes to the `__init__` function. First, we have `authors`, which holds a list; then year_values, which is a dictionary holding years as keys for the tool's popularity value, and finally total_value, which is an integer that we store the total popularity of the tool.

Do you notice that the syntax here looks like a dictionary? That's because we can use a similar pattern to dictionaries for creating our attributes.

To make it even more clear this is what the `__init__` method would look like as a function that return a dictionary:

```python

def make_dh_tool(name):
    dh_tool = dict() #Could also write {}
    dh_tool.tool_name = name
    dh_tool.authors = alist()
    dh_tool.year_values = dict()
    dh_tool.total_value = 0
```

While this function is somewhat similar to our `DHTool` class, we've also added a number of functions, which we call **methods** to the class.

The first method `add_authors` takes two arguments, `self` and `authors`. Just like in the `__init__` method, `self` represents the class instance and authors is a list of authors. In the `__init__` method, we define the authors attribute on `self` as an empty list. Then in our `add_authors` method we extend this initial empty list, adding the new authors to the list.

What would happen if we decided to add an additional author of Python from the [list of core contributors](https://devguide.python.org/developers/)?

```python
a_tool.add_authors("Joannah Nanjekye")
print(a_tool.authors)
```

We can check by printing out the authors attribute, and we'll see that the list now contains both Guido and Joannah.

Our `DHTool` class also has methods for add popularity values for each year, and also for calculating the total popularity for the tool.

What if we wanted to add a method for printing out an entire tool's information? We could try looping through the class object and print out each attribute. Or we could use the built-in `__dict__` functionality that prints out the values for a class.

```python
def get_tool_info(self):
    print(a_tool.__dict__)

```

### Quick Assignment

1. Try adding the `get_tool_info` to our `DHTool` class and then call it in your script.
2. Try adding a second tool with all the relevant information, and then calculate it's total popularity and print out it's full information.

#### Additional Reading

1. [An Introduction to Python Classes and Inheritance](http://www.jesshamrick.com/2011/05/18/an-introduction-to-classes-and-inheritance-in-python/)
2. [Here is a very helpful video series on class inheritance](https://www.youtube.com/playlist?list=PL-osiE80TeTsqhIuOqKhwlXsIBIdSeYtc)
3. [Python Documentation on Classes](https://docs.python.org/3/tutorial/classes.html)

## Python Libraries

So you're probably wondering when to use classes? Mostly we won't be delving into code complicated enough to require to write your own classes, but you will be using lots of code that is based on this pattern. That's because the class is the primary way that Python organizes its standard library and the wider ecosystem of external libraries.

Let's dig into the Python documentation to understand more! [Here's the Python Standard Library](https://docs.python.org/3/library/). We've already been using this documentation, but let's scroll down to the [Pathlib module](https://docs.python.org/3/library/pathlib.html).

## Imports are Important

We can easily bring classes into other code using the `import` keyword, which does some voodoo to allow us to use classes defined in other files. This is how we can use parts of the Python Standard Library that aren't directly built into the base language ([Random](https://docs.python.org/3/library/random.html) is an example that we've encountered before). It's also how we use modules written by other programmers from the larger Python community. More on that next week!

We can also use `import` to import our own classes. It gets complicated if we have to specify the path, so for now it's easier to open the Python interpreter inside the same directory to import.

## File Input and Output

File IO is easy in Python using the [`open` built-in function](https://docs.python.org/3/library/functions.html#open).

To open a file for reading (input), we just do:

```python
input_file = open("text.txt","r")
text = input_file.read()
print(text)
input_file.close()
```

Here, we use the `open` function to open a file in mode "r" (read). `open` returns a [File Object](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files) that provides methods for reading and writing. In this case, the `read` method reads in data from the `input_file` file object and stores it as a string in `text`.

At the end, we can close the file to save some memory, but even if we don't do it explicitly, Python will eventually catch on to the fact that it isn't being used anymore and do it for us. This isn't a huge concern unless you're opening thousands of files or the files you're opening are very, very large.

One convenient way to read files is to combine the file object with a for loop to read text files line by line.

```python
input_file = open("text.txt","r")
for line in input_file:
    print(line.upper())
input_file.close()
```

This will loop through the file, line by line, until it's closed.

Often, you'll see file handling used with the `with` keyword:

```python
with open("text.txt","r") as input_file:
    for line in input_file:
        print(line.upper())
```

This is functionally the same as our last example. The only difference is that the file is automatically closed at the end of the block. You can use whichever convention you like, but keep both in mind because they're both pretty common in the wild.

## File Output

File output is very similar. We just use mode 'w', which overwrites the file specified. We can also use 'x' (which only works for new files) or 'a' (which appends data at the end of the file). Read the [`open` docs](https://docs.python.org/3/library/functions.html#open) for all the details.

```python
f = open("text.txt","w")
for i in range(0,100):
    f.write(str(i**2)+"\n")
```

Here, there's something just a little tricky. `i` and `i**2` are  integers, but the file object in this case expects a string. If we try to use `f.write(i**2)` instead, we'll get an error: `TypeError: write() argument must be str, not int`.

To get around this, we can convert from int to string using the built-in `str` class constructor `str()` to create a new string that's the *string representation* of the integer we pass in. So even though the inteher 4 and the string "4" look the same, they're different objects and different ways to represent data.

For the same reason that we can't do `f.write(4)`, we also can't do `"4"+4` (or, for that matter, `4+"4"`). We have to explicitly tell python whether we want it to treat each value as an integer or a string. `"4"+str(4)` produces "44" while `int("4")+4` returns 8.

The second new thing here is the newline character "\n", which is technically called a LF or Line Feed. This inserts a line break between characters in a string.

## Assignment: Putting it Together

Here's part of what you did in the command line assignment earlier, but now in Python.

```python
jane = 'The Project Gutenberg EBook of Pride and Prejudice, by Jane Austen This eBook is for the use of anyone anywhere at no cost and with almost no restrictions whatsoever. You may copy it, give it away or re-use it under the terms of the Project Gutenberg License included with this eBook or online at www.gutenberg.org Title: Pride and Prejudice Author: Jane Austen Posting Date: August 26, 2008 Release Date: June, 1998 Last Updated: March 10, 2018 Language: English Character set encoding: UTF-8 *** START OF THIS PROJECT GUTENBERG EBOOK PRIDE AND PREJUDICE *** Produced by Anonymous Volunteers PRIDE AND PREJUDICE By Jane Austen Chapter 1 It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife.'
jane = jane.replace("man","person").replace("wife","partner")
print(jane)
```

Let's say that we want to "modernize" every classic book on Project Gutenberg. The first step is to write a program to read a single book in, replace the words, and then write it back out to a file.

I've pushed the Gutenberg version of Pride and Prejudice to this directory [here]({{site.baseurl}}/assets/files/PrideandPrejudice.txt) and the skeleton of the code needed to convert the text:

```python
filename = "pg42671.txt"

### input code to read file goes here

jane = jane.replace("man","person").replace("wife","partner")

### output code to print file changes goes here
```