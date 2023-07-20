``nbsphinx``
============

:doc:`nbsphinx <nbsphinx:index>` ist eine :doc:`Sphinx
<sphinx:index>`-Erweiterung, die einen Parser für :file:`*.ipynb`-Dateien
bereitstellt: Jupyter Notebook-Code-Zellen werden sowohl in der HTML- wie auch
in der LaTeX-Ausgabe angezeigt. Notebooks ohne gespeicherte Ausgabezellen werden
automatisch während des Sphinx-Build-Prozesses erstellt.

Installation
------------

.. code-block:: console

    $ pipenv install sphinx nbsphinx

Requirements
~~~~~~~~~~~~

* :doc:`../nbconvert`

Konfiguration
-------------

Sphinx konfigurieren
~~~~~~~~~~~~~~~~~~~~

#. Erstellen einer Dokumentation mit Sphinx:

   .. code-block:: console

    $ pipenv run python -m sphinx.cmd.quickstart

#. Danach befindet sich im neu erstellten Verzeichnis die
   Sphinx-Konfigurationsdatei :file:`conf.py`. In dieser  wird ``nbsphinx`` als
   Erweiterung hinzugefügt und generierte Notebooks ausgeschlossen:

   .. code-block:: python

    extensions = [
        ...
        'nbsphinx',
    ]
    ...
    exclude_patterns = [
        ...
        '**/.ipynb_checkpoints',
    ]

   Ein Beispiel findet ihr in der :download:`/conf.py`-Datei dieses
   Jupyter-Tutorials.

Ihr könnt noch weitere Konfigurationen für ``nbsphinx`` vornehmen.

Timeout
    In der Standardeinstellung von ``nbsphinx`` ist der Timeout für eine Zelle
    auf 30 Sekunden eingestellt. Ihr könnt dies für euer Sphinx-Projekt in der
    ``conf.py``-Datei ändern mit :samp:`nbsphinx_timeout = {60}`.

    Alternativ könnt ihr dies auch für einzelne Code-Zellen in den Metadaten der
    Code-Zelle angeben:

    .. code-block:: json

     {
      "cells": [
       {
        "cell_type": "markdown",
        "nbsphinx": {
          "timeout": 60
        },
       }
      ],
     }

    Soll das Timeout deaktiviert werden, kann ``-1`` angegeben werden.

Benutzerdefinierte Formate
    Bibliotheken wie :abbr:`z.B. (zum Beispiel)` `jupytext
    <https://github.com/mwouts/jupytext>`_ speichern Notebooks in anderen
    Formaten ab, :abbr:`z.B. (zum Beispiel)` als *R-Markdown* mit dem Suffix
    ``Rmd``. Damit diese von ``nbsphinx`` ebenfalls ausgeführt werden können,
    können in der Sphinx-Konfigurationsdatei :file:`conf.py` mit
    ``nbsphinx_custom_formats`` weitere Formate angegeben werden, :abbr:`z.B.
    (zum Beispiel)`

    .. code-block:: python

        import jupytext


        nbsphinx_custom_formats = {
            ".Rmd": lambda s: jupytext.reads(s, ".Rmd"),
        }

Zellen konfigurieren
~~~~~~~~~~~~~~~~~~~~

Zelle nicht anzeigen
    .. code-block:: json

     {
      "cells": [
       {
        "cell_type": "markdown",
        "metadata": {
         "nbsphinx": "hidden"
        },
       }
      ],
     }

``nbsphinx-toctree``
    Mit dieser Anweisung könnt ihr innerhalb einer Notebook-Zelle von Sphinx ein
    Inhaltsverzeichnis erstellen lassen, :abbr:`z.B. (zum Beispiel)`

    .. code-block:: json

     {
      "cells": [
       {
        "cell_type": "markdown",
        "metadata": {
         "nbsphinx-toctree": {
           "maxdepth": 2
         }
        "source": [
         "Der folgende Titel wird als ``toctree caption`` gerendert.\n",
         "\n",
         "## Inhalt\n",
         "\n",
         "[Ein Notebook](ein-notebook.ipynb)\n",
         "\n",
         "[Ein externer HTML-Link](https://jupyter-tutorial.readthedocs.io/)\n",
        ]
        },
       }
      ],
     }

    Weitere Optionen findet ihr in der :label:`Sphinx-Dokumentation
    <sphinx:toctree-directive>`.

Build
-----

#. Nun könnt ihr im Inhaltsverzeichnis eurer ``index.rst``-Datei eure
   :file:`*.ipynb`-Datei hinzufügen, siehe :abbr:`z.B. (zum Beispiel)`
   `jupyter-tutorial/notebook/testing/index.rst
   <https://jupyter-tutorial.readthedocs.io/de/latest/_sources/notebook/testing/index.rst.txt>`_.

#. Schließlich könnt ihr die Seiten generieren, :abbr:`z.B. (zum Beispiel)` HTML
   mit :samp:`$ pipenv run python -m sphinx {SOURCE_DIR} {BUILD_DIR}` oder
   :samp:`$ pipenv run python -m sphinx {SOURCE_DIR} {BUILD_DIR} -j
   {NUMBER_OF_PROCESSES}`.

   wobei ``-j`` die Zahl der Prozesse angibt, die parallel ausgeführt werden
   sollen.

   Wenn ihr eine LaTeX-Datei erzeugen wollt, könnt ihr dies mit :samp:`$ pipenv
   run python -m sphinx {SOURCE_DIR} {BUILD_DIR} -b latex}`.

#. Alternativ könnt ihr euch mit ``sphinx-autobuild`` die Dokumentation auch
   automatisch generieren lassen. Es kann installiert werden mit

   .. code-block:: console

    $ pipenv run python -m pip install sphinx-autobuild

   Anschließend kann die automatische Erstellung gestartet werden mit :samp:`$
   pipenv run python -m sphinx_autobuild {SOURCE_DIR} {BUILD_DIR}`.

   Dadurch wird ein lokaler Webserver gestartet, der die generierten HTML-Seiten
   unter ``http://localhost:8000/`` bereitstellt. Und jedes Mal, wenn ihr
   Änderungen in der Sphinx-Dokumentation speichert, werden die entsprechenden
   HTML-Seiten neu generiert und die Browseransicht aktualisiert.

   Ihr könnt dies auch nutzen, um die LaTeX-Ausgabe automatisch zu erstellen:
   :samp:`$ pipenv run python -m sphinx_autobuild {SOURCE_DIR} {BUILD_DIR} -b
   latex`.

#. Eine andere Alternative ist die Publikation auf `readthedocs.org
   <https://readthedocs.org/>`_.

   Hierfür müsst ihr  zunächst ein Konto unter https://readthedocs.org/
   erstellen und dann euer GitLab-, Github- oder Bitbucket-Konto verbinden.

Markdown-Zellen
~~~~~~~~~~~~~~~

Gleichungen
    Gleichungen können *inline* zwischen ``$``-Zeichen angegeben werden,
    :abbr:`z.B. (zum Beispiel)`

    .. code-block:: latex

        $\text{e}^{i\pi} = -1$

    Und auch zeilenweise können Gleichungen ausgedrückt werden :abbr:`z.B. (zum
    Beispiel)`

    .. code-block:: latex

        \begin{equation}
        \int\limits_{-\infty}^\infty f(x) \delta(x - x_0) dx = f(x_0)
        \end{equation}

    .. seealso::
        * `Equation Numbering
          <https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/nbextensions/equation-numbering/readme.html>`_

Zitate
    ``nbsphinx`` unterstützt dieselbe Syntax für Zitate wie `nbconvert
    <https://nbconvert.readthedocs.io/en/latest/latex_citations.html>`_:

    .. code-block:: html

        <cite data-cite="kluyver2016jupyter">Kluyver et al. (2016)</cite>

Alarmierungsboxen
    .. code-block:: html

       <div class="alert alert-block alert-info">

       **Note**

       This is a notice!
       </div>

       <div class="alert alert-block alert-success">

       **Success**

       This is a success notice!
       </div>

       <div class="alert alert-block alert-warning">

       **Warning**

       This is a warning!
       </div>

       <div class="alert alert-block alert-danger">

       **Danger**

       This is a danger notice!

Links zu anderen Notebooks
    .. code-block:: md

        a link to a notebook in a subdirectory](subdir/notebook-in-a-subdir.ipynb)

Links zu :file:`*.rst`-Dateien
    .. code-block:: md

        [reStructuredText file](rst-file.rst)

Links zu lokalen Dateien
    .. code-block:: md

        [Pipfile](Pipfile)

Code-Zellen
~~~~~~~~~~~

Javascript
    Für das generierte HTML kann Javascript verwendet werden, :abbr:`z.B. (zum
    Beispiel)`:

    .. code-block:: javascript

        %%javascript

        var text = document.createTextNode("Hello, I was generated with JavaScript!");
        // Content appended to "element" will be visible in the output area:
        element.appendChild(text);

Galerien
--------

nbsphinx bietet Unterstützung für die Erstellung von `Thumbnail-Galerien aus
einer Liste von Jupyter-Notebooks
<https://nbsphinx.readthedocs.io/en/0.9.2/subdir/gallery.html>`_. Diese
Funktionalität basiert auf `Sphinx-Gallery <https://sphinx-gallery.github.io/>`_
und erweitert diese, um mit Jupyter-Notebooks statt mit Python-Skripten zu
arbeiten.

Sphinx-Gallery unterstützt auch direkt :doc:`pyviz:matplotlib/index`,
:doc:`pyviz:matplotlib/seaborn/index` und `Mayavi
<https://docs.enthought.com/mayavi/mayavi/>`_.

Installation
~~~~~~~~~~~~

Sphinx-Gallery lässt sich für Sphinx ≥ 1.8.3 installieren mit

.. code-block:: console

    $ pipenv install sphinx-gallery

Konfiguration
~~~~~~~~~~~~~

Damit Sphinx-Gallery genutzt werden kann, muss sie zudem noch in die ``conf.py``
eingetragen werden:

.. code-block:: python

    extensions = [
       'nbsphinx',
       'sphinx_gallery.load_style',
    ]

Anschließend könnt ihr Sphinx-Gallery auf zwei verschiedene Arten nutzen:

#. Mit der reStructuredText-Direktive ``.. nbgallery::``.

   .. seealso::
      `Thumbnail Galleries
      <https://nbsphinx.readthedocs.io/en/0.9.2/a-normal-rst-file.html#thumbnail-galleries>`_

#. In einem Jupyter-Notizbuch, indem ein ``nbsphinx-gallery``-Tag zu den
   Metadaten einer Zelle hinzugefügt wird:

   .. code-block:: javascript

      {
          "tags": [
              "nbsphinx-gallery"
          ]
      }
