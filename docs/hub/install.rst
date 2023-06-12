Installation
============

#. Python≥3.6 und :term:`pip` installieren:

   .. code-block:: console

    $ sudo apt update
    $ sudo apt install python3
    $ python3 -V
    Python 3.10.6
    $ sudo apt install python3-pip

#. Service-User ``jupyter`` erstellen:

   .. code-block:: console

    $ sudo useradd -s /bin/bash -rmd /srv/jupyter jupyter

#. Zum Service-User ``jupyter`` wechseln:

   .. code-block:: console

    $ sudo -u jupyter -i

#. :term:`Pipenv` installieren:

   .. code-block:: console

    $  python3 -m pip install --user pipenv

   Dies installiert Pipenv in ``USER_BASE``.

#. ``USER_BASE`` ermitteln und in ``PATH`` eintragen:

   .. code-block:: console

    $  python3 -m site --user-base
    /srv/jupyter/.local

   Anschließend muss noch das ``bin``-Verzeichnis angehängt und zu ``PATH``
   in ``~/.profile`` hinzugefügt werden, also:

   .. code-block:: console

    export PATH=/srv/jupyter/.local/bin:$PATH

   Schließlich wird das geänderte Profil eingelesen mit:

   .. code-block:: console

    $  source ~/.profile

#. Virtuelle Umgebung erstellen und JupyterHub installieren:

   .. code-block:: console

    $ mkdir jupyterhub_env
    $ cd jupyterhub_env
    $ pipenv install jupyterhub

#. ``nodejs`` und ``npm`` installieren:

   .. code-block:: console

    $ sudo apt install nodejs npm
    $ node -v
    v12.22.9
    $ npm -v
    8.5.1

#. Installieren des HTTP-Proxy:

   .. code-block:: console

    $ sudo npm install -g configurable-http-proxy

#. Wenn JupyterLab und Notebook in derselben Umgebung laufen sollen, müssen
   diese ebenfalls hier installiert werden:

   .. code-block:: console

    $  pipenv install jupyterlab notebook

#. Testen der Installation:

   .. code-block:: console

    $  pipenv run jupyterhub -h
    $  configurable-http-proxy -h

#. Starten des JupyterHub:

   .. code-block:: console

    $  pipenv run jupyterhub
    ...
    [I 2019-07-31 22:47:26.617 JupyterHub app:1912] JupyterHub is now running at http://:8000

   Mit :kbd:`ctrl-c` könnt ihr den Prozess wieder beenden.
