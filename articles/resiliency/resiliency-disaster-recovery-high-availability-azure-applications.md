<properties
   pageTitle="Disaster Recovery und hohe Verfügbarkeit für Azure Applications | Microsoft Azure"
   description="Technischen Übersichten und detaillierte Informationen über Anträge auf hohe Verfügbarkeit und Disaster Recovery bei Microsoft Azure entwerfen."
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

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Disaster Recovery und hohe Verfügbarkeit bei Microsoft Azure

##<a name="introduction"></a>Einführung

Dieser Artikel konzentriert sich auf hohe Verfügbarkeit in Azure ausgeführt. Eine umfassende Strategie für hohe Verfügbarkeit auch den Aspekt der Wiederherstellung. Ausfällen und Katastrophen in der Cloud planen, müssen Sie den Fehler schnell erkennen. Anschließend implementieren Sie eine Strategie, die die Toleranz bei der Ausfallzeit entspricht. Darüber hinaus müssen Sie das Ausmaß von Datenverlusten berücksichtigen, die die Anwendung tolerieren kann, ohne nachteilige Business folgen wiederhergestellt wird.

Die meisten Unternehmen sagen sie temporäre und umfangreichen Fehlern vorbereitet. Vor der Beantwortung dieser Frage selbst testen Ihr Unternehmen diese Fehler? Testen Sie die Wiederherstellung von Datenbanken, um sicherzustellen, dass Sie die richtigen Prozesse haben? Wahrscheinlich nicht. Deshalb startet erfolgreichen Disaster Recovery Planung und Architektur, um diese Prozesse zu implementieren. Wie viele andere nicht-funktionale Anforderungen wie Sicherheit ruft Wiederherstellung selten vorab Analyse und Verteilung Zeit, die erforderlich ist. Die meisten Unternehmen haben auch nicht das Budget für geografisch verteilte Regionen mit redundanten. Daher werden auch umwandeln häufig richtigen Disaster Recovery-Planung ausgeschlossen.

Cloud-Plattformen wie Azure, geografisch Regionen auf der ganzen Welt. Diese Plattformen bieten auch Funktionen, die Verfügbarkeit und eine Vielzahl von Disaster Recovery-Szenarien zu unterstützen. Jetzt jede Mission wichtige Anwendung angegeben werden Berücksichtigung Disaster Überprüfung des Systems. Azure hat Resiliency und Disaster Recovery für viele Dienste integriert. Sie müssen diese Plattformfeatures aufmerksam und mit Anwendung ergänzen.

Dieser Artikel erläutert die erforderlichen Architektur Schritte zu Disaster Azure-Bereitstellung müssen. Sie können größere Business Continuity-Prozesses implementieren. Business Continuity-Plan ist ein Wegweiser operative unter widrigen Umständen. Dies könnte ein Fehler mit der Technologie wie ein ausgefallener Dienst oder einer Naturkatastrophe wie Sturm oder Stromausfall. Anwendung Resiliency für Notfälle ist nur ein Teil der größeren Wiederherstellung dieses NIST: [Leistungsanspruch Planning Guide for Information Technology Systems](https://www.fismacenter.com/sp800-34.pdf).

Die folgenden Abschnitte definieren unterschiedliche Techniken und Architekturen, die diese Techniken zu Fehlern. Diese Informationen stellen Eingaben für Ihre Disaster Recovery-Prozesse und Verfahren, um sicherzustellen, dass Ihre Disaster Recovery-Strategie korrekt und effizient funktioniert.

##<a name="characteristics-of-resilient-cloud-applications"></a>Merkmale der stabilen Cloudanwendungen

Eine gut entwickelte Anwendung übersteht Capability-Fehler auf der taktischen Ebene und toleriert auch strategische systemweite Fehler auf der Regionsebene. In den folgenden Abschnitten definieren die Terminologie im gesamten Dokument beschreiben verschiedene Aspekte der stabilen Clouddienste verwiesen.

###<a name="high-availability"></a>Hohe Verfügbarkeit

Hochverfügbare Cloudanwendung implementiert Strategien zum Ausfall Abhängigkeiten wie Cloud-Plattform Dienstleistungen aufnehmen. Trotz möglicher Fehler Cloud-Plattform Funktionen ermöglicht dieser Ansatz die Anwendung die erwarteten funktionale und nicht funktionale systemische Merkmale aufweisen. Dies fällt der Channel 9 video Serie detaillierte [Failsafe: Hinweise zur Cloud-Architekturen sowie](https://channel9.msdn.com/Series/FailSafe).

Wenn Sie die Anwendung implementieren, müssen Sie die Wahrscheinlichkeit eines Ausfalls Funktion berücksichtigen. Darüber hinaus sollten Sie die Auswirkung ein Ausfalls der Anwendung aus geschäftlicher Sicht zunächst in die Strategien haben. Ohne Berücksichtigung der Unternehmen und der Wahrscheinlichkeit der Risikos die Implementierung kann teuer und möglicherweise unnötig.

Automotive Analogie für hohe Verfügbarkeit berücksichtigt. Qualität auch überlegene Technik verhindert nicht, dass gelegentliche Fehler. Wenn Ihr Auto einen Platten wird, läuft das Auto ein, jedoch arbeitet mit Funktionalität beeinträchtigt. Geplant für dieses potenzielle Vorkommnis können Sie eines dieser dünn umrandeten Ersatzreifen bis zu eine Werkstatt. Zwar der Ersatzreifen Übertragungsraten nicht zulässt, können Sie das Fahrzeug bis Reifen ersetzen noch bedienen. Ebenso kann ein Clouddienst Datenverluste Funktionen plant ein relativ kleines Problem verhindern Senkung der gesamten Anwendung. Dies gilt selbst wenn Cloud-Dienst beeinträchtigt Funktionen ausführen muss.

Einige wichtige Eigenschaften hochverfügbare Clouddienste: Verfügbarkeit, skalierbar und Fehlertoleranz. Obwohl diese Eigenschaften zusammenhängen, ist es wichtig jedes und wie sie für die allgemeine Verfügbarkeit der Lösung verstehen.

###<a name="availability"></a>Verfügbarkeit

Eine verfügbare Anwendung berücksichtigt die Verfügbarkeit der zugrunde liegenden Infrastruktur und Dienste. Anwendungsmöglichkeiten entfernen einzelne Fehlerquellen durch Redundanz und robustes Design. Sie erweitern auf Verfügbarkeit in Azure wird das Konzept der effektiven Verfügbarkeit der Plattform zu beachten. Effektive Verfügbarkeit hält Service Level Agreements (SLA) jeder abhängigen Dienst und die kumulative Auswirkung auf die Verfügbarkeit des Systems.

Verfügbarkeit ist das Maß Anteil ein Zeitfenster System arbeiten werden. Beispielsweise ist die Verfügbarkeit von mindestens zwei Instanzen einer Web- oder Arbeitskraft Rolle in Azure SLA 99,95 % (von 100 Prozent). Sie misst nicht die Leistung oder Funktionalität Dienste, die auf diesen Rollen. Jedoch ist die effektive Verfügbarkeit der Cloud-Dienst auch verschiedene SLAs abhängige Dienste betroffen. Mehr bewegliche Teile innerhalb des Systems mehr Sorgfalt nehmen Sie müssen die Anwendung erfüllen bei Verfügbarkeit an die Endbenutzer.

Berücksichtigen Sie die folgenden SLAs für Azure Service, die Azure Services verwendet: Compute Azure SQL-Datenbank und Azure-Speicher.

|Azure service|SLA   |Mögliche Minuten Ausfallzeit pro Monat (30 Tage)|
|:------------|:-----|:----------------------------------------:|
|Berechnen      |99,95 %|21,6 Minuten                              |
|SQL-Datenbank |99,99 %|4.3 Minuten                              |
|Speicher      |99,90 %|43,2 Minuten                              |

Sie müssen alle Dienste möglicherweise gehen zu unterschiedlichen Zeiten planen. In diesem vereinfachten Beispiel ist die Gesamtanzahl der Minuten pro Monat, die die Anwendung Sie 108 Minuten. Ein Monat mit 30 Tagen ist 43.200 Minuten. 108 Minuten ist.25 Prozent der Gesamtzahl der Minuten in einem Monat mit 30 Tagen (43.200 Minuten). Dadurch eine effektive Verfügbarkeit von 99,75 % für den Clouddienst.

Jedoch kann Verfügbarkeit Techniken in diesem Artikel beschriebenen dadurch verbessern. Beispielsweise der Entwurf die Anwendung weiter ausgeführt, wenn der SQL-Datenbank nicht verfügbar ist, können Sie aus der Formel entfernen. Dies bedeutet möglicherweise, dass die Anwendung mit weniger Funktionen ausgeführt wird, auch Anforderungen zu berücksichtigen sind. Eine vollständige Liste der Azure-SLAs finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

###<a name="scalability"></a>Skalierung

Skalierung beeinflusst direkt die Verfügbarkeit. Eine erhöhte Belastung fehlschlägt Anwendung ist nicht mehr verfügbar. Skalierbare Applikationen können mit konsistenten Ergebnissen Zeit Windows erhöhte Nachfrage. Wenn ein System skalierbar ist, wird Sie horizontal oder vertikal Anstieg laden konsistente Leistung zu verwalten. In einfachen Worten Fügt horizontale Skalierung mehrere Computer dieselbe Größe (Prozessor, Speicher und Bandbreite) und vertikale Skalierung erhöht die Größe der vorhandenen Computer. Für Azure müssen Sie vertikale Skalierungsoptionen für verschiedene Computer Größen für Compute auswählen. Ändern der Größe des Computers erfordert jedoch eine erneute Bereitstellung. Daher sind der flexibelste Lösung für horizontale Skalierung entwickelt. Dies gilt für Compute, da einfach die Anzahl der ausgeführten Instanzen einer Web- oder Worker-Rolle erhöhen können. Diese Instanzen behandeln erhöhten Datenverkehr Azure Webportal, PowerShell-Skripts oder Code. Basieren Sie diese Entscheidung auf überwachten Metriken. In diesem Szenario Leiden Leistung oder Metriken keinen spürbaren Abfall Belastung. Normalerweise speichern die Web- und Workerrollen Rollen beliebig extern. Dies ermöglicht flexible Lastenausgleich und ordnungsgemäße Behandlung von Änderungen Instanz zählt. Horizontale Skalierung arbeitet auch Services wie Azure Storage tiered Optionen für vertikale Skalierung nicht bereitstellt.

Cloud-Bereitstellungen sollte als eine Sammlung von Mengen-Einheiten angezeigt. Dadurch kann die Anwendung elastisch im Dienste der Durchsatz von Endbenutzern. Die Maßeinheiten sind einfacher auf Serverebene Web- und visualisieren. Dies liegt Azure bereits über Web-und Workerrollen statusfreie Compute-Knoten. Hinzufügen mehr Maßeinheiten der Bereitstellung berechnen verursacht Anwendung Zustand Management Nebeneffekte keine da Compute Skalierungseinheiten statusfrei sind. Eine Skalierungseinheit Speicher ist verantwortlich für eine Partition von Daten (strukturiert oder unstrukturiert). Skalierung Speichereinheiten zählen Azure Tabellenpartition Azure BLOB-Container und Azure SQL-Datenbank. Auch die Verwendung von mehreren Azure-Speicherkonten wirkt sich direkt auf die Anwendungsskalierbarkeit. Entwerfen Sie einen hochgradig skalierbare Cloud-Dienst mehrere Speicher Skalierungseinheiten integrieren. Wenn ein relationale Daten der Anwendung Partitionieren der Daten beispielsweise über mehrere SQL-Datenbanken. Dies ermöglicht die Speicherung zu elastische Compute Skalierungseinheiten Modell. Ebenso können Azure Storage Daten Partitionierungsschemas, die absichtlich Designs Durchsatz Bedürfnisse der Compute-Ebene benötigen. Eine Liste der best Practices zum Entwerfen von skalierbaren Cloud-Dienste finden Sie unter [Best Practices für Design Scale Dienste in Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

###<a name="fault-tolerance"></a>Fehlertoleranz

Applikationen ausgehen, dass alle abhängigen Cloud-Funktion kann und irgendwann Zeit. Eine Fault tolerant Anwendung erkennt und fehlerhafte Elemente zu weiterhin die richtigen Ergebnisse innerhalb eines bestimmten Zeitrahmens Manöver. Für die vorübergehende Fehlerzustände wird ein Fault tolerant System wiederholungsrichtlinie beschäftigen. Schwerwiegender Fehler die Anwendung Probleme erkennen und ein Failover auf alternative Hardware oder Alternativpläne, bis der Fehler behoben ist. Eine zuverlässige Anwendung kann ordnungsgemäß Fehler ein oder mehrere Teile verwalten und weiterhin ordnungsgemäß funktioniert. Fault tolerante Applikationen können mindestens Entwurfsstrategien wie Redundanz, Replikation oder Funktionalität beeinträchtigt.

##<a name="disaster-recovery"></a>Disaster recovery

Cloudbereitstellung kann aufgrund eines systemische abhängige Dienste oder die zugrunde liegende Infrastruktur funktioniert nicht mehr. Unter diesen Umständen löst ein Business Continuity-Plan der Wiederherstellung. Dieser Prozess umfasst in der Regel Betriebspersonal und automatisierten Verfahren um Anwendung verfügbaren Bereich zu reaktivieren. Dies erfordert die Übertragung von Benutzern, Daten und Dienste auf den neuen Bereich. Es umfasst auch die Verwendung von backup-Medien oder die Replikation.

Betrachten Sie die vorherige Analogie, die hohen Verfügbarkeit die Möglichkeit von Platten durch ein Spare verglichen. Im Gegensatz dazu umfasst Wiederherstellung Schritte nach einem Absturz Auto ist das Fahrzeug nicht mehr betriebsbereit. In diesem Fall ist die beste Lösung effizient Autos, ändern Sie eine Reiseagentur oder einen Freund zu. In diesem Szenario soll gibt es wahrscheinlich eine längere Verzögerung wieder unterwegs. Es gibt auch komplexer Reparatur und Rückgabe des ursprünglichen Fahrzeugs. Auf gleiche Weise wird Disaster Recovery in eine andere Region eine komplexe Aufgabe, die in der Regel einige Ausfälle und Datenverlust umfasst. Um besser zu verstehen und Bewerten von Disaster Recovery-Strategien, unbedingt zwei Begriffe definieren: Recovery Time Objective (RTO) und Recovery point Objective (RPO).

###<a name="recovery-time-objective"></a>Wiederherstellungszeit

RTO ist die Zeitspanne für die Wiederherstellung der Anwendungsfunktionalität zugeordnet. Dies basiert auf Business und bezieht sich auf die Bedeutung der Anwendung. Unternehmenskritische Anwendung erfordern eine niedrige RTO.

###<a name="recovery-point-objective"></a>RPO

Das RPO ist akzeptabel Zeitfenster Datenverlust durch den Wiederherstellungsprozess. Beispielsweise ist das RPO eine Stunde, müssen Sie vollständig sichern oder replizieren die Daten mindestens einmal pro Stunde. Sobald die Anwendung in einer anderen Region bringen können backup-Daten bis zu einer Stunde Daten fehlen. Kritische Applikationen Ziel wie RTO und eine viel kleinere RPO.

##<a name="checklist"></a>Prüfliste

Wir fassen die wichtigsten Punkte, die in diesem Artikel (und dessen Artikeln auf [hohe Verfügbarkeit](./resiliency-high-availability-azure-applications.md) und [Disaster Recovery](./resiliency-disaster-recovery-azure-applications.md) für Azure Applications) behandelt. Diese Zusammenfassung fungiert als Checkliste für für Verfügbarkeit und Disaster Recovery-Planung sollten. Diese sind für Kunden nützlich, die eine erfolgreiche Lösung ernsthaft zu Vorgehensweisen.

1. Führen Sie eine Risikoanalyse für jede Anwendung, da Anforderungen verfügen. Einige Programme sind wichtiger als andere und die zusätzlichen Kosten werden für die Wiederherstellung Architekten rechtfertigen würde.
1. Mithilfe dieser Informationen die RTO und RPO für jede Anwendung definieren.
1. Entwerfen Sie für Fehler, die Anwendungsarchitektur ab.
1. Implementieren der best Practices für hohe Verfügbarkeit gleichzeitig Kosten, Komplexität und Risiken.
1. Implementieren Sie Disaster Recovery-Pläne und Prozesse.
  * Sollten Sie Fehler, die Modulebene zu einem Ausfall vollständig Cloud umfassen.
  * Richten Sie Strategien für alle Verweis und Transaktionsdaten.
  * Wählen Sie eine Multi-Site-Disaster Recovery-Architektur.
1. Definieren eines bestimmten Besitzers für Disaster Recovery-Prozesse, Automatisierung und testen. Der Besitzer muss verwalten und den gesamten Prozess besitzen.
1. Dokumentieren Sie die Prozesse leicht wiederholbar sind. Zwar einen Besitzer, sollen mehrere Personen verstehen und die Prozesse im Notfall.
1. Das Zugpersonal den Prozess implementieren.
1. Verwenden Sie reguläre Disaster Simulationen für Schulung und Validierung des Prozesses.

##<a name="summary"></a>Zusammenfassung

Bei einem oder Hardware in Azure Ausfall unterscheiden sich die Techniken und Strategien für die Verwaltung tritt der Fehler auf lokalen Systemen. Der Hauptgrund dafür ist, dass Cloudlösungen normalerweise weitere Abhängigkeiten Infrastruktur, der Azure-Region verteilt und als separate Dienste verwaltet. Teilweise Fehlschläge mit hoher Verfügbarkeit müssen behandelt werden. Zum Verwalten von Schwerer Fehlers möglicherweise durch einen Katastrophenfall verwenden Sie Disaster Recovery-Strategien.

Azure erkennt und behandelt viele Fehler, aber es gibt viele Arten von Fehlern, die anwendungsspezifischen Strategien erfordern. Sie müssen aktiv vorbereiten und verwalten Fehler Programme, Dienste und Daten.

Berücksichtigen von Verfügbarkeit und Disaster Recovery-Plan erstellen, die Unternehmen folgen der Anwendung. Prozesse, Richtlinien und Prozeduren Systeme Wiederherstellen nach Katastrophen Zeit definieren, Planen und Engagement. Und nachdem Sie die Pläne haben, Sie nicht möglich. Sie müssen regelmäßig analysieren, testen und verbessern Pläne basierend auf Ihr Anwendungsportfolio, Unternehmen und die Technologie zur Verfügung. Azure bietet neue Funktionen und löst neue Aufgaben erstellen Sie stabile Anwendung, die Fehler standhalten.

##<a name="additional-resources"></a>Zusätzliche Ressourcen

[Hohe Verfügbarkeit bei Microsoft Azure](resiliency-high-availability-azure-applications.md)

[Disaster Recovery bei Microsoft Azure](resiliency-disaster-recovery-azure-applications.md)

[Technische Anleitung Azure Stabilität](resiliency-technical-guidance.md)

[Übersicht: Cloud Business Continuity und Datenbank-Wiederherstellung mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md)

[Hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[Failsafe: Hinweise sowie Cloud-Architekturen](https://channel9.msdn.com/Series/FailSafe)

[Empfohlene Vorgehensweisen für den Entwurf der umfangreiche Dienstleistungen Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe von Artikeln konzentriert, Disaster Recovery und hohe Verfügbarkeit für Azure Applications. Im nächste Artikel dieser Reihe ist eine [hohe Verfügbarkeit bei Microsoft Azure](resiliency-high-availability-azure-applications.md).
