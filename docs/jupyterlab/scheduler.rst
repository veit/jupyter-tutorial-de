Scheduler
=========

`Jupyter Scheduler
<https://jupyter-scheduler.readthedocs.io/en/latest/index.html>`_ ist eine
Sammlung von Erweiterungen zur Programmierung von Jobs, die sofort oder nach
einem Zeitplan ausgeführt werden sollen. Er hat eine Lab- (Client-) und eine
Server-Erweiterung. Beide werden benötigt, um Notebooks planen und ausführen zu
können. Wenn ihr Jupyter Scheduler über den JupyterLab-Extension-Manager
installiert, installiert ihr möglicherweise nur die Client-Erweiterung und nicht
die Server-Erweiterung. Installiert den Jupyter Scheduler daher mit :term:`pip`:

.. code-block:: console

   $ python -m pip install jupyter_scheduler

Dadurch werden die Lab- und Servererweiterungen automatisch aktiviert. Ihr
könnt dies überprüfen mit

.. code-block:: console

   $ jupyter server extension list
   ...
       jupyter_scheduler enabled
       - Validating jupyter_scheduler...
   Package jupyter_scheduler took 0.0785s to import
         jupyter_scheduler 1.3.2 OK
   ...

und

.. code-block:: console

   $ jupyter labextension list
   ...
           @jupyterlab/scheduler v1.3.2 enabled  X
   ...

#. Um einen Jog aus einem geöffneten Notebook zu erstellen, klickt in der oberen
   Symbolleiste des geöffneten Notizbuchs auf die Schaltfläche :guilabel:`Create
   a notebook job`.
#. Gebt eurem Notebook-Job einen Namen, wählt die Ausgabeformate und gebt
   Parameter an, die bei der Ausführung eures Notebooks als lokale Variablen
   gesetzt werden. Diese parametrisierte Ausführung ähnelt der in
   :doc:`Papermill </notebook/parameterise/index>` verwendeten.
#. Um einen Job zu erstellen, der einmalig ausgeführt wird, wählt
   :guilabel:`Run now` und klickt auf :guilabel:`Create`.
#. Um eine Job-Definition zu erstellen, die wiederholt nach einem Zeitplan
   ausgeführt wird, wählt :guilabel:`Run on a schedule`.
