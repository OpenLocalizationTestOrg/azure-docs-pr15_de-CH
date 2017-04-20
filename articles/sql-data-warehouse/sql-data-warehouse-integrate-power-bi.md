<properties
   pageTitle="Verwenden von Power BI mit SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Power BI Azure SQL Data Warehouse Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-power-bi-with-sql-data-warehouse"></a>Verwenden von Power BI mit SQL Datawarehouse
Als Anwender mit SQL Azure SQL Data Warehouse Direct Connect leistungsfähige logische Pushdown neben den Analysefunktionen von Power BI nutzen.  Mit Direct Connect Abfragen auf SQL Azure Data Warehouse in Echtzeit erhalten die Daten unterstützen.  Zusammen mit der SQL Data Warehouse können Benutzer dynamische Berichte in Minuten Terabyte Daten erstellen.  Darüber hinaus ermöglicht die Einführung in Power BI-Schaltfläche Öffnen direkt Power BI ihre SQL Data Warehouse verbinden, ohne Informationen aus anderen Teilen von Azure.

Verwendung von Direct Connect Bitte beachten:

+ Geben Sie vollständig qualifizierte Verbindung (siehe unten für Weitere Informationen)
+ Sicherstellen Sie, dass die Firewall-Regeln für die Datenbank "Zugriff auf Azure Services" konfiguriert sind.
+ Jede Aktion oder Auswählen einer Spalteninhalts einen Filter hinzufügen wird das Datawarehouse direkt Abfragen.
+ Kacheln aktualisiert alle 15 Minuten (Aktualisierung muss nicht geplant werden)
+ Q & A ist nicht verfügbar für direkte Verbindung von datasets
+ Geändert werden nicht automatisch übernommen
+ Alle direkte Verbindung Abfragen werden nach 2 Minuten

Diese Einschränkungen und Hinweise können weiterhin Erfahrungen zu ändern. Die Schritte für die Verbindung werden nachfolgend beschrieben.  

## <a name="using-the-open-in-power-bi-button"></a>Mithilfe der Schaltfläche 'Power BI öffnen'
Die einfachste Methode zum Verschieben zwischen SQL Data Warehouse und Power BI ist im Power BI-Schaltfläche geöffnet. Diese Schaltfläche ermöglicht die nahtlose erstellen neue Dashboards in Power BI.  

1.  Einführung an die Instanz SQL Data Warehouse in Azure-Verwaltungsportal zu navigieren.
2.  'Power BI öffnen' klicken.
3.  Wenn wir nicht direkt anmelden oder nicht Power BI-Konto verfügen, müssen Sie sich anmelden.  
4.  Sie gelangen zur Seite Verbindung SQL Data Warehouse mit Daten aus SQL Data Warehouse gefüllt.
5.  Nachdem Sie Ihre Anmeldeinformationen eingegeben werden Sie vollständig mit SQL Data Warehouse verbunden werden.

## <a name="connecting-through-the-power-bi-portal"></a>Verbinden über das Portal Power BI
Zusätzlich in Power BI-Schaltfläche öffnen, können Benutzer auch ihre SQL Data Warehouse Power BI-Portal.

1.  Klicken Sie auf "Daten abrufen" am unteren Rand des Navigationsbereichs.
2.  Wählen Sie 'Datenbanken'.
3.  Einmal auf der Seite Wählen Sie "Azure SQL Data Warehouse", und klicken Sie dann auf 'Verbinden'.
4.  Geben Sie die erforderlichen Verbindungsinformationen.  Die Servernamen und Datenbanknamen finden im Azure-Portal.
5.  Sie gelangen auf die Hauptseite des Power BI und nach dem herstellen die Verbindung einen neuen Eintrag unter 'Datasets' mit dem Namen der Instanz angezeigt.  
6.   Sie können auf das neue Dataset aller Tabellen und Sichten in der Datenbank zu klicken. Auswählen einer Spalteninhalts sendet eine Abfrage an Quelle, dynamisch Ihre Visual. Diese Bilder können in einem neuen Bericht gespeichert und wieder in Ihr Dashboard fixiert.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
