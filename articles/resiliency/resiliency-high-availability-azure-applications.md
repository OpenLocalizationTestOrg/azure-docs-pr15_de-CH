<properties
   pageTitle="Hohe Verfügbarkeit für Azure Applications | Microsoft Azure"
   description="Technische Übersicht und detaillierte Informationen zum Entwerfen und Erstellen von hoher Verfügbarkeit auf Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>Hohe Verfügbarkeit bei Microsoft Azure

Eine hochverfügbare Anwendung nimmt Fluktuationen Verfügbarkeit, laden und vorübergehende Ausfälle der Dienste und Hardware. Die Anwendung weiterhin zulässigen Benutzer und systemische Reaktionszeit Unternehmen oder Application Service Level Agreements (SLAs) definiert.

##<a name="azure-high-availability-features"></a>Azure Funktionen für hohe Verfügbarkeit

Azure bietet viele integrierte Plattform, die hohe Verfügbarkeit unterstützen. Dieser Abschnitt beschreibt die wichtigsten Funktionen. Anzeigen Sie für eine umfassendere Analyse der Plattform [Azure Resiliency technische Anleitung](./resiliency-technical-guidance.md)

###<a name="fabric-controller"></a>Fabric controller

Azure Fabric Controller Vorschriften und den Zustand der Azure Computinginstanzen überwacht. Der Fabric Controller überprüft den Status der Hardware und Software Instanzen Computer Host und Gast. Wenn einen Fehler erkannt wird, erzwingt es SLAs von VM-Instanzen automatisch verschieben. Weitere Fehler und Upgrade Domänen unterstützt die Compute-SLA.

Wenn mehrere Instanzen Cloud-Dienst bereitgestellt werden, bereitgestellt Azure diese Instanzen in anderen Fehlerdomänen. Eine Grenze Fehler ist im Grunde ein Rack anderer Hardware im selben Bereich. Fehlerdomänen verringert sich die Wahrscheinlichkeit, dass ein lokalisierte Hardwarefehler Dienst einer Anwendung unterbrochen wird. Sie können nicht Anzahl Fehlerdomänen verwalten, die die Arbeitskraft oder Webrollen zugeordnet sind. Der Fabric Controller verwendet dedizierte Ressourcen getrennt von Azure gehostete Anwendung. Es hat eine Betriebszeit von 100 Prozent, da als Kern der Azure-System dient. Es überwacht und Fehlerdomänen Instanzen verwaltet.

Das folgende Diagramm zeigt Azure Ressourcen der Fabric Controller bereitgestellt und anderen Fehlerdomänen verwaltet.

![Vereinfachte Ansicht Fehler der Isolation von Domänen](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Upgrade Domänen Fehlerdomänen Funktion ähnlich, aber sie unterstützen Upgrades als Fehler. Eine Aktualisierungsdomäne ist eine logische Einheit Instanz Trennung, die bestimmt Instanzen in einen bestimmten Dienst zu einem Zeitpunkt aktualisiert werden. Für die Bereitstellung gehosteter Dienst standardmäßig sind fünf Upgrade Domänen definiert. Allerdings können Sie den Wert in der dienstdefinitionsdatei ändern. Beispielsweise haben Sie acht Instanzen Ihrer Web-Rolle. Es werden zwei Instanzen in drei Upgrade Domänen und zwei Instanzen in einer Domäne aktualisieren. Azure definiert die Updatesequenz, jedoch basiert auf der Anzahl der Domänen aktualisieren. Weitere Informationen zu Aktualisierung Domänen finden Sie in der [Cloud-Dienst aktualisieren](../cloud-services/cloud-services-update-azure-service.md).

###<a name="features-in-other-services"></a>Funktionen in anderen Diensten

Neben der Plattformfunktionen, mit denen Compute hohe Verfügbarkeit, bettet Azure Hochverfügbarkeitsfunktionen in seine Dienste. Azure-Speicher verwaltet z. B. drei Replikate Blob, Tabelle und Warteschlangendaten. Außerdem können die Option Geo-Replikation in einem zweiten Bereich Backups von Blobs und Tabellen gespeichert. Azure Content Delivery Network ermöglicht Blobs weltweit für Redundanz und Skalierbarkeit zwischengespeichert werden. Azure SQL-Datenbank verwaltet sowie mehrere Replikate.

Neben der [Stabilität technische Anleitung](https://aka.ms/bctechguide) Artikelreihe finden Sie [Bewährte Methoden für den Entwurf Scale Dienste auf Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) Papier. Diese Studien bieten eine eingehendere Erläuterung der Azure-Plattform-Verfügbarkeit.

Azure mehrere Features bietet, die hohen Verfügbarkeit zu unterstützen, ist es wichtig, ihre Grenzen zu verstehen:

- Compute garantiert Azure Rollen verfügbar sind und ausgeführt werden, dass es nicht klar, ob die Anwendung ausgeführt wird oder überlastet ist.
- Azure SQL-Datenbank werden die Daten im Bereich synchron repliziert. Aktive georeplikation können bis zu vier zusätzliche Kopien in derselben Region (oder Regionen). Diese Datenbankreplikate sind keine Point-in-Time-Backups. SQL-Datenbanken bieten Point-in-Time-backup-Funktionen. Um weitere Informationen zu SQL-Datenbank Point-in-Time-Funktionen lesen Sie [Azure SQL Datenbank Punkt wiederherstellen](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Für Azure-Speicher ist standardmäßig auf einen anderen Bereich Tabellen-und Blob repliziert. Allerdings kann erst Microsoft wählt Failover auf den anderen Standort Replikate zugreifen. Region-Failovers bei längeren flächendeckende serviceunterbrechung, und es besteht kein DIENSTVERTRAG für Geo-Failover-Zeit. Es ist auch zu beachten, dass Datenfehlern schnell Replikate verteilt.

Aus diesen Gründen müssen Sie Verfügbarkeit Plattformen mit anwendungsspezifischen Verfügbarkeit ergänzen. Anwendungsspezifische Verfügbarkeit gehören die BLOB-Snapshot-Funktion zum Erstellen von Point-in-Time-Backups von BLOB-Daten.

###<a name="availability-sets-for-azure-virtual-machines"></a>Verfügbarkeit wird für Azure Virtual Machines

Die meisten dieser Artikel konzentriert sich auf Cloud Services eine Plattform als (PaaS) Servicemodell verwenden. Es gibt auch bestimmte Verfügbarkeit für Azure-Computer, die eine Infrastruktur als Service (IaaS) Modell verwendet. Um hohe Verfügbarkeit mit virtuellen Maschinen zu erzielen, verwenden Sie Verfügbarkeit legt. Ein Satz Verfügbarkeit ist eine ähnliche Funktion Fehler und der Aktualisierung. Azure Positionen innerhalb einer Verfügbarkeit der virtuellen Computer auf eine Weise, die verhindert, dass lokalisierte Hardware- und Wartungsaktivitäten schalten Sie alle Computer in dieser Gruppe. Verfügbarkeit legt müssen der Azure-SLA für die Verfügbarkeit der virtuellen Maschinen zu erreichen.

Die folgende Abbildung enthält eine Darstellung von zwei Verfügbarkeit dieser Gruppe Web- und SQL Server virtueller Maschinen bzw..

![Verfügbarkeit wird für Azure Virtual Machines](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] Im obigen Diagramm ist SQL Server auf virtuellen Computern installiert. Dies unterscheidet sich von der vorherigen Diskussion Azure SQL-Datenbank, die eine als verwalteter Dienst Datenbank.

##<a name="application-strategies-for-high-availability"></a>Von anwendungsstrategien für hohe Verfügbarkeit

Die meisten anwendungsstrategien für hohe Verfügbarkeit umfassen Redundanz oder harte Abhängigkeit zwischen Komponenten entfernen. Anwendungsentwurf unterstützt Fehlertoleranz bei sporadischen Ausfällen Azure oder Dienste von Drittanbietern. Die folgenden Abschnitte beschreiben Anwendungsmuster für die Verfügbarkeit von Cloud-Diensten.

###<a name="asynchronous-communication-and-durable-queues"></a>Asynchrone Kommunikation und dauerhaften Warteschlangen

Asynchronen Kommunikation zwischen lose gekoppelter Dienste Verfügbarkeit in Azure Applications zu berücksichtigen. In diesem Muster speicherwarteschlangen oder Azure Service Bus-Warteschlangen für die spätere Verarbeitung schreiben Sie Nachrichten. Beim Schreiben der Nachricht in die Warteschlange zurückgegeben Steuerung sofort an den Absender der Nachricht. Eine weitere Ebene von der Anwendung verarbeitet die Nachricht verarbeitet, in der Regel als eine Worker-Rolle implementiert. Wenn Worker-Rolle ausfällt, werden Nachrichten in der Warteschlange ansammeln, bis der Verarbeitungsdienst wiederhergestellt. Solange die Warteschlange vorhanden ist, ist keine direkte Beziehung zwischen dem Front-End-Absender und Nachrichtenprozessor. Dadurch entfällt die Notwendigkeit für synchrone Serviceanrufe ein Engpass in verteilten Anwendung sein kann.

Eine Variante dieses verwendet als Failover-Standort für fehlgeschlagene Datenbankaufrufe Azure Storage (Blobs, Tabellen, Warteschlangen) oder Servicebuswarteschlangen. Ein synchroner Aufruf innerhalb einer Anwendung an einen anderen Dienst (z. B. Azure SQL-Datenbank) kann beispielsweise wiederholt. Sie können diese Daten in permanenten Speicher serialisieren können. Später wird der Dienst oder die Datenbank wieder online kann die Anwendung die Anforderung aus dem Speicher erneut. In diesem Modell unterscheidet, Lagerplatz nicht Konstante Anwendungsworkflow gehört. Es wird nur in Szenarien verwendet.

In Szenarien, die asynchrone Kommunikation und Zwischenlagerung verhindern Sie stillgelegte Back-End-Dienst die gesamte Anwendung. Warteschlangen dienen als logische Vermittler. Weitere Hinweise zur Auswahl der richtigen Warteschlangendienst finden Sie unter [Azure und Azure Service Bus-Warteschlangen - verglichen und gegenübergestellt](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>Fehler erkennen und Wiederholung Logik

Ein Kernpunkt hochverfügbare Anwendungsentwurf ist Wiederholungslogik in Code verwenden, um einen Dienst ordnungsgemäß zu behandeln, der vorübergehend ausgefallen ist. Die [Vorübergehende Fehler Handling Application Block](https://msdn.microsoft.com/library/hh680934.aspx)von Microsoft Patterns and Practices-Team entwickelte unterstützt Entwickler dabei. Das Wort "vorübergehend" bedeutet ein vorübergehendes Problem, das nur von kurzer Dauer. Im Kontext dieses Artikels gehört vorübergehender Fehler behandeln eine hochverfügbare Anwendung entwickeln. Beispiele für vorübergehende Zustände Netzwerkprobleme Fehler und Datenbankverbindungen.

Die vorübergehende Fehler Handling Application Block ist vereinfacht Sie Fehler im Code ordnungsgemäß zu behandeln. Sie können die Verfügbarkeit der Anwendung erhöhen, indem Sie stabile transiente Fehlerbehandlungslogik. In den meisten Fällen Wiederholungslogik kurze Unterbrechung behandelt und Absender und Empfänger nach mindestens Versuche verbindet. Ein erfolgreicher Wiederholungsversuch unbemerkt in der Regel den Benutzern.

Entwickler haben drei Optionen für die Verwaltung ihrer Wiederholungslogik: inkrementelle festen Intervall und exponentiellen. Inkrementelle wartet wiederholen jede mehr vor eine zunehmende lineare Weise (z. B. 1, 2, 3 und 4 Sekunden). Zeitspanne wartet derselben Zeitspanne zwischen den einzelnen Neuversuchen (z. B. 2 Sekunden). Je zufälliger Option wartet der exponentielle Backoff mehr zwischen Wiederholungsversuchen. Allerdings verwendet exponentielle Verhalten (z. B. 2, 4, 8 und 16 Sekunden).

Die Strategie im Code ist:

1. Definieren Sie die Wiederholung und Strategie.
1. Versuchen Sie, die ein vorübergehender Fehler führen.
1. Wenn vorübergehende Fehler rufen Sie wiederholungsrichtlinie auf.
1. Wenn alle Versuche fehlschlagen, fangen Sie eine letzte Ausnahme.

Testen Sie die Wiederholungslogik in simulierter Fehler um sicherzustellen, dass Versuche für aufeinander folgende Operationen nicht unvorhergesehene erhebliche Verzögerung führen. Dies vor dem gesamten Vorgang fehlschlagen.

###<a name="reference-data-pattern-for-high-availability"></a>Verweis Datenmuster für hohe Verfügbarkeit

Referenzdaten sind schreibgeschützte Daten einer Anwendung. Diese Daten enthalten Geschäftskontext, in dem die Anwendung im Verlauf eines Geschäftsvorgangs Transaktionsdaten generiert. Transaktionsdaten ist eine Point-in-Time-Funktion der Referenzdaten. Daher hängt die Integrität der Snapshot der Daten zum Zeitpunkt der Buchung. Dies ist etwas lose Definition sollte für unsere Zwecke ausreichend.

Daten im Kontext einer Anwendung ist für das Funktionieren der Anwendung erforderlich. Die jeweiligen Programme erstellen und Verwalten von Daten; Master Data Management (MDM) Systeme führen häufig diese Funktion. Diese Systeme sind für den Lebenszyklus der Daten verantwortlich. Daten gehören Produktkatalog, Mitarbeiter Master Teile-Master und Geräte-Master. Daten können auch aus außerhalb der Organisation z. B. Postleitzahlen oder Steuersätze. Strategien für die Verfügbarkeit von Daten sind in der Regel leichter als Transaktionsdaten. Daten hat den Vorteil, meist unveränderlich.

Sie können Azure Web- und Worker-Rollen, die Daten zur Laufzeit autonomen nutzen durch die Daten zusammen mit der Anwendung bereitstellen. Die Größe des lokalen Speicher eine Bereitstellung ermöglicht, ist ein Idealzustand. Eingebettete Datenbanken (SQL, NoSQL) oder XML-Dateien auf einem lokalen Dateisystem bereitgestellt helfen Autonomie Azure Compute Maßeinheiten. Allerdings benötigen Sie einen Mechanismus zum Aktualisieren von Daten in jeder Rolle ohne erneute Bereitstellung. Dazu platzieren Sie alle Updates für die Daten an einen Endpunkt der Cloud-Speicher (z. B. Azure BLOB-Speicher oder SQL-Datenbank). Fügen Sie Code für jede Rolle, die die Aktualisierung von Daten in den Compute-Knoten Rolle beim Systemstart downloadet. Fügen Sie Code hinzu, der ein Administrator einen erzwungenen Download in die Rolleninstanzen kann alternativ.

Zum Verbessern der Verfügbarkeit sollte die Rollen auch eine Reihe von Referenzdaten enthalten Speicher unten ist. Dadurch können die Rollen mit grundlegenden Daten erst Speicherressourcen Updates verfügbar.

![Hohe Verfügbarkeit durch autonome Compute-Knoten](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Eine Überlegung für dieses Muster ist die Bereitstellung und startgeschwindigkeit für Ihre Rollen. Wenn bereitstellen oder große Mengen von Daten beim Start downloaden können dies die Zeitspanne zu Neuinstallationen oder Instanzen drehen. Dies kann ein akzeptabler Kompromiss für die Autonomie, die Daten sofort auf jede Rolle als externe Speicher-Services abhängig sein.

###<a name="transactional-data-pattern-for-high-availability"></a>Transaktionsdaten Muster für hohe Verfügbarkeit

Transaktionale Daten sind Daten, die die Anwendung in einem Geschäftskontext erzeugt. Transaktionsdaten ist eine Kombination des Satzes von Geschäftsprozessen, die die Anwendung implementiert und die Daten, die diese Prozesse unterstützt. Transaktionsdaten können erweiterte Versand Hinweise, Debitorennummer, und Geschäftschancen Beziehung Management (CRM) gehören. Externe Systeme für Aufbewahrung oder zur weiteren Verarbeitung werden damit Transaktionsdaten zugeführt.

Denken Sie daran, Referenz, die Daten innerhalb des Systems ändern können, die für diese Daten. Aus diesem Grund müssen Transaktionsdaten Datenkontext Point-in-Time-Referenz speichern minimale externe Abhängigkeiten semantische Konsistenz aufweist. Betrachten Sie beispielsweise das Entfernen eines Produkts aus dem Katalog Monaten nach Bestellung erfüllt ist. Am besten ist soviel Datenkontext Verweis wie möglich in die Buchung einbetten. Diese behält die Semantik der Transaktion selbst wenn die Daten ändert, nachdem die Transaktion erfasst wird.

Wie bereits erwähnt, für Sicherungszwecke Architekturen, die lose Kopplung und asynchrone Kommunikation höhere Verfügbarkeit. Dies gilt für Transaktionsdaten sowie die Implementierung ist jedoch komplexer. Traditionelle Transaktions Begriffe sind in der Datenbank für die Gewährleistung der Buchung. Bei der Einführung Zwischenschichten muss der Anwendungscode Daten auf verschiedenen Ebenen ausreichend Konsistenz und Haltbarkeit ordnungsgemäß behandeln.

Der folgende Abschnitt beschreibt einen Workflow, der die Verarbeitung die Erfassung der Transaktionsdaten trennt:

1. Compute-Knoten Web: vorhanden Daten verweisen.
1. Externer Speicher: transaktionale Daten speichern.
1. Compute-Knoten Web: Endbenutzer abschließen.
1. Compute-Knoten Web: abgeschlossenen Transaktionsdaten mit Datenkontext Verweis an temporären dauerhaften Speicher garantiert eine vorhersagbare Antwort senden.
1. Compute-Knoten Web: Endbenutzer Abschluss der Transaktion zu signalisieren.
1. Hintergrund compute-Knoten: Extrahieren von Transaktionsdaten und Nachbearbeitung ggf. im aktuellen System an seinem endgültigen Speicherort senden.

Das folgende Diagramm zeigt eine mögliche Implementierung dieser Entwurf in einem gehosteten Azure Cloud Service.

![Hohe Verfügbarkeit durch Kopplung](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

Gestrichelte Pfeile in der obigen Abbildung zeigen asynchronen Verarbeitung. Die Front-End-Web-Rolle ist nicht über asynchrone Verarbeitung. Dies führt in den Speicher des Geschäftes am Bestimmungsort anhand des aktuellen Systems. Aufgrund der Latenz dieser asynchrone Modell führt, ist die Transaktionsdaten nicht sofort Abfrage. Daher jede Einheit die Transaktionsdaten in einem Cache gespeichert werden oder einer Benutzer-Sitzung sofort Benutzeroberfläche zu.

Die Web-Rolle ist unabhängig vom Rest der Infrastruktur. Die Verfügbarkeitsprofil ist eine Kombination der Webrolle Azure-Warteschlange und nicht die gesamte Infrastruktur. Neben hoher Verfügbarkeit ermöglicht dieser Ansatz die Webrolle horizontal skaliert unabhängig von der Back-End-Speicher. Dieses hohe Verfügbarkeit kann sich auf die Wirtschaftlichkeit von Vorgängen auswirken. Zusätzliche Komponenten wie Azure-Warteschlangen und Workerrollen beeinflussen die monatlichen Kosten.

Beachten Sie, dass im vorherige Diagramm eine Implementierung dieses Ansatzes entkoppelte Transaktionsdaten. Es gibt viele möglichen Implementierungen. Die folgende Liste enthält einige Alternativen:

 * Eine Worker-Rolle kann die Web-Rolle und die Speicherwarteschlange abgeschirmt werden.
 * Service Bus-Warteschlange kann statt einer Warteschlange Azure-Speicher verwendet werden.
 * Das endgültige Ziel möglicherweise Azure Storage oder einen anderen Anbieter.
 * Azure Cache kann zu Zwischenspeichern erforderlichen unmittelbaren nach der Transaktion in der Webschicht verwendet werden.

###<a name="scalability-patterns"></a>Skalierung Muster

Es ist wichtig zu beachten, dass die Skalierung der Cloud-Dienst direkt auf die Verfügbarkeit. Erhöhte Auslastung der Service reagiert nicht verursacht, ist Benutzer Eindruck die Anwendung nach unten. Verfahren Sie empfohlene Skalierbarkeit basierend auf der erwarteten Anwendungslast und zukünftige erwartet. Höchste Skala umfasst viele Aspekte, wie einzelne oder mehrere Speicherkonten in mehreren Datenbanken freigeben und Strategien Zwischenspeichern. Einen detaillierten Einblick in diese Muster finden Sie unter [Bewährte Methoden für den Entwurf Scale Dienste auf Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe von Artikeln, [Disaster Recovery und hohe Verfügbarkeit bei Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md)konzentriert. Im nächste Artikel dieser Reihe ist [Disaster Recovery bei Microsoft Azure](./resiliency-disaster-recovery-azure-applications.md).
