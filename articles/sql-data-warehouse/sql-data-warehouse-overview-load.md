   <properties
   pageTitle="Laden von Daten in Azure SQL Data Warehouse | Microsoft Azure"
   description="Die Szenarien in SQL Data Warehouse Daten enthält. Dazu gehören PolyBase, Azure BLOB-Speicher, Flatfiles und Datenträger-Versand verwenden. Sie können auch Tools von Drittanbietern verwenden."
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
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Laden von Daten in Azure SQL Data Warehouse

Eine Zusammenfassung der Szenario-Optionen und Vorschläge zum Laden von Daten in SQL Data Warehouse.

Der schwierigste Teil beim Laden von Daten wird in der Regel Daten für die Last vorbereitet. Azure vereinfacht das Laden von Azure BLOB-Speicher als gemeinsamen Datenspeicher für viele Dienste, und Azure Data Factory auf Kommunikation und Datentransfer zwischen Azure Services koordinieren. Diese Prozesse sind PolyBase Technologie integriert die parallele Verarbeitung (MPP) verwendet, um die Daten parallel in SQL Data Warehouse von Azure BLOB-Speicher geladen. 

Lernprogramme, die Beispieldatenbanken Laden finden Sie [Beispieldatenbanken zu laden][].

## <a name="load-from-azure-blob-storage"></a>Laden von Azure BLOB-Speicher
Die schnellste Möglichkeit zum Importieren von Daten in SQL Data Warehouse ist mit PolyBase Daten aus Azure BLOB-Speicher geladen. PolyBase verwendet SQL Data Warehouse parallele Verarbeitung (MPP) Daten von Azure BLOB-Speicher geladen. Wenn Sie PolyBase verwenden, können Sie T-SQL-Befehle oder eine Azure Data Factory-Pipeline.

### <a name="1-use-polybase-and-t-sql"></a>1. verwenden Sie 1. PolyBase und T-SQL

Zusammenfassung der laden:

2. Formatieren Sie die Daten als UTF-8, da PolyBase derzeit UTF-16 nicht unterstützt.
2. Verschieben der Daten zum Azure BLOB-Speicher und in Textdateien gespeichert.
3. Konfigurieren von externe Objekte im SQL Data Warehouse den Speicherort und das Format der Daten definieren
4. Führen Sie einen T-SQL-Befehl Daten parallel in die neue Datenbank zu laden.

<!-- 5. Schedule and run a loading job. --> 

Eine Anleitung finden Sie unter [Daten Laden von Azure BLOB-Speicher in SQL Data Warehouse (PolyBase)][].

### <a name="2-use-azure-data-factory"></a>2. Azure Data Factory

Für eine einfachere Möglichkeit zum Verwenden von PolyBase können Sie eine Pipeline Azure Data Factory erstellen, die polybase zum Laden von Daten aus dem Azure BLOB-Speicher in SQL Data Warehouse verwendet. Dies ist schnell zu konfigurieren, da Sie die T-SQL-Objekte definieren müssen. Möchten Sie die externe Daten Abfragen, ohne ihn zu importieren, verwenden Sie T-SQL. 

Zusammenfassung der laden:

2. Formatieren Sie die Daten als UTF-8, da PolyBase derzeit UTF-16 nicht unterstützt.
2. Verschieben der Daten zum Azure BLOB-Speicher und in Textdateien gespeichert.
3. Erstellen einer Pipeline Azure Data Factory Daten aufnehmen. Verwenden Sie die Option PolyBase.
4. Planen und Ausführen der Pipeline.

Eine Anleitung finden Sie unter [Daten Laden von Azure BLOB-Speicher in SQL Data Warehouse (Azure Data Factory)][].


## <a name="load-from-sql-server"></a>Laden von SQL Server
Zum Laden von Daten aus SQL Server in SQL Data Warehouse können Sie Integration Services (SSIS), Flatfiles übertragen oder Schiff Datenträgern an Microsoft. Lesen Sie weiter, sehen Sie eine Zusammenfassung der verschiedenen laden Prozesse und Links zu Lernprogrammen.

Planen Sie eine Migration aller Daten aus SQL Server in SQL Data Warehouse [Migrationsübersicht][]anzeigen 

### <a name="use-integration-services-ssis"></a>Verwenden von Integrationsservices (SSIS)
Verwenden Sie bereits Integration Services (SSIS)-Pakete in SQL Server laden, können Sie Ihre Pakete als Quelle und SQL Data Warehouse SQL Server als Ziel verwenden aktualisieren. Dies ist schnell und einfach, und ist eine gute Wahl, wenn Sie nicht der Ladevorgang Verwendung Daten bereits in der Cloud migrieren möchten. Der Nachteil ist, dass die Last wird langsamer als PolyBase verwenden, da diese SSIS die Last nicht parallel ausgeführt.

Zusammenfassung der laden:

1. Überarbeiten des Integration Services-Pakets auf der SQL Server-Instanz für die Quelle und das Ziel SQL Data Warehouse-Datenbank.
2. Migrieren Sie Schema in SQL Data Warehouse, wenn sie nicht bereits angezeigt wird.
3. Ändern der Zuordnung der Pakete verwenden nur die Datentypen, die von SQL Data Warehouse unterstützt.
3. Planen Sie und führen Sie des Pakets aus.

Ein Lernprogramm finden Sie unter [Laden von Daten aus SQL Server zu Azure SQL Data Warehouse (SSIS)][].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Verwenden Sie AZCopy (< 10 TB Daten empfohlen)
Ist die Datengröße < 10 TB, können Sie die Daten aus SQL Server in Dateien exportieren Dateien Azure Blob-Speicher und PolyBase können die Daten in SQL Data Warehouse laden

Zusammenfassung der laden:

1. Verwenden Sie das Befehlszeilen-Dienstprogramm Bcp, um Daten aus SQL Server in Dateien exportieren.
2. Verwenden Sie das Befehlszeilenprogramm AZCopy Daten aus Flatfiles in Azure BLOB-Speicher kopiert.
3. Verwenden Sie PolyBase in SQL Data Warehouse laden.

Eine Anleitung finden Sie unter [Daten Laden von Azure BLOB-Speicher in SQL Data Warehouse (PolyBase)][].

### <a name="use-bcp"></a>Verwenden Sie bcp
Haben Sie eine kleine Datenmenge können Bcp Sie um direkt in Azure SQL Data Warehouse zu laden.

Zusammenfassung der laden:
1. Verwenden Sie das Befehlszeilen-Dienstprogramm Bcp, um Daten aus SQL Server in Dateien exportieren.
2. Verwenden Sie Bcp Daten aus Flatfiles direkt mit SQL Data Warehouse zu laden.

Eine Anleitung finden Sie unter [Laden von Daten aus SQL Server in Azure SQL Data Warehouse (Bcp)][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Exportieren Sie importieren / (> 10 TB Daten empfohlen)
Die Datengröße ist > 10 TB und Azure verschieben möchten, sollten Sie unser Versandservice [Import/Export][]verwenden. 

Zusammenfassung der Ladevorgang
2. Verwenden Sie das Befehlszeilen-Dienstprogramm Bcp Daten aus SQL Server in Dateien auf Datenträgern übertragbar exportieren.
3. Die Datenträger an Microsoft geliefert.
4. Microsoft lädt die Daten in SQL Data Warehouse

## <a name="load-from-hdinsight"></a>Laden von HDInsight
SQL Data Warehouse unterstützt laden Daten von HDInsight über PolyBase. Der Prozess ist der Ladevorgang von Azure BLOB-Speicher - PolyBase Verbindung mit HDInsight zum Laden von Daten mit identisch. 

### <a name="1-use-polybase-and-t-sql"></a>1. verwenden Sie 1. PolyBase und T-SQL

Zusammenfassung der laden:

2. Formatieren Sie die Daten als UTF-8, da PolyBase derzeit UTF-16 nicht unterstützt.
2. Verschieben der Daten zum HDInsight und speichern in Textdateien, ORK oder Parkett Format.
3. Konfigurieren Sie externe Objekte im SQL Data Warehouse definiert den Speicherort und das Format der Daten.
4. Führen Sie einen T-SQL-Befehl Daten parallel in die neue Datenbank zu laden.

Eine Anleitung finden Sie unter [Daten Laden von Azure BLOB-Speicher in SQL Data Warehouse (PolyBase)][].

## <a name="recommendations"></a>Empfehlung

Viele unserer Partner Solutions geladen haben. Mehr erfahren, sehen Sie eine Liste unserer [Solution Partner][]. 

Wenn Ihre Daten aus einer nicht-relationalen Datenquelle und in SQL Data Warehouse laden möchten müssen Sie in Zeilen und Spalten umwandeln, bevor Sie es laden. Die transformierten Daten müssen nicht in einer Datenbank gespeichert werden, in Textdateien gespeichert werden.

Erstellen Sie Statistiken neu geladene Daten. Azure SQL Data Warehouse ist noch Unterstützung automatisch erstellen oder auto Update Statistics.  Um die Leistung von Abfragen zu erhalten, ist es wichtig zum Erstellen von Statistiken für alle Spalten aller Tabellen nach dem ersten Laden oder wesentlichen Änderungen in den Daten.  Weitere Informationen finden Sie unter [Statistiken][].


## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->

<!--Article references-->
[Laden von Daten aus Azure BLOB-Speicher in SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Laden von Daten aus Azure BLOB-Speicher in SQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Laden von Daten aus SQL Server zu Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Laden von Daten aus SQL Server in Azure SQL Data Warehouse (Bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Laden Sie Beispieldatenbanken]: ./sql-data-warehouse-load-sample-databases.md
[Übersicht über die Migration]: ./sql-data-warehouse-overview-migrate.md
[Solution-Partnern]: ./sql-data-warehouse-partner-business-intelligence.md
[Übersicht über die Anwendungsentwicklung]: ./sql-data-warehouse-overview-develop.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
