---
mapped_pages:
  - https://www.elastic.co/guide/en/beats/devguide/current/python-beats.html
---

# Python in Beats [python-beats]

Python is used for Beats development, it is the language used to implement system tests and some other tools. Python dependencies are managed by the use of virtual environments, supported by [venv](https://docs.python.org/3/library/venv.html).

Beats development requires Python >= 3.7.

## Installing Python and venv [installing-python]

Python uses to be installed in many operating systems. If it is not installed in your system you can follow the instructions available in [https://www.python.org/downloads/](https://www.python.org/downloads/)

In Ubuntu/Debian systems, Python 3 can be installed with:

```sh
sudo apt-get install python3 python3-venv
```

There are packages for specific minor versions, so for example if Python 3.7 wants to be used, it can be installed with the following command:

```sh
sudo apt-get install python3.7 python3.7-venv
```

It is recommended to use Python >= 3.7.


## Working with virtual environments [python-virtual-environments]

All `make` and `mage` targets manage their own virtual environments in a transparent way, so for the most common operations required when contributing to beats, nothing special needs to be done.

Virtual environments used by `make` can be found in most Beats directories under `build/python-env`, they are created by targets that need it, or can be explicitly created by running `make python-env`. The ones used by `mage` are created when required under `build/ve`.

There are some environment variables that can be used to customize the creation of these virtual environments:

* `PYTHON_EXE`: Python executable to be used in the virtual environment. It has to exist in the path.
* `PYTHON_ENV`: Path to the virtual environment to use. If it doesn’t exist, it is created by `make` or `mage` targets when needed.

Virtual environments can also be used without `make` or `mage`, this is usual for example when running individual system tests with `pytest`. There are two ways to run commands from the virtual environment:

* "Activating" the virtual environment in your current terminal running `source ./build/python-env/bin/activate`. Virtual environment can be deactivated by running `deactivate`.
* Directly running commands from the virtual environment path. For example `pytest` can be executed as `./build/python-env/bin/pytest`.

To recreate a virtual environment, remove its directory. All virtual environments are also removed with `make clean`.


## Working with older versions [python-older-versions]

Older versions of Beats were not compatible with Python 3, if you need to temporary work on one of these versions of Beats, and you don’t want to remove your current virtual environments, you can use environment variables to run commands in a temporary virtual environment.

For example you can run `make update` with Python 2.7 with the following command:

```sh
PYTHON_EXE=python2.7 PYTHON_ENV=/tmp/venv2 make update
```

If you need to run tests you can also create a virtual environment and then activate it to run commands from there:

```sh
PYTHON_EXE=python2.7 PYTHON_ENV=/tmp/venv2 make python-env
source /tmp/venv2/bin/activate
...
```


