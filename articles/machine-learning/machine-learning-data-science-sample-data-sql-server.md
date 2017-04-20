<properties 
    pageTitle="Daten in SQL Server auf Azure | Microsoft Azure" 
    description="Beispieldaten in Azure SQL Server" 
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
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Beispieldaten in Azure SQL Server


Dieses Dokument veranschaulicht Beispiele für Daten aus SQL Server in Azure mit SQL oder Python-Programmiersprache. Außerdem veranschaulicht gesammelten Daten in einer Datei speichern und Hochladen in Azure Blob in Azure Machine Learning Studio Lesen in Azure Computer verschieben.

Python-Sampling verwendet [Pyodbc](https://code.google.com/p/pyodbc/) ODBC-Bibliothek Verbindung zu SQL Server auf Azure und Bibliothek [Panda](http://pandas.pydata.org/) Sampling.

>[AZURE.NOTE] Der SQL-Beispielcode in diesem Dokument wird davon ausgegangen, dass die Daten in einer SQL Server-Azure. Ist er nicht finden Sie [Verschieben Sie Daten von SQL Server auf Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) Thema wie die Daten in SQL Server auf Azure zu verschieben.

**Warum Beispieldaten?**
Dataset zu analysieren groß ist, ist es in der Regel sollten Sie Daten auf eine kleinere aber repräsentativ und überschaubarer Größe reduzieren Stichproben. Dies erleichtert die Daten verstehen, Untersuchung und Featureentwicklung. Seine Rolle im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) soll schnell Prototypen Datenverarbeitung und maschinelles lernen Modelle.

Das **Menü** unten Links zu Themen, die beschreiben, wie Sie Daten aus verschiedenen Speicherausfällen Beispiel. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Diese Aufgabe Sampling ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>Mithilfe von SQL

Dieser Abschnitt beschreibt verschiedene Methoden, mit SQL einfache Stichproben der Daten in der Datenbank. Wählen Sie eine Methode basierend auf der Datengröße und deren Verteilung.

Zwei Elemente unten zeigen wie Newid in SQL Server mit der Probenahme. Welche Methode Sie auswählen hängt davon ab, wie zufällig Beispiel werden soll (Pk_id im folgenden Beispielcode wird ein automatisch generierter Primärschlüssel verwendet).

1. Weniger strenge Stichprobe

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Weitere Stichprobe 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

TABLESAMPLE kann zur Probenahme sowie werden unten gezeigt. Ein besserer Ansatz möglicherweise Ihre Daten (sofern die Daten auf anderen Seiten nicht korreliert) groß ist und für die Abfrage in einer angemessenen Zeit abgeschlossen.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] Untersuchen und Generieren von Funktionen aufgenommenen Daten in eine neue Tabelle speichern


###<a name="sql-aml"></a>Herstellen einer Verbindung mit Azure maschinelles lernen

Sie können direkt Beispielabfragen über in Azure Machine Learning- [Daten importieren] [ import-data] Down-Beispiel die Daten dynamisch und in einem Experiment Azure Machine Learning-Modul. Nachfolgend finden Sie ein Bildschirmabbild des aufgenommenen Daten lesen mit dem Reader-Modul:
   
![Reader sql][1]

##<a name="python"></a>Mithilfe der Programmiersprache Python 

Dieser Abschnitt veranschaulicht die Verwendung der [Pyodbc-Bibliothek](https://code.google.com/p/pyodbc/) zu ODBC mit einer SQL Server-Datenbank in Python. Die Verbindungszeichenfolge lautet wie folgt: (Servername, Dbname Benutzername und Kennwort mit Ihrer Konfiguration ersetzen):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Panda](http://pandas.pydata.org/) -Bibliothek in Python bietet zahlreiche Datenstrukturen und Tools zur Datenanalyse zur Datenmanipulation für Python-Programmierung. Der folgende Code liest die Beispieldaten 0,1 % Panda Daten aus einer Tabelle in SQL Azure-Datenbank:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Sie können nun mit der erfassten Daten in Datenrahmen Panda arbeiten. 

###<a name="python-aml"></a>Herstellen einer Verbindung mit Azure maschinelles lernen

Im folgenden Beispielcode können Sie aufgenommenen Daten in einer Datei speichern und Hochladen in Azure Blob. Die Daten im Blob können direkt gelesen werden eine Azure Machine Learning-Experiment mit den [Importierten Daten] [ import-data] Modul. Die Schritte lauten: 

1. Schreiben Sie Panda Datenrahmen in eine lokale Datei

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Lokalen Datei in Azure Blob hochladen

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Lesen von Daten aus Azure Blob Azure Machine Learning [Importdaten] [ import-data] Modul wie im folgenden Screenshot gezeigt:
 
![Reader-blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Team wissenschaftliche Daten im Beispiel

Beispielsweise End to End-Anleitung Team wissenschaftliche Daten mithilfe eines öffentlichen Datasets finden Sie unter [Team wissenschaftliche Daten in Aktion: mit SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
