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
