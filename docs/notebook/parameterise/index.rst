Parametrisierung und Zeitplanung
================================

Mit :doc:`../../jupyterlab/index` könnt ihr den :doc:`Jupyter Scheduler
<../../jupyterlab/scheduler>` zum Parametrisieren und zeitgesteuertem Ausführen
verwenden. Für Jupyter Notebooks steht euch `papermill
<https://papermill.readthedocs.io/en/latest/>`_ zur Verfügung.

Installieren
------------

.. code-block:: console

   $ uv add papermill

Verwenden
---------

#. Parametrisieren

   Der erste Schritt ist die Parametrisierung des Notebook. Dazu werden die
   Zellen in :menuselection:`View --> Cell Toolbar --> Tags` als Parameter
   markiert.

#. Überprüfen

   Ihr könnt das Notebook inspizieren, :abbr:`z.B. (zum Beispiel)` mit

   .. code-block:: console

        $ uv run papermill --help-notebook docs/refactoring/parameterise/input.ipynb
        Usage: papermill [OPTIONS] NOTEBOOK_PATH [OUTPUT_PATH]

        Parameters inferred for notebook 'docs/refactoring/parameterise/input.ipynb':
          msg: Unknown type (default None)

#. Ausführen

   Es gibt zwei Möglichkeiten, ein Notebook mit Parametern auszuführen:

   * … via Python API

     Die Funktion  :func:`execute_notebook` kann aufgerufen werden, um ein
     Notebook mit einem Dict von Parametern auszuführen:

     .. code-block:: python

        execute_notebook(INPUT_NOTEBOOK, OUTPUT_NOTEBOOK, DICTIONARY_OF_PARAMETERS)

     :abbr:`z.B. (zum Beispiel)` für :file:`input.ipynb`:

     .. code-block:: ipython

        In [1]: import papermill as pm

        In [2]: pm.execute_notebook(
                    "PATH/TO/INPUT_NOTEBOOK.ipynb",
                    "PATH/TO/OUTPUT_NOTEBOOK.ipynb",
                    parameters=dict(salutation="Hello", name="pythonistas"),
                )

     Das Ergebnis ist :file:`output.ipynb`:

     .. code-block:: ipython

        In [1]: salutation = None
                name = None

        In [2]: # Parameters
                salutation = "Hello"
                name = "pythonistas"

        In [3]: from datetime import date


                today = date.today()
                print(
                    salutation,
                    name,
                    "– welcome to our event on this " + today.strftime("%A, %d %B %Y"),
                )

        Out[3]: Hello pythonistas – welcome to our event on this Monday, 26 June 2023

     .. code-block:: python

        import papermill as pm


        pm.execute_notebook(
            "PATH/TO/INPUT_NOTEBOOK.ipynb",
            "PATH/TO/OUTPUT_NOTEBOOK.ipynb",
            parameters=dict(salutation="Hello", name="pythonistas"),
        )

     .. seealso::
        * `Workflow reference
          <https://papermill.readthedocs.io/en/latest/reference/papermill-workflow.html>`_

   * … via CLI

     .. code-block:: console

        $ uv run papermill input.ipynb output.ipynb -p salutation 'Hello' -p name 'pythonistas'

     Alternativ kann auch eine YAML-Datei mit den Parametern angegeben werden,
     :abbr:`z.B. (zum Beispiel)` :file:`params.yaml`:

     .. literalinclude:: params.yaml
        :caption: params.yaml
        :name: params.yaml

     .. code-block:: console

        $ uv run papermill input.ipynb output.ipynb -f params.yaml

     Mit ``-b`` kann ein base64-kodierte YAML-String angegeben werden, die die
     Parameterwerte enthält:

     .. code-block:: console

        $ uv run papermill input.ipynb output.ipynb -b c2FsdXRhdGlvbjogIkhlbGxvIgpuYW1lOiAiUHl0aG9uaXN0YXMi

     .. seealso::
        * `CLI reference
          <https://papermill.readthedocs.io/en/latest/usage-cli.html>`_

     Ihr könnt dem Dateinamen auch einen Zeitstempel hinzufügen:

     .. code-block:: console

        $ dt=$(date '+%Y-%m-%d_%H:%M:%S')
        $ uv run papermill input.ipynb output_$(date '+%Y-%m-%d_%H:%M:%S').ipynb -f params.yaml

     Dies erzeugt eine Ausgabedatei, deren Dateiname einen Zeitstempel enthält,
     :abbr:`z.B. (zum Beispiel)` :download:`output_2023-06-26_15:57:33.ipynb`.

     Schließlich könnt ihr ``crontab -e`` verwenden, um die beiden Befehle
     automatisch zu bestimmten Zeiten auszuführen, z.B. am ersten Tag eines
     jeden Monats:

     .. code-block::

        dt=$(date '+%Y-%m-%d_%H:%M:%S')
        0 0 1 * * cd ~/jupyter-notebook && uv run papermill input.ipynb output_$(date '+%Y-%m-%d_%H:%M:%S').ipynb -f params.yaml

#. Speichern

   Papermill kann Notebooks an einer Reihe von Orten speichern, einschließlich
   S3, Azure Data Blobs und Azure Data Lakes. Papermill erlaubt auch, neue
   Datenspeicher hinzuzufügen.

   .. seealso::
        * `papermill Storage
          <https://papermill.readthedocs.io/en/latest/reference/papermill-storage.html>`_
        * `Extending papermill through entry points
          <https://papermill.readthedocs.io/en/latest/extending-entry-points.html>`_
