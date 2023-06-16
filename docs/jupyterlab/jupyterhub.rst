JupyterLab auf JupyterHub
=========================

JupyterLab funktioniert standardmäßig mit :doc:`../hub/index` ≥ 1.0 und kann
sogar neben den klassischen :doc:`Notebooks <../notebook/index>` laufen.

Wenn JupyterLab mit JupyterHub eingesetzt wird, werden im
:menuselection:`File`-Menü zusätzliche Menüpunkte angezeigt zum
:menuselection:`Log Out` oder zum Aufruf des :menuselection:`Hub Control Panel`.

Falls JupyterLab noch nicht der Standard ist, könnt ihr die Konfiguration in
:file:`jupyterhub_config.py` ändern:

.. code-block:: python

   c.Spawner.default_url = "/lab"
