JupyterLab-Erweiterungen
========================

JupyterLab ist als erweiterbare Umgebung konzipiert. Dabei können
JupyterLab-Erweiterungen jeden Teil von JupyterLab anpassen. Sie können neue
Themen, Dateibetrachter und -editoren oder Renderer für umfangreiche Ausgaben
in :doc:`../notebook/index` bereitstellen.

.. seealso::
   `JupyterLab Extensions by Examples
   <https://github.com/jupyterlab/extension-examples>`_

Installieren von Erweiterungen
------------------------------

Eine JupyterLab-Erweiterung enthält JavaScript, das in Jupyterlab installiert
und im Browser ausgeführt wird. Die meisten JupyterLab-Erweiterungen können mit :term:`pip` installiert werden. Diese Pakete können auch serverseitige
Komponenten enthalten, die für die Funktion der Erweiterung erforderlich sind.

Seit JupyterLab ≥ 4 verwendet der Standard-Extension-Manager :term:`PyPI` als
Quelle für die verfügbaren Erweiterungen und :term:`pip`, um sie zu
installieren. Eine Erweiterung wird aufgelistet, wenn das Python-Paket den
:term:`Trove-Klassifizierer <trove-classifiers>` ``Framework :: Jupyter ::
JupyterLab :: Extensions :: Prebuilt`` hat.

.. warning::
   Es wird nicht überprüft, ob die Erweiterung mit der aktuellen
   JupyterLab-Version kompatibel ist.

.. danger::
   Die Installation einer Erweiterung ermöglicht die Ausführung von beliebigem
   Code auf dem Server, dem Kernel und dem Browser. Vermeidet daher die
   Installation von Erweiterungen, denen ihr nicht vertraut.

Konfigurieren des Extension Manager
-----------------------------------

Standardmäßig gibt es zwei Erweiterungsmanager, die von JupyterLab
bereitgestellt werden:

``pypi``
    Standardeinstellung, die das Installieren von :term:`pypi.org` erlaubt.
``readonly``
    zeigt die installierten Erweiterungen an mit der Möglichkeit, sie zu
    deaktivieren oder aktivieren.

Ihr könnt den Manager mit der Kommandozeilenoption
``--LabApp.extension_manager`` angeben, :abbr:`z.B. (zum Beispiel)`
:samp:`jupyter lab --LabApp.extension_manager={readonly}`.

Bei der Suche nach Erweiterungen im Erweiterungsmanager zeigt JupyterLab
üblicherweise alle Suchergebnisse an und jede beliebige Quell-Erweiterung kann
installiert werden. Um die Sicherheit zu erhöhen, kann JupyterLab jedoch so
konfiguriert sein, dass die Erweiterungen nur anhand der Block- oder
Allow-Listen aktiviert werden können.

Das Laden der Listen könnt ihr definieren mit ``blocked_extensions_uris`` oder
``allowed_extensions_uris``, die eine Liste von Komma-getrennten URIs enthalten,
:abbr:`z.B. (zum Beispiel)`
:samp:`--LabServerApp.blocked_extensions_uris=http://example.com/blocklist.json`
mit folgender :file:`blocklist.json`-Datei:

.. code-block:: json

    {
      "blocked_extensions": [
        {
          "name": "@jupyterlab-examples/launcher",
          "type": "jupyterlab",
          "reason": "@jupyterlab-examples/launcher is blocklisted for test purpose - Do NOT take this for granted!!!",
          "creation_date": "2020-03-11T03:28:56.782Z",
          "last_update_date":  "2020-03-11T03:28:56.782Z"
        }
      ]
    }

Ein anderes Beispiel zeigt eine :file:`allowlist.json`-Datei, die alle
Erweiterungen der `JupyterLab-Organisation
<https://www.npmjs.com/org/jupyterlab>`_ erlauben:

.. code-block:: json

    {
      "allowed_extensions": [
        {
          "name": "@jupyterlab/*",
          "type": "jupyterlab",
          "reason": "All @jupyterlab org extensions are allowed, of course…",
          "creation_date": "2020-03-11T03:28:56.782Z",
          "last_update_date":  "2020-03-11T03:28:56.782Z"
        }
      ]
    }
