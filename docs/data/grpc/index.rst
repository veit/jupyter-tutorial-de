gRPC
====

gRPC ist ein modernes Open-Source-RPC (High Performance Remote Procedure
Call)-Framework. Standardmäßig verwendet gRPC
:doc:`../serialisation-formats/protobuf` als Interface Definition Language (IDL)
zur Beschreibung sowohl des Interfaces als auch der Struktur der Payload
Messages. In gRPC kann eine Clientanwendung direkt eine Methode auf einer
entfernten Serveranwendung aufrufen als wäre es ein lokales Objekt, sodass
verteilte Anwendungen und Dienste einfacher erstellt werden können. Wie in
vielen RPC-Systemen basiert gRPC auf der Idee, einen Service zu definieren und
die Methoden anzugeben, die mit ihren Parametern und Rückgabetypen remote
aufgerufen werden können. Auf der Serverseite implementiert der Server diese
Schnittstelle und führt einen gRPC-Server aus, um Clientaufrufe zu verarbeiten.
Auf der Clientseite verfügt der Client über einen Stub, der dieselben Methoden
wie der Server bereitstellt.

.. graphviz::

    digraph grpc_concept {
        rankdir="LR";
        graph [fontname = "Calibri", fontsize="16", overlap=false];
        node [fontname = "Calibri", fontsize="16", style="bold", penwidth="5px"];
        edge [fontname = "Calibri", fontsize="16", style="bold", penwidth="5px"];
        tooltip="gRPC concept";

        // C++ service
        subgraph "cluster_cpp_service" {
            label="C++ Service";
            shape=box;
            style= "rounded, filled";
            color="#00FFFF";
            "grpc_server" [
                label="gRPC server",
                shape=box,
                style= "rounded, filled",
                color="#4da6ff"
                ];
            }

        // Python-Stub
        subgraph cluster_python_client {
            label="Python client";
            shape=box;
            style= "rounded, filled";
            color="#00FFFF";
            grpc_python_stub [
                label="gRPC Python-Stub",
                shape=box,
                style= "rounded, filled",
                color="#4da6ff"
                ];
            }

        // Android-Java-Stub
        subgraph cluster_android_java_client {
            label="Android-Java client";
            shape=box;
            style= "rounded, filled";
            color="#00FFFF";
            grpc_android_java_stub [
                label="gRPC Android-Java-Stub",
                shape=box,
                style= "rounded, filled",
                color="#4da6ff"
                ];
            }

        grpc_python_stub -> grpc_server [label="Proto-Request", color="#4da6ff"]
        grpc_server -> grpc_python_stub [label="Proto-Response", color="#4da6ff"]
        grpc_server -> grpc_android_java_stub [label="Proto-Response", color="#4da6ff"]
        grpc_android_java_stub -> grpc_server [label="Proto-Request", color="#4da6ff"]

    }

Ausgehend von einer Schnittstellendefinition in einer ``.proto``-Datei bietet
gRPC Protocol-Compiler-Plugins, die clientseitige und serverseitige APIs
generieren. Sowohl die synchrone als auch die asynchrone Kommunikation wird in
den meisten Sprachen unterstützt. gRPC unterstützt auch das Streaming von
Nachrichten in einem einzelnen RPC-Aufruf. Das `gRPC-Protokoll
<https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md>`_ gibt abstrakt
die Kommunikation zwischen Clients und Servern an:

#. Zuerst wird der Stream vom Client mit einem obligatorischen ``Call Header``
   gestartet

   #. gefolgt von optionalen ``Initial-Metadata``
   #. fefolgt von optionalen ``Payload Messages``.

   Die Inhalte von ``Call Header`` und ``Initial Metadata`` werden als
   HTTP/2-Headers mit ``HPACK`` komprimiert.

#. Der Server antwortet mit optionalen ``Initial-Metadata``

   #. gefolgt von ``Payload Messages``
   #. und schließlich mit obligatorischem ``Status`` und optionalen
      ``Status-Metadata``.

   Payload-Nachrichten werden in einen Byte-Stream serialisiert, der in
   HTTP/2-Frames fragmentiert ist. ``Status`` und ``Trailing-Metadata`` werden
   als HTTP/2-Trailing-Headers gesendet.

.. seealso::
    * `Home <https://grpc.io/>`_
    * `GitHub <https://github.com/grpc/grpc>`_
    * `gRPC Load Balancing <https://grpc.io/blog/grpc-load-balancing/#why-grpc>`_

.. toctree::
    :hidden:
    :titlesonly:
    :maxdepth: 0

    install
    example
    test
