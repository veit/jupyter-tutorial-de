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

#. :term:`uv` installieren:

   .. code-block:: console

    $  curl -LsSf https://astral.sh/uv/install.sh | sh

#. Automatische Shell-Vervollständigung aktivieren:

   .. code-block:: console

    $ echo 'eval "$(uv generate-shell-completion bash)"' >> ~/.bashrc

#. Virtuelle Umgebung erstellen und JupyterHub installieren:

   .. code-block:: console

    $ uv init --package jupyterhub_env
    $ cd jupyterhub_env
    $ uv add jupyterhub

#. ``nodejs`` und ``npm`` installieren:

   .. code-block:: console

    $ sudo apt install nodejs npm
    $ node -v
    v23.3.0
    $ npm -v
    10.9.0

#. Installieren des HTTP-Proxy:

   .. code-block:: console

    $ sudo npm install -g configurable-http-proxy

#. Wenn JupyterLab und Notebook in derselben Umgebung laufen sollen, müssen
   diese ebenfalls hier installiert werden:

   .. code-block:: console

    $  uv add jupyterlab notebook

#. Testen der Installation:

   .. code-block:: console

    $  uv run jupyterhub -h
    $  configurable-http-proxy -h

#. Starten des JupyterHub:

   .. code-block:: console

    $  uv run jupyterhub
    ...
    [I 2025-01-10 18:07:29.993 JupyterHub app:3770] JupyterHub is now running at http://:8000

   Mit :kbd:`ctrl-c` könnt ihr den Prozess wieder beenden.
