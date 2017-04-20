<properties
    pageTitle="Identifizieren von Datenbanken und Tabellen Stretch Datenbank mit Stretch Datenbank Advisor | Microsoft Azure"
    description="Informationen Sie zu Datenbanken und Tabellen für die Stretch-Datenbank geeignet sind."
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
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Identifizieren von Datenbanken und Tabellen Stretch Datenbank mit Stretch Datenbank Advisor

Ermittlung von Datenbanken und Tabellen, die Kandidaten für die Stretch-Datenbank SQL Server 2016 Upgrade Advisor herunterladen und Strecken Datenbank Advisor ausführen. Dehnen Datenbank Advisor identifiziert auch blockierenden Probleme.

## <a name="download-and-install-upgrade-advisor"></a>Herunterladen und Installieren von Upgrade Advisor
Downloaden und Installieren von Upgrade Advisor [hier](http://go.microsoft.com/fwlink/?LinkID=613421). Dieses Tool ist nicht auf den SQL Server enthalten.

## <a name="run-the-stretch-database-advisor"></a>Dehnen Datenbank Advisor ausführen

1.  Ausführen des Updateratgebers

2.  Wählen Sie **Szenarien**und dann **DEHNEN Datenbank ADVISOR ausführen**.

3.  Blade **Stretch Datenbank Advisor ausführen** klicken Sie auf **Wählen Sie Datenbanken zu ANALYSIEREN**.

4.  Geben Sie auf die **Datenbanken auswählen** ein oder wählen Sie den Servernamen und den Authentifizierungsinformationen aus. Klicken Sie auf **Verbinden**.

5.  Eine Liste der Datenbanken auf dem ausgewählten Server wird angezeigt. Wählen Sie die Datenbanken, die Sie analysieren möchten. Klicken Sie auf **auswählen**.

6.  Blade **Stretch Datenbank Advisor ausführen** klicken Sie auf **Ausführen**.  Analyse wird ausgeführt.

## <a name="review-the-results"></a>Überprüfen Sie die Ergebnisse

1.  Nach Abschluss der Analyse auf die **Datenbanken analysiert** , wählen Sie eine der Datenbanken analysiert, um das Blade **Ergebnisse** anzeigen.

    **Analyseergebnisse** Blade Tabellen empfohlen in der ausgewählten Datenbank, die die Empfehlung Standardkriterien entsprechen.

2.  In der Liste der Tabellen auf die **Analyseergebnisse** Auswählen eines der empfohlenen Blade **Tabellenergebnisse** anzeigen

    Wenn es Probleme blockieren, listet **Tabellenergebnisse** Blade blockierende Probleme für die ausgewählte Tabelle. Informationen Blockierungsprobleme Stretch Datenbank Advisor erkannt anzeigen Sie [Grenzen für Strecken Datenbank](sql-server-stretch-database-limitations.md)

3.  Wählen Sie in der blockierenden Probleme auf der **Tabellenergebnisse** eines der Themen mehr über das ausgewählte Thema angezeigt und schlägt Maßnahmen. Implementieren Sie die vorgeschlagenen Maßnahmen, möchten Sie die ausgewählte Tabelle Stretch-Datenbank konfigurieren.

## <a name="next-step"></a>Nächstes
Aktivieren Sie Dehnen Datenbank.

-   Stretch-Datenbank in einer **Datenbank**finden Sie [Strecken Datenbank für eine Datenbank zu aktivieren](sql-server-stretch-database-enable-database.md).

-   Stretch-Datenbank in einer anderen **Tabelle**Strecken bereits in der Datenbank aktiviert finden Sie [Stretch Datenbank für eine Tabelle aktivieren](sql-server-stretch-database-enable-table.md).

## <a name="see-also"></a>Siehe auch

[Grenzen für Stretch-Datenbank](sql-server-stretch-database-limitations.md)

[Stretch-Datenbank für eine Datenbank aktivieren](sql-server-stretch-database-enable-database.md)

[Stretch-Datenbank für eine Tabelle aktivieren](sql-server-stretch-database-enable-table.md)

[Alle Themen für Azure SQL Server Stretch-Datenbankdienst](sql-server-stretch-database-index-all-articles.md)
