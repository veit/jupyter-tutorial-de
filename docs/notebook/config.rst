Jupyter-Pfade und -Konfiguration
================================

Konfigurationsdateien werden üblicherweise im :file:`~/.jupyter`-Verzeichnis
gespeichert. Mit der Umgebungsvariablen :envvar:`JUPYTER_CONFIG_DIR` kann jedoch
auch ein anderes Verzeichnis festgelegt werden. Falls Jupyter im
:envvar:`JUPYTER_CONFIG_DIR` keine Konfiguration findet, durchläuft Jupyter den
Suchpfad mit :file:`/{SYS.PREFIX}/etc/jupyter/` und anschließend für Unix
:file:`/usr/local/etc/jupyter/` und :file:`/etc/jupyter/`, für Windows
:file:`%PROGRAMDATA%\\jupyter\\`.

Ihr könnt Euch die aktuell verwendeten Konfigurationsverzeichnisse aufzulisten
lassen mit:

.. code-block:: console

    $ jupyter --paths
    config:
        /Users/veit/.jupyter
        /Users/veit/.local/share/virtualenvs/jupyter-tutorial--q5BvmfG/bin/../etc/jupyter
        /usr/local/etc/jupyter
        /etc/jupyter
    ...

Erstellen der Konfigurationsdateien
-----------------------------------

Ihr könnt eine Standardkonfiguration erstellen mit:

.. code-block:: console

    $ jupyter notebook --generate-config
    Writing default config to: /Users/veit/.jupyter/jupyter_notebook_config.py

Allgemeiner lassen sich Konfigurationsdateien für alle Jupyter-Applikationen
anlegen mit :samp:`jupyter {APPLICATION} --generate-config`.

Ändern der Konfiguration
------------------------

… durch Bearbeiten der Konfigurationsdatei
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

z.B. in ``jupyter_notebook_config.py``:

.. code-block:: python

    c.NotebookApp.port = 8754

Sofern die Werte als ``list``, ``dict`` oder ``set`` gespeichert werden, können
diese auch ergänzt werden mit ``append``, ``extend``, ``prepend``, ``add`` und
``update``, z.B.:

.. code-block:: python

    c.TemplateExporter.template_path.append('./templates')

… mit der Befehlszeile
~~~~~~~~~~~~~~~~~~~~~~

:abbr:`z.B. (zum Beispiel)`:

.. code-block:: console

    $ jupyter notebook --NotebookApp.port=8754

Dabei gibt es für häufig verwendete Optionen Aliase wie z.B. für ``--port``
oder ``--no-browser``.

Die Befehlszeilenoptionen überschreiben die in einer Konfigurationsdatei
festgelegten Optionen.

.. seealso::
   `traitlets.config
   <https://traitlets.readthedocs.io/en/latest/config.html#module-traitlets.config>`_
