<properties
    pageTitle="Data Factory Lernprogramm: erste Datenpipeline | Microsoft Azure"
    description="In diesem Lernprogramm Azure Data Factory wird das Erstellen und planen eine Daten-Factory, die Daten mit Struktur-Skript in einem Cluster Hadoop veranschaulicht."
    services="data-factory"
    keywords="Azure Data Factory Lernprogramm Hadoop Cluster Hadoop Struktur"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Lernprogramm: Erstellen Sie Ihre erste Pipeline Hadoop Cluster mit Daten 
> [AZURE.SELECTOR]
- [Übersicht und Komponenten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressourcen-Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In diesem Lernprogramm erstellen Sie Ihre erste Azure Data Factory mit einer Datenpipeline, die Daten von Hive-Skript in einem Cluster Azure HDInsight (Hadoop) verarbeitet. 

> [AZURE.NOTE] Dieser Artikel bietet eine grundlegende Übersicht über Azure Data Factory nicht. Eine grundlegende Übersicht über den Dienst finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md). Finden Sie unter [Data Factory Lernpfad](https://azure.microsoft.com/documentation/learning-paths/data-factory/) für ein empfohlenes Verfahren zum Navigieren durch unsere Inhalte über Data Factory.

## <a name="whats-covered-in-this-tutorial"></a>Was ist in diesem Lernprogramm abgedeckt? 
**Azure Data Factory** können Sie Daten **Verschieben** und Daten **verarbeiten** Aufgaben als datengesteuerten Workflows (sogenannte Datenpipelines) erstellen. Erläutert, wie die erste Aktivität des Daten-Pipeline Datenverarbeitung (oder Datentransformation) erstellen. Diese Aktivität verwendet HDInsight Hadoop Cluster transformieren und Beispiel Protokolle analysieren.  

In dieser Übung führen Sie die folgenden Schritte aus:

1.  Erstellen einer **Data Factory**. Eine Factory Daten enthalten eine oder mehr Daten Rohrleitungen, verschieben und Verarbeiten von Daten. 
2.  Erstellen von **verknüpften Diensten**. Verknüpften Serviceartikel verknüpft einen Datenspeicher oder ein Dienst Compute Data Factory erstellen. Datenspeicher wie Azure-Speicher enthält Daten, e/a-Aktivitäten in der Pipeline. Ein Compute-Dienst wie HDInsight Hadoop Cluster Prozesse/Transformationen Daten.    
3.  Erstellen von Eingabe- und Ausgabe **Datasets**. Eingabedatasets Eingabe für eine Aktivität in der Pipeline und ein ausgabedataset darstellt die Ausgabe für die Aktivität.
3.  Erstellen der **Pipeline**. Eine Pipeline kann eine oder mehrere Aktivitäten haben (Beispiele: Kopieraktivität, HDInsight Struktur Aktivität). Dieses Beispiel verwendet HDInsight Struktur-Aktivität, die in einem Cluster HDInsight Hadoop Hive-Skript ausgeführt wird. Das Skript erstellt zunächst eine Tabelle, die auf die unformatierte Webprotokolldaten in Azure BLOB-Speicher gespeichert und dann die Rohdaten nach Jahr und Monat Partitionen.

    Es gibt zwei Arten von Aktivitäten von Azure Data Factory unterstützt. Sie sind: [Daten verschieben](data-factory-data-movement-activities.md) und [Data Transformationsaktivitäten](data-factory-data-transformation-activities.md). Es ist nur eine bewegungsaktivität, Aktivität kopieren. In diesem Lernprogramm verwenden Sie nicht die Kopieraktivität. Ein Lernprogramm zum Verwenden der Kopieraktivität finden Sie unter [Einführung: Daten aus einer Azure, SQL Azure BLOB-](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). HDInsight-Struktur Aktivität in diesem Lernprogramm verwendeten ist eine der Data Transformation von Data Factory unterstützt.  
 
Hier ist die **Diagrammansicht** Sample Data Factory in diesem Lernprogramm erstellen. 

![Diagrammansicht in Data Factory-Lernprogramm](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

In diesem Lernprogramm enthält Container Azure Blob **Adfgetstarted** **Inputdata** Ordner eine Datei mit dem Namen input.log. Diese Protokolldatei enthält Einträge aus drei Monate: Januar, Februar und März 2016. Hier sind die Beispielzeilen für jeden Monat in der Eingabedatei. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Beim Verarbeiten der Datei durch die Pipeline mit HDInsight Struktur führt die Aktivität eine Struktur Skript auf HDInsight Cluster Partitionen Daten nach Jahr und Monat eingeben. Das Skript erstellt drei Ausgabeordner, die eine Datei aus jeden Monat enthalten.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

Aus dem Beispiel oben, (mit 2016-01-01) bezieht sich auf die Datei 000000_0 im Monat = 1 Ordner. Ebenso zweite bezieht sich auf die Datei im Monat = 2 Ordner und der dritte eine bezieht sich auf die Datei im Monat = 3 Ordner.  


## <a name="pre-requisites"></a>Erforderliche Komponenten
Bevor Sie dieses Lernprogramm beginnen, müssen Sie die folgenden Komponenten:

1.  **Azure-Abonnement** - Wenn Sie Azure-Abonnement haben, können Sie in wenigen Minuten ein kostenloses Testabo erstellen. Finden Sie [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) auf wie ein kostenloses Testabo erhalten.

2.  **Azure-Speicher** – ein Konto Azure-Speicher zum Speichern der Daten in diesem Lernprogramm verwenden. Wenn ein Azure Storage-Konto haben, finden Sie im Artikel [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) . Nachdem Sie das Speicherkonto erstellt haben, notieren Sie den **Namen** und die **Tastenkombination**. [Anzeigen, kopieren und Zugriffstasten Regenerieren Speicher](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys)anzeigen 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Hochladen von Dateien in den Azure-Speicher für das Lernprogramm
Bevor Sie das Lernprogramm starten, müssen Sie Ihr Konto Azure Storage mit Beispieldateien für das Lernprogramm vorbereiten.

1. Hochladen Sie Hive Abfrage (HQL), **Skript** -Ordner des Containers BLOB- **Adfgetstarted** .
2. Hochladen Sie Eingabedatei **Inputdata** **Adfgetstarted** BLOB-Container Ordner. 

#### <a name="create-hql-script-file"></a>HQL-Skriptdatei erstellen 

1. Starten Sie den **Editor** , und fügen Sie das folgende Skript HQL. Das Hive-Skript erstellt zwei Tabellen: **WebLogsRaw** und **WebLogsPartitioned**. **Klicken Sie auf das Menü** und wählen Sie **Speichern unter**. Wechseln Sie zum Ordner **C:\adfgetstarted** auf der Festplatte. Wählen Sie * *Alle Dateien (*.*) **für die** Speichern als Typ** Feld. Geben Sie **partitionweblogs.hql** für den **Namen**. Überprüfen Sie, ob das **** Feld am unteren Rand des Dialogfelds ****ANSI-Codierung. Wenn nicht, **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Zur Laufzeit übergibt die Struktur Aktivität in der Pipeline "Data Factory" Werte für die **inputtable** und **Partitionedtable** wie im folgenden Codeausschnitt dargestellt:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

**Storageaccountname** ist der Name Ihres Kontos Azure-Speicher.
 
#### <a name="create-a-sample-input-file"></a>Erstellen einer Beispieleingabedatei
Erstellen Sie mit Editor eine Datei namens **input.log** in der **c:\adfgetstarted** mit dem folgenden Inhalt: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Datei hochladen und HQL Datei Azure BLOB-Speicher

Dieser Abschnitt beschreibt Werkzeug **AzCopy** input.log und partitionweblogs.hql-Dateien in Azure BLOB-Speicher kopieren. Beliebiges Tool Ihrer Wahl verwenden (Beispiel: [Microsoft Azure Storage Explorer](http://storageexplorer.com/), [CloudXPlorer von ClumsyLeaf Software](http://clumsyleaf.com/products/cloudxplorer) für diese Aufgabe.   
     
1. Downloaden Sie die [neueste Version von **AzCopy**](http://aka.ms/downloadazcopy)oder die [neueste Vorabversion](http://aka.ms/downloadazcopypr). [Wie Sie AzCopy](../storage/storage-use-azcopy.md) finden Sie im Artikel zur Verwendung des Dienstprogramms.
2. Navigieren Sie zum Ordner c:\adfgetstarted, und führen Sie den folgenden Befehl: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    Dieser Befehl lädt die Datei **input.log** an das Speicherkonto (**Adfgetstarted** Container und Ordner **Inputdata** ). Ersetzen Sie **Storageaccountname** durch den Namen Speicherkonto und **Storageaccesskey** zusammen mit der Zugriffstaste Speicher.

    > [AZURE.NOTE] Dieser Befehl erstellt einen Container mit dem Namen **Adfgetstarted** in Azure Blob-Speicher und kopiert die Datei **input.log** von der lokalen Festplatte in den Ordner **Inputdata** in den Container. 
    
3. Nachdem die Datei erfolgreich hochgeladen wurde, sehen Sie die Ausgabe ähnlich der folgenden aus AzCopy.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. Führen Sie den folgenden Befehl zum Hochladen der Datei **partitionweblogs.hql** auf **der Skriptordner **Adfgetstarted** Container** . Hier ist der Befehl: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

Sie haben die erforderlichen Komponenten. Sie können eine Data Factory mithilfe einer der folgenden Arten erstellen. Klicken Sie auf die Registerkarten oben oder folgende Links, das Lernprogramm auszuführen. 

- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressourcen-Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)