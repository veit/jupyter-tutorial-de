Installation
============

Ihr könnt Panel in der virtuellen Umgebung eurer Jupyter-Kernels installieren
mit:

.. code-block:: console

    $ uv add panel

.. tip::
   `watchfiles <https://watchfiles.helpmanual.io>`_ unterstützt die Autoreload-Funktionen von Panel, wenn der ``--dev``-Modus aktiviert ist:

   .. code-block:: console

      $ uv add --dev watchfiles

.. tip::
   Für die Syntax-Hervorhebung sollte auch `pygments <https://pygments.org/>`_
   installiert werden:

   .. code-block:: console

      $ uv add pygments

.. seealso::
   Wenn Panel :abbr:`z.B. (zum Beispiel)` in VSCode oder Google Colab verwendet
   werden soll, schaut euch `Develop in other notebook environments
   <https://panel.holoviz.org/how_to/notebook/other_nb.html>`_  an.
