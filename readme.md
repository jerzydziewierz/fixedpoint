# Fixed Point Python Package

The fixedpoint package is a fixed point arithmetic library for python, released
under the [BSD license](LICENSE).

Install fixedpoint with `pip install fixedpoint`.

Package documentation can be found at [readthedocs](https://fixedpoint.readthedocs.io/), or
build your own local documentation:

1. From the root directory, run `pip install .[docs]` (setup.cfg tells pip
   all the necessary dependencies).
2. From the `./docs` directory, run `make html` (tells sphinx to build
   the documentation in html format).
3. View the documentation at `./docs/build/html/index.html`.

## FixedPoint Development and Testing

The `./tests` folder contains the infrastructure for testing this library.

The source code and tests are intended for Python 3.8. Both code and tests are
organized into the following sections:

* Initialization methods
* Property accessors and mutators
* Operators
  * Arithmetic operators
  * Unary operators
  * Comparisons
* Context management
* Built-in functions and type conversion
* Bit resizing
  * Rounding
  * Overflow handling
* Alerts and error handling
* Functions
* Property resolution

The test modules are titled the more or less the same way the code sections are.
There is some overlap between tests, but organizing it in such a way guarantees
that the functionality is tested in the intended manner.

### Running Tests

The following modules are required for testing

* nose (main test runner)
* coverage (measures code coverage)
* mypy (verifies typing)
* tqdm (progress bar)
* sphinx (doctests for documentation)

Run `py ./setup.py nosetests` from the root directory, which installs all
necessary testing dependencies (except MATLAB), and then runs the entire suite
of tests.

If you already have all the test dependencies, you can run `./test.bat` in the
root directory to run the entire test suite. `./setup.cfg` has options that are
used to run nose tests, but you can also supplement `./test.bat` with other
[nose options](https://nose.readthedocs.io/en/latest/usage.html#options).

The coverage report can be found at `./tests/COVERAGE/index.html` after the
tests are run.

#### Common `test.bat` Options

To run individual tests, the syntax is
`./test.bat tests/<test_folder_name>:test_function_name`. For example, to run
the `test_resize` function from `./tests/test_bit_resizing/__init__.py`, the
command would be `./test.bat ./tests/test_bit_resizing:test_resize`.

Alternatively, when tests are run, nose will number them on the printout. Once
they're numbered (which is recorded in `./tests/.noseids`), you can simply
execute `./test.bat <test_number>`.

Run only previously failed tests with `./test.bat --failed`.

#### MATLAB Stimulus

MATLAB is used to generate stimulus for some tests. Versions __R2018a__ and
__R2019a__ were used during development; no other versions are guaranteed to
work.

`./tests/tools.py` includes a MATLAB class that is used to generate MATLAB
stimulus. It requires that MATLAB be in your system path. If this is not the
case, those tests will be skipped. It does not account for unavailability of
licenses.

The `tools.MATLAB` class will not regenerate stimulus if it already exists. You
can either delete the stimulus file to force regeneration, call the script
itself, or call `./tests/gen_data.m` from MATLAB, which loops through all test
directories and generates all data at once. Stimulus generated by MATLAB are
binary files and have a `.stim` extension.

## Deployment Checklist

* Make sure unit tests pass and coverage is sufficient.
* Update `./setup.cfg`; specifically:
    * [Version](https://setuptools.readthedocs.io/en/latest/setuptools.html#specifying-your-project-s-version)
    * [Classifiers](https://pypi.org/classifiers/), specifically:
        * Development Status
        * Programming Language
* Make sure documentation builds and is up to date
    * Update `./docs/source/conf.py` with new [version/release](https://setuptools.readthedocs.io/en/latest/setuptools.html#specifying-your-project-s-version)
* Create/update the changelog and add
  [versionadded](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-versionadded),
  [versionchanged](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-versionchanged),
  [deprecated](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-versionchanged), or
  [seealso](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-seealso)
  directives in the documentation where appropriate.
* Create a wheel: `py setup.py sdist bdist_wheel`
* Deploy to PyPI:
  `py -m twine upload -repository-url https://upload.pypi.org/legacy/ dist/*`
  * You may need to point to your `.pypirc` file with the `--configuration-file`
    switch
