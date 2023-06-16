================
Jupyter-Tutorial
================

`Jupyter-Notebooks <https://jupyter-notebook.readthedocs.io/>`_ erfreuen sich
in den Datenwissenschaften wachsender Beliebtheit und wurden zum
De-facto-Standard für schnelles Prototyping und explorative Analysen. Sie
beflügeln nicht nur Experimente und Innovationen enorm, sie machen auch den
gesamten Forschungsprozess schneller und zuverlässiger. Zudem entstehen viele
zusätzliche Komponenten, die die ursprünglichen Grenzen ihrer Nutzung erweitern
und neue Verwendungsmöglichkeiten eröffnen.

.. graphviz::

    digraph decide_jupyter {
        graph [fontname = "Calibri", fontsize="16"];
        node [fontname = "Calibri", fontsize="16"];
        edge [fontname = "Calibri", fontsize="16"];
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
        hub [
            label="JupyterHub",
            tooltip="JupyterHub\ninstallieren",
            target="_top",
            href="../hub/index.html"]
        nbconvert [
            label="nbconvert",
            tooltip="nbconvert installieren und nutzen",
            target="_top",
            href="../nbconvert.html"]
        nbviewer [
            label="nbviewer",
            tooltip="nbviewer installieren und nutzen",
            target="_top",
            href="../nbviewer.html"]
        kernels [
            label="Kernels",
            tooltip="Kernels installieren, anzeigen und starten",
            target="_top",
            href="../kernels/install.html"]
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
            href="../use-cases.html"]
        // 3rd Level
        notebook [
            label="Jupyter-\nNotebook",
            tooltip="Notebook lokal installieren",
            target="_top",
            href="../notebook/index.html"]
        jupyterlab [
            label="JupyterLab",
            tooltip="JupyterLab lokal installieren",
            target="_top",
            href="../jupyterlab/index.html"]
        widgets [
            label="Widgets",
            tooltip="ipywidgets installieren und nutzen",
            target="_top",
            href="../ipywidgets/index.html"]
        extend [
            label="nbextensions",
            tooltip="Installieren und Verwenden verschiedener Notebook-Erweiterungen",
            target="_top",
            href="../nbextensions/index.html"]
        viz [
            label="Daten\nvisualisieren",
            tooltip="Bibliotheken zur Datenvisualisierung",
            target="_top",
            href="../viz/index.html"]
        dash [
            label="Dashboards",
            tooltip="Installieren und Verwenden von Dashboards",
            target="_top",
            href="../dashboards/index.html"]
        html [
            label="HTML",
            tooltip="Einbinden von Notebooks in statisches HTML",
            target="_top",
            href="../ipywidgets/embedding.html"]
        sphinx [
            label="Sphinx",
            tooltip="Einbinden von Notebooks in den Sphinx Document Generator",
            target="_top",
            href="../nbsphinx.html"]
        // Edges
        what -> singleuser [label="Einzel-\nnutzer"]
        what -> hub [label="Team-\narbeit"]
        what -> nbconvert [label="Konvertieren"]
        nbconvert -> nbviewer [label="Konvertier-\nservice"]
        what -> kernels [label="Java, R,\nJulia etc."]
        what -> extensions [label="Notebook\nerweitern"]
        what -> embed [label="Notebooks\neinbetten"]
        what -> examples [label="Beispiele"]
        singleuser -> {notebook jupyterlab}
        extensions -> {widgets extend viz dash}
        embed -> {html sphinx}
        // Arrangement
        {rank = same; what;}
        {rank = same; singleuser; hub; nbconvert; kernels; extensions; embed; examples;}
        {rank = same; notebook; jupyterlab; widgets; extend; viz; dash;}
        {rank = same; html; sphinx}
    }

.. toctree::
    :hidden:
    :titlesonly:
    :maxdepth: 0

    intro
    notebook/index
    jupyterlab/index
    hub/index
    nbconvert
    nbviewer
    kernels/index
    ipywidgets/index
    nbextensions/index
    viz/index
    dashboards/index
    nbsphinx
    use-cases
    genindex

.. Indices and tables
   ==================

   * :ref:`genindex`
   * :ref:`modindex`
   * :ref:`search`
