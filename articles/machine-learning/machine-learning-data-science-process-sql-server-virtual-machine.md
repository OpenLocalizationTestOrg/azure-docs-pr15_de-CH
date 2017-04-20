<properties 
    pageTitle="Daten aus SQL Azure | Microsoft Azure" 
    description="Prozessdaten aus SQL Azure" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Daten in Azure SQL Server virtueller Computer

Dieses Dokument erläutert, wie Daten und Funktionen für Daten in einer SQL Server-VM auf Azure zu generieren. Dies können Sie Daten mit SQL Rechtsstreitigkeiten oder mithilfe einer Programmiersprache wie Python.


> [AZURE.NOTE] Beispiel-SQL-Anweisungen in diesem Dokument wird davon ausgegangen, dass Daten in SQL Server. Andernfalls finden Sie in der Cloud Daten Wissenschaft Prozessdiagramm erfahren, wie SQL Server Daten verschieben.

##<a name="SQL"></a>Mithilfe von SQL

Die folgenden gerangel Aufgaben in diesem Abschnitt SQL erläutert:

1. [Durchsuchen von Daten](#sql-dataexploration)
2. [KE-Erzeugung](#sql-featuregen)

###<a name="sql-dataexploration"></a>Durchsuchen von Daten
Hier sind einige SQL Beispielskripts, die zu Datenspeichern in SQL Server verwendet werden können.


> [AZURE.NOTE] Für ein praktisches Beispiel können [NYC Taxi Dataset](http://www.andresmh.com/nyctaxitrips/) verwenden und IPNB für ein End-to-End-Anleitung Titel [NYC Daten Rechtsstreitigkeiten IPython Notebook mit SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) verweisen.

1. Rufen Sie die Anzahl der Messwerte pro Tag

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Die Ebenen in eine Kategoriespalte abrufen

    `select  distinct <column_name> from <databasename>`

3. Die Anzahl der Ebenen in Kombination zweier kategorische Spalten abrufen 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Die Verteilung für numerische Spalten abrufen

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="sql-featuregen"></a>KE-Erzeugung

In diesem Abschnitt beschreiben wir Wege zur Generierung mit SQL:  

1. [KE-Erzeugung basierend](#sql-countfeature)
2. [Binning Funktion generieren](#sql-binningfeature)
3. [Rollout von Funktionen aus einer einzelnen Spalte](#sql-featurerollout)


> [AZURE.NOTE] Nach zusätzlichen Features generieren können Sie als Spalten in der vorhandenen Tabelle hinzufügen oder Erstellen einer neuen Tabelle mit zusätzlichen Features und Primärschlüssel, die mit der ursprünglichen Tabelle verknüpft werden können. 

###<a name="sql-countfeature"></a>KE-Erzeugung basierend

Dieses Dokument veranschaulicht zweierlei Zählfunktionen generieren. Die erste Methode verwendet Summe und die zweite Methode 'where'-Klausel. Diese können dann mit der ursprünglichen Tabelle (mit Primärschlüsselspalten) verknüpft werden, um Zählfunktionen die ursprünglichen Daten.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="sql-binningfeature"></a>Binning Funktion generieren

Im folgenden Beispiel wird veranschaulicht, wie gebinnten Features von binning (mit 5 Lagerplätze) generieren eine numerische Spalte stattdessen als eine Funktion verwendet werden kann:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="sql-featurerollout"></a>Rollout von Funktionen aus einer einzelnen Spalte

In diesem Abschnitt demonstrieren wir eine einzelne Spalte in einer Tabelle mit zusätzlichen Funktionen einzuführen. Es wird angenommen, dass die Breiten- oder Längenkoordinate Spalte in der Tabelle aus der gesuchten Funktionen gibt.

Hier ist eine kurze Einführung in Breiten-und Längengrad Positionsdaten (Ressourcen aus Stackoverflow [wie die Genauigkeit der Breiten- und Längenkoordinaten?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Dies ist vor dem Featurizing Standortfeld vertraut machen:

- Die Vorzeichen erzählt werden, ob Norden oder Süden, Osten oder Westen auf der ganzen Welt.
- Ungleich NULL Hunderte Ziffer erzählt wir Längengrad, Breitengrad nicht verwenden.
- Zweistelligen Zahl gibt eine Position auf über 1.000 Kilometer. Es gibt uns Informationen welcher Kontinent oder Ozeans auf.
- Die Einerstelle (ein decimal Grad) gibt eine Position 111 Kilometer (60 Seemeilen, ca. 69 Meilen). Sie können etwa uns was große Land oder Region sind wir.
- Die erste Dezimalstelle ist 11.1 km: unterscheiden die Position einer Großstadt aus benachbarten Großstadt.
- Die zweite Dezimalstelle ist 1.1 km: Es kann Dorf voneinander trennen.
- Dritten Dezimalstelle ist 110 m großen landwirtschaftlichen Feld oder institutionellen Campus identifizieren können.
- Vierten Dezimalstelle ist bis zu 11 m, ein Grundstück identifiziert werden kann. Es ist normalerweise Genauigkeit von einem unkorrigierten GPS-Gerät ohne Einfluss vergleichbar.
- Die fünfte Dezimalstelle ist bis zu 1,1 m es Strukturen voneinander unterscheiden. Auf dieser Ebene mit professionellen GPS-Geräte können nur mit differenzielle Genauigkeit.
- Der sechsten Dezimalstelle ist bis 0,11 m: Hiermit können für das Layout von Strukturen im Detail zum Entwerfen von Landschaften, Straßen. Mehr als ausreichend zum Verfolgen von Bewegung Gletscher und sollte. Dies kann durch sorgfältige Maßnahmen mit GPS wie differenziell korrigierte GPS erreicht werden.

Die Standortinformationen möglich Featurized:, Region, Ort und Stadt trennen. Beachten Sie, dass Sie auch einen REST-Endpunkt wie Bing Maps-API verfügbar [Punkt Ausführungsort](https://msdn.microsoft.com/library/ff701710.aspx) der Region-Bezirk Informationen aufrufen können.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Die oben beschriebenen standortbasierte Features Weitere lässt sich zusätzliche Anzahl Funktionen wie oben beschrieben. 


> [AZURE.TIP] Sie können programmgesteuert die Datensätze mit der Sprache Ihrer Wahl einfügen. Sie müssen Daten in Blöcken Effizienz schreiben (für Beispiel, verwenden Pyodbc finden [ein HelloWorld-Beispiel auf SQLServer Python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)) einfügen. Eine andere Alternative ist zum Einfügen von Daten in der Datenbank mit dem [Dienstprogramm BCP](https://msdn.microsoft.com/library/ms162802.aspx).

###<a name="sql-aml"></a>Herstellen einer Verbindung mit Azure maschinelles lernen

Neu erstellte Funktion kann als eine Spalte einer vorhandenen Tabelle hinzugefügt oder in einer neuen Tabelle gespeichert und verknüpft mit der ursprünglichen Tabelle für maschinelles lernen. Funktionen generiert oder zugegriffen, wenn bereits erstellt, mit den [Importierten Daten] [ import-data] Modul in Azure Machine Learning wie folgt:

![Azureml Leser][1] 

##<a name="python"></a>Mithilfe einer Programmiersprache wie Python

Python ähnelt Daten und Funktionen generieren, wenn die Daten in SQL Server Daten in Azure Blob mit Python [Prozess Azure BLOB-Daten in Sie Wissenschaft Umgebung](machine-learning-data-science-process-data-blob.md)dokumentiert. Die Daten in ein Panda Daten aus der Datenbank geladen werden müssen und können dann weiter verarbeitet werden. Wir Dokumentieren der Verbindung mit der Datenbank und Laden von Daten in Datenrahmen in diesem Abschnitt.

Format der folgenden Verbindungszeichenfolge kann mit Python mit Pyodbc (ersetzen Servername Dbname, Benutzername und Kennwort mit bestimmten Werten) eine Verbindung zu einer SQL Server-Datenbank verwendet werden:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Panda-Bibliothek](http://pandas.pydata.org/) in Python bietet zahlreiche Datenstrukturen und Tools zur Datenanalyse zur Datenmanipulation für Python-Programmierung. Der folgende Code liest die Ergebnisse aus einer SQL Server-Datenbank in ein Panda Daten zurückgegeben:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Jetzt können Sie mit Panda Daten arbeiten, wie in Artikel [Prozess Azure BLOB-Daten in Sie Wissenschaft Umgebung](machine-learning-data-science-process-data-blob.md)behandelt.

## <a name="azure-data-science-in-action-example"></a>Azure Data Science im Beispiel

End-to-End Exemplarische Vorgehensweise beispielsweise von Azure wissenschaftliche Daten über öffentliche Dataset finden Sie unter [Azure wissenschaftliche Daten in Aktion](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
