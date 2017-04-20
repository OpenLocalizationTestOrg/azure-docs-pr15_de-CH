<properties 
    pageTitle="Untersuchen von SQL Server virtueller Computer auf Azure | Microsoft Azure" 
    description="Wie Sie Daten untersuchen, die in einer SQL Server-VM in Azure gespeichert." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Untersuchen von SQL Server virtueller Computer auf Azure


Dieses Dokument beschreibt die Daten untersuchen, die in einer SQL Server-VM in Azure gespeichert. Dies können Sie Daten mit SQL Rechtsstreitigkeiten oder mithilfe einer Programmiersprache wie Python.

Das folgende **Menü** enthält Links zu Themen über die Verwendung von Tools zu Daten aus verschiedenen Speicher. Diese Aufgabe ist ein Schritt im Cortana Analytics Prozess (Gap).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] Beispiel-SQL-Anweisungen in diesem Dokument wird davon ausgegangen, dass Daten in SQL Server. Andernfalls finden Sie in der Cloud Daten Wissenschaft Prozessdiagramm erfahren, wie SQL Server Daten verschieben.



## <a name="sql-dataexploration"></a>Untersuchen von SQL mit SQL-Skripts

Hier sind einige SQL Beispielskripts, die zu Datenspeichern in SQL Server verwendet werden können.

1. Rufen Sie die Anzahl der Messwerte pro Tag

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Die Ebenen in eine Kategoriespalte abrufen

    `select  distinct <column_name> from <databasename>`

3. Die Anzahl der Ebenen in Kombination zweier kategorische Spalten abrufen 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Die Verteilung für numerische Spalten abrufen

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Für ein praktisches Beispiel können [NYC Taxi Dataset](http://www.andresmh.com/nyctaxitrips/) verwenden und IPNB für ein End-to-End-Anleitung Titel [NYC Daten Rechtsstreitigkeiten IPython Notebook mit SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) verweisen.

##<a name="python"></a>Untersuchen von SQL mit Python

Python mit Daten und Funktionen generieren, wenn die Daten in SQL Server ähnelt der Verarbeitung von Daten in Azure Blob mit Python, [Prozess Azure BLOB-Daten in Ihrer Umgebung Daten Wissenschaft](machine-learning-data-science-process-data-blob.md)dokumentiert. Die Daten in Panda Datensatz aus der Datenbank geladen werden müssen und können dann weiter verarbeitet werden. Wir Dokumentieren der Verbindung mit der Datenbank und Laden von Daten in den Datensatz in diesem Abschnitt.

Format der folgenden Verbindungszeichenfolge kann mit Python mit Pyodbc (ersetzen Servername Dbname, Benutzername und Kennwort mit bestimmten Werten) eine Verbindung zu einer SQL Server-Datenbank verwendet werden:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Panda-Bibliothek](http://pandas.pydata.org/) in Python bietet zahlreiche Datenstrukturen und Tools zur Datenanalyse zur Datenmanipulation für Python-Programmierung. Der folgende Code liest die Ergebnisse aus einer SQL Server-Datenbank in ein Panda Daten zurückgegeben:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Jetzt können Sie mit Panda Datensatz arbeiten, wie im Thema [Prozess Azure BLOB-Daten in Ihrer Umgebung Daten Wissenschaft](machine-learning-data-science-process-data-blob.md)behandelt.

## <a name="cortana-analytics-process-in-action-example"></a>Cortana-Analyseprozess im Beispiel

End-to-End Exemplarische Vorgehensweise beispielsweise der Cortana Analytics über öffentliche Dataset finden Sie unter [das Team wissenschaftliche Daten in Aktion: mit SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

 
