<properties
   pageTitle="Daten Sie SQL Data Warehouse mit Power BI Microsoft Azure"
   description="Daten Sie SQL Data Warehouse mit Power BI"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Daten Sie mit Power BI

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure maschinelles lernen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

In diesem Lernprogramm wird veranschaulicht, wie mit Power BI SQL Data Warehouse und einige grundlegende Visualisierung.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm durchgehen, müssen wie folgt vor:

- Ein SQL Data Warehouse AdventureWorksDW Datenbank vorab geladen. Um diese Bestimmung finden Sie unter [Erstellen einer SQL Data Warehouse][] und Laden Sie die Beispieldaten. Wenn Sie bereits ein Datawarehouse haben jedoch keinen Beispieldaten können Sie [Beispieldaten manuell laden][].


## <a name="1-connect-to-your-database"></a>1. Verbinden Sie mit der Datenbank

Power BI öffnen und Ihre AdventureWorksDW Datenbank:

1. [Azure-Portal][]anmelden.
2. Klicken Sie auf **SQL-Datenbanken** , und wählen Sie AdventureWorks SQL Data Warehouse-Datenbank.

    ![Suchen Sie Ihre Datenbank][1]

3. 'Power BI öffnen' klicken.

    ![BI-Netzschalter][2]

4. Die Webadresse Ihrer Datenbank mit SQL Data Warehouse Verbindungsseite sollte jetzt angezeigt werden. Klicken Sie auf Weiter.

    ![BI-Netzanschluss][3]

6. Geben Sie Ihren SQL Azure Server Benutzernamen und Kennwort wird vollständig mit SQL Data Warehouse-Datenbank verbunden werden.

    ![Power BI anmelden][4]

7. Klicken Sie in Power BI angemeldet haben, AdventureWorksDW Dataset auf der linken. Die Datenbank wird geöffnet.

    ![Power BI öffnen AdventureWorksDW][5]



## <a name="2-create-a-report"></a>2. Erstellen eines Berichts

Sie können jetzt mit Power BI die Beispieldaten AdventureWorksDW analysieren. Um die Analyse hat AdventureWorksDW Ansicht AggregateSales Diese Ansicht enthält einige der Schlüsselkriterien für die Analyse der Umsatz des Unternehmens.

1. Klicken Sie zum Erstellen einer Zuordnung Umsatz nach Postleitzahl, klicken Sie im rechten Bereich auf AggregateSales Ansicht erweitern. Klicken Sie auf der Postleitzahl und Verkaufsbetrag Spalten auswählen.

    ![Power BI auswählen AggregateSales][6]

    Power BI erkennt automatisch das geografische Daten und in einer Karte für Sie.

    ![BI-Karte][7]

2. Dieser Schritt erstellt ein Balkendiagramm, das pro Kunde Einnahmen zeigt. Hierzu gehen Sie zur erweiterten Ansicht AggregateSales erstellen. Klicken Sie auf Feld Verkaufsbetrag. Ziehen Sie das Feld Kunde Einnahmen links und Achse ablegen.

    ![Power BI Achse][8]

    Wir verschoben Balkendiagramm links.

    ![Power BI-Leiste][9]

3. Dieser Schritt erstellt ein Liniendiagramm mit Umsatz pro Bestellung. Hierzu gehen Sie zur erweiterten Ansicht AggregateSales erstellen. Klicken Sie auf Verkaufsbetrag und OrderDate. In der Visualisierung Spalte klicken Liniendiagramm; Dies ist das erste Symbol in der zweiten Zeile unter Visualisierung.

    ![Power BI select Liniendiagramm][10]

    Sie haben nun einen Bericht mit drei verschiedenen Visualisierung der Daten.

    ![BI Stromleitung][11]

Sie können Ihren Fortschritt jederzeit indem **Datei** und **Speichern**speichern.

## <a name="next-steps"></a>Nächste Schritte
Nun, wir einige Zeit mit den Beispieldaten Aufwärmen gegeben haben, wie Sie [entwickeln][], [Laden][]oder [Migrieren][]. Oder sehen Sie sich die [Power BI-Website][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[Migrieren]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md
[Laden]: sql-data-warehouse-overview-load.md
[Laden Sie Beispieldaten manuell]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Erstellen Sie ein SQL Datawarehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure-portal]: https://portal.azure.com/
[Power BI-website]: http://www.powerbi.com/
