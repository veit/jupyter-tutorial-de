systemdspawner
==============

Der systemdspawner ermöglicht es JupyterHub, Einzelbenutzer-Notebookserver mit
`systemd <https://de.wikipedia.org/wiki/Systemd>`_ zu erzeugen. Ihr erhaltet
Isolation und Sicherheit ohne Docker, rkt :abbr:`o.ä. (oder ähnliches)`
verwenden zu müssen. Zudem bietet systemdspawner weitere Features:

* der maximal zulässige Speicher und die maximal verfügbare CPU pro Person kann
  über cgroups begrenzt und mit ``systemd-cgtop`` überprüft werden.
* alle erhalten ein eigenes :file:`/tmp`-Verzeichnis um die Isolation zu erhöhen
* Notebook-Server können als bestimmte lokale User auf dem System gestartet
  werden
* die Nutzung von sudo in Notebooks kann eingeschränkt werden
* die Pfade, in die gelesen und geschrieben werden können, lassen sich
  einschränken
* Protokolle für jedes einzelne Notebook können verwaltet werden

Anforderungen
-------------

systemdspawner setzt systemd ≥ v211 voraus; die sicherheitsrelevanten Funktionen
erfordern systemd ≥ v228. Ihr könnt überprüfen, welche Version von systemd bei
euch verfügbar ist mit

.. code-block:: console

   $ systemctl --version | head -1
   systemd 249 (249.11-0ubuntu3.7)

Zum Limitieren der Speicher- und CPU-Zuweisungen müssen zudem bestimmte
Kernel-Optionen zur Verfügung stehen. Dies können mit dem `check-kernel.bash
<https://github.com/jupyterhub/systemdspawner/blob/master/check-kernel.bash>`_
überprüft werden.

Wenn die Standardeinstellung :samp:`c.SystemdSpawner.dynamic_users = False`
verwendet wird, wird der Server mit dem lokalen Unix-User-Account gestartet.
Daher erfordert dieser Spawner, dass alle User, bereits ein lokales Konto auf
der Maschine haben. Mit :samp:`c.SystemdSpawner.dynamic_users = True` sind hingegen keine lokalen User-Accounts erforderlich; sie werden durch systemd bei
Bedarf dynamisch erstellt.

Installation und Konfiguration
------------------------------

Ihr könnt systemdspawner installieren mit

.. code-block:: console

   $ pipenv install jupyterhub-systemdspawner

Anschließend kann er in der :file:`jupyterhub_config.py` aktiviert werden mit

.. code-block:: python

   c.JupyterHub.spawner_class = 'systemdspawner.SystemdSpawner'

Es stehen euch viele weitere Konfigurationsmöglichkeiten offen, :abbr:`z.B. (zum
Beispiel)`

:samp:`c.SystemdSpawner.mem_limit = '4G'`
    gibt den maximalen Speicherplatz an, der von jedem einzelnen User verwendet
    werden kann. Die Einstellung ``None`` deaktiviert die Speicherbegrenzung.

    Auch wenn einzelne User so viel Speicher wie möglich verwenden können
    sollen, ist es dennoch sinnvoll, eine Speicherbegrenzung von 80–90 % des
    gesamten physischen Speichers festzulegen. Dadurch wird verhindert, dass ein
    User den Rechner versehentlich im Alleingang lahmlegen kann.

:samp:`c.SystemdSpawner.cpu_limit = 4.0`
    Eine Fließkommazahl, die die Anzahl der CPU-Kerne angibt, die jeder User
    verwenden kann.
:samp:`c.SystemdSpawner.user_workingdir = '/home/{USERNAME}'`
    Das Verzeichnis, in dem der Notebook-Server eines jeden User gestartet wird.
    Dieses Verzeichnis sehen auch die User, wenn sie ihre Notebook-Server
    öffnen. Normalerweise ist dies das Heimatverzeichnis des Benutzers.
:samp:`c.SystemdSpawner.username_template = 'jupyter-{USERNAME}'`
    Vorlage für den Unix-User-Namen, unter dem jeder User angelegt werden soll.
:samp:`c.SystemdSpawner.default_shell = '/bin/bash'`
    Die Standard-Shell, die für das Terminal im Notebook verwendet wird. Setzt
    die Umgebungsvariable ``SHELL`` auf diesen Wert.
:samp:`c.SystemdSpawner.extra_paths = ['/home/{USERNAME}/conda/bin']`
    Liste der Pfade, die der Umgebungsvariablen ``PATH`` für den gespawnten
    Notebook-Server vorangestellt werden sollen. Dies ist einfacher als das
    Setzen der ``env``-Eigenschaft, da ihr ``PATH`` hinzufügen und nicht
    komplett ersetzen wollt. Dies ist sehr nützlich, wenn ihr eine virtualenv-
    oder conda-Installation standardmäßig in ``PATH`` des Users aufnehmen wollt.
:samp:`c.SystemdSpawner.unit_name_template = 'jupyter-{USERNAME}-singleuser'`
    Namensvorlage der Systemd-Service-Einheit für jeden User-Notebook-Server.
    Dies ermöglicht die Unterscheidung zwischen mehreren JupyterHubs mit
    systemd-Spawner auf derselben Maschine. Sollte nur ``[a-zA-Z0-9_-]``
    enthalten.
:samp:`c.SystemdSpawner.unit_extra_properties = {'LimitNOFILE': '16384'}`
    Dict von Schlüssel-Wert-Paaren, die verwendet werden, um beliebige
    Eigenschaften zu den gespawnten Jupyerhub-Units hinzuzufügen – :abbr:`s.a.
    (siehe auch)` ``man systemd-run`` für  Details zu den Eigenschaften.
:samp:`c.SystemdSpawner.isolate_tmp = True`
    Wenn dieser Wert auf ``True`` gesetzt wird, wird für jeden User ein
    separates, privates :file:`/tmp`-Verzeichnis angelegt. Dies ist sehr
    nützlich, um sich gegen das versehentliche Durchsickern von ansonsten
    privaten Informationen zu schützen.

    Dies erfordert systemd Version > 227. Wenn ihr dies in früheren Versionen
    aktiviert, wird das Spawnen fehlschlagen.

:samp:`c.SystemdSpawner.isolate_devices = True`
    Wenn ihr diese Option auf ``True`` setzt, wird für jeden User ein
    separates, privates :file:`/dev`-Verzeichnis eingerichtet. Dadurch wird
    verhindert, dass User direkt auf Hardware-Devices zugreifen, was eine
    potenzielle Quelle für Sicherheitsprobleme sein könnte. :file:`/dev/null`,
    :file:`/dev/zero`, :file:`/dev/random` und die ``ttyp``-Pseudo-Geräte sind
    bereits gemountet, so dass die meisten User keine Veränderung bemerken
    sollten, wenn dies aktiviert ist.
:samp:`c.SystemdSpawner.disable_user_sudo = True`
    Wenn Ihr diese Option auf ``True`` setzt, wird verhindert, dass User
    ``sudo`` oder andere Mittel verwenden können, um andere User zu werden.
    Dies hilft dabei, den Schaden einzudämmen, der durch die Kompromittierung
    der Anmeldeinformationen eines Benutzers entsteht, wenn dieser auch
    ``sudo``-Rechte auf dem Rechner hat – ein webbasierter Exploit kann nun nur
    noch die eigenen Daten des Users beschädigen.

    Dies erfordert systemd Version > 228. Wenn ihr dies in früheren Versionen
    aktiviert, wird das Spawnen fehlschlagen.

:samp:`c.SystemdSpawner.readonly_paths = ['/']`
    Liste der Dateisystempfade, die für den Notebook-Server des Users
    schreibgeschützt eingehängt werden sollen. Damit werden eventuell
    vorhandene Dateisystemberechtigungen außer Kraft gesetzt. Unterpfade von
    Pfaden, die ``readonly`` gemountet sind, können mit ``readwrite_paths`` als
    ``readwrite`` markiert werden. Dies ist nützlich, um ``/`` als
    schreibgeschützt zu markieren und nur die Pfade aufzulisten, in die
    Notebook-User schreiben dürfen. Wenn die hier aufgeführten Pfade nicht
    existieren, erhaltet ihr eine Fehlermeldung.

    Dies erfordert systemd Version > 228. Wenn ihr diese Funktion in früheren
    Versionen aktiviert, wird das Spawnen fehlschlagen. Bis zur systemd-Version
    231 kann es auch nur Verzeichnisse und keine Dateien enthalten.

:samp:`c.SystemdSpawner.readwrite_paths = ['/home/{USERNAME}']`
    Liste der Dateisystempfade, die für den Notebook-Server des Users
    schreibgeschützt eingehängt werden sollen. Dies macht nur Sinn, wenn
    ``readonly_paths`` verwendet wird, um einige Pfade schreibgeschützt zu
    machen. Dies setzt die Dateisystemberechtigungen nicht außer Kraft – der
    User muss über die entsprechenden Rechte verfügen, um auf diese Pfade zu
    schreiben.

    Dies erfordert systemd Version > 228. Wenn ihr diese Funktion in früheren
    Versionen aktiviert, wird das Spawnen fehlschlagen. Bis systemd Version 231
    kann es auch nur Verzeichnisse und keine Dateien enthalten.

.. seealso::
   * `systemdspawner <https://github.com/jupyterhub/systemdspawner>`_
