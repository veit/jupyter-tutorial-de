Konfiguration
=============

JupyterHub-Konfiguration
------------------------

Konfigurationsdatei erstellen:

.. code-block:: console

    $  uv run jupyterhub --generate-config
    Writing default config to: jupyterhub_config.py

.. seealso::

   * :doc:`JupyterHub Configuration Basics
     <jupyterhub:tutorial/getting-started/config-basics>`
   * :doc:`JupyterHub Networking basics
     <jupyterhub:tutorial/getting-started/networking-basics>`

System-Service für JupyterHub
-----------------------------

#. Ermitteln des *Python Virtual Environment*:

   .. code-block:: console

    $ cd jupyterhub_env
    $ pwd
    /srv/jupyter/jupyterhub_env

#. Konfigurieren des absoluten Pfades zu :file:`jupyterhub-singleuser` in der
   :file:`jupyterhub_config.py`-Datei:

   .. code-block:: python

    c.Spawner.cmd = ["/srv/jupyter/jupyterhub_env/.venv/bin/jupyterhub-singleuser"]

#. Hinzufügen einer neuen systemd-Unit-Datei
   :file:`/etc/systemd/system/jupyterhub.service` mit dem Befehl :samp:`$ sudo
   systemctl edit --force --full jupyterhub.service`

   Fügt eure entsprechende Python-Umgebung ein.

   .. code-block:: ini

    [Unit]
    Description=Jupyterhub

    [Service]
    User=root
    Environment="PATH=/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/srv/jupyter/.local/share/virtualenvs/jupyterhub-aFv4x91W/bin"
    ExecStart=/srv/jupyter//srv/jupyter/jupyterhub_env/.venv/bin/jupyterhub -f /srv/jupyter/jupyterhub_env/jupyterhub_config.py

    [Install]
    WantedBy=multi-user.target

#. Laden der Konfiguration mit ``sudo systemctl daemon-reload``.

#. Der JupyterHub lässt sich verwalten mit :samp:`sudo systemctl
   {START}|{STOP}|{STATUS} jupyterhub`

#. Um sicherzustellen, dass der Dienst auch bei einem Systemstart mitgeladen
   wird, wird folgendes aufgerufen:

   .. code-block:: console

    $ sudo systemctl enable jupyterhub.service
    Created symlink /etc/systemd/system/multi-user.target.wants/jupyterhub.service → /etc/systemd/system/jupyterhub.service.

#. Um den ``jupyterhub-singleuser``-Spawner nutzen und einen eigenen Server
   starten zu können, müssen die ix-User in der Gruppe ``jupyter`` eingetragen
   werden, :abbr:`z.B. (zum Beispiel)` mit :samp:`usermod -aG jupyter {VEIT}`.

TLS-Verschlüsselung
-------------------

Da JupyterHub eine Authentifizierung beinhaltet und die Ausführung von
beliebigem Code erlaubt, sollte es nicht ohne SSL (HTTPS) ausgeführt werden.
Dazu muss ein offizielles, vertrauenswürdiges SSL-Zertifikat erstellt werden.
Nachdem ihr einen Schlüssel und ein Zertifikat erhalten und installiert habt,
konfiguriert ihr jedoch nicht das JupyterHub selbst sondern den vorgeschalteten
Apache Webserver.

#. Hierfür werden zunächst die Zusatzmodule aktiviert mit

   .. code-block:: apacheconf

      # a2enmod ssl rewrite proxy proxy_http proxy_wstunnel

#. Anschließend kann der VirtualHost in
   ``/etc/apache2/sites-available/jupyter.cusy.io.conf`` konfiguriert
   werden mit

   .. code-block:: console

      # redirect HTTP to HTTPS
      <VirtualHost 172.31.50.170:80>
          ServerName jupyter.cusy.io
          ServerAdmin webmaster@cusy.io

          ErrorLog ${APACHE_LOG_DIR}/jupyter.cusy.io_error.log
          CustomLog ${APACHE_LOG_DIR}/jupyter.cusy.io_access.log combined

          Redirect / https://jupyter.cusy.io/
      </VirtualHost>

      <VirtualHost 172.31.50.170:443>
        ServerName jupyter.cusy.io
        ServerAdmin webmaster@cusy.io

        # configure SSL
        SSLEngine On
        SSLCertificateFile /etc/ssl/certs/jupyter.cusy.io_cert.pem
        SSLCertificateKeyFile /etc/ssl/private/jupyter.cusy.io_sec_key.pem
        # for an up-to-date SSL configuration see e.g.
        # https://ssl-config.mozilla.org/

        # Use RewriteEngine to handle websocket connection upgrades
        RewriteEngine On
        RewriteCond %{HTTP:Connection} Upgrade [NC]
        RewriteCond %{HTTP:Upgrade} websocket [NC]
        RewriteRule /(.*) ws://127.0.0.1:8000/$1 [P,L]

        <Location "/">
          # preserve Host header to avoid cross-origin problems
          ProxyPreserveHost on
          # proxy to JupyterHub
          ProxyPass         http://127.0.0.1:8000/
          ProxyPassReverse  http://127.0.0.1:8000/
        </Location>

        ErrorLog ${APACHE_LOG_DIR}/jupyter.cusy.io_error.log
        CustomLog ${APACHE_LOG_DIR}/jupyter.cusy.io_access.log combined
      </VirtualHost>

#. Dieser VirtualHost wird aktiviert mit :samp:`# a2ensite
   {JUPYTER.CUSY.IO}.conf`.

#. Schließlich könnt ihr den Status des Apache-Webserver überprüfen mit

   .. code-block:: console

      # systemctl status apache2
      ● apache2.service - The Apache HTTP Server
         Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
         Active: active (running) (Result: exit-code) since Mon 2019-03-25 16:50:26 CET; 1 day 22h ago
        Process: 31773 ExecReload=/usr/sbin/apachectl graceful (code=exited, status=0/SUCCESS)
       Main PID: 20273 (apache2)
          Tasks: 55 (limit: 4915)
         CGroup: /system.slice/apache2.service
                 ├─20273 /usr/sbin/apache2 -k start
                 ├─31779 /usr/sbin/apache2 -k start
                 └─31780 /usr/sbin/apache2 -k start

      Mar 27 06:25:01 jupyter.cusy.io systemd[1]: Reloaded The Apache HTTP Server.

Cookie-Secret
-------------

Das Cookie secret ist zum Verschlüsseln der Browser-Cookies, die zur
Authentifizierung verwendet werden.

#. Das Cookie-Secret kann z.B. erstellt werden mit

   .. code-block:: console

    $ openssl rand -hex 32 > /srv/jupyter/venv/jupyterhub_cookie_secret

#. Die Datei sollte weder für ``group`` noch für ``anonymous`` lesbar sein:

   .. code-block:: console

    $ chmod 600 /srv/jupyter/venv/jupyterhub_cookie_secret

#. Schließlich wird es in die ``jupyterhub_config.py``-Datei eingetragen:

   .. code-block:: python

    c.JupyterHub.cookie_secret_file = "jupyterhub_cookie_secret"

Proxy authentication token
--------------------------

Der Hub authentifiziert seine Anforderungen an den Proxy unter Verwendung
eines geheimen Tokens, auf das sich der Hub und der Proxy einigen.
Üblicherweise muss der Proxy authentication token nicht festgelegt werden,
da der Hub selbst einen zufälligen Schlüssel generiert. Dies bedeutet, dass
der Proxy jedes Mal neu gestartet werden muss sofern der Proxy nicht ein
Unterprozess des Hubs ist.

#. Alternativ kann Der Wert z.B. generiert werden mit

   .. code-block:: console

    $ openssl rand -hex 32

#. Anschließend kann er in der Konfigurationsdatei eingetragen werde, z.B. mit

   .. code-block:: python

    c.JupyterHub.proxy_auth_token = (
        "18a0335b7c2e7edeaf7466894a32bea8d1c3cff4b07860298dbe353ecb179fc6"
    )
