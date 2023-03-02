Deployment und Release Branches
===============================

.. _deployment-branches:

Deployment-Branches
-------------------

Deployment-Branches empfehlen sich, wenn ihr :abbr:`z.B. (zum Beispiel)` den
Release-Zeitpunkt nicht selbst bestimmen könnt, wie bei einer iOS-Anwendung, die
die App-Store-Validierung bestehen muss, oder wenn euch nur ein bestimmtes
Zeitfenster für die Bereitstellung zur Verfügung steht. In diesen Fällen
empfiehlt sich ein *Production*-Branch, der den bereitgestellten Code
widerspiegelt. Ein solcher Arbeitsablauf verhindert dann zusätzliche
Arbeitsaufwände bei :doc:`../rebase` und :doc:`../tag`.

Angenommen, ihr verfügt über eine ``development``-, ``staging``- und
``production``-Umgebung, dann wird zunächst ein Merge- oder Pull-Request für
eine Feature-Entwicklung beim ``staging``-Branch gestellt. Sofern die
Qualitätsprüfung dort bestanden wurde und der Code
produktionsreif ist, können die Änderungen in den ``main``-Branch übernommen
werden. Dieser Vorgang kann sich für neue Features mehrfach wiederholen, bis
:abbr:`z.B. (zum Beispiel)` der Zeitpunkt für das *Going Life* dieser Änderungen
gekommen ist und ein Deployment-Branch erstellt werden kann.

.. graphviz::

    strict digraph DeploymentBranches {
    nodesep=0.5;
    ranksep=0.25;
    splines=line;
    forcelabels=false;

    // general
    node [style=filled, color="black",
        fontcolor="black", font="Consolas", fontsize="8pt" ];
    edge [arrowhead=vee, color="black", penwidth=2];

    // graph
    node [width=0.2, height=0.2, fixedsize=true, label="", margin="0.11,0.055", shape=circle, penwidth=2, fillcolor="#FF0000"]

    // branches
    node  [group="main", fillcolor="#27E4F9"];
    main1;
    main2;
    main3;
    subgraph {
        mainstart [label="", width=0, height=0, penwidth=0];
    }
    subgraph {
        mainend [label="", width=0, height=0, penwidth=0];
    }
    mainstart -> main1 [color="#b0b0b0", style=dashed, arrowhead=none];
    main1 -> main2 -> main3;
    main3 -> mainend [color="#b0b0b0", style=dashed, arrowhead=none];

    node  [group="deployment", fillcolor="#52C322"];
    deployment1;

    node  [group="staging", fillcolor="#FFE333"];
    staging1;
    staging2;
    staging3;
    staging4;
    subgraph {
        stagingstart [label="", width=0, height=0, penwidth=0];
    }
    subgraph {
        stagingend [label="", width=0, height=0, penwidth=0];
    }
    stagingstart -> staging1 [color="#b0b0b0", style=dashed, arrowhead=none ];
    staging1 -> staging2 -> staging3 -> staging4;
    staging4 -> stagingend [color="#b0b0b0", style=dashed, arrowhead=none ];

    node  [group="17-some-feature", fillcolor="#FB3DB5"];
    feature171;
    feature172;
    feature173;
    subgraph features17 {
        feature171 -> feature172 -> feature173;
    }

    node  [group="42-other-feature", fillcolor="#FB3DB5"];
    feature421;
    feature422;
    feature423;
    feature424;
    feature425;
    feature426;
    subgraph{ rank=same; feature171; feature421; }
    feature421 -> feature422 -> feature423 -> feature424 -> feature425 -> feature426;

    node  [group="43-some-feature", fillcolor="#FB3DB5"];
    feature431;
    feature432;
    feature433;
    subgraph features43 {
        feature431 -> feature432 -> feature433;
    }

    // branching and merging
    main1 -> feature171;
    feature173 -> staging1;
    staging1 -> main2;
    main2-> deployment1;

    staging3 -> main3;

    main2 -> feature431;
    feature433 -> staging4;

    main1 -> feature421;
    feature424 -> staging2;
    feature426 -> staging3;

    }

.. _release-branches:

Release-Branches
----------------

Wenn Software an Kunden geliefert werden soll, empfehlen sich :abbr:`sog.
(sogenannte)` Release-Branches. In diesen Fällen sollte jeder Branch eine *Minor
Version* erhalten, also :abbr:`z.B. (zum Beispiel)` ``2.7`` oder ``3.10``.
Üblicherweise werden diese Branches so spät wie möglich aus dem ``main``-Branch
erzeugt um bei Bugfixes die Anzahl der Merges, die auf mehrere Branches verteilt
werden müssen, zu reduzieren. Nachdem ein neuer Release-Branch erstellt wurde,
erhält dieser nur noch Bugfixes. Meist werden diese zunächst in den
``main``-Branch übernommen und kommen anschließend von dort mit
:doc:`../cherry-pick` in den Release-Branch, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: console

    $ git switch 3.10
    $ git cherry-pick 61de025
    [3.10 b600967] Fix bug #17
     Date: Thu Sep 15 11:17:35 2022 +0200
     1 file changed, 9 insertions(+)

Dieser *upstream first*-Ansatz wird :abbr:`u.a. (unter anderem)` von `Google
<https://www.chromium.org/chromium-os/chromiumos-design-docs/upstream-first>`_
und `Red Hat
<https://www.redhat.com/en/blog/a-community-for-using-openstack-with-red-hat-rdo>`_
verwendet. Jedes Mal, wenn ein Bugfix in einen Release-Branch übernommen wurde,
wird das Release mit einem :doc:`../tag` um eine Patch-Version angehoben,
:abbr:`s.a. (siehe auch)` `Semantic Versioning <https://semver.org/>`_.

.. graphviz::

    strict digraph ReleaseBranches {
    nodesep=0.5;
    ranksep=0.25;
    splines=line;
    forcelabels=false;

    // general
    node [style=filled, color="black",
        fontcolor="black", font="Consolas", fontsize="8pt" ];
    edge [arrowhead=vee, color="black", penwidth=2];

    // tags
    node [shape=cds, fixedsize=false, fillcolor="#C6C6C6", penwidth=1, margin="0.11,0.055"]
    tag270 [label="2.7.0"]
    tag278 [label="2.7.8"]
    tag3100 [label="3.10.0"]
    tag3101 [label="3.10.1"]

    // graph
    node [width=0.2, height=0.2, fixedsize=true, label="", margin="0.11,0.055", shape=circle, penwidth=2, fillcolor="#FF0000"]

    // branches
    node  [group="main", fillcolor="#27E4F9"];
    main1;
    main2;
    main3;
    subgraph {
        rank=source;
        mainstart [label="", width=0, height=0, penwidth=0];
    }
    subgraph {
        mainend [label="", width=0, height=0, penwidth=0];
    }
    mainstart -> main1 [color="#b0b0b0", style=dashed, arrowhead=none];
    main1 -> main2 -> main3;
    main3 -> mainend [color="#b0b0b0", style=dashed, arrowhead=none];

    node  [group="27", fillcolor="#FFE333"];
    release270;
    release278;
    subgraph {
        release27end [label="", width=0, height=0, penwidth=0];
    }
    release270 -> release278 [color="#b0b0b0", style=dashed];
    release278 -> release27end [color="#b0b0b0", style=dashed, arrowhead=none];

    node  [group="310", fillcolor="#52C322"];
    release3100;
    release3101;
    subgraph {
        release310end [label="", width=0, height=0, penwidth=0];
    }
    release3100 -> release3101;
    release3101 -> release310end [color="#b0b0b0", style=dashed, arrowhead=none ];

    node  [group="hotfix", fillcolor="#FD5965"];
    hotfix17;

    // branching and merging
    main1 -> release270;
    main2 -> release3100;
    main2 -> hotfix17;
    hotfix17 -> main3;
    main3 -> release278 [color="#6D031C", style=dashed];
    main3 -> release3101 [color="#6D031C", style=dashed];

    // tags connections
    edge [color="#b0b0b0", style=dotted, len=0.3, arrowhead=none, penwidth=1];
    subgraph  {
        rank="same";
        tag270 -> release270;
    }
    subgraph  {
        rank="same";
        tag278 -> release278;
    }
    subgraph  {
        rank="same";
        tag3100 -> release3100;
    }
    subgraph  {
        rank="same";
        tag3101 -> release3101;
    }
    }
