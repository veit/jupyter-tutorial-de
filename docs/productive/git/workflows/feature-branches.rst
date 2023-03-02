Feature-Branch-Workflows
========================

Die Grundidee hinter den Feature-Branch-Workflows ist, dass die Entwicklung
einzelner Features jeweils in einem dedizierten Branch und nicht im
``main``-Branch stattfinden sollte. Diese Kapselung erleichtert in einem
Entwicklungsteam die Arbeit, da Veränderungen im ``main``-Branch nicht stören
und zunächst vernachlässigt werden können. Umgekehrt sollte so vermieden werden,
dass der ``main``-Branch durch unfertigen Code verunreinigt wird. Dieses zweite
Argument erleichtert dann auch die `kontinuierliche Integration
<https://de.wikipedia.org/wiki/Kontinuierliche_Integration>`_ mit anderen
Komponenten.

.. seealso::
   * `Feature Driven Development
     <https://de.wikipedia.org/wiki/Feature_Driven_Development>`_
   * Martin Fowler: `Feature Branch
     <https://martinfowler.com/bliki/FeatureBranch.html>`_

Merge- oder Pull-Requests
-------------------------

Die Kapselung der Entwicklung einzelner Features in einem Branch ermöglicht
zudem die Verwendung von :abbr:`sog. (sogenannten)` Merge- oder Pull-Requests
um Änderungen mit anderen im Team diskutieren zu können und ihnen die
Möglichkeit zu geben, ein Feature freizugeben, bevor es in das offizielle
Projekt integriert wird. Wenn ihr in eurer Feature-Entwicklung auf Probleme
stoßt, könnt ihr Merge- oder Pull-Requests jedoch auch nutzen um mit anderen im
Team Lösungsmöglichkeiten zu erörtern. Merge- oder Pull-Requests werden von
Web-basierten Diensten wie `GitHub <https://github.com/>`_, `GitLab
<https://about.gitlab.com/>`_ und `Atlassian <https://bitbucket.org/>`_ zum
Review und Kommentieren der Änderungen bereitgestellt. Mit :samp:`@{ID}` in
euren Kommentaren könnt ihr auch bestimmte Personen aus dem Projektteam direkt
nach Feedback fragen. Sofern ihr automatisiert testet, könnt ihr hier auch die
Testergebnisse sehen; :abbr:`evtl. (eventuell)` entspricht ja der Coding Style
nicht euren Projektrichtlinien, oder die Testabdeckung ist ungenügend. In den
Merge- oder Pull-Requests werden solche Diskussionen gefördert und dokumentiert
ohne dass sie als unmittelbar als Commits im Repository erscheinen.

.. warning::
   Merge- oder Pull-Requests sind kein Bestandteil von Git selbst, sondern des
   jeweiligen Web-basierten Dienstes. Sie sind auch nicht standardisiert, sodass
   sie beim Wechsel auf einen anderen Dienst nur mühsam übernommen werden
   können.

.. seealso::
   * `About pull requests
     <https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests>`_
   * `Making a Pull Request
     <https://www.atlassian.com/git/tutorials/making-a-pull-request>`_
   * `Merge requests
     <https://docs.gitlab.com/ee/user/project/merge_requests/>`_

GitHub Flow
-----------

`GitHub Flow <https://docs.github.com/en/get-started/quickstart/github-flow>`_
war als stark vereinfachte Alternative zu :doc:`git-flow` gedacht, wobei es
neben dem ``main``-Branch nur verschiedene Feature-Branches geben sollte. Der
Lebenszyklus eines Git-Feature-Branches könnte dann so aussehen:

#. Alle Feature-Branches starten auf Basis des aktuellen ``main``-Branches.

   Hierzu wechseln wir zunächst in den ``main``-Branch, holen uns die neuesten
   Änderungen vom Server und aktualisieren unsere lokale Kopie des Repositories:

   .. code-block:: console

      $ git switch main
      $ git fetch origin
      $ git reset --hard origin/main

#. Erstellen des Feature-Branches.

   Wir erstellen einen Feature-Branch mit ``git switch -c`` und der Nummer des
   Tickets in der Aufgabenverwaltung, das dieses Feature beschreibt.

   .. code-block:: console

      $ git switch -c 17-some-feature

#. Hinzufügen und Committen von Änderungen.

   .. code-block:: console

      $ git add SOMEFILE
      $ git commit

#. Pushen des Feature-Branches mit den Änderungen.

   Durch das Pushen des Feature-Branches mit Deinen Änderungen erstellt Ihr
   nicht nur eine Sicherungskopie eurer Änderungen, sondern ihr ermöglicht auch
   anderen im Team, sich die Änderungen anzuschauen.

   .. code-block:: console

      $ git push -u origin 17-some-feature

   Der ``-u``-Parameter fügt den ``17-some-feature``-Branch dem
   Upstream-Git-Server (``origin``)  als Remote-Branch hinzu. Zukünftig könnt
   ihr dann in diesen Branch pushen ohne weitere Parameter angeben zu müssen.

#. Merge- oder Pull-Request stellen

   Sobald ihr ein Feature fertiggestellt habt, wird dieses nicht sofort in den
   ``main``-Branch gemergt, sondern ein Merge- oder Pull-Request erstellt, durch
   den andere aus dem Entwicklungsteam die Gelegenheit erhalten, eure Änderungen
   zu überprüfen. Alle Änderungen an diesem Branch werden nun ebenfalls in
   diesem Merge- oder Pull-Request angezeigt.

#. Zusammenführen

   Sobald euer Merge- oder Pull-Request akzeptiert wird, müsst ihr zunächst
   sicherstellen, dass euer lokaler ``main``-Branch mit dem
   Upstream-``main``-Branch synchronisiert ist; erst dann könnt ihr den
   Feature-Branch in den ``main``-Branch mergen und schließlich den
   aktualisierten ``main``-Branch zurück in den Upstream-``main``-Branch pushen.
   Dies wird jedoch nicht selten zu einem Merge-Commit führen. Dennoch hat
   dieser Workflow den Vorteil, dass klar zwischen der Feature-Entwicklung und
   dem Zusammenführen unterschieden werden kann.

Simple-Git-Workflow
-------------------

Auch Atlassian empfiehlt eine `ähnliche Strategie
<https://www.atlassian.com/blog/git/simple-git-workflow-is-simple>`_, wobei sie
jedoch ein :doc:`rebase <../rebase>` der Feature-Branches empfehlen. Hiermit
erhaltet ihr einen linearen Verlauf, indem die Änderungen im Feature-Branch vor
dem Zusammenführen mit einem Fast-Forward-Merge an die Spitze des ``main``-Branch verschoben werden.

#. Verwendet ``rebase``, um euren Feature-Branch auf dem neuesten Stand von
   ``main`` zu halten:

   .. code-block:: console

      $ git fetch origin
      $ git rebase -i origin/main

   In dem selteneren Fall, dass andere aus dem Team auch im selben Feature-Zweig
   arbeiten, solltet ihr auch deren Änderungen übernehmen:

   .. code-block:: console

      $ git rebase -i origin/17-some-feature

   Löst zu diesem Zeitpunkt alle Konflikte, die sich aus ``rebase`` ergeben.
   Dies sollte am Ende der Feature-Entwicklung zu einer Reihe von sauberen
   Merges geführt haben. Außerdem bleibt die Historie eurer Feature-Zweige
   sauber und fokussiert, ohne störendes Rauschen.

#. Wenn ihr bereit für Feedback seid, pusht euren Zweig:

   .. code-block:: console

      $ git push -u origin 17-some-feature

   Anschließend könnt ihr einen Merge- oder Pull-Request stellen.

   Nach diesem Push könnt ihr als Reaktion auf Feedback den entfernten Zweig
   immer wieder aktualisieren.

#. Nachdem die Überprüfung abgeschlossen solltet ihr eine letzte Bereinigung
   der Commit-Historie des Feature-Zweiges vornehmen, um unnötige Commits zu
   entfernen, die keine relevanten Informationen liefern.

#. Wenn die Entwicklung abgeschlossen ist, führt die beiden Zweige mit
   ``-no-ff`` zusammen.  Dadurch bleibt der Kontext der Arbeit erhalten und es
   wird einfach sein, das gesamte Feature bei Bedarf zurückzunehmen:

   .. code-block:: console

      $ git switch main
      $ git pull origin main
      $ git merge --no-ff 17-some-feature

Zusammenfassung
---------------

Die Vorteile von Feature-Branches-Workflows sind vor allem

* Features werden in einzelnen Branches isoliert, sodass jedes Teammitglied
  unabhängig arbeiten kann.
* Gleichzeitig wird die Zusammenarbeit im Team enger über Merge- oder
  Pull-Requests.
* Das zu verwaltende Code-Inventar bleibt relativ klein da die Feature-Branches
  üblicherweise schnell in den ``main`` übernommen werden können.
* Die Workflows entsprechen den üblichen Methoden kontinuierlicher Integration.

Sie können jedoch nicht beantworten, wie Deployments in unterschiedliche
Umgebungen oder die Aufteilung in verschiedene Releases erfolgen sollen.
Mögliche Antworten hierfür werden in :doc:`deploy-branches` beschrieben.
