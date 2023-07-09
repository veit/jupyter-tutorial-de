Binder
======

`Binder <https://jupyter.org/binder>`_ bietet eine einfache Möglichkeit, um
Computerumgebungen für alle freizugeben. Binder wird verwendet für

Lehre und Ausbildung
    Mit Binder können Links zu interaktiven Datenanalyseumgebungen geteilt
    werden. Dies eignet sich hervorragend für Workshops, Tutorien und Kurse und
    ermöglicht euch, Studierende viel schneller mit dem Code vertraut zu machen.
Technische Dokumentation
    Binder-Tools können verwendet werden, um die Dokumentation und
    Demonstrationen von Tools interaktiv zu gestalten.
Offene Bildungsressourcen
    Binder kann öffentlich zugängliche interaktive Bildungsmaterialien bieten
    und damit reichhaltigere Erfahrung ermöglichen.
Reproduzierbare wissenschaftliche Analysen
    Binder ermöglicht euch, eine interaktive Umgebung zusammen mit eurem Code
    und euren Analysen zu teilen. Ihr könnt einen Link freigeben, über den
    andere eure Arbeit reproduzieren und mit ihr interagieren können. Das
    `Neurolibre <https://neurolibre.org>`_-Projekt nutzt :abbr:`z.B. (zum
    Beispiel)` Binder, um neurowissenschaftliche Analysen zu reproduzieren.

Binder bietet einen vollständigen Open-Source-Infrastruktur-Stack. Die
wichtigsten Tools sind

``BinderHub``
    stellt den Binder-Dienst in der Cloud bereit

    .. seealso::
       * `Repository <https://github.com/jupyterhub/binderhub>`_
       * `Docs <https://binderhub.readthedocs.io/en/latest/>`_
       * `Examples <https://github.com/binder-examples>`_

``repo2docker``
    erzeugt reproduzierbare Docker-Images aus einem Git-Repository

    .. seealso::
       * `Repository <https://github.com/jupyter/repo2docker>`__
       * `Docs <https://repo2docker.readthedocs.io/en/latest/>`__

`mybinder.org <https://mybinder.org/>`_
    öffentliches BinderHub-Deployment
