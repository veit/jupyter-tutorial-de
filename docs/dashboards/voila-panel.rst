Voilà vs. Panel
===============

Ein großer Unterschied zwischen Panel und Voilà besteht in der Verarbeitung der
Notebooks: Voilà baut direkt auf dem Notebookformat auf und übernimmt die
gesamte Ausgabe in das Voilà-Dashboard, während in Panel die Ausgabe einer
Notebook-Zelle explizit als Panel-Objekt deklariert werden muss. Voilà hat daher
den Vorteil, dass bestehende Notizbücher unverändert verwendet werden können.
Wenn jedoch nicht alle Notebook-Zellen in das Dashboard übernommen werden
sollen, müssen zwei ähnliche Notebooks gepflegt werden – eines für `Literate
Programming <https://de.wikipedia.org/wiki/Literate_Programming>`_ und eines
für das Dashboard. Soll in einem Dashboard jedoch `die umgekehrte Pyramide
<https://slides.cusy.io/data-visualisation/#/2/6/0>`_ für das Storytelling
verwendet werden, sind in beiden Fällen zwei Notebooks erforderlich.

Skalierbarkeit
--------------

Voilà und Panel  basieren auf `Tornado
<https://www.tornadoweb.org/en/stable/>`_, aber sie unterscheiden sich dadurch,
dass Voilà für jede Person einen neuen Jupyter-Kernel startet, während der
Bokeh-Server mehrere Personen mit demselben Python-Prozess bedient. Dieser
Unterschied hat im Wesentlichen zwei Auswirkungen:

* Der Overhead pro Person für ein Dashboard ist beim Bokeh-Server viel geringer
  als bei Voilà: Nachdem die Bibliotheken importiert sind, fällt nur noch ein
  winziger Overhead für die Erstellung jeder neuen Session an. Für eine Session,
  die pandas und Matplotlib importiert, beträgt der Overhead pro Benutzer ca.
  75 MB, und die Anzahl der Personen, die ein Voilà-Server für eine bestimmte
  Anwendung verarbeiten kann, verringert. Auch sind die Start- und
  Datenzugriffszeiten meist langsamer.
* Da sich ein Bokeh-Server einen einzigen Prozess für mehrere Sessions teilt,
  können Daten oder Verarbeitungen gegebenenfalls auch zwischen den
  verschiedenen Sessions geteilt werden.

Multi-Page-App
--------------

Voilà ist nicht für mehrseitige Anwendungen konzipiert während Panel mehrere
Optionen bietet für die Erstellung von mehrseitigen Apps bietet, einschließlich
:doc:`panel/pipelines` und Übersichtsseite mit einer Sammlung unabhängiger
Dashboards und Apps.

Autorisierung und Authentifizierung
-----------------------------------

Üblicherweise sollte das Dashboard nicht für die Authentifizierung und
Autorisierung verwendet werden, sondern an einen Dienst delegiert werden.
`ContainDS Dashboards <https://cdsdashboards.readthedocs.io/en/stable/>`_ ist
ein Beispiel für eine :doc:`/hub/index`-Erweiterung, die dies
Dashboard-unabhängig tut. Authentifizierung und Autorisierung kann mit einem der
folgenden Tools auch über einen Webserver erfolgen:

* `Traefik Forward Auth <https://github.com/thomseddon/traefik-forward-auth>`_
* `OAuth2 Proxy <https://oauth2-proxy.github.io/oauth2-proxy>`_
* `PyCasbin <https://github.com/casbin/pycasbin>`_

Wenn ihr dennoch die Dashboarding-Bibliothek die Authentifizierung machen lassen
wollt, gibt es einige Optionen unterschiedlicher Reife:

* Panel basiert auf Bokeh, das Authentifizierung bietet, und Panel wird mit
  einer Reihe von OAuth-Anbietern ausgeliefert, :abbr:`z.B. (zum Beispiel)`
  GitHub, GitLab und Azure.

  .. seealso::
     `Configuring Authentication
     <https://panel.holoviz.org/how_to/authentication/index.html>`_

* Voilà kann die Authentifizierung von :doc:`/hub/index` wiederverwenden.

BI-Tool
-------

Sowohl Voilà wie auch Panel setzen Programmierkenntnisse voraus, um Dashboards
erstellen zu können. Das auf Panel aufbauende `Lumen
<https://github.com/holoviz/lumen>`_ bietet jedoch eine vielversprechend
BI-ähnliche Benutzeroberfläche.
