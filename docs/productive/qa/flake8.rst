``flake8``
==========

`flake8 <https://pypi.org/project/flake8/>`_ stellt sicher, dass euer Code
größtenteils :pep:`8` folgt. Eine automatische Formatierung, :abbr:`z.B. (zum
Beispiel)` mit :doc:`black`, ist jedoch noch komfortabler. Zudem prüft
``flake8`` auf nicht verwendete Importe.

Installation
------------

.. code-block:: console

    $ spack env activate python-374
    $ spack install py-flake8 ^python@3.7.4

Überprüfen
----------

.. code-block:: console

    $ flake8 path/to/your/code


``flake8`` kann für :doc:`python-basics:test/tox` konfiguriert werden in der
``tox.ini``-Datei eines Pakets, z.B.:

.. code-block:: ini

    [tox]
    envlist = py37, py38, flake8, docs

    [testenv:flake8]
    basepython = python
    deps =
        flake8
        flake8-isort
    commands =
        flake8 src tests setup.py conftest.py docs/conf.py

.. seealso::
    * `Configuring flake8
      <https://flake8.pycqa.org/en/latest/user/configuration.html>`_
    * `flake8 error/violation codes
      <https://flake8.pycqa.org/en/latest/user/error-codes.html>`_
    * `pycodestyle error codes
      <https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes>`_
