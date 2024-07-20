================
Jupyter-Tutorial
================

`Jupyter-Notebooks <https://jupyter-notebook.readthedocs.io/en/stable/>`_
erfreuen sich in den Datenwissenschaften wachsender Beliebtheit und wurden zum
De-facto-Standard für schnelles Prototyping und explorative Analysen. Sie
beflügeln nicht nur Experimente und Innovationen enorm, sie machen auch den
gesamten Forschungsprozess schneller und zuverlässiger. Zudem entstehen viele
zusätzliche Komponenten, die die ursprünglichen Grenzen ihrer Nutzung erweitern
und neue Verwendungsmöglichkeiten eröffnen.

.. graphviz::

    digraph decide_jupyter {
        graph [fontname = "Calibri", fontsize="10", penwidth="1px",
               overlap=false];
        node [fontname = "Calibri", fontsize="10", style="bold",
              penwidth="1px", fontcolor="#640FFB"; color="#640FFB";];
        edge [fontname = "Calibri", fontsize="10", style="bold",
              penwidth="1px", fontcolor="#640FFB"; color="#640FFB";];
        tooltip="Wie entscheide ich, welche Jupyter-Pakete ich benötige?";
        // Top Level
        what [
            shape=diamond,
            label="Was wollt ihr machen?",
            tooltip="Jupyter bietet euch verschiedene Möglichkeiten, wie ihr die Notebooks nutzen könnt"]
        // Second Level
        singleuser [
            shape=plaintext,
            label=" ",
            tooltip="Single user"]
        team [
            shape=plaintext,
            label=" ",
            tooltip="Team"]
        nbconvert [
            label="nbconvert",
            tooltip="nbconvert installieren und nutzen",
            target="_top",
            href="nbconvert.html"]
        kernels [
            label="Kernels",
            tooltip="Kernels installieren, anzeigen und starten",
            target="_top",
            href="kernels/install.html"]
        extensions [
            shape=plaintext,
            label=" ",
            tooltip="Notebook-Erweiterungen installieren"]
        embed [
            shape=plaintext,
            label="",
            tooltip="Notebooks in andere Anwendungen einbinden"]
        examples [
            label="Unternehmens-\nanwendungen",
            tooltip="Anwendungsbeispiele bei Netflix, Bloomberg etc.",
            target="_top",
            href="use-cases.html"]
        // 3rd Level
        notebook [
            label="Jupyter-\nNotebook",
            tooltip="Notebook lokal installieren",
            target="_top",
            href="notebook/index.html"]
        jupyterlab [
            label="JupyterLab",
            tooltip="JupyterLab lokal installieren",
            target="_top",
            href="jupyterlab/index.html"]
        hub [
            label="JupyterHub",
            tooltip="JupyterHub\ninstallieren",
            target="_top",
            href="hub/index.html"]
        binder [
            label="Binder",
            tooltip="Binder tools",
            target="_top",
            href="binder.html"]
        nbviewer [
            label="nbviewer",
            tooltip="nbviewer installieren und nutzen",
            target="_top",
            href="nbviewer.html"]
        widgets [
            label="Widgets",
            tooltip="ipywidgets installieren und nutzen",
            target="_top",
            href="ipywidgets/index.html"]
        extend [
            label="nbextensions",
            tooltip="Installieren und Verwenden verschiedener Notebook-Erweiterungen",
            target="_top",
            href="nbextensions/index.html"]
        viz [
            label="Daten\nvisualisieren",
            tooltip="Bibliotheken zur Datenvisualisierung",
            target="_top",
            href="viz/index.html"]
        dash [
            label="Dashboards",
            tooltip="Installieren und Verwenden von Dashboards",
            target="_top",
            href="dashboards/index.html"]
        html [
            label="HTML",
            tooltip="Einbinden von Notebooks in statisches HTML",
            target="_top",
            href="ipywidgets/embedding.html"]
        nbsphinx [
            label="nbsphinx",
            tooltip="Einbinden von Notebooks in den Sphinx Document Generator",
            target="_top",
            href="sphinx/nbsphinx.html"]
        executablebooks [
            label="Executable Books",
            tooltip="Bücher aus Jupyter Notebooks und MyST",
            target="_top",
            href="sphinx/executablebooks.html"]
        // Edges
        what -> singleuser [label="Einzel-\narbeit"]
        what -> team [label="Team-\narbeit"]
        what -> nbconvert [label="Konvertieren"]
        nbconvert -> nbviewer [label="Konvertier-\nservice"]
        what -> kernels [label="Java, R,\nJulia etc."]
        what -> extensions [label="Notebook\nerweitern"]
        what -> embed [label="Notebooks\neinbetten"]
        what -> examples [label="Beispiele"]
        singleuser -> {notebook jupyterlab}
        team -> {hub binder}
        extensions -> {widgets extend viz dash}
        embed -> {html nbsphinx executablebooks}
        // Arrangement
        rankdir="LR"
        {rank = same; what;}
        {rank = same; singleuser; team; nbconvert; kernels; extensions; embed;
                examples;}
        {rank = same; notebook; jupyterlab; hub; binder; widgets; extend; viz;
                dash; html}
    }

Das Jupyter-Tutorial ist jedoch nur ein Teil einer Reihe von Tutorials zur
Datenanalyse und -visualisierung:

* :doc:`python-basics:index`
* :doc:`python4datascience:index`
* :doc:`PyViz Tutorial <pyviz:index>`
* `cusy Design-System: Datenvisualisierung
  <https://www.cusy.design/de/latest/viz/index.html>`_

Alle Tutorials dienen als Seminarunterlagen für unsere aufeinander abgestimmten
Trainings:

+---------------+--------------------------------------------------------------+
| Dauer         | Titel                                                        |
+===============+==============================================================+
| 3 Tage        | `Einführung in Python`_                                      |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Fortgeschrittenes Python`_                                  |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Entwurfsmuster in Python`_                                  |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Effizient Testen mit Python`_                               |
+---------------+--------------------------------------------------------------+
| 1 Tag         | `Software-Dokumentation mit Sphinx`_                         |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Technisches Schreiben`_                                     |
+---------------+--------------------------------------------------------------+
| 3 Tage        | `Jupyter-Notebooks für effiziente Data-Science-Workflows`_   |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Numerische Berechnungen mit NumPy`_                         |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Daten analysieren mit pandas`_                              |
+---------------+--------------------------------------------------------------+
| 3 Tage        | `Daten lesen, schreiben und bereitstellen mit Python`_       |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Daten bereinigen und validieren mit Python`_                |
+---------------+--------------------------------------------------------------+
| 5 Tage        | `Daten visualisieren mit Python`_                            |
+---------------+--------------------------------------------------------------+
| 1 Tag         | `Datenvisualisierungen gestalten`_                           |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Dashboards erstellen`_                                      |
+---------------+--------------------------------------------------------------+
| 3 Tage        | `Code und Daten versioniert und reproduzierbar speichern`_   |
+---------------+--------------------------------------------------------------+
| Abonnement    | `Neues aus Python für Data-Science`_                         |
| à 2 h im      |                                                              |
| Quartal       |                                                              |
+---------------+--------------------------------------------------------------+

.. _`Einführung in Python`:
   https://cusy.io/de/unsere-schulungsangebote/einfuehrung-in-python
.. _`Fortgeschrittenes Python`:
   https://cusy.io/de/unsere-schulungsangebote/fortgeschrittenes-python
.. _`Entwurfsmuster in Python`:
   https://cusy.io/de/unsere-schulungsangebote/entwurfsmuster-in-python
.. _`Effizient Testen mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/effizient-testen-mit-python
.. _`Software-Dokumentation mit Sphinx`:
   https://cusy.io/de/unsere-schulungsangebote/software-dokumentation-mit-sphinx
.. _`Technisches Schreiben`:
   https://cusy.io/de/unsere-schulungsangebote/technisches-schreiben
.. _`Jupyter-Notebooks für effiziente Data-Science-Workflows`:
   https://cusy.io/de/unsere-schulungsangebote/jupyter-notebooks-fuer-effiziente-data-science-workflows
.. _`Numerische Berechnungen mit NumPy`:
   https://cusy.io/de/unsere-schulungsangebote/numerische-berechnungen-mit-numpy
.. _`Daten analysieren mit pandas`:
   https://cusy.io/de/unsere-schulungsangebote/daten-analysieren-mit-pandas
.. _`Daten lesen, schreiben und bereitstellen mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/daten-lesen-schreiben-und-bereitstellen-mit-python
.. _`Daten bereinigen und validieren mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/daten-bereinigen-und-validieren-mit-python
.. _`Daten visualisieren mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/daten-visualisieren-mit-python
.. _`Datenvisualisierungen gestalten`:
   https://cusy.io/de/unsere-schulungsangebote/datenvisualisierungen-gestalten
.. _`Dashboards erstellen`:
   https://cusy.io/de/unsere-schulungsangebote/dashboards-erstellen
.. _`Code und Daten versioniert und reproduzierbar speichern`:
   https://cusy.io/de/unsere-schulungsangebote/code-und-daten-versioniert-und-reproduzierbar-speichern
.. _`Neues aus Python für Data-Science`:
   https://cusy.io/de/unsere-schulungsangebote/neues-aus-python-fuer-data-science

.. toctree::
    :hidden:
    :titlesonly:
    :maxdepth: 0

    intro
    whatsnew
    notebook/index
    jupyterlab/index
    hub/index
    binder
    nbconvert
    nbviewer
    kernels/index
    ipywidgets/index
    nbextensions/index
    viz/index
    dashboards/index
    sphinx/index
    use-cases
    genindex

.. Indices and tables
   ==================

   * :ref:`genindex`
   * :ref:`modindex`
   * :ref:`search`
