<properties
    pageTitle="Verschieben von Daten aus einer lokalen SQL Server, SQL Azure Azure Data Factory | Azure"
    description="Richten Sie eine ADF-Pipeline, die zwei Aktivitäten für die Migration von Daten besteht, die Daten täglich zwischen Datenbanken vor Ort und in der Cloud zusammenrücken."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


# <a name="move-data-from-an-on-premise-sql-server-to-sql-azure-with-azure-data-factory"></a>Verschieben von Daten von einem lokalen SQL Server, SQL Azure Azure Data Factory

Dieses Thema zeigt, wie zum Verschieben von Daten aus einer lokalen SQL Server-Datenbank in eine SQL Azure-Datenbank über Azure BLOB-Speicher mit Azure Data Factory (ADF).

Die folgenden **Menü** Links zu Themen, die beschreiben, wie Daten in zielumgebungen aufnehmen, wo die Daten gespeichert und beim Team Daten Wissenschaft verarbeitet.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="intro"></a>Einführung: Was ist ADF und wenn dienen Datenmigration?

Azure Data Factory ist eine vollständig verwaltete Cloud-basierte Integration Datendienst, der koordiniert und automatisiert die Verlagerung und Transformation von Daten. Die ADZ-Modell ist Pipeline. Eine Pipeline ist eine logische Gruppierung von Aktivitäten, die jeweils die Daten in Datasets auszuführenden Aktionen definieren. Verknüpfte Dienste dienen Informationen Verbindung mit Datenquellen für Daten definieren.

Mit ADF können vorhandene Datenverarbeitungsdienste in Datenpipelines bestehen, die hoch verfügbare, verwaltete in der Cloud. Aufnahme vorbereiten, transformieren, analysieren und veröffentlichen diese Datenpipelines geplant werden können und ADF verwaltet und koordiniert komplexe Daten und Verarbeitung abhängig. Projektmappen schnell erstellt und bereitgestellt in der Cloud verbinden zahlreiche lokale und Datenquellen.

ADF in Betracht:

- Wenn Daten ständig im Hybrid-Szenario migriert zugreift, lokale und Cloud-Ressourcen 
- Wenn die Daten Transaktionen oder geändert werden oder Geschäftslogik hinzugefügt, wenn migriert. 

ADF kann für die Planung und Überwachung von mit einfachen JSON-Skripts, die zur Verwaltung von der Verlagerung von Daten in regelmäßigen Abständen. ADF hat auch andere Funktionen wie Unterstützung für komplexe Vorgänge. Weitere Informationen zu ADF finden Sie in der Dokumentation unter [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).


## <a name="scenario"></a>Das Szenario

Wir richten ein ADF-Pipeline, die zwei Data Migrationsaktivitäten erstellt. Verschieben sie gemeinsam Daten täglich zwischen einer lokalen SQL-Datenbank und einer Azure SQL-Datenbank in der Cloud. Zwei Aktivitäten sind:

* Kopieren von Daten aus einer lokalen SQL Server-Datenbank ein Konto Azure BLOB-Speicher
* Daten Sie aus dem Azure BLOB-Speicher-Konto mit einer Azure SQL-Datenbank.

>[AZURE.NOTE] Hier wurden ausführliche Anleitung ADF-Teams dargestellten Schritte: [Verschieben von Daten zwischen lokalen und Cloud Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) Verweise auf den entsprechenden Abschnitten dieses Themas werden bei Bedarf bereitgestellt.


## <a name="prereqs"></a>Erforderliche Komponenten
In diesem Lernprogramm vorausgesetzt:

* Ein **Azure-Abonnement**. Wenn Sie nicht über ein Abonnement verfügen, können Sie eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)anmelden.
* Ein **Azure-Speicherkonto**. Zum Speichern der Daten in diesem Lernprogramm verwenden Sie ein Konto Azure-Speicher. Wenn ein Azure Storage-Konto haben, finden Sie im Artikel [erstellen ein Speicherkonto](storage-create-storage-account.md#create-a-storage-account) . Nachdem Sie das Speicherkonto erstellt haben, müssen Sie Zugriff auf den Speicher verwendet Konto abrufen. [Anzeigen, kopieren und Zugriffstasten Regenerieren Speicher](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)anzeigen
* Zugriff auf eine **SQL Azure-Datenbank**. Wenn Sie eine SQL Azure-Datenbank einrichten müssen, enthält Thema [Erste Schritte mit Microsoft Azure SQL-Datenbank](../sql-database/sql-database-get-started.md) Informationen eine neue Instanz einer Azure SQL-Datenbank bereitstellen.
* Installierte und konfigurierte **Azure PowerShell** lokal. Eine Anleitung hierzu finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

> [AZURE.NOTE] Dieses Verfahren wird der [Azure-Portal](https://portal.azure.com/)verwendet.


##<a name="upload-data"></a>Hochladen Sie die Daten auf der lokalen SQL Server

Wir verwenden [NYC Taxi Dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) , um den Migrationsprozess zu veranschaulichen. NYC Taxi Dataset steht, wie bereits erwähnt, Post auf Azure blob-Speicher [NYC Taxi Daten](http://www.andresmh.com/nyctaxitrips/). Daten hat zwei Dateien, die Datei trip_data.csv enthält REISEDETAILS und die Datei trip_far.csv enthält Details für jede Reise gezahlten Flugpreis. Ein Beispiel und Beschreibung dieser Dateien sind in [NYC Taxi Trips Datasetbeschreibung](machine-learning-data-science-process-sql-walkthrough.md#dataset)bereitgestellt.


Das hier Verfahren auf Ihre eigenen Daten anpassen oder gehen Sie entsprechend des NYC Taxi Datasets. Um NYC Taxi Dataset in der lokalen SQL Server-Datenbank hochzuladen, folgen Sie dem Verfahren in [Massendaten in SQL Server-Datenbank importieren](machine-learning-data-science-process-sql-walkthrough.md#dbload). Diese Anleitung gilt für SQL Server ein Azure Virtual Machine, aber das Verfahren zum Hochladen auf lokale SQL Server entspricht.


##<a name="create-adf"></a>Erstellen einer Azure Data Factory

Die Schritte zum Erstellen einer neuen Azure Data Factory und einer Ressourcengruppe im [Azure-Portal](https://portal.azure.com/) stehen [Azure Data Factory erstellen](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory). Name der neuen ADF Instanz *Adfdsp* und Name Ressource Gruppe *Adfdsprg*.


## <a name="install-and-configure-up-the-data-management-gateway"></a>Installieren und Konfigurieren von Data Management Gateway

Um Pipelines in einer Azure Daten mit einer lokalen SQL Server zu aktivieren, müssen Sie als verknüpfte Daten Werk hinzufügen. Um einen verknüpften Dienst für eine lokale SQL Server erstellen, müssen Sie:

- Downloaden Sie und installieren Sie Microsoft Data Management Gateway auf dem lokalen Computer. 
- Konfigurieren Sie den verknüpften Dienst für lokale Datenquelle das Gateway verwendet. 

Data Management Gateway serialisiert und deserialisiert Quelle und Empfänger Daten auf dem Computer, gehostet wird.

Setup-Informationen und Daten-Management-Gateways finden Sie unter [Verschieben von Daten zwischen lokalen und Cloud Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)


## <a name="adflinkedservices"></a>Erstellen Sie Verbindung mit Datenquellen verknüpften Dienste

Verknüpfter Dienst definiert Informationen für Azure Data Factory Daten herstellen. Anleitung zum Erstellen von verknüpften Diensten erfolgt in [verknüpften Dienste erstellen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-2-create-linked-services).

Wir haben drei Ressourcen in diesem Szenario für die verknüpften Dienste benötigt werden.

1. [Verknüpfte Dienst für lokale SQL Server](#adf-linked-service-onprem-sql)
2. [Verknüpfte Dienst für Azure BLOB-Speicher](#adf-linked-service-blob-store)
3. [Verknüpfte Dienst für SQL Azure-Datenbank](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>Verknüpfte Dienst für lokale SQL Server-Datenbank

Den verknüpften Dienst für lokale SQL Server erstellen:

- Klicken Sie auf den **Datenspeicher** in die ADZ Zielseite auf Azure-Verwaltungsportal 
- Wählen Sie **SQL** und Anmeldeinformationen Sie der *Benutzername* und das *Kennwort* für das lokale SQL Server. Sie müssen den Servernamen als einen **vollständig qualifizierte Servername umgekehrten Instanznamen (Servername\instancename)**eingeben. Name der verknüpften Serviceartikel *Adfonpremsql*.

###<a name="adf-linked-service-blob-store"></a>Verknüpfte Dienst für BLOBs

Verknüpften Dienst Azure BLOB-Speicher-Konto zu erstellen:

- Klicken Sie auf den **Datenspeicher** in die ADZ Zielseite auf Azure-Verwaltungsportal
- **Azure Storage-Konto** auswählen 
- Geben Sie den Azure BLOB-Speicher Konto Schlüssel und Container. Name verknüpfter Dienst *Adfds*.

###<a name="adf-linked-service-azure-sql"></a>Verknüpfte Dienst für SQL Azure-Datenbank

Den verknüpften Dienst für Azure SQL-Datenbank zu erstellen:

- Klicken Sie auf den **Datenspeicher** in die ADZ Zielseite auf Azure-Verwaltungsportal
- Wählen Sie **SQL Azure** und Anmeldeinformationen Sie den *Benutzernamen* und das *Kennwort* für Azure SQL-Datenbank. Der *Benutzername* muss angegeben werden, wie *user@servername*.   


##<a name="adf-tables"></a>Definieren und Erstellen von Tabellen angeben, wie Datensätze zugreifen

Erstellen von Tabellen, die mit den folgenden Verfahren skriptbasierte Struktur, Speicherort und Verfügbarkeit der Datensätze angeben. JSON Dateien werden verwendet, um die Tabellen zu definieren. Weitere Informationen zur Struktur dieser Dateien finden Sie unter [Datasets](../data-factory/data-factory-create-datasets.md).

> [AZURE.NOTE]  Ausführen sollten die `Add-AzureAccount` Cmdlet vor der Ausführung des [Neu AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) Cmdlets rechts Azure-Abonnement für die Ausführung ausgewählt wird. Dokumentation dieses Cmdlet finden Sie [AzureAccount hinzufügen](https://msdn.microsoft.com/library/azure/dn790372.aspx).

Die JSON-basierte Definitionen in den Tabellen verwenden die folgenden Namen:

* der **Tabellenname** in der lokalen SQL Server ist *nyctaxi_data*
* der **Containername** in Azure BLOB-Speicher-Konto ist *containername*  

Für diese Pipeline ADF sind drei Tabellendefinitionen erforderlich:

1. [SQL lokale Tabelle](#adf-table-onprem-sql)
2. [BLOB-Tabelle](#adf-table-blob-store)
3. [SQL Azure-Tabelle](#adf-table-azure-sql)

> [AZURE.NOTE]  Mithilfe dieser Verfahren Azure PowerShell definieren und erstellen Sie die ADZ-Aktivitäten. Aber dieser Aufgaben können auch mithilfe des Azure-Portals. Details finden Sie unter [Erstellen und Datensätze ausgegeben](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-3-create-input-and-output-datasets).

###<a name="adf-table-onprem-sql"></a>SQL lokale Tabelle

Die Tabellendefinition für lokale SQL Server wird in der folgenden JSON-Datei angegeben:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

Die Spaltennamen wurden hier nicht enthalten. Sie können auf den Spaltennamen Sub auswählen auch hier (für Details überprüfen Thema [ADF-Dokumentation](../data-factory/data-factory-data-movement-activities.md ) .

JSON-Definition der Tabelle in eine Datei namens *onpremtabledef.json* Datei kopieren und an einen bekannten Speicherort (hier angenommen *C:\temp\onpremtabledef.json*) speichern. Erstellen Sie die Tabelle mit den folgenden Azure PowerShell-Cmdlet in ADF:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>BLOB-Tabelle
Definition für die Tabelle Blob Ausgabespeicherort ist im folgenden (aufgenommenen Daten vom lokalen Azure Blob zugeordnet wird):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

JSON-Definition der Tabelle in eine Datei namens *bloboutputtabledef.json* Datei kopieren und an einen bekannten Speicherort (hier angenommen *C:\temp\bloboutputtabledef.json*) speichern. Erstellen Sie die Tabelle mit den folgenden Azure PowerShell-Cmdlet in ADF:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>SQL Azure-Tabelle
Definition für die Tabelle für SQL Azure ausgegeben wird (dieses Schema ordnet die Daten aus dem Blob) Folgendes:

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

JSON-Definition der Tabelle in eine Datei namens *AzureSqlTable.json* Datei kopieren und an einen bekannten Speicherort (hier angenommen *C:\temp\AzureSqlTable.json*) speichern. Erstellen Sie die Tabelle mit den folgenden Azure PowerShell-Cmdlet in ADF:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


##<a name="adf-pipeline"></a>Definieren und Erstellen der pipeline

Geben Sie die Aktivitäten der Pipeline gehören und die Pipeline mit den folgenden Verfahren skriptbasierte erstellen. JSON-Datei dient die Pipelineeigenschaften definieren.

* Das Skript übernimmt die **pipeline namens** *AMLDSProcessPipeline*.
* Beachten Sie, dass wir die Periodizität der Pipeline täglich ausgeführt und die Zeitdauer für die Ausführung für den Auftrag (12 Uhr UTC) festgelegt.

> [AZURE.NOTE]Die folgenden Verfahren mithilfe von Azure PowerShell definieren und erstellen Sie die ADZ-Pipeline. Aber diese Aufgabe kann auch mit der Azure-Portal ausgeführt werden. Weitere Informationen finden Sie unter [Erstellen und Ausführen einer Rohrleitung](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-4-create-and-run-a-pipeline).

Mit der bisher Definitionen, Pipeline-Definition für die Anwendungsdefinitionsdatei wird wie folgt angegeben:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Diese JSON-Definition der Pipeline in eine Datei namens *pipelinedef.json* Datei kopieren und an einen bekannten Speicherort (hier angenommen *C:\temp\pipelinedef.json*) speichern. Erstellen der Pipeline in ADF mit folgenden Azure PowerShell-Cmdlet:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Bestätigen, dass die Rohrleitung ADF in Azure-Verwaltungsportal folgendermaßen anzeigen, (Wenn Sie das Diagramm klicken) angezeigt

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)


##<a name="adf-pipeline-start"></a>Starten der Pipeline
Die Pipeline kann nun mit dem folgenden Befehl ausgeführt werden:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

Die Parameterwerte *Startdate* und *Enddate* müssen mit den tatsächlichen Daten ersetzt die Pipeline ausgeführt werden soll.

Sobald die Pipeline führt, sollte Sie die Daten in den Container für das Blob täglich ausgewählt angezeigt.

Beachten Sie, dass die Funktionalität von ADF zum Übertragen von Daten inkrementell nicht genutzt haben. Weitere Informationen über diese und andere Funktionen von ADF Vorgehensweise finden Sie unter der [ADF-Dokumentation](https://azure.microsoft.com/services/data-factory/).
