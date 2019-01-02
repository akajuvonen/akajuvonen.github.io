---
layout: post
title:  "Notes about Python Workflow"
---

I have been trying to find an optimal workflow for my work and personal needs.
Managing Python packaging and dependencies is not always simple. Ultimately
every workflow is different, and this leads to different tools being used.
I will write some notes about what I'm currently doing, as well as some
experiments with Pipenv to see how that compares in my case. This post will
not be as structured as I would like, it is closer to personal notes that
may be useful to someone else, as well. In the future, I hope to return
to this topic as my workflow changes.

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

```bash
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

```bash
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

```bash
pip install -e . --process-dependency-links
```

It must be noted that [PEP 508][pep508] describes a way to specify
URL dependencies, and it should be already [merged][pep508merge] to pip.
However, it seems that this does not fully replace dependency links,
at least for some people, as mentioned in [this issue][issue].

# Trying out Pipenv

Recently I tried [Pipenv][pipenv] to see if that would improve my workflow.
It handles the use of virtualenv and pip automatically. A virtual environment
is automatically created if it does not already exist. Instead of using
`requirements.txt`, Pipenv uses `Pipfile` and `Pipfile.lock`.

I can install whatever packages I need with `pipenv install`, and they are
automatically added to `Pipfile`. Dependencies can be flagged as development
only, and those can be installed with `pipenv install --dev`. Creating a
lockfile can be done with `pipenv lock`, but unfortunately this might
take a long time. This seems to be a common complaint, although in my use
cases the time taken was acceptable. It is also recommended to include
the lockfile in version control, so creating it all the time is not necessary.

Running commands inside virtual environment happens like this: `pipenv run COMMAND`.
This is rather simple, but it is a bit annoying to type every time I want to run
something. Alternatively, I can just open a new shell with `pipenv shell`.
Unfortunately, this does not work very well without additional configuration
(I'm using MacOS and [iTerm2][iterm]). For example, my custom iTerm colors are lost.

One thing that I do like is `pipenv graph`, which prints out a nice dependency
graph. I did not end up using this too often, but it is a nice feature.

The main problem I had with pipenv is that for some projects `pipenv install`
did not succeed. I had an existing `requirements.txt` which should be fine,
but for some reason installation did not succeed. `pip install` works fine.
I have to look into this a bit more. In the mean time, I could just use
`pipenv run pip install ...` but this really makes no sense, it's basically
an anti-pattern. Also, doing that did not update my Pipfile, so the whole
point of using Pipenv is lost.

I also had an issues where I thought I could not use dependency links, since
`pipenv install` does not have an argument for that. In the end it did work
by setting `PIP_PROCESS_DEPENDENCY_LINKS=1` environment variable first.

Overall, it seems that Pipenv does not really work well for at the moment.
It does not solve the issues that I have been having, and outright
does not work for some existing projects. Locking is very slow and
typing `pipenv run` is tedious. I might end up using for some new
personal projects in the future, however, and see if I get used
to the new workflow.

# Current workflow

I am still sticking to my old workflow, which is
1. set local python version: `pyenv local VERSION`
2. init a new virtual environment: `python -m venv VENV_NAME`
3. activate virtual env
4. `pip install` whatever is needed, manage `requirements.txt` with `pip freeze`

`setup.py` needs to manually kept up to date in any case. Also it's probably
a good idea to use some tool to manage multiple virtual environments and
switch between them. Other than that, for my basic workflow these commands work
fine and there are very few extra dependencies.

I will keep experimenting with Pipenv, and probably try out other tools like
[poetry][poetry], although it has its own issues. One thing I'm especially
interested in is reliable dependency resolution, since I have had some
issues with that.

It's obvious that Python packaging and dependency management are pain points.
There are multiple different workflows for different needs, instead of one
good one.

[pyenv]: https://github.com/pyenv/pyenv
[pyenvvenv]: https://github.com/pyenv/pyenv-virtualenvo
[wrapper]: https://virtualenvwrapper.readthedocs.io/en/latest/
[pew]: https://github.com/berdario/pew
[pep508]: https://www.python.org/dev/peps/pep-0508/
[pep508merge]: https://github.com/pypa/pip/pull/5571
[issue]: https://github.com/pypa/pip/issues/5898
[pipenv]: https://github.com/pypa/pipenv
[iterm]: https://www.iterm2.com/
[poetry]: https://github.com/sdispater/poetry
