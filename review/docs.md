## **Python 3.11 Alpha**

A new version of Python is released in October each year. The code is developed and tested over a [seventeen-month period](https://www.python.org/dev/peps/pep-0602/) before the release date. New features are implemented during the [alpha phase](https://en.wikipedia.org/wiki/Software_release_life_cycle#Alpha), which lasts until May, about five months before the final release.

About [once a month](https://www.python.org/dev/peps/pep-0664/) during the alpha phase, Python’s core developers release a new **alpha version** to show off the new features, test them, and get early feedback. Currently, the latest version of Python 3.11 is **3.11.0 alpha 5**, released on February 3, 2022.

The first **beta release** of Python 3.11 is planned for May 6, 2022. Typically, no new features are added during the [beta phase](https://en.wikipedia.org/wiki/Software_release_life_cycle#Beta). Instead, the time between the feature freeze and the release date is used to test and solidify the code.

## Cool new features

Some of the currently announced highlights of Python 3.11 include:

° **Enhanced error messages**, which will help you more effectively debug your code.

° **Exception groups**, which will allow programs to raise and handle multiple exceptions at the same time.

° **Optimizations**, promising to make Python 3.11 significantly faster than previous versions.

° **Static typing** improvements, which will let you annotate your code more precisely.

There’s a lot to look forward to in the upcoming Python 3.11 release! In this tutorial, you’ll focus on how the enhanced error reporting can improve your developer experience by letting you debug your code more efficiently. You’ll also get a peek at some of the other, smaller features that’ll be shipping with Python 3.11.

## Installation

To play with the code examples in this tutorial, you’ll need to install a version of Python 3.11 onto your system. In this section, you’ll learn about a few different ways to do this: using **Docker**, using **pyenv**, or installing from **source**. Pick the one that works best for you and your system.

If you have access to [Docker](https://docs.docker.com/get-docker/) on your system, then you can download the latest version of Python 3.11 by pulling and running the python:3.11-rc-slim [Docker image](https://hub.docker.com/_/python):

```shell
$ docker pull python:3.11-rc-slim
Unable to find image 'python:3.11-rc-slim' locally
latest: Pulling from library/python
[...]

$ docker run -it --rm python:3.11-rc-slim
```

This drops you into a Python 3.11 REPL. Check out [Run Python Versions in Docker](https://realpython.com/python-versions-docker/#running-python-in-a-docker-container) for more information about how you can work with Python through Docker, including how you can run scripts.

The [pyenv](https://realpython.com/intro-to-pyenv/) tool is great for managing different versions of Python on your system, and you can use it to install Python 3.11 Alpha if you like. It comes with two different versions, one for Windows and one for Linux and macOS:

On Linux and macOS, you can use pyenv. First update your pyenv installation, using the pyenv-update plugin:

```shell
$ pyenv update
Updating /home/realpython/.pyenv...
[...]
```

Doing an update ensures that you can install the latest version of Python. If you don’t want to use the update plugin, you can [update pyenv manually](https://github.com/pyenv/pyenv#upgrading).

Use pyenv install --list to check which versions of Python 3.11 are available. Then, install the latest one:

```shell
$ pyenv install 3.11.0a5
Downloading Python-3.11.0a5.tar.xz...
[...]
```

The installation may take a few minutes. Once your new alpha version is installed, then you can create a [virtual environment](https://realpython.com/python-virtual-environments-a-primer/) where you can play with it:

```shell
$ pyenv virtualenv 3.11.0a5 311_preview
$ pyenv activate 311_preview
```

On Linux and macOS, you use the [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) plugin to set up the virtual environment and activate it.

You can also install Python from one of pre-release versions available on [python.org](https://www.python.org/). Choose the [latest pre-release](https://www.python.org/download/pre-releases/) and scroll down to the Files section at the bottom of the page. Download and install the file corresponding to your system. See [Python 3 Installation & Setup Guide](https://realpython.com/installing-python/) for more information.

In the rest of the tutorial, `python3.11` is used to indicate that you should start your Python 3.11 executable. Exactly how you run it depends on how you installed it. See the relevant tutorial on [Docker](https://realpython.com/python-versions-docker/#running-python-in-a-docker-container), [pyenv](https://realpython.com/intro-to-pyenv/), [virtual environments](https://realpython.com/python-virtual-environments-a-primer/), or [installing from source](https://realpython.com/installing-python/) if you’re uncertain.

## **Even Better Error Messages in Python 3.11**

From its beginnings, Python used a homemade and explicitly basic [LL(1) parser](https://en.wikipedia.org/wiki/LL_parser) with a one-token [lookahead](https://en.wikipedia.org/wiki/Parsing#Lookahead) and no ability to backtrack. According to [Guido van Rossum](https://twitter.com/gvanrossum), the creator of Python, this was a conscious choice:

> Python’s parser generator is […] lame, but that in turn is intentional – it is so lame to prevent me from inventing syntax that is either hard to write a parser for or hard to disambiguate by human readers, who always come first in Python’s design. ([Source](https://www.artima.com/weblogs/viewpost.jsp?thread=85551))

The limitations in the LL(1) parser led to several work-arounds that complicated Python’s grammar rules and its parser generation. Eventually, Guido [suggested](https://medium.com/@gvanrossum_83706/peg-parsing-series-de5d41b2ed60) that Python’s grammar be updated to a **Parsing Expression Grammar (PEG)** with infinite lookahead and backtracking. A new [parser](https://en.wikipedia.org/wiki/Parsing#Parser) was created for [Python 3.9](https://realpython.com/python39-new-features/#a-more-powerful-python-parser).

[Python 3.10](https://realpython.com/python310-new-features/) took advantage of the new [PEG parser](https://en.wikipedia.org/wiki/Parsing_expression_grammar) to implement [structural pattern matching](https://realpython.com/python310-new-features/#structural-pattern-matching) and [better error messages](https://realpython.com/python310-new-features/#better-error-messages). This work has continued in Python 3.11, with even more improvements to Python’s error messages.

## Challenges Before Python 3.11

You’ll soon see examples of the new and improved error messages. First, though, you’ll create a few errors with Python 3.10 or older, so that you’ll appreciate the current challenges.

Say that you have a dataset with some inconsistent data about [famous scientists](https://www.famousscientists.org/). For each scientist, their name, birth date, and death date is recorded:

```python
# scientists.py

scientists = [
    {
        "name": {"first": "Grace", "last": "Hopper"},
        "birth": {"year": 1906, "month": 12, "day": 9},
        "death": {"year": 1992, "month": 1, "day": 1},
    },
    {"name": {"first": "Euclid"}},
    {"name": {"first": "Abu Nasr", "last": "Al-Farabi"}, "birth": None},
    {
        "name": {"first": "Srinivasa", "last": "Ramanujan"},
        "birth": {"year": 1887},
        "death": {"month": 4, "day": 26},
    },
    {
        "name": {"first": "Ada", "last": "Lovelace"},
        "birth": {"year": 1815},
        "death": {"year": 1852},
    },
    {
        "name": {"first": "Charles", "last": "Babbage"},
        "birth": {"year": 1791, "month": 12, "day": 26},
        "death": {"year": 1871, "month": 10, "day": 18},
    },
]
```

Note that information about each scientist is recorded in a nested dictionary with `name`, `birth`, and `death` fields. However, some information is incomplete. For example, Euclid only has a name, and Ramanujan is missing his year of death.

In order to process this data, you decide to create a [named tuple](https://realpython.com/python-namedtuple/) and a function that can convert the nested dictionary into named tuples:

```python
# scientists.py

from typing import NamedTuple

class Person(NamedTuple):
    name: str
    life_span: tuple

def dict_to_person(info):
    """Convert a dictionary to a Person object"""
    return Person(
        name=f"{info['name']['first']} {info['name']['last']}",
        life_span=(info["birth"]["year"], info["death"]["year"]),
    )

scientists = ...  # As above
```

Note that you’re not doing any validation or error handling in `dict_to_person()`, so you’ll run into issues when you try to process some of the scientists with incomplete data. The rest of the examples in this section are run on Python 3.10 and show that some error messages are ambigous and imprecise.

To see what happens when you process incomplete data, you first attempt to convert the information about [Euclid](https://www.famousscientists.org/euclid/).

Note that `convert_pair()` calls `dict_to_person()` twice, once for each scientist. You can use it to see information about [Ada Lovelace](https://www.famousscientists.org/ada-lovelace/) and [Charles Babbage](https://www.famousscientists.org/charles-babbage/).

The technical reason for such ambiguous error messages is that Python internally uses a line in the source code as the reference for each instruction in a program, even though a line can contain several instructions. That changes in Python 3.11.

## Improvements in Python 3.11

Python 3.11 improves all the error messages from the previous section. You can check out the details in [PEP 657 – Include Fine Grained Error Locations in Tracebacks](https://www.python.org/dev/peps/pep-0657/). Python’s error messages, including the function calls that led to the error, are called [tracebacks](https://realpython.com/python-traceback/). In this section, you’ll learn how more precise error messages can help you in your debugging efforts.

To start exploring, load scientists.py interactively into your Python 3.11 interpreter:

```shell
$ python3.11 --version
Python 3.11.0a5

$ python3.11 -i scientists.py
```

As in the previous section, this drops you into the interactive REPL, with scientists, `dict_to_person()`, and `convert_pair()` already defined.

You can still create Person objects as long as the information is well-formed. However, observe what happens if you encounter an error.

You still get the same KeyError because of a missing `last` field. But now a visible marker points to the exact location in the source code line, so you can immediately see that `last` is an expected field nested inside `name`.

This is already an improvement, as you don’t need to study the error message so closely. However, the benefit becomes really clear in cases where the original error message is ambigous.

While the message, `'NoneType' object is not subscriptable`, doesn’t tell you much about which part of your data structure happens to be `None`, the marker makes it clear. Here, `info["birth"]` is `None`, so you can’t get the `year` item from it.

The added clarity in error messages will help you quickly track down problems as they come up, so you can fix them.

## Technical Background

Marking which part of a line causes an error may seem like a quick and obvious improvement. Why hasn’t Python included this before?

To appreciate the technical details, you should know a little about how CPython [runs your source code](https://devguide.python.org/compiler/):

1. Your code is [tokenized](https://docs.python.org/3/library/tokenize.html).

2. The tokens are parsed into an [abstract syntax tree (AST)](https://docs.python.org/3/library/ast.html).

3. The AST is transformed into a [control flow graph (CFG)](https://en.wikipedia.org/wiki/Control-flow_graph).

4. The CFG is converted into [bytecode](https://en.wikipedia.org/wiki/Bytecode).

At runtime, the Python interpreter only concerns itself with the bytecode, which is several steps removed from your source code.

Several modules in the standard library allow you to peek behind the curtain of this process. You can, for example, use [dis](https://docs.python.org/3/library/dis.html) to disassemble the bytecode. Remember the definition of `convert_pair()`:

```python
def convert_pair(first, second):
    """Convert two dictionaries to Person objects"""
    return dict_to_person(first), dict_to_person(second)
```

As noted, this code is tokenized, parsed, and ultimately converted into bytecode. You can investigate the bytecode of this function as follows:

The meaning of each instruction isn’t important here. Just take note of the numbers in the leftmost column: 17 and 19 are the line numbers of the original source code. You can see that line 19 has been converted into ten bytecode instructions. If any of those instructions fail, earlier versions of Python only had enough information to conclude that the error happened somewhere on line 19.

Python 3.11 introduces a new tuple of four numbers for each bytecode instruction. They indicate the **start line, end line, start column offset,** and **end column offset** of each instruction. You can access these tuples by calling the new [.co_positions()](https://docs.python.org/3.11/reference/datamodel.html#codeobject.co_positions) method on a code object:

For example, the first LOAD_GLOBAL instruction has the positions (19, 19, 11, 25). Look at line 19 of your source code. By counting from 0, you find that d is the 11th character in the line. You discover that column offsets 11 to 25 correspond to the text `dict_to_person`. Connect all line numbers and column offsets to your source code and match them to the bytecode instructions to create the following table:

| Bytecode | Source code |
|----------|-------------|
| RESUME	               |
|-------------|----------------|
| LOAD_GLOBAL	| dict_to_person |
|-------------|----------------|
| LOAD_FAST	| first |
|-----------|-------|
| PRECALL_FUNCTION | dict_to_person(first) |
|-------------|----------------------------|
| CALL | dict_to_person(first) |
|-------------|----------------|
| LOAD_GLOBAL |	dict_to_person |
|-------------|----------------|
| LOAD_FAST |	second |
|-----------|--------|
| PRECALL_FUNCTION | dict_to_person(second) |
|------------------|------------------------|
| CALL |	dict_to_person(second) |
|------|-------------------------|
| BUILD_TUPLE |	dict_to_person(first), dict_to_person(second) |
|-------------|-----------------------------------------------|
| RETURN_VALUE | return dict_to_person(first), dict_to_person(second) |
|--------------|------------------------------------------------------|

The new information about line numbers and column offsets allows your tracebacks to be more detailed. You’ve seen how the built-in traceback in Python 3.11 takes advantage of this. As Python 3.11 becomes more widely used, some third-party packages will likely use this information as well.

Storing these offsets takes up some space in Python’s cached bytecode files and in memory during runtime. If this is a concern, you can remove them by setting the PYTHONNODEBUGRANGES environment variable or by using the `-X no_debug_ranges` command-line option:

```shell
$ python3.11 -X no_debug_ranges -i scientists.py
```

Note that there’s no marker showing which field is missing year, and `.co_positions()` only contains information about the line number. The fields marked None are not stored on disk or in memory.

The benefit of this is that your .pyc files are smaller and that the code objects take up correspondingly less space in memory.

In this case, you can see that removing the extra information saves four hundred bytes. Normally, this won’t affect your program. You only need to consider turning off this information when you’re running in a [restricted environment](https://realpython.com/embedded-python/) where you really need to optimize your memory usage.

## Even-Even Better Error Messages Using Third-Party Libraries

There are a couple of third-party packages that you can use to enhance error messages, including on Python versions older than 3.11. These don’t rely on the improvements that you’ve learned about so far. Instead, they complement those developments, and you can use them to set up an even better debugging workflow for yourself.

The better_exceptions package adds information about variable values to your tracebacks. To try it out, you first need to install it from PyPI:

$ python -m pip install better_exceptions
There are a few ways that you can use better_exceptions in your own work. You can, for example, activate it using an environment variable:

Windows
Linux + macOS
C:\> set BETTER_EXCEPTIONS=1
C:\> python -i scientists.py
By setting the BETTER_EXCEPTIONS environment variable, you let the package format your tracebacks. For other ways to invoke better_exceptions, you can consult the documentation.

Now that you’ve set the environment variable, notice what happens if you call convert_pair() and try to pair up Euclid with himself:

>>> convert_pair(scientists[1], scientists[1])
Traceback (most recent call last):
  ...
  File "/home/realpython/scientists.py", line 19, in convert_pair
    return dict_to_person(first), dict_to_person(second)
           │              │       │              └ {'name': {'first': 'Euclid'}}
           │              │       └ <function dict_to_person at 0x7fe2f2c0c040>
           │              └ {'name': {'first': 'Euclid'}}
           └ <function dict_to_person at 0x7fe2f2c0c040>
  File "/home/realpython/scientists.py", line 12, in dict_to_person
    name=f"{info['name']['first']} {info['name']['last']}",
            │                       └ {'name': {'first': 'Euclid'}}
            └ {'name': {'first': 'Euclid'}}
KeyError: 'last'
Notice that each variable name in the traceback is annotated with its corresponding value. This allows you to quickly figure out that the KeyError happens because Euclid’s information is missing the last field.

Note: The currently latest version of better_exceptions, version 0.3.3, replaces Python 3.11’s markers with its own. In other words, the arrows that you learned about in the previous sections are gone. Hopefully, a future version of better_exceptions will be able to show both.

The Friendly project offers a different take on tracebacks. Its original purpose is “to make it easier for beginners […] to understand what caused a program to generate a traceback.” To try Friendly out yourself, install it with pip:

$ python -m pip install friendly
As the documentation explains, you can use Friendly in different environments, including the console, notebooks, and editors. One neat option is that you can start Friendly after you encounter an error:

>>> dict_to_person(scientists[2])
Traceback (most recent call last):
  ...
  File "/home/realpython/scientists.py", line 13, in dict_to_person
    life_span=(info["birth"]["year"], info["death"]["year"]),
               ~~~~~~~~~~~~~^^^^^^^^
TypeError: 'NoneType' object is not subscriptable

>>> from friendly import start_console
>>> start_console()
The Friendly console acts as a wrapper around your regular Python REPL. You can now execute a few new commands that give you more insight into the most recent error:

>>> why()
Subscriptable objects are typically containers from which you can retrieve
item using the notation [...]. Using this notation, you attempted to
retrieve an item from an object of type NoneType which is not allowed.

Note: NoneType means that the object has a value of None.

>>> what()
A TypeError is usually caused by trying to combine two incompatible types
of objects, by calling a function with the wrong type of object, or by
trying to do an operation not allowed on a given type of object.
The why() function gives you information about your specific error, while what() adds some background on the kind of error you encountered, in this case a TypeError. You can also try out where(), explain(), and www().

Note: Friendly works well with Python 3.11. However, when using development versions of Python, you may experience some issues with library support. Remember that all the libraries you use in this section also work on older versions of Python.

A more recent alternative is Rich, which offers support for annotated tracebacks. To try out Rich, you should first install it:

$ python -m pip install rich
You activate the enhanced traceback by installing Rich’s exception hook. If you encounter an error, then you’ll get a colored, well-formatted traceback with information about the values of all available variables, as well as more context for the line where the error occurred:

>>> from rich import traceback
>>> traceback.install(show_locals=True)
<built-in function excepthook>

>>> dict_to_person(scientists[3])
╭───────────────── Traceback (most recent call last) ──────────────────╮
│ <stdin>:1 in <module>                                                │
│ ╭───────────────────────────── locals ─────────────────────────────╮ │
│ │ __annotations__ = {}                                             │ │
│ │    __builtins__ = <module 'builtins' (built-in)>                 │ │
│ │         __doc__ = None                                           │ │
│ │      __loader__ = <_frozen_importlib_external.SourceFileLoader   │ │
│ │                   object at 0x7f933c7b05d0>                      │ │
│ │        __name__ = '__main__'                                     │ │
│ │     __package__ = None                                           │ │
│ │        __spec__ = None                                           │ │
│ │    convert_pair = <function convert_pair at 0x7f933c628680>      │ │
│ │  dict_to_person = <function dict_to_person at 0x7f933c837380>    │ │
│ │      NamedTuple = <function NamedTuple at 0x7f933c615080>        │ │
│ │          Person = <class '__main__.Person'>                      │ │
│ │      scientists = [ ... ]                                        │ │
│ │       traceback = <module 'rich.traceback' from                  │ │
│ │                   '/home/realpython/.pyenv/versions/311_preview/…│ │
│ ╰──────────────────────────────────────────────────────────────────╯ │
│ /home/realpython/scientists.py:13 in dict_to_person                  │
│                                                                      │
│   10 │   """Convert a dictionary to a Person object"""               │
│   11 │   return Person(                                              │
│   12 │   │   name=f"{info['name']['first']} {info['name']['last']}", │
│ ❱ 13 │   │   life_span=(info["birth"]["year"], info["death"]["year"])│
│   14 │   )                                                           │
│   15                                                                 │
│   16                                                                 │
│                                                                      │
│ ╭──────────────────────────── locals ─────────────────────────────╮  │
│ │ info = {                                                        │  │
│ │        │   'name': {                                            │  │
│ │        │   │   'first': 'Srinivasa',                            │  │
│ │        │   │   'last': 'Ramanujan'                              │  │
│ │        │   },                                                   │  │
│ │        │   'birth': {'year': 1887},                             │  │
│ │        │   'death': {'month': 4, 'day': 26}                     │  │
│ │        }                                                        │  │
│ ╰─────────────────────────────────────────────────────────────────╯  │
╰──────────────────────────────────────────────────────────────────────╯
KeyError: 'year'
See the Rich documentation for more information and other examples of its output.

There are also other projects attempting to improve on Python’s tracebacks and error messages. Several of them were highlighted in Creating Beautiful Tracebacks with Python’s Exception Hooks and discussed on the Python Bytes podcast. All of them also work on versions of Python prior to 3.11.

Other New Features
In every new version of Python, a handful of features get most of the buzz. However, most of the evolution of Python has happened in small steps, by adding a function here or there, improving some existing functionality, or fixing a long-standing bug.

Python 3.11 is no different. This section shows a few of the smaller improvements waiting for you in Python 3.11.

Cube Roots and Powers of Two
The math module contains basic math functions and constants. Most of them are wrappers around similar C functions. Python 3.11 adds two new functions to math:

cbrt() calculates cube roots.
exp2() calculates powers of two.
Similar to other math functions, these are implemented as wrappers around the corresponding C functions. You can, for example, use cbrt() to confirm Ramanujan’s observation that you can express 1729 as the sum of two cubes in two different ways:

>>> import math

>>> 1 + 1728
1729
>>> math.cbrt(1)
1.0
>>> math.cbrt(1728)
12.000000000000002

>>> 729 + 1000
1729
>>> math.cbrt(729)
9.000000000000002
>>> math.cbrt(1000)
10.0
Despite some rounding errors, you note that 1729 can be written as either 1³ + 12³ or 9³ + 10³. In other words, 1729 can be expressed as two different sums of cube numbers.

In earlier versions of Python, you could calculate cube roots and powers of two using exponentiation (**) or math.pow(). Now, cbrt() allows you to find cube roots without explicitly specifying 1/3. Similarly, exp2() gives you a shortcut for calculating powers of two. In Python 3.11, you have several options for doing these calculations:

>>> math.cbrt(729)
9.000000000000002
>>> 729**(1/3)
8.999999999999998
>>> math.pow(729, 1/3)
8.999999999999998

>>> math.exp2(16)
65536.0
>>> 2**16
65536
>>> math.pow(2, 16)
65536.0
Note that you might get slightly different results from the different methods because of floating point representation errors. In particular, it seems that exp2() is less accurate than math.pow() on Windows. Sticking to the old approaches for now should serve you well.

You’ll also get different results when calculating cube roots of negative numbers:

>>> math.cbrt(-8)
-2.0

>>> (-8)**(1/3)
(1.0000000000000002+1.7320508075688772j)

>>> math.pow(-8, 1/3)
Traceback (most recent call last):
  ...
ValueError: math domain error
Any number has three cube roots. For real numbers, one of these roots will be a real number, while the two other roots will be a pair of complex numbers. cbrt() returns the principal cube root, including for negative numbers. Exponentiation returns one of the complex cube roots, while math.pow() only handles negative numbers with integer exponents.

Underscores in Fractions
Python has supported adding underscores to literal numbers since Python 3.6. Usually, you use underscores to group digits in large numbers in order to make them more readable:

>>> number = 60481729
>>> readable_number = 60_481_729
In this example, it may not be immediately obvious whether number is approximately six million or sixty million. By grouping the digits into groups of three, it’s clear that readable_number is about sixty million.

Note that this feature is a convenience that allows your source code to be more readable. The underscores have no effect on calculations or how Python represents the number, although you can use f-strings to format a number with underscores:

>>> number == readable_number
True

>>> readable_number
60481729

>>> f"{number:_}"
'60_481_729'
Note that Python doesn’t care where you put the underscores. You should take care so that they don’t end up adding confusion:

>>> confusing_number = 6_048_1729
The value of confusing_number is also about sixty million, but you could easily think that it was six million. If you use underscores to separate thousands, then you should be aware that there are different conventions for grouping digits around the world.

Python can accurately represent rational numbers with the fractions module. For example, you can specify the fraction 6048 over 1729 using a string literal as follows:

>>> from fractions import Fraction
>>> print(Fraction("6048/1729"))
864/247
For some reason, underscores weren’t allowed in Fraction string arguments before Python 3.11. Now, you can use underscores when specifying fractions as well:

>>> print(Fraction("6_048/1_729"))
864/247
As with other numbers, Python doesn’t care where you put the underscores. It’s up to you to use underscores to improve the readability of your code.

Flexible Calling of Objects
The operator module contains functions that can be useful when using some of the functional programming features of Python. As a quick example, you can use operator.abs to sort the numbers -3, -2, -1, 0, 1, 2, and 3 by their absolute values:

>>> sorted([-3, -2, -1, 0, 1, 2, 3], key=operator.abs)
[0, -1, 1, -2, 2, -3, 3]
By specifying key, you’re sorting the list by first calculating the absolute value of each item.

Python 3.11 adds call() to operator. You can use call() to call functions. For example, you can write the previous example as follows:

>>> operator.call(sorted, [-3, -2, -1, 0, 1, 2, 3], key=operator.abs)
[0, -1, 1, -2, 2, -3, 3]
In general, using call() like this isn’t useful. You should stick to calling functions directly. One possible exception is when you call functions that are referenced by variables, as adding call() can make your code more explicit.

The next example shows a better use case for call(). You implement a calculator that can do basic calculations in Norwegian. It uses the parse library to parse a text string and then call() to perform the correct arithmetic operation:

import operator
import parse

OPERATIONS = {
    "pluss": operator.add,        # Addition
    "minus": operator.sub,        # Subtraction
    "ganger": operator.mul,       # Multiplication
    "delt på": operator.truediv,  # Division
}
EXPRESSION = parse.compile("{operand1:g} {operation} {operand2:g}")

def calculate(text):
    if (ops := EXPRESSION.parse(text)) and ops["operation"] in OPERATIONS:
        operation = OPERATIONS[ops["operation"]]
        return operator.call(operation, ops["operand1"], ops["operand2"])
OPERATIONS is a mapping that specifies which commands your calculator understands and defines their corresponding functions. EXPRESSION is a template defining the kinds of text strings that you’ll parse. calculate() parses your string and calls the relevant operation if it recognizes it.

Note: You could’ve returned operation(ops["operand1"], ops["operand2"]) instead of using operator.call().

You can use calculate() to do Norwegian arithmetic as follows:

>>> calculate("3 pluss 11")
14.0

>>> calculate("3 delt på 11")
0.2727272727272727
Your calculator figures out that 3 plus 11 equals 14, while 3 divided by 11 is about 0.27.

operator.call() is similar to apply(), which was available in Python 2 and fell out of favor with the introduction of argument unpacking. call() gives you a bit more flexibility in how you call functions. However, as these examples show, you’re usually better off calling functions directly.

Conclusion
Now you’ve seen some of what Python 3.11 will bring to the table when it’s released in October 2022. You’ve learned about some of its new features and explored how you can already play with the improvements.

In particular, you’ve:

Installed Python 3.11 Alpha on your computer
Played with the enhanced error tracebacks in Python 3.11 and used them to more efficiently debug your code
Learned how Python 3.11 builds on Python 3.10’s PEG parser and better error messages
Explored how third-party libraries can make your debugging even more efficient
Tried out a few of the smaller improvements in Python 3.11, including new math functions and more readable fractions
Try out the better error messages in Python 3.11! What do you think about these enhanced tracebacks? Comment below to share your experience.