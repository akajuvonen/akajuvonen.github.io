---
layout: post
title:  "Notes about Python Workflow"
---

I have been trying to find an optimal workflow for my work and personal needs.
Managing Python packaging and dependencies is not always simple. Ultimately
every workflow is different, and this leads to different tools being used.
I will write some notes about what I'm currently doing, as well as some
experiments with Pipenv to see how that compares in my case.

# Managing multiple Python versions

Currently I frequently need different versions of Python and separate virtual
environments for each project. Most of my development is happening on a Mac.
That basically means that I ended up using `pyenv` for managing multiple
Python versions. In addition, MacOS comes with an old version of Python 2
and some packages installed in a custom location, and they are used by the OS
as far as I understand. I want to leave that untouched and not contaminate
the system libraries with my own requirements.

So far I have settled to using [pyenv][pyenv], which allows me install
and manage any Python versions rather easily. It does one thing, and does
it reasonably well in my experience. I can set both global and local versions
separately. For example:

```python
# Install specific version
pyenv install 3.7.1

# Set global version
pyenv global 3.7.1

# Set local python version in any dir
mkdir folder
cd folder/
pyenv local 3.6.5
```

# Virtual environments

This is a pretty obvious one. For every project I create a separate virtual
environment. I have tried several solutions for this, but eventually wanted
to keep it simple, and just use the `venv` module that now comes with Python.

In practice, I first set the local python version in the project directory.
This means that `python` points to the specified Python version installed
by pyenv. It ends up being used like this:

```python
# Set python version for this folder
pyenv local 3.7.1

# Create a virtual environment
python -m venv virtual-env-dir-name
```

I do not use any tool that helps me manage virtual envs. I like to keep it simple,
and have not felt the need for managing all the virtual environments in one place.
Some tools that I have considered or used in the past:
- [pyenv-virtualenv][pyenvvenv] is a pyenv plugin for managing virtual envs
- [virtualenvwrapper][wrapper] contains extensions to virtualenv tools, including
  the awesome command `workon` for easily switching between venvs
- [pew][pew] also helps managing multiple environments easily in one location

I will most likely use one the above tools in the future. But it's not as big
of a deal as I thought it would be. Also, using only the included `venv` module
there are less tools/dependencies, for whatever that is worth.

One thing that I really like is the ability to activate an environment, and use
installed python commands in a completely different directory. For example, I
might have a tool for manipulating data, install it inside a virtualenv to be
able to use whatever commands it comes with, and actually use those commands
in the actual data directory.

# About dependency links

I also often work with projects that use dependency links. They should be
deprecated, but since it is still often used, it's convenient for me to be
able to use them still. Currently I can just use them with pip:

```python
pip install -e . --process-dependency-links
```

It must be noted that [PEP 508][pep508] describes a way to specify
URL dependencies, and it should be already [merged][pep508merge] to pip.
However, it seems that this does not fully replace dependency links,
at least for some people, as mentioned in [this issue][issue].

[pyenv]: https://github.com/pyenv/pyenv
[pyenvvenv]: https://github.com/pyenv/pyenv-virtualenvo
[wrapper]: https://virtualenvwrapper.readthedocs.io/en/latest/
[pew]: https://github.com/berdario/pew
[pep508]: https://www.python.org/dev/peps/pep-0508/
[pep508merge]: https://github.com/pypa/pip/pull/5571
[issue]: https://github.com/pypa/pip/issues/5898
[pipenv]: https://github.com/pypa/pipenv
