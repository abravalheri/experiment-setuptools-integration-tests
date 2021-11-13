Experiment
==========

This repository corresponds to the code used in an experiment to verify
`proposed integration tests`_ for setuptools_, by running Github Actions.
It tries to replicate the same proposed test (and conditions) that would run in
setuptools CI.

Specifically:

- The following files where copied from setuptools and `pypa/setuptools#2863`_::

    .
    ├── conftest.py
    ├── pytest.ini
    └── setuptools/
        └── tests
            ├── contexts.py
            ├── fixtures.py
            └── integration
                ├── helpers.py
                └── test_pip_install_sdist.py  * minor change

- The following file were created empty::

    .
    └── setuptools/
        ├── __init__.py
        └── tests
            ├── __init__.py
            └── integration
                └── __init__.py

- The following files were copied and then adapted::

    .
    ├── tox.ini
    ├── pyproject.toml
    └── .github
        └── workflows
            └── main.yml


In ``test_pip_install_sdist.py``, the following change is introduced:


.. code-block:: diff

    - correct_setuptools = os.getenv("PROJECT_ROOT") or SETUPTOOLS_ROOT
    + # correct_setuptools = os.getenv("PROJECT_ROOT") or SETUPTOOLS_ROOT
    + correct_setuptools = (
    +    "git+https://github.com/pypa/setuptools@main#egg=setuptools"
    + )

This is required since this repository does not contain setuptools codebase.
Instead it is installed directly from GitHub.


More information can be found in:

- https://github.com/pypa/setuptools/pull/2863
- https://github.com/pypa/setuptools/pull/2862

.. _setuptools: https://setuptools.pypa.io/en/latest/userguide/declarative_config.html
.. _pypa/setuptools#2863: https://github.com/pypa/setuptools/pull/2863
.. _proposed integration tests: https://github.com/pypa/setuptools/pull/2862
