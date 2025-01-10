Plugin erstellen
================

Neben den bestehenden Notebook Extensions können auch weitere Plugins
hinzugefügt werden. Das Verzeichnis, in dem
``jupyter_contrib_nbextensions/nbextensions`` liegt, bekommt ihr mit
``pip show`` heraus:

.. code-block:: console

    $ uv pip show jupyter_contrib_nbextensions
    Name: jupyter-contrib-nbextensions
    Version: 0.7.0
    Location: /Users/veit/cusy/trn/jupyter-tutorial/.venv/lib/python3.13/site-packages
    Requires: ipython-genutils, jupyter-contrib-core, jupyter-core, jupyter-highlight-selected-word, jupyter-nbextensions-configurator, lxml, nbconvert, notebook, tornado, traitlets
    Required-by: jupyter-tutorial

In diesem Verzeichnis liegen die einzelnen Notebook-Erweiterungen, :abbr:`z.B.
(zum Beispiel)` mit folgender Struktur:

.. code-block:: console

   $ tree
   .
   ├── main.js
   ├── main.yaml
   └── readme.md

``main.js``
    enthält die eigentliche Logik der Erweiterung, :abbr:`z.B. (zum Beispiel)`:


    .. code-block:: javascript

       define([
           'require',
           'base/js/namespace',
       ], function (
           requirejs
           $,
           Jupyter,
       ) {
           "use strict";

           // define default values for config parameters
           var params = {
               my_config_value : 100
           };

           var initialize = function () {
               $.extend(true, params, Jupyter.notebook.config.myextension);

               $('<link/>')
                   .attr({
                       rel: 'stylesheet',
                       type: 'text/css',
                       href: requirejs.toUrl('./myextension.css')
                   })
                   .appendTo('head');
           };

           var load_ipython_extension = function () {
               return Jupyter.notebook.config.loaded.then(initialize);
           };

           return {
               load_ipython_extension : load_ipython_extension
           };
       });

``main.yaml``
    `yaml <https://de.wikipedia.org/wiki/YAML>`_-Datei, die die Erweiterung
    für den Jupyter Extensions Configurator beschreibt.

    .. code-block:: yaml

       Type: Jupyter Notebook Extension
       Compatibility: 3.x, 4.x, 5.x, 6.x
       Name: My notebook extensions
       Main: main.js
       Link: README.md
       Description: |
         My notebook extension helps with the use of Jupyter notebooks.
       Parameters:
       - none

    Weitere Informationen zu den vom *Configurator* unterstützten Optionen
    findet ihr auf GitHub: `jupyter_nbextensions_configurator
    <https://github.com/jupyter-contrib/jupyter_nbextensions_configurator>`_.

``readme.md``
    Markdown-Datei, die die Erweiterung beschreibt und angibt, wie sie
    verwendet werden kann. Dies wird auch im Reiter
    :menuselection:`Nbextensions` angezeigt.

.. seealso::
   * :doc:`jupyter-contrib-nbextensions:internals`

Setup Jupyter Notebook Extension
--------------------------------

Dies ist eine Erweiterung, die einige Probleme beim Arbeiten mit Notebooks
behebt, die Joel Grus auf der JupyterCon 2018 vorgetragen hat: `I Don’t Like
Notebooks <https://www.youtube.com/watch?v=7jiPeIFXb6U>`_:

* sie fordert euch auf, das Notebook zu benennen
* sie erstellt eine Vorlage, um die Dokumentation zu verbessern
* sie importiert und konfiguriert häufig verwendete Bibliotheken

Installation
~~~~~~~~~~~~

#. Findet heraus, wo die Notebook-Extensions installiert sind:

   .. code-block:: console

      $ uv pip show jupyter_contrib_nbextensions
      Name: jupyter-contrib-nbextensions
      Version: 0.7.0
      Location: /Users/veit/cusy/trn/jupyter-tutorial/.venv/lib/python3.13/site-packages
      Requires: ipython-genutils, jupyter-contrib-core, jupyter-core, jupyter-highlight-selected-word, jupyter-nbextensions-configurator, lxml, nbconvert, notebook, tornado, traitlets
      Required-by: jupyter-tutorial

#. Ladet das `Setup
   <https://github.com/WillKoehrsen/Data-Analysis/tree/master/setup>`_-Verzeichnis
   in ``jupyter_contrib_nbextensions/nbextensions/`` herunter.

#. Installiert die Erweiterung mit

   .. code-block:: console

      $ uv run --with jupyter jupyter contrib nbextensions install --user
      …
      [I 10:54:46 InstallContribNbextensionsApp] Installing /Users/veit/.local/share/virtualenvs/jupyter-tutorial--q5BvmfG/lib/python3.7/site-packages/jupyter_contrib_nbextensions/nbextensions/setup -> setup
      [I 10:54:46 InstallContribNbextensionsApp] Making directory: /Users/veit/Library/Jupyter/nbextensions/setup/
      [I 10:54:46 InstallContribNbextensionsApp] Copying: /Users/veit/.local/share/virtualenvs/jupyter-tutorial--q5BvmfG/lib/python3.7/site-packages/jupyter_contrib_nbextensions/nbextensions/setup/setup.yaml -> /Users/veit/Library/Jupyter/nbextensions/setup/setup.yaml
      [I 10:54:46 InstallContribNbextensionsApp] Copying: /Users/veit/.local/share/virtualenvs/jupyter-tutorial--q5BvmfG/lib/python3.7/site-packages/jupyter_contrib_nbextensions/nbextensions/setup/README.md -> /Users/veit/Library/Jupyter/nbextensions/setup/README.md
      [I 10:54:46 InstallContribNbextensionsApp] Copying: /Users/veit/.local/share/virtualenvs/jupyter-tutorial--q5BvmfG/lib/python3.7/site-packages/jupyter_contrib_nbextensions/nbextensions/setup/main.js -> /Users/veit/Library/Jupyter/nbextensions/setup/main.js
      [I 10:54:46 InstallContribNbextensionsApp] - Validating: OK
      …

#. Aktiviert die *Setup*-Extension in :menuselection:`Nbextensions`.

Schließlich könnt ihr ein neues Notebook erstellen, das dann folgende Struktur
aufweist: `setup.ipynb <setup.ipynb>`_.

.. seealso::
   * `Set Your Jupyter Notebook up Right with this Extension
     <https://towardsdatascience.com/set-your-jupyter-notebook-up-right-with-this-extension-24921838a332>`_
   * `GitHub <https://github.com/WillKoehrsen/Data-Analysis/tree/master/setup>`_
