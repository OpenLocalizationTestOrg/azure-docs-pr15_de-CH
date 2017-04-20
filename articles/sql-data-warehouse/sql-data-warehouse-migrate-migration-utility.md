<properties
   pageTitle="Migrieren Sie Data Warehouse-Migrationsprogramm | Microsoft Azure"
   description="Migrieren Sie zu SQL Datawarehouse."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Data Warehouse Migration Utility (Vorschau)

> [AZURE.SELECTOR]
- [Migration Utility Herunterladen][]

Data Warehouse Migration Utility ist ein Tool zur Migration von Schema und Daten von SQL Server und Azure SQL-Datenbank in Azure SQL Data Warehouse. Während der Migration Schema ordnet das Tool automatisch das entsprechende Schema aus der Quelle zum Ziel. Nach der Migration des Schemas Tools bietet die Möglichkeit, Daten mit automatisch generierten Skripts.

Neben Schema und Datenmigration gibt dieses Tool können Inkompatibilitäten zwischen den Ziel- und Instanzen zusammenfassen optimierte Migration verhindern würde Kompatibilitätsberichte generieren.

## <a name="get-started"></a>Erste Schritte
Als Voraussetzung für die Installation benötigen Sie BCP-Befehlszeilenprogramm Migrationsskripts und Office Compatibility-Bericht anzeigen. Nach dem Starten der ausführbaren Datei, die heruntergeladen werden Sie aufgefordert eine standard-EULA zustimmen, bevor das Tool installiert werden.

Zusätzlich zum Ausführen der Migration als Sie benötigen die folgenden Berechtigungen für die Datenbank, die Sie migrieren auf: CREATE DATABASE, ALTER ANY DATABASE oder VIEW ANY DEFINITION.

### <a name="launching-the-tool-and-connecting"></a>Starten des Tools und verbinden
Durch Klicken auf das Desktopsymbol erscheint nach der Installation starten. Beim Öffnen des Tools werden Sie mit einer ersten Seite aufgefordert, Quelle und Ziel für das Migrationsprogramm auswählen können. Zu diesem Zeitpunkt werden SQL Server und Azure SQL-Datenbank als Quellen und SQL Data Warehouse als Ziel unterstützen. Nach Auswahl dieser müssen Sie auf Ihrem Quellserver verbinden Servernamen ausfüllen und Authentifizierung und dann auf 'Verbinden'.

Nach der Authentifizierung, zeigt das Tool eine Liste der Datenbanken, die auf dem Server vorhanden sind, verbunden. Wählen Sie eine Datenbank, die Sie migrieren möchten, und klicken auf 'Migrieren ausgewählt' können mit die Migration beginnen.

## <a name="migration-report"></a>Migrationsbericht
Auswählen "Datenbankkompatibilität überprüfen" im Tool generieren einen Bericht in der Datenbank alle Objekt Inkompatibilitäten angeforderten migrieren. Umfassendere Liste der SQL Server-Funktionen, die nicht im SQL Data Warehouse finden in unseren [Migrationsdokumentation][]. Nachdem der Bericht generiert werden Sie speichern und den Bericht in Excel öffnen.

Beachten Sie, dass beim Generieren des Schemas Migration, die meisten Probleme 'Object' um unmittelbare Migration der Daten kann angepasst werden. Überprüfen Sie die Änderungen um sicherzustellen, dass nicht weitere Anpassungen vornehmen, bevor das Schema anwenden möchten.

## <a name="migrate-schema"></a>Migrieren von Schemas

Nach dem anschließen, "Schema migrieren" Auswählen einer Migration Schemaskript für die ausgewählten Tabellen generiert. Dieses Skript Ports die Struktur der Tabelle Karten inkompatiblen Daten Typen kompatibler Formulare und Anmeldeinformationen und Schema erstellt, wenn dies vom Benutzer in die Migration Settings. Dieser Code gegen gezielte SQL Data Warehouse-Instanz ausgeführt werden kann, in einer Datei gespeichert, in die Zwischenablage kopiert oder sogar vor weiteren Zeile bearbeitet.  

Wie bereits erwähnt, beim Migrieren von Schema Überprüfen der Migration ändert sich das Tool hat um sicherzustellen, dass Sie vollständig verstehen.  

## <a name="migrate-data"></a>Migrieren von Daten

Durch Klicken auf die Option "Daten migrieren" erzeugen Sie BCP-Skripts, die Daten zuerst in Dateien auf dem Server verschoben werden und dann direkt in SQL Data Warehouse. Es empfiehlt sich dabei verschieben Sie kleine Mengen von Daten und Versuche sind nicht integrierte Fehler können auftreten, wenn ein Verlust der Verbindung. Um dies auszuführen, müssen BCP-Befehlszeilenprogramm installiert haben und das Schema für die Daten bereits erstellt wurde.

Nachdem Sie die oben aufgeführten Parameter ausgefüllt haben, müssen Sie lediglich zur Migration klicken und zwei Pakete an den angegebenen Speicherort erstellt. Führen Sie Exportdatei aus, um Daten aus der Migrationsquelle in Dateien exportieren, und führen Sie die Datei importieren zum Importieren von Daten in SQL Data Warehouse.

## <a name="next-steps"></a>Nächste Schritte
Da einige Daten migriert haben, lesen Sie [entwickeln][].

<!--Image references-->

<!--Article references-->
[Migrationsdokumentation]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Migration Utility Herunterladen]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
