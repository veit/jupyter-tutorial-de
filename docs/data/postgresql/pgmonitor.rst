pgMonitor
=========

`pgMonitor <https://access.crunchydata.com/documentation/pgmonitor/latest/>`_
ist eine Umgebung um den Zustand und die Leistung eines PostgreSQL-Cluster zu
visualisieren. Es kombiniert eine Reihe von Werkzeugen, um die Erfassung
wichtiger Metriken zu erleichtern, darunter:

* Anzahl der Verbindungen
* Datenbankgröße
* Replikationsverzögerung
* Transaktionsumlauf
* Zusätzlicher Speicherplatz, der von Ihren Tabellen und Indizes belegt wird
* CPU, Speicher, I/O und Betriebszeit

Es kombiniert mehrere Open-Source-Softwarepakete, um eine robuste
PostgreSQL-Überwachungsumgebung zu schaffen, einschließlich:

`PostgreSQL Exporter <https://github.com/wrouesnel/postgres_exporter>`_
    Datenexport zu Prometheus, der das Sammeln von Metriken von jedem
    PostgreSQL-Server ≥ 9.1 unterstützt.
`Prometheus <https://prometheus.io/>`_
    sammelt Metriken und ist in hohem Maße anpassbar.
`Grafana <https://grafana.com/>`_
    visualisiert Daten in vielen verschiedenen Arten von Diagrammen und Graphen.

Installation und Konfiguration
------------------------------

Installations- und Konfigurationsanleitungen für die verschiedenen Pakete werden
bereitgestellt:

#. `PostgreSQL Exporter
   <https://access.crunchydata.com/documentation/pgmonitor/latest/exporter>`_
#. `Prometheus
   <https://access.crunchydata.com/documentation/pgmonitor/latest/prometheus>`_
#. `Grafana
   <https://access.crunchydata.com/documentation/pgmonitor/latest/grafana>`_
