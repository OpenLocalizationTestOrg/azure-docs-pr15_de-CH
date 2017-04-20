<properties
    pageTitle="Dehnen Datenbank Übersicht | Microsoft Azure"
    description="Erfahren Sie, wie Stretch Datenbank kalten Daten transparent und sicher in Microsoft Azure Cloud migriert."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Stretch-Datenbank (Übersicht)

Dehnen Datenbank migriert kalten Daten transparent und sicher in Microsoft Azure-Cloud.

Wenn Sie sofort mit Stretch Datenbank beginnen möchten, finden Sie [zunächst die Datenbank aktivieren Stretch-Assistent ausgeführt](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Was sind die Vorteile der Stretch-Datenbank?
Stretch-Datenbank bietet die folgenden Vorteile:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Kosten\-effektive Verfügbarkeit kalte Daten
Warme und kalte Transaktionsdaten dynamisch von SQL Server, Microsoft Azure mit SQL Server Stretch Datenbank gestreckt. Im Gegensatz zu normalen kalt Datenspeicher sind Ihre Daten immer online und für Abfragen verfügbar. Mehr Daten Aufbewahrung Zeitachsen bieten ohne für große Tabellen wie Kunden bestellen. Profitieren von niedrigen Kosten von Azure als Skalierung auf teure\-lokale Speicher. Sie wählen den Tarif und Einstellungen in der Azure-Verwaltungsportal Kontrolle über Kosten. Bedarf nach oben oder unten skaliert werden soll. Seite [SQL Server Stretch Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) Details.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Keine Änderungen so Abfragen
Zugriff auf die SQL Server-Daten nahtlos unabhängig davon, ob es auf\-Räume oder in die Cloud gestreckt.  Die Richtlinie, die bestimmt, wo Daten und SQL Server behandelt den Datentransfer im Hintergrund festlegen. Die gesamte Tabelle ist immer online und abgefragt werden. Und Strecken Datenbank nicht Änderungen Abfragen oder Clientanwendungen benötigen – der Speicherort der Daten für die Anwendung transparent ist.

### <a name="streamlines-on-premises-data-maintenance"></a>Optimiert für\-lokale Daten
Reduzieren\-lokale Verwaltung und Speicherung Ihrer Daten. Backups für Ihre auf\-lokale Daten schneller und innerhalb des Wartungsfensters abgeschlossen. Backups für Cloud Teil Ihrer Daten automatisch ausgeführt. Die auf\-lokale Speicherbedarf erheblich reduziert. Azure-Speicher kann 80 % günstiger als hinzufügen auf\-lokale SSD.

### <a name="keeps-your-data-secure-even-during-migration"></a>Sichert Ihre Daten sogar während der migration
Genießen Sie beruhigt, wie Sie Ihre wichtigsten Applikationen sicher in der Cloud. SQL Server verschlüsselt ermöglicht die Verschlüsselung der Daten. Zeile Level Security (RLS) und andere erweiterten Sicherheitsfunktionen von SQL Server arbeiten mit Stretch-Datenbank zum Schutz Ihrer Daten.

## <a name="what-does-stretch-database-do"></a>Was Stretch-Datenbank?
Nach der Aktivierung Stretch-Datenbank eine SQL Server-Instanz, einer Datenbank und mindestens eine Tabelle beginnt Stretch-Datenbank automatisch kalten Daten in Azure migrieren.

-   Wenn Sie in einer separaten Tabelle kalte Daten speichern, können Sie die gesamte Tabelle migrieren.

-   Wenn Ihre Tabelle und Daten enthält, soll eine Filterfunktion, markieren Sie die Zeilen migrieren.

**Sie müssen vorhandene Abfragen und Clientanwendungen zu ändern.** Sie weiterhin nahtlosen Zugriff auf lokale und remote-Daten auch bei der Datenmigration. Es ist wenig Wartezeit für Remoteabfragen nur treten diese Wartezeit kalten Daten abzufragen.

Wenn während der Migration ein Fehler auftritt, **Stretch-Datenbank wird sichergestellt, dass keine Daten verloren** . Es hat auch Wiederholungslogik Verbindungsprobleme behandeln, die während der Migration auftreten. Eine dynamische Ansicht zeigt den Status der Migration.

**Datenmigration anhalten** von Problemen auf dem lokalen Server oder die verfügbare Netzwerkbandbreite zu maximieren.

![Stretch-Datenbank (Übersicht)][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Ist Stretch-Datenbank?
Bei folgenden Aussagen kann Stretch Datenbank zu Ihren Anforderungen Problemen helfen.

|Wenn ein Entscheidungsträger sind|Wenn Sie ein DBA sind|
|------------------------------|-------------------|
|Ich habe Transaktionsdaten lange beibehalten.|Die Größe von Tabellen immer außer Kontrolle.|
|Manchmal habe ich kalten Daten abzufragen.|Benutzer sagen, dass sie Zugriff auf kalte Daten, aber sie nur selten verwenden.|
|Ich habe Applikationen ältere apps nicht aktualisieren möchten.|Ich habe kaufen und mehr Speicher hinzufügen.|
|Eine Möglichkeit zum Speicher sparen möchte.|Sichern kann nicht oder Wiederherstellen dieser großen Tabellen in der Vereinbarung zum SERVICELEVEL.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Welche Datenbanken und Tabellen sind Kandidaten für die Stretch-Datenbank?
Erweitern Sie Datenbank Ziele Transaktionsdatenbanken mit großen Datenmengen kalte Daten, normalerweise eine kleine Anzahl von Tabellen. Diese Tabellen enthalten mehr als eine Milliarde Zeilen.

Verwenden Sie das temporäre Tabellenfeature SQL Server 2016 Stretch Datenbank migrieren oder einen Teil der zugeordneten Tabelle Kosten verwenden\-effizientes in Azure. Weitere Informationen finden Sie unter [Verwalten Aufbewahrung von Verlaufsdaten im System Versionsangabe temporäre Tabellen](https://msdn.microsoft.com/library/mt637341.aspx).

Verwenden Sie Stretch Datenbank Berater ein Feature von SQL Server 2016 Upgrade Advisor zum Identifizieren von Datenbanken und Tabellen Stretch-Datenbank. Weitere Informationen finden Sie unter [identifizieren Datenbanken und Tabellen für die Stretch-Datenbank](sql-server-stretch-database-identify-databases.md). Um weitere Informationen zu blockierende Probleme anzeigen Sie [Grenzen für Strecken Datenbank](sql-server-stretch-database-limitations.md)

## <a name="test-drive-stretch-database"></a>Testversion verlängern Datenbank
**Testen Sie Stretch-Datenbank mit der Beispieldatenbank AdventureWorks.** Zum Abrufen der AdventureWorks-Beispieldatenbank herunterladen Sie mindestens die Datenbankdatei und die Beispiele und Skripts [hier](https://www.microsoft.com/download/details.aspx?id=49502) Nach dem Wiederherstellen der Datenbank in eine Instanz von SQL Server 2016 Entpacken Sie Samples Datei und öffnen Sie die Datei Beispiele für DB-Strecken Strecken Datenbankordner. Führen Sie die Skripts in dieser Datei überprüfen Sie den Speicherplatz durch die Daten vor und nach der Aktivierung Stretch Datenbank Datenmigration zu verfolgen und zu bestätigen, dass Sie vorhandene Daten Abfragen und neue Daten sowohl während als auch weiterhin nach Datenmigration.

## <a name="next-step"></a>Nächstes
**Identifizieren Sie Datenbanken und Tabellen für die Stretch-Datenbank geeignet sind.** Herunterladen Sie SQL Server 2016 Upgrade Advisor und führen Sie der Stretch Datenbank Ratgeber, um Datenbanken und Tabellen für die Stretch-Datenbank geeignet sind aus. Dehnen Datenbank Advisor identifiziert auch blockierenden Probleme. Weitere Informationen finden Sie unter [identifizieren Datenbanken und Tabellen für die Stretch-Datenbank](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
