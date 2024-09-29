Einführung
==========

Zielgruppe
----------

Die Nutzung von Jupyter-Notebooks ist vielfältig und reicht von den
Datenwissenschaften über Data-Engineering und Datenanalyse bis hin zu
System-Engineering. Dabei sind die Fähigkeiten und Arbeitsabläufe der einzelnen
Zielgruppen sehr unterschiedlich. Eine der großen Stärken von Jupyter-Notebooks
ist jedoch, dass sie eine enge Zusammenarbeit dieser unterschiedlichen
Fachgruppen in funktionsübergreifenden Teams ermöglichen.

Data-Scientists
    untersuchen Daten mit verschiedenen Parametern und fassen die Ergebnisse
    zusammen.
Data-Engineers
    prüfen die Qualität des Codes und machen ihn robuster, effizienter und
    skalierbar.
Data-Analysts
    nutzen den von Data-Engineers bereitgestellten Code, um die Daten
    systematisch zu analysieren.
System-Engineers
    stellen die Forschungsplattform auf Basis von :doc:`hub/index` bereit, auf
    der die anderen Rollen ihre Arbeit verrichten können.

In diesem Tutorial wenden wir uns an System-Engineers, die eine auf
Jupyter-Notebooks basierende Plattform aufbauen und betreiben wollen. Wir
erklären dann, wie diese Plattform von Data-Scientists, Data-Engineers und
-Analysts effektiv genutzt werden kann.

Warum Jupyter?
--------------

Wie können nun diese vielfältigen Aufgaben vereinfacht werden? Es wird sich
kaum ein Werkzeug finden, das all diese Aufgaben abdeckt und selbst für
einzelne Aufgaben sind häufig mehrere Werkzeuge notwendig. Daher suchen wir
auf einer abstrakteren Ebene allgemeinere Muster für Tools und Sprachen, mit
denen Daten analysiert und visualisiert sowie ein Projekt dokumentiert und
präsentiert werden kann. Genau dies streben wir mit dem
`Project Jupyter <https://jupyter.org/>`_ an.

Das Projekt Jupyter startete 2014 mit dem Ziel, ein konsistentes Set von
Open-Source-Tools für wissenschaftliche Forschung, reproduzierbare Workflows,
`Computational Narratives
<https://blog.jupyter.org/project-jupyter-computational-narratives-as-the-engine-of-collaborative-data-science-2b5fb94c3c58>`_
und Datenanalyse zu erstellen. Bereits 2017 wurde Jupyter dann mit dem `ACM
Software Systems Award
<https://blog.jupyter.org/jupyter-receives-the-acm-software-system-award-d433b0dfe3a2>`_
ausgezeichnet - eine prestigeträchtige Auszeichnung, die es u.a. mit Unix und
dem Web teilt.

Um zu verstehen, warum Jupyter-Notebooks so erfolgreich sind, schauen wir uns
die Kernfunktionen einmal genauer an:

`Jupyter Notebook Format <https://nbformat.readthedocs.io/en/latest/>`_
    Jupyter Notebooks sind ein offenes, auf JSON basierendes Dokumentenformat
    mit vollständigen Aufzeichnungen der Sitzungen des Benutzers und des
    enthaltenen Codes.
Interactive Computing Protocol
    Das Notebook kommuniziert mit einem Rechenkernel über das *Interactive Computing
    Protocol*, einem offenen Netzwerkprotokoll basierend auf JSON-Daten über
    `ZMQ <https://zeromq.org/>`_ und `WebSockets
    <https://de.wikipedia.org/wiki/WebSocket>`_.
:doc:`/kernels/index`
    Rechenkernel sind Prozesse, die interaktiven Code in einer bestimmten
    Programmiersprache ausführen und die Ausgabe an den Benutzer zurückgeben.

.. seealso::
   * `Jupyter celebrates 20 years
     <https://cdss.berkeley.edu/news/project-jupyter-celebrates-20-years-fernando-perez-reflects-how-it-started-open-sciences>`_

Jupyter-Infrastruktur
---------------------

Eine Plattform für die oben genannten Use Cases erfordert eine umfangreiche
Infrastruktur, die nicht nur die Bereitstellung der Kernel sowie die
Parametrisierung, Zeitsteuerung und Parallelisierung von Notebooks erlaubt,
sondern darüberhinaus auch die gleichmäßige Bereitstellung der Ressourcen.

Mit diesem Tutorial wird eine Plattform bereitgestellt, die über Jupyter
Notebooks hinaus schnelle, flexible und umfassende Datenanalysen ermöglicht.
Aktuell gehen wir jedoch noch nicht darauf ein, wie sie sich um *Streaming
Pipelines* und *Domain Driven Data Stores* erweitern lässt.

Die Beispiele des Jupyter-Tutorials könnt ihr jedoch auch lokal erstellen und
ausführen.

Arbeitsbereich
--------------

Die Einrichtung des Arbeitsbereichs umfasst die Installation und Konfiguration
von :doc:`python4datascience:workspace/ipython/index` und
:doc:`Jupyter-Notebooks <notebook/install>`, :doc:`nbextensions/index` und
:doc:`ipywidgets/index`.
