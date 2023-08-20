Kernel installieren, anzeigen und starten
=========================================

Kernel installieren
-------------------

Kernel werden :abbr:`z.B. (zum Beispiel)` in folgenden Verzeichnissen gesucht:

* :samp:`/srv/jupyter/.local/share/jupyter/kernels`
* :samp:`/usr/local/share/jupyter/kernels`
* :samp:`/usr/share/jupyter/kernels`
* :samp:`/srv/jupyter/.ipython/kernels`

Um eure neue Umgebung in einem der Verzeichnisse als Jupyter Kernel verfügbar
zu machen, solltet ihr ipykernel installieren:

.. code-block:: console

   $ pipenv install ipykernel

Anschließend könnt ihr euren Kernel registrieren, :abbr:`z.B. (zum Beispiel)`
mit

.. code-block:: console

   $ pipenv run python -m ipykernel install --prefix=/srv/jupyter/.ipython/kernels --name python311 --display-name 'Python 3.11 Kernel'

:samp:`--prefix={/PATH/TO/KERNEL}`
    gibt den Pfad an, in dem der Jupyter-Kernel installiert werden soll.
:samp:`--user`
    installiert den Kernel für den aktuellen Nutzer und nicht systemweit.
:samp:`name {NAME}`
    gibt einen Namen für die ``kernelspec`` an. Dieser wird benötigt, um
    mehrere IPython-Kernel gleichzeitig verwenden zu können.

Mit ``ipykernel install`` wird eine ``kernelspec``-Datei im JSON-Format für die
aktuelle Python-Umgebung erstellt, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: json

    {
     "display_name": "My Kernel",
     "language": "python"
     "argv": [
      "/srv/jupyter/.ipython/kernels/python311_kernel-7y9G693U/bin/python",
      "-m",
      "ipykernel_launcher",
      "-f",
      "{connection_file}"
     ],
    }

:samp:`display_name`
    Der Name des Kernels, wie er im Browser angezeigt werden soll. Im Gegensatz
    zum in der API verwendeten Kernelnamen kann dieser beliebige Unicode-Zeichen
    enthalten.
:samp:`language`
    Der Name der Sprache des Kernels. Wenn beim Laden von Notebooks kein
    passender ``kernelspec``-Schlüssel gefunden wird, wird ein Kernel mit einer
    passenden Sprache verwendet. Auf diese Weise kann ein für ein Python- oder
    Julia-Kernel geschriebenes Notebook mit dem Python- oder Julia-Kernel des
    Benutzers verknüpft werden, auch wenn dieser nicht demselben Namen wie der
    des Autors hat.
:samp:`argv`
    Eine Liste von Befehlszeilenargumenten, die zum Starten des Kernels
    verwendet werden.

    :samp:`{connection_file}` verweist auf eine Datei, die die IP-Adresse, die
    Ports und den Authentifizierungsschlüssel enthält, die für die Verbindung
    benötigt werden. Üblicherweise wird diese JSON-Datei an einem sicheren Ort
    des aktuellen Profils gespeichert:

    .. code-block:: json

        {
          "shell_port": 61656,
          "iopub_port": 61657,
          "stdin_port": 61658,
          "control_port": 61659,
          "hb_port": 61660,
          "ip": "127.0.0.1",
          "key": "a0436f6c-1916-498b-8eb9-e81ab9368e84"
          "transport": "tcp",
          "signature_scheme": "hmac-sha256",
          "kernel_name": ""
        }

:samp:`interrupt_mode`
    Kann entweder ``signal`` oder ``message`` sein und gibt an, wie ein Client
    die Ausführung einer Zelle auf diesem Kernel unterbrechen soll.

    ``signal``
        sendet ein Interrupt, :abbr:`z.B. (zum Beispiel)` :samp:`SIGINT` auf
        *POSIX*-Systemen
    ``message``
        sendet einen ``interrupt_request``, :abbr:`s.a. (siehe auch)` `Kernel
        Interrupt
        <https://jupyter-client.readthedocs.io/en/latest/messaging.html#kernel-interrupt>`_.

:samp:`env`
    ``dict`` mit Umgebungsvariablen, die für den Kernel festgelegt werden
    sollen. Diese werden zu den aktuellen Umgebungsvariablen hinzugefügt, bevor
    der Kernel gestartet wird.
:samp:`metadata`
    ``dict`` mit zusätzlichen Attributen zu diesem Kernel. Wird von Clients zur
    Unterstützung der Kernelauswahl verwendet. Hier hinzugefügte Metadaten
    sollten einen Namensraum für das Tool zum Lesen und Schreiben dieser
    Metadaten haben.

Verfügbare Kernel anzeigen
--------------------------

.. code-block:: console

    $ pipenv run jupyter kernelspec list
    Available kernels:
      mykernel    /Users/veit/Library/Jupyter/kernels/mykernel
      python2    /Users/veit/Library/Jupyter/kernels/python2
      python3    /Users/veit/.local/share/virtualenvs/jupyter-tutorial--q5BvmfG/bin/../share/jupyter/kernels/python3

Kernel starten
--------------

.. code-block:: console

    $ pipenv run jupyter console --kernel mykernel
    Jupyter console 6.0.0
    Python 2.7.15 (default, Oct 22 2018, 19:33:46)
    ...

    In [1]:

Mit :kpd:`ctrl-d` könnt ihr den Kernel wieder beenden.

Kernel löschen
--------------

.. code-block:: console

   $ pipenv run jupyter kernelspec uninstall mykernel

Starndard-Kernel deinstallieren
-------------------------------

Sofern noch nicht geschehen, kann eine Konfigurationsdatei erstellt werden,
:abbr:`z.B. (zum Beispiel)` mit

.. code-block:: console

   $ pipenv run jupyter lab --generate-config

Anschließend könnt ihr in dieser Konfigurationsdatei folgende Zeile einfügen:

.. code-block:: python

   c.KernelSpecManager.ensure_native_kernel = False
