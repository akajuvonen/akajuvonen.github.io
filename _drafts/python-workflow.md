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

I also often work with projects that use dependency links. They should be
deprecated, but since it is still often used, it's convenient for me to be
able to use them still.

Example link: [Pipenv link][pipenv].

[pyenv]: https://github.com/pyenv/pyenv
[pipenv]: https://github.com/pypa/pipenv
