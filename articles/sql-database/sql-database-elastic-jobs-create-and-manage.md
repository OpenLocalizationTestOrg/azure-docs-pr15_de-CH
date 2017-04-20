<properties
    pageTitle="Erstellen und Verwalten von Azure SQL-Datenbanken mit elastische skalierten | Microsoft Azure"
    description="Erstellung und Verwaltung eines Auftrags elastische Datenbank durchgehen."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Erstellen und Verwalten von skalierten Azure SQL-Datenbanken mit elastische (Vorschau)

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)


**Elastische Datenbank Aufträge** vereinfachen die Verwaltung von Gruppen von Datenbanken durch Verwaltungsvorgänge oder geändert, Verwaltung von Anmeldeinformationen, Verweis Datenupdates, Leistungsdaten Mieter (Kunde) Telemetrie Auflistung ausführen. Aufträge für die elastische Datenbank ist derzeit durch den Azure-Portal und PowerShell-Cmdlets. Jedoch verringert Azure Portal Flächen Funktionalität zur Ausführung in allen Datenbanken in einem [Pool für elastische Datenbanken (Vorschau)](sql-database-elastic-pool.md)beschränkt. Zugriff auf zusätzliche Features und Ausführung von Skripts über eine Gruppe von Datenbanken, einschließlich einer benutzerdefinierten Auflistung oder einen Splitter-Satz (erstellt mit [elastische Datenbank-Clientbibliothek](sql-database-elastic-scale-introduction.md)) finden Sie unter [Erstellen und Verwalten von Aufträgen mithilfe von PowerShell](sql-database-elastic-jobs-powershell.md). Weitere Informationen zu Aufträgen finden Sie unter [elastische Datenbank Aufträge (Übersicht)](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Abonnement. Eine kostenlose Testversion finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/).
* Ein Pool für elastische Datenbanken. Finden Sie unter [über elastische datenbankpools](sql-database-elastic-pool.md)
* Installation der elastische Job Service Datenbankkomponenten. Siehe [Auftragsdienst elastische Datenbank installieren](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Erstellen von Aufträgen

1. [Azure-Portal](https://portal.azure.com)aus einem vorhandenen elastische Datenbank Job Pool klicken Sie **Auftrag erstellen**.
2. Geben Sie den Benutzernamen und das Kennwort des Datenbankadministrators (erstellt bei der Installation von Aufträgen) für die Aufträge Datenbank (Metadaten Speicher für Aufträge).

    ![Benennen Sie Auftrag, geben Sie oder fügen Sie Code und klicken Sie auf Ausführen][1]
2. Geben Sie einen Namen für den Auftrag Blatt **Auftrag erstellen** .
3. Geben Sie den Benutzernamen und das Kennwort für die Zieldatenbank mit ausreichenden Berechtigungen für die Ausführung von Skripten erfolgreich.
4. Fügen Sie oder geben Sie im T-SQL-Skript.
5. Klicken Sie auf **Speichern** und dann auf **Ausführen**.

    ![Aufträge erstellen und ausführen][5]

## <a name="run-idempotent-jobs"></a>Idempotente Aufträge ausführen

Beim Ausführen eines Skripts für mehrere Datenbanken müssen Sie sicher sein, dass das Skript Idempotent ist. Also muss das Skript mehrmals ausführen können auch wenn konnte nicht in einem unvollständigen Zustand vor. Beispielsweise fällt ein Skript der Auftrag automatisch wiederholt wird, bis sie erfolgreich ist (Rahmen, wie der Wiederholung Logik schließlich nicht mehr die Wiederholung). Die Möglichkeit ist die Verwendung der eine "IF EXISTS"-Klausel und jede gefundene Instanz löschen, bevor ein neues Objekt erstellen. Ein Beispiel ist hier dargestellt:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Vor dem Erstellen einer neuen Instanz verwenden Sie eine "IF NOT EXISTS"-Klausel auch:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Dieses Skript aktualisiert dann die zuvor erstellte Tabelle.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Überprüfen des Auftragsstatus

Nachdem ein Auftrag begonnen hat, können Sie auf den Status überprüfen.

1. Klicken Sie auf Poolauslagerungsseite elastische Datenbank **Aufträge verwalten**.

    ![Klicken Sie auf "Aufträge verwalten"][2]

2. Klicken Sie auf (a) eines Auftrags. Der **STATUS** kann "Abgeschlossen" oder "Fehlgeschlagen". Die Auftragsdetails angezeigt (b) mit Datum und Uhrzeit der Erstellung und Ausführung. Die Liste (c) das zeigt den Fortschritt des Skripts für jede Datenbank im Pool der Datums- und Angaben.

    ![Einen abgeschlossen Auftrag überprüfen][3]


## <a name="checking-failed-jobs"></a>Überprüfung fehlgeschlagen Aufträge

Wenn ein Auftrag fehlschlägt, kann ein Protokoll der Ausführung gefunden. Um die Details der fehlerhafte Auftrag klicken.

![Überprüfen eines Auftragsfehlers][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
