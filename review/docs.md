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
