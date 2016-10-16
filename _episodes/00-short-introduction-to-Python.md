---
title: "Python introduction"
teaching: ??
exercises: ??
questions:
- "What is Python?"
objectives:
- "Short introduction to programming in Python"
keypoints:
- "Core concepts in python: Python interpreter, data types, operators, functions."

# training: http://swcarpentry.github.io/instructor-training
training: do-we-have-a-repo-of-python-training-resources ?
---

# The Basics of Python

Python is a general purpose programming language that supports rapid development
of scripts and applications.

Python's main advantages:

* Open Source software, supported by Python Software Foundation
* Available on all platforms
* "Batteries Included" philosophy - libraries for common tasks available in
  standard installation
* Supports multiple programming paradigms
* Very large community

## Interpreter

Python is an interpreted language. As a consequence, we can use it in two ways:

* Using interpreter as an "advanced calculator" in interactive mode:

~~~
user:host:~$ python
Python 3.5.1 (default, Oct 23 2015, 18:05:06)
[GCC 4.8.3] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 2 + 2
~~~
{: .source}
~~~
4
~~~
{: .output}
~~~
>>> print("Hello World")
~~~
{: .source}
~~~
Hello World
~~~
{: .output}

* Executing programs/scripts saved as a text file, usually with `*.py` extension:

~~~
user:host:~$ python my_script.py
~~~
{: .source}
~~~
Hello World
~~~
{: .output}

## Introduction to Python built-in data types

### Strings, integers and floats

The most basic data types in Python are strings, integers and floats:

~~~
text = "Library Carpentry"
number = 42
pi_value = 3.1415
~~~
{: .source}

Here we've assigned data to variables, namely `text`, `number` and `pi_value`,
using the assignment operator `=`. The variable called `text` is a string which
means it can contain letters and numbers. We could reassign the variable `text`
to an integer too - but be careful reassigning variables as this can get
confusing.

To print out the value stored in a variable we can simply type the name of the
variable into the interpreter:

~~~
>>> text
~~~
{: .source}
~~~
"Data Carpentry"
~~~
{: .output}

however, in scripts we must use the `print` function:

~~~
# Comments start with #
# Next line will print out text
print(text)
~~~
{: .source}
~~~
"Data Carpentry"
~~~
{: .output}

### Operators

We can perform mathematical calculations in Python using the basic operators
 `+, -, /, *, %`:

~~~
>>> 2 + 2
~~~
{: .source}
~~~
4
~~~
{: .output}
~~~
>>> 6 * 7
~~~
{: .source}
~~~
42
~~~
{: .output}
~~~
>>> 2 ** 16  # power
~~~
{: .source}
~~~
65536
~~~
{: .output}
~~~
>>> 13 % 5  # modulo
~~~
{: .source}
~~~
3
~~~
{: .output}

We can also use comparison and logic operators:
`<, >, ==, !=, <=, >=` etc.
`and, or, not`

~~~
>>> 3 > 4
~~~
{: .source}
~~~
False
~~~
{: .output}
~~~
>>> True and True
~~~
{: .source}
~~~
True
~~~
{: .output}
~~~
>>> True or False
~~~
{: .source}
~~~
True
~~~
{: .output}

## Sequential types: Lists and Tuples

### Lists

**Lists** are a common data structure to hold an ordered sequence of
elements. Each element can be accessed by an index:

~~~
>>> numbers = [1,2,3]
>>> numbers[0]
~~~
{: .source}
~~~
1
~~~
{: .output}

A `for` loop can be used to access the elements in a list or other Python data
structure one at a time:

~~~
for num in numbers:
    print(num)
~~~
{: .source}
~~~
1
2
3
~~~
{: .output}

**Indentation** is very important in Python. Note that the second line in the
example above is indented. This is Python's way of marking a block of code. We will
discuss this in more detail later.

To add elements to the end of a list, we can use the `append` method:

~~~
>>> numbers.append(4)
>>> print(numbers)
~~~
{: .source}
~~~
[1,2,3,4]
~~~
{: .output}

Methods are a way to interact with an object (a list, for example). We can invoke
a method using the dot `.` followed by the method name and a list of arguments in parentheses.
To find out what methods are available for an object, we can use the built-in `help` command:

~~~
help(numbers)
~~~
{: .source}
~~~
Help on list object:

class list(object)
 |  list() -> new empty list
 |  list(iterable) -> new list initialized from iterable's items
 ...
~~~
{: .output}

We can also access a list of methods using `dir`. Some methods names are
surrounded by double underscores. Those methods are called "special", and
usually we access them in a different way. For example `__add__` method is
responsible for the `+` operator.

~~~
>>> dir(numbers)
~~~
{: .source}
~~~
['__add__', '__class__', '__contains__', ...]
~~~
{: .output}

### Tuples

A tuple is similar to a list in that it's an ordered sequence of elements. However,
tuples can not be changed once created (they are "immutable"). Tuples are
created by placing comma-separated values inside parentheses `()`.

~~~
# tuples use parentheses
a_tuple= (1,2,3)
another_tuple = ('blue','green','red')
# Note: lists use square brackets
a_list = [1,2,3]
~~~
{: .source}


> ## What is the difference?
>
> What happens when you type
>
> ~~~
> a_tuple[2]=5
> ~~~
> {: .source}
> vs
> ~~~
> a_list[1]=5
> ~~~
> {: .source}
> ?
{: .challenge}

> ## What is the object type?
>
> Type:
> ~~~
> type(a_tuple)
> ~~~
> {: .source}
{: .challenge}

## Dictionaries

A **dictionary** is a container that holds pairs of objects - keys and values.

~~~
>>> translation = {"one" : 1, "two" : 2}
>>> translation["one"]
~~~
{: .source}
~~~
1
~~~
{: .output}

Dictionaries work a lot like lists - except that you index them with *keys*.
You can think about a key as a name for or a unique identifier for a set of values
in the dictionary. Keys can only have particular types - they have to be
"hashable". Strings and numeric types are acceptable, but lists aren't.

~~~
>>> rev = {1 : "one", 2 : "two"}
>>> rev[1]
~~~
{: .source}
~~~
'one'
~~~
{: .output}
~~~
>>> bad = {[1,2,3] : 3}
~~~
{: .source}
~~~
...
TypeError: unhashable type: 'list'
~~~
{: .output}


To add an item to the dictionary we assign a value to a new key:

~~~
>>> rev = {1 : "one", 2 : "two"}
>>> rev[3] = "three"
>>> rev
~~~
{: .source}
~~~
{1: 'one', 2: 'two', 3: 'three'}
~~~
{: .output}

Using `for` loops with dictionaries is a little more complicated. We can do this
in two ways:

~~~
>>> for key, value in rev.items():
...     print(key, "->", value)
...
~~~
{: .source}
~~~
1 -> one
2 -> two
3 -> three
~~~
{: .output}

or

~~~
>>> for key in rev.keys():
...     print(key, "->", rev[key])
...
~~~
{: .source}
~~~
1 -> one
2 -> two
3 -> three
~~~
{: .output}


> ## Reassignment
> Can you do reassignment in a dictionary? Give it a try.
{: .challenge}

First check what `rev` is right now (remember `rev` is the name of our dictionary).

Type:
~~~
>>> rev
~~~
{: .source}


You should see the following output:
~~~
{1: 'one', 2: 'two', 3: 'three'}
~~~
{: .output}


Try to reassign the second value (in the *key value pair*) so that it no longer reads "two" but instead reads "apple-sauce".

~~~
>>> rev[2] = "apple-sauce"
~~~
{: .source}

Now display `rev` again to see if it has changed; you should see the following:

~~~
{1: 'one', 2: 'apple-sauce', 3: 'three'}
~~~
{: .output}

It is important to note that dictionaries are "unordered" and do not remember the
sequence of their items (i.e. the order in which key:value pairs were added to
the dictionary). Because of this, the order in which items are returned from loops
over dictionaries might appear random and can even change with time.

## Functions

A **function** allows you to group code together for reuse and readability.

Defining part of a program in Python as a function is done using the `def`
keyword. For example a function named `sub_function` which has two parameters `a` and `b` and returns their sum
can be defined as:

~~~
def sub_function(a, b):
    result = a - b
    return result
~~~
{: .source}

When we call the function using it's name, the values we pass are assigned to the variables defined by the parameter names by position:

~~~
z = sub_function(44, 2)
print(z)
~~~
{: .source}

~~~
42
~~~
{: .output}

We can also use the parameter names when calling the function.

~~~
z = sub_function(a=44, b=2)
print(z)
~~~
{: .source}

~~~
42
~~~
{: .output}


> ## Notice
>* definition starts with `def`
>* function body is indented
>* `return` keyword precedes returned value
{: .callout}

> ## Parameter order
> What do you think will happen if we change the order of the named parameters? 
> What happens if you type:
>
> ~~~
> print(sub_function(b=2, a=44))
> ~~~
> {: .source}
> 
> vs
> 
> ~~~
> print(sub_function(2, 44))
> ~~~
> {: .source}
{: .challenge}
