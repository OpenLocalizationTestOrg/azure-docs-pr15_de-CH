<properties 
   pageTitle="Kopieren von Daten zwischen See Datenspeicher und Azure SQL-Datenbank mit Sqoop | Microsoft Azure"
   description="Verwenden Sie Sqoop Kopieren von Daten zwischen Azure SQL-Datenbank und See Datenspeicher" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Kopieren von Daten zwischen See Datenspeicher und Azure SQL-Datenbank mit Sqoop

Informationen Sie zur Verwendung von Apache Sqoop importieren und Exportieren von Daten zwischen Azure SQL-Datenbank und See Datenspeicher.
 

## <a name="what-is-sqoop"></a>Was ist Sqoop?

Große Daten sind eine gute Wahl für die Verarbeitung von unstrukturierter und semistrukturierten Daten, wie Protokolle und Dateien. Es kann auch jedoch muss strukturierte Daten verarbeiten, die in relationalen Datenbanken gespeichert sind.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) ist ein Tool zum Übertragen von Daten zwischen relationalen Datenbanken und eine große Daten-Repository wie See Datenspeicher. Sie können Datenspeicher See Daten aus einer relationalen Datenbank-Managementsystem (RDBMS) wie Azure SQL-Datenbank importieren. Sie können transformieren und Datenanalyse mit großen Daten-Arbeitslasten und exportieren Sie die Daten in einem RDBMS. In diesem Lernprogramm verwenden Sie eine SQL Azure-Datenbank als relationale Datenbank importieren/exportieren aus.
 

## <a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).
- Für Datenspeicher See public Preview **Azure Abonnement aktivieren** . [Siehe.](data-lake-store-get-started-portal.md#signup) 
- **Azure HDInsight Cluster** Zugriff auf ein Konto See Datenspeicher. Siehe [Erstellen eines Clusters HDInsight mit dem Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md). Es wird vorausgesetzt, dass ein HDInsight Linux Cluster See Datenspeicher Zugriff haben.
- **Azure SQL-Datenbank**. Informationen zu erstellen, finden Sie unter [Erstellen einer Azure SQL-Datenbank](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Lernen Sie mit Videos?

[In diesem Video](https://mix.office.com/watch/1butcdjxmu114) zum Kopieren von Daten zwischen Azure Storage Blobs und Datenspeicher Lake DistCp.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Erstellen von Tabellen in Azure SQL-Datenbank

1. Erstellen Sie zwei Tabellen, in Azure SQL-Datenbank. Verwenden Sie [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) oder Visual Studio Azure SQL-Datenbank und führen Sie die folgenden Abfragen.

    **Erstellen von Tabelle1**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Tabelle2 erstellen**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. Fügen Sie in **Tabelle1**Beispieldaten hinzu. **Tabelle2** leer. Wir werden Daten aus **Tabelle1** in Lake Datenspeicher importieren. Anschließend werden wir Daten See Datenspeicher in **Tabelle2**exportieren. Führen Sie den folgenden Codeausschnitt.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Mit der Sqoop von einem HDInsight-Cluster mit See Datenspeicher

Ein HDInsight-Cluster bereits Sqoop Pakete. Wenn den HDInsight Cluster-See Datenspeicher als zusätzlichen Speicher verwenden konfiguriert haben, können Sie Sqoop (ohne Konfiguration) Importieren/Exportieren von Daten zwischen einer relationalen Datenbank (in diesem Beispiel Azure SQL-Datenbank) und einen See Datenspeicher. 

1. Für dieses Lernprogramm wird davon ausgegangen, dass Linux-Cluster erstellt, sodass SSH Verbindung zum Cluster verwendet werden soll. [Mit einem Linux-basierten HDInsight Cluster](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)anzeigen

2. Überprüfen Sie, ob Cluster See Datenspeicher-Konto zugreifen können. Führen Sie den folgenden Befehl aus der SSH:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Eine Liste der Dateien und Ordner im Datenspeicher See Konto beinhalten.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Datenspeicher See importieren Sie Daten von Azure SQL-Datenbank

3. Navigieren Sie zu dem Verzeichnis, in dem Sqoop-Pakete verfügbar sind. Normalerweise werden unter `/usr/hdp/<version>/sqoop/bin`. 

4. Importieren Sie die Daten aus **Tabelle1** See Datenspeicher berücksichtigt. Verwenden Sie die folgende Syntax:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Hinweis Diese **Sql-Datenbank-Server-Name** den Namen des Servers steht, auf dem SQL Azure-Datenbank läuft. **SQL-Datenbankname** steht der tatsächliche Datenbankname.

    Zum Beispiel

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Überprüfen Sie, ob die Daten auf See Datenspeicher Konto übertragen wurde. Führen Sie den folgenden Befehl ein:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Die folgende Ausgabe sollte angezeigt werden.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Jede **Teil-m -** * entspricht einer Zeile in der Quelltabelle * *Tabelle1**. Zeigen Sie den Inhalt des Webparts-m -* Dateien.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Exportieren von Daten aus See Datenspeicher in Azure SQL-Datenbank

6. Exportieren der Daten vom Datenspeicher See Konto leere Tabelle **Tabelle2**in Azure SQL-Datenbank. Verwenden Sie die folgende Syntax.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Zum Beispiel

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Stellen Sie sicher, dass die Daten in die SQL-Datenbanktabelle hochgeladen wurde. Verwenden Sie [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) oder Visual Studio Azure SQL-Datenbank und führen die folgende Abfrage.

        
        SELECT * FROM TABLE2

    Die folgende Ausgabe sollte sein.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Siehe auch

- [Daten von Azure Storage Blobs See Datenspeicher](data-lake-store-copy-data-azure-storage-blob.md)
- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
