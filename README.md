# pylsl

[![Build status](https://ci.appveyor.com/api/projects/status/ggouc09585l2518i/branch/master?svg=true)](https://ci.appveyor.com/project/cboulay/liblsl-python/branch/master)
[![PyPI version](https://badge.fury.io/py/pylsl.svg)](https://badge.fury.io/py/pylsl)

This is the Python interface to the [Lab Streaming Layer (LSL)](https://github.com/sccn/labstreaminglayer).
LSL is an overlay network for real-time exchange of time series between applications,
most often used in research environments. LSL has clients for many other languages
and platforms that are compatible with each other.

Let us know if you encounter any bugs (ideally using the issue tracker on
the GitHub project).

# Installation

## Prerequisites

On all non-Windows platforms and for some Windows-Python combinations, you must first obtain a liblsl shared library:

* On many platforms it can be installed with `conda install -c conda-forge liblsl`
* Alternatively on Mac it can be installed with `brew install labstreaminglayer/tap/lsl`
* You might be able to find the appropriate liblsl shared object (*.so on Linux, *.dylib on MacOS, or *.dll on Windows) from the [liblsl release page](https://github.com/sccn/liblsl/releases).
* Otherwise you might try to clone liblsl and use its `standalone_compilation_linux.sh` script (works on raspberry pi).

## Prepared distributions

Install from [pypi](https://pypi.org/project/pylsl/)
using [pip](https://pip.pypa.io/en/stable/installing/): `pip install pylsl`

For several distributions, the pip distribution ships with lsl.dll. For every other case, liblsl must be installed somewhere on the PATH (see Prerequisites above) or downloaded and copied somewhere on the search path. We recommend you copy it to the pylsl installed module path's `lib` subfolder. You can find the pylsl installed module path with `python -c "import pylsl; print(pylsl.__file__)"`

(use `cp -L` on platforms that use symlinks)

## Self-built
  
* Copy the shared object (see Prerequisites above) into `liblsl-Python/pylsl/lib`.
* From the `liblsl-Python` working directory, run `pip install .`.
    * Note: You can use `pip install -e .` to install while keeping the files in-place. This is convenient for developing pylsl.

# Usage

See the examples in pylsl/examples. Note that these can be run directly from the commandline with (e.g.) `python -m pylsl.examples.SendStringMarkers`.

# For maintainers

## Continuous Integration

pylsl uses continuous integration and distribution.

Whenever a new commit is pushed, AppVeyor downloads liblsl from its releases page, copies it to the package, then builds wheels for distribution. This process is repeated for each of Windows, Mac, and Linux.

In addition, whenever a new `git tag` is used on a commit that is pushed to the master branch, the CI systems will deploy the wheels to pypi.

## Manual Distribution

1. Manual way:
    1. `rm -Rf build dist *.egg-info`
    1. `python setup.py sdist bdist_wheel`
    1. Additional steps on Linux:
        * `auditwheel repair dist/*.whl -w dist`
        * `rm dist/*-linux_x86_64.whl`
    1. `twine upload dist/*`
1. For conda
    1. build liblsl: `conda build ../liblsl/`
    1. `conda build .`

# Known Issues

* On Linux one currently cannot call ``pylsl`` functions from a thread that is not the main thread.

# Acknowledgments

Pylsl was primarily written by Christian Kothe while at Swartz Center for Computational Neuroscience, UCSD. The LSL project was funded by the Army Research Laboratory under Cooperative Agreement Number W911NF-10-2-0022 as well as through NINDS grant 3R01NS047293-06S1. pylsl is maintained primarily by Chadwick Boulay. Thanks for contributions, bug reports, and suggestions go to Bastian Venthur, David Medine, Clemens Brunner, and Matthew Grivich.
