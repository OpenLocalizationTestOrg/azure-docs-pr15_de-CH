<properties
   pageTitle="PolyBase im SQL Data Warehouse Tutorial | Microsoft Azure"
   description="Erfahren Sie, was PolyBase ist und für Datawarehousing-Szenarien verwenden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Laden von Daten mit SQL Data Warehouse PolyBase

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)

Dieses Lernprogramm zeigt Informationen zum Laden von Daten in SQL Data Warehouse mit AzCopy und PolyBase. Abschließend sehen wie:

- Verwenden Sie AzCopy, Daten in Azure BLOB-Speicher kopieren
- Erstellen von Datenbankobjekten Daten definieren
- Führen Sie eine T-SQL-Abfrage zum Laden der Daten

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm durchgehen, müssen Sie

- Eine SQL Data Warehouse-Datenbank.
- Ein Azure-Speicherkonto vom Typ Standard lokal redundanter Speicher (Standard LRS) Standard Geo redundante Speicher (Standard-GRS) oder Lesezugriff Geo redundante Standardspeicher (Standard-RAGRS).
- AzCopy Befehlszeilenprogramm. Downloaden Sie und installieren Sie die [neueste Version von AzCopy][] mit Microsoft Azure Storage-Tools installiert ist.

    ![Azure-Speicher-Tools](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Schritt 1: Hinzufügen von Beispieldaten Azure Blob-Speicher

Um Daten zu laden, müssen wir einige Beispieldaten in Azure Blob-Speicher. In diesem Schritt füllen wir einen Blob Azure Storage mit Beispieldaten. PolyBase nutzen wir später Beispieldaten in SQL Data Warehouse-Datenbank laden.

### <a name="a-prepare-a-sample-text-file"></a>A. Bereiten Sie eine Beispieltextdatei

Eine Beispieltextdatei vorbereiten:

1. Öffnen Sie Notepad und kopieren Sie die folgenden Zeilen der Daten in eine neue Datei. Speichern Sie diese in das lokale temporäre Verzeichnis als % temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Den Blob-Endpunkt gefunden

Den Blob-Endpunkt zu finden:

1. Wählen Sie aus dem Azure-Portal **Durchsuchen** > **Speicherkonten**.
2. Klicken Sie auf das Speicherkonto, das Sie verwenden möchten.
3. Klicken Sie im Blatt Konto Speicher auf Blobs

    ![Klicken Sie auf Blobs](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. Speichern Sie der BLOB-Dienst-Endpunkt-URL für später.

    ![BLOB-Endpunkt](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Der Azure-Speicher Schlüssel

Zu den Azure-Speicher-Schlüssel:

1. Wählen Sie das Azure-Portal **Durchsuchen** > **Speicherkonten**.
2. Klicken Sie auf das Speicherkonto, das Sie verwenden möchten.
3. Wählen Sie **Alle** > **Zugriffstasten**.
4. Aktivieren Sie das Kopie eines der Zugriffstasten in die Zwischenablage kopieren.

    ![Azure-Speicherschlüssel kopieren](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D. Kopieren Sie die Beispieldatei in Azure BLOB-Speicher

So kopieren Daten in Azure BLOB-Speicher:

1. Öffnen Sie ein Eingabeaufforderungsfenster, und wechseln Sie zum Installationsverzeichnis von AzCopy. Dieser Befehl ändert das Standardverzeichnis auf einem 64-Bit-Windows-Client.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Führen Sie den folgenden Befehl die Datei hochzuladen. Geben Sie die BLOB-Dienst Endpunkt-URL für <blob service endpoint URL> und der Azure-Speicher Konto für < Azure_storage_account_key >.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Siehe auch [AzCopy-Befehlszeilen-Dienstprogramm starten][].

### <a name="e-explore-your-blob-storage-container"></a>E. Untersuchen der Blob-Behälter

Um die Datei hochgeladen Sie Blob-Speicher:

1. Um die BLOB-Blatt zurückzukehren.
2. Doppelklicken Sie unter Container **Datencontainers**.
3. Zu den Pfad zu den Daten, klicken Sie auf den Ordner **Datedimension** , und Sie sehen die übertragene Datei **DimDate2.txt**.
4. Um Eigenschaften anzuzeigen, klicken Sie auf **DimDate2.txt**.
5. Beachten Sie, dass BLOB-Eigenschaften Blatt können Sie herunterladen oder löschen Sie die Datei.

    ![Ansicht Azure-Speicher-blob](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Schritt 2: Erstellen einer externen Tabelle für die Beispieldaten

In diesem Abschnitt erstellen Sie eine externe Tabelle, die die Beispieldaten definiert.

PolyBase verwendet externe Tabellen Zugriff auf Daten in Azure BLOB-Speicher. Da die Daten nicht im SQL Data Warehouse gespeichert, verarbeitet PolyBase Authentifizierung für externen Daten unter Verwendung von Anmeldeinformationen-Datenbank beschränkt.

Das Beispiel in diesem Schritt verwendet diese Transact-SQL-Anweisung zum Erstellen einer externen Tabelle.

- [Hauptschlüssel erstellen (Transact-SQL)][] das Geheimnis der Datenbank verschlüsseln beschränkt Anmeldeinformationen.
- [Erstellen Datenbank begrenzt Anmeldeinformationen (Transact-SQL)][] Authentifizierungsinformationen für Ihr Konto Azure-Speicher an.
- [Externe Datenquelle erstellen (Transact-SQL)][] , geben Sie den Speicherort der Azure BLOB-Speicher.
- [Erstellen externer Dateiformat (Transact-SQL)][] , das Format der Daten.
- [Erstellen Sie externe Tabelle (Transact-SQL)][] die Tabellendefinition und die Position der Daten angeben.

Abfrage mit SQL Data Warehouse-Datenbank. Erstellt eine externe Tabelle mit dem Namen DimDate2External im Dbo-Schema, das die DimDate2.txt Probe Daten in Azure BLOB-Speicher verweist.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


In SQL Server Objekt-Explorer in Visual Studio sehen Sie externes Dateiformat, externe Datenquelle und die DimDate2External-Tabelle.

![Externe Tabelle anzeigen](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Schritt 3: Laden von Daten in SQL Data Warehouse

Nach dem Erstellen der externen Tabelle laden die Daten in eine neue Tabelle oder einer vorhandenen Tabelle einfügen.

- Führen Sie die Anweisung [CREATE TABLE AS SELECT (Transact-SQL)][] zum Laden der Daten in eine neue Tabelle. Die neue Tabelle müssen die Spalten in der Abfrage. Die Datentypen der Spalten entspricht die Datentypen der externen Tabellendefinition.
- Verwenden Sie zum Laden von Daten in eine vorhandene Tabelle [Einfügen... SELECT (Transact-SQL)][] Anweisung.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Schritt 4: Erstellen Sie Statistiken neu geladene Daten

SQL Data Warehouse nicht automatisch erstellen oder automatisch aktualisierte Statistiken. Daher unbedingt um hohe Leistung zu erreichen, zum Erstellen von Statistiken für die einzelnen Spalten jeder Tabelle nach dem ersten Laden. Außerdem ist es wichtig, Statistiken nach wesentlichen Änderungen in den Daten aktualisieren.

Dieses Beispiel erstellt einzelne Spaltenstatistiken für die neue DimDate2-Tabelle.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

[Weitere Statistiken.][]  


## <a name="next-steps"></a>Nächste Schritte
Finden Sie im [Handbuch PolyBase][] Informationen sollten Sie wissen, wie Sie eine Lösung entwickeln, die polybase verwendet.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[PolyBase-Handbuch]: ./sql-data-warehouse-load-polybase-guide.md
[Erste Schritte mit dem Befehlszeilenprogramm AzCopy]: ../storage/storage-use-azcopy.md
[neueste Version von AzCopy]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[Erstellen Sie externe Datenquelle (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[Erstellen von EXTERNEN DATEIFORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[Erstellen Sie externe Tabelle (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[Erstellen der Tabelle AS SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[EINFÜGEN... SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[Erstellen Sie HAUPTSCHLÜSSEL (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[Erstellen Sie Datenbank ausgelegte Anmeldeinformationen (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
