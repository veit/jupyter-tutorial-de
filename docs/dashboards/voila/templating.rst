Templating
==========

.. _voila-gridstack:

Voila-Gridstack
---------------

`gridstack.js <https://gridstackjs.com/>`_ ist ein jQuery-Plugin für
Widget-Layouts. Dies ermöglicht mehrspaltige Drag & Drop-Raster und anpassbare,
für `Bootstrap v3 <https://getbootstrap.com/docs/3.4/>`_ geeignete Layouts.
Zudem funktioniert es  mit `knockout.js <https://knockoutjs.com/>`_ und
Touch-Geräten.

Das Gridstack-Voilà-Template verwendet die Metadaten der Notebook-Zellen, um das
Layout des Notebooks zu gestalten. Es soll die gesamte Spezifikation für die
veralteten :doc:`../jupyter-dashboards/index` unterstützen.

.. image:: voila-gridstack.png
   :scale: 53%
   :alt: Beispiel für Voilà-Gridstack

voila-vuetify
-------------

`voila-vuetify <https://github.com/voila-dashboards/voila-vuetify>`_ ist ein
Template zur Verwendung von Voilà mit dem `Material Design Component Framework
<https://m3.material.io>`_ `Vuetify.js <https://vuetifyjs.com/>`_.

Installation
~~~~~~~~~~~~

.. code-block:: console

    $ uv add bqplot ipyvuetify voila-vuetify

Verwendung
~~~~~~~~~~

Um ``voila-vuetify`` in einem Notebook zu verwenden, müsst ihr zunächst
``ipyvuetify`` importieren:

.. code-block:: python

    import ipyvuetify as v

Anschließend könnt ihr ein Layout erstellen :abbr:`z.B. (zum Beispiel)` mit:

.. code-block:: python

    v.Tabs(
        _metadata={"mount_id": "content-main"},
        children=[
            v.Tab(children=["Tab1"]),
            v.Tab(children=["Tab2"]),
            v.TabItem(
                children=[
                    v.Layout(
                        row=True,
                        wrap=True,
                        align_center=True,
                        children=[
                            v.Flex(
                                xs12=True,
                                lg6=True,
                                xl4=True,
                                children=[fig, slider],
                            ),
                            v.Flex(
                                xs12=True,
                                lg6=True,
                                xl4=True,
                                children=[figHist2, sliderHist2],
                            ),
                            v.Flex(xs12=True, xl4=True, children=[fig2]),
                        ],
                    )
                ]
            ),
            v.TabItem(children=[v.Container(children=["Lorum ipsum"])]),
        ],
    )

:doc:`bqplot_vuetify_example`. könnt ihr nutzen mit:

.. code-block:: console

    $ uv run voila --template vuetify-default bqplot_vuetify_example.ipynb

Anschließend öffnet sich euer Standardbrowser mit der URL
``http://localhost:8866/`` und zeigt euch die Plots im Responsive Material
Design.

Beispiel für Voilà-vuetify mit der Monitorauflösung eines Laptop MDPI-Screen:

.. image:: voila-vuetify-laptop.png
   :scale: 53%

Beispiel für Voilà-vuetify mit der Monitorauflösung eine iPhone X:

.. image:: voila-vuetify-iphone.png
   :scale: 53%

voila-reveal
------------

`voila-reveal <https://github.com/voila-dashboards/voila-reveal>`_ ist ein
Template für Slideshows basierend auf `RevealJS <https://revealjs.com/>`_.

Installation
~~~~~~~~~~~~

.. code-block:: console

    $ uv add voila-reveal

Verwendung
~~~~~~~~~~

Ihr könnt das Template nutzen mit:

.. code-block:: console

    $ uv run voila --template=reveal reveal.ipynb

Durch zusätzliche Optionen können die Standardeinstellungen überschrieben
werden, :abbr:`z.B. (zum Beispiel)` um den Standardwert für den Übergang
``Fade`` mit ``Zoom`` zu überschrieben mit:

.. code-block:: console

    $ uv run voila --template=reveal --VoilaConfiguration.resources="{'reveal': {'transition': 'zoom'}}" reveal.ipynb

Sollen Konfigurationsoptionen dauerhaft gespeichert werden, so kann eine Datei
``conf.json`` in ``share/jupyter/voila/templates/reveal/`` angelegt werden:

.. code-block:: javascript

    {
      "traitlet_configuration": {
        "resources": {
          "reveal": {
            "scroll": false,
            "theme": "simple",
            "transition": "zoom"
          }
        }
      }
    }

Ihr könnt euer Notebook dann in eine Slideshow verwandeln in
:menuselection:`View --> Cell Toolbar --> Slideshow`. In der Werkzeugleiste
einer könnt ihr auswählen zwischen

Slide
    von links nach rechts
Sub-Slide
    von oben nach unten
Fragment
    Stop innerhalb einer Folie
Notes
    Anmerkungen für Sprecher*innen, die beim Drücken der ``t``-Taste in einem
    neuen Fenster geöffnet werden

Wenn Ihr eure Vortragsfolien auf `binder <https://mybinder.org/>`_
veröffentlichen wollt, müsst Ihr den folgenden Tag in die Metadaten eures
Notebooks schreiben in :menuselection:`Edit --> Edit Notebook Metadata`:

.. code-block:: javascript

    "rise": {
        "autolaunch": true
    }

Ihr könnt ebenfalls das `chalkboard reveal-Plugin
<https://github.com/rajgoel/reveal.js-plugins/tree/master/chalkboard>`_
verwenden wenn Ihr die Metadaten des Notebooks erweitert um:

.. code-block:: javascript

    "rise": {
      "enable_chalkboard": true
    }

Eigene Templates erstellen
--------------------------

Ein Voilà-Template ist ein Ordner, der sich im Virtual-environment unter
``share/jupyter/voila/templates`` befindet und :abbr:`z.B. (zum Beispiel)`
Folgendes enthält:

.. code-block:: console

    /Users/veit/.local/share/virtualenvs/jupyter-tutorial--q5BvmfG/share/jupyter/voila/templates/mytheme
    ├── conf.json
    ├── nbconvert_templates
    │   └── voila.tpl
    ├── static
    │   ├── mytheme.js
    │   └── mytheme.css
    └── templates
        ├── 404.html
        ├── browser-open.html
        ├── error.html
        ├── page.html
        └── tree.html

``conf.json``
    Konfigurationsdatei, die :abbr:`z.B. (zum Beispiel)` auf das Basis-Template
    verweist:

    .. code-block:: json

        {"base_template": "default"}

``nbconvert_templates``
    Benutzerdefinierte Templates für :doc:`/nbconvert`.
``static``
    Verzeichnis für statische Dateien.
``templates``
    Benutzerdefinierte Tornado-Templates.
