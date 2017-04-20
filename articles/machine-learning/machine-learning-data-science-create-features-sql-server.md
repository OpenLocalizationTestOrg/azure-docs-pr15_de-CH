<properties
    pageTitle="Funktionen für Daten in SQL Server mithilfe von SQL und Python erstellen | Microsoft Azure"
    description="Prozessdaten aus SQL Azure"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;fashah;garye" />


# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>Erstellen Sie Funktionen für Daten in SQL Server mithilfe von SQL und Python


Dieses Dokument veranschaulicht die Features für Daten in einer SQL Server-VM in Azure, mit denen die Daten effizienter lernen Algorithmen generieren. Kann dies mithilfe von SQL oder mithilfe einer Programmiersprache wie Python, die hier gezeigt.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Dieses **Menü** enthält Links zu Themen, in denen Funktionen für Daten in unterschiedlichen Umgebungen erstellen. Diese Aufgabe ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

> [AZURE.NOTE] Für ein praktisches Beispiel können finden Sie in [NYC Taxi Dataset](http://www.andresmh.com/nyctaxitrips/) und IPNB mit dem Titel [NYC Daten Rechtsstreitigkeiten IPython Notebook mit SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) für eine End-to-End-Anleitung finden.


## <a name="prerequisites"></a>Erforderliche Komponenten
Dieser Artikel setzt voraus:

* Ein Azure Storage-Konto erstellt. Anleitung, finden Sie unter [Azure Storage-Konto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account)
* Die Daten in SQL Server gespeichert. Wenn Sie keinen finden Sie unter [Verschieben einer Azure SQL-Datenbank für Azure maschinelles lernen](machine-learning-data-science-move-sql-azure.md) Anleitung zum Verschieben der Daten vorhanden.


## <a name="sql-featuregen"></a>Feature-Generation mit SQL

In diesem Abschnitt beschreiben wir Wege zur Generierung mit SQL:  

1. [KE-Erzeugung basierend](#sql-countfeature)
2. [Binning Funktion generieren](#sql-binningfeature)
3. [Rollout von Funktionen aus einer einzelnen Spalte](#sql-featurerollout)


> [AZURE.NOTE] Nach zusätzlichen Features generieren können Sie als Spalten in der vorhandenen Tabelle hinzufügen oder Erstellen einer neuen Tabelle mit zusätzlichen Features und Primärschlüssel, die mit der ursprünglichen Tabelle verknüpft werden können.

### <a name="sql-countfeature"></a>KE-Erzeugung basierend

Dieses Dokument veranschaulicht zweierlei Zählfunktionen generieren. Die erste Methode verwendet Summe und die zweite Methode 'where'-Klausel. Diese können dann mit der ursprünglichen Tabelle (mit Primärschlüsselspalten) verknüpft werden, um Zählfunktionen die ursprünglichen Daten.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Binning Funktion generieren

Im folgenden Beispiel wird veranschaulicht, wie gebinnten Features von binning (mit 5 Lagerplätze) generieren eine numerische Spalte stattdessen als eine Funktion verwendet werden kann:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Rollout von Funktionen aus einer einzelnen Spalte

In diesem Abschnitt demonstrieren wir eine einzelne Spalte in einer Tabelle mit zusätzlichen Funktionen einzuführen. Es wird angenommen, dass die Breiten- oder Längenkoordinate Spalte in der Tabelle aus der gesuchten Funktionen gibt.

Hier ist eine kurze Einführung in Breiten-und Längengrad Daten (von Stackoverflow Ressourcen `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Dies ist vor dem Featurizing Standortfeld vertraut machen:

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

Die Informationen kann Featurized wie folgt sein, Region, Ort und Ortsangaben trennen. Notiz, die auch einmal einen REST-Endpunkt wie Bing Maps-API unter aufrufen können `https://msdn.microsoft.com/library/ff701710.aspx` Region-Bezirk Informationen abgerufen werden.

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

Die oben beschriebenen standortbasierte Features können weitere zusätzliche Zählfunktionen generiert, wie oben beschrieben verwendet werden.


> [AZURE.TIP] Sie können programmgesteuert die Datensätze mit der Sprache Ihrer Wahl einfügen. Sie müssen die Daten in Blöcken zu Schreibeffizienz [Auschecken wird dazu mit Pyodbc hier](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)einfügen.
Eine weitere Möglichkeit ist zum Einfügen von Daten in der Datenbank mithilfe des [Dienstprogramms BCP](https://msdn.microsoft.com/library/ms162802.aspx)

### <a name="sql-aml"></a>Herstellen einer Verbindung mit Azure maschinelles lernen

Neu erstellte Funktion kann als eine Spalte einer vorhandenen Tabelle hinzugefügt oder in einer neuen Tabelle gespeichert und verknüpft mit der ursprünglichen Tabelle für maschinelles lernen. Funktionen können erstellt oder bereits erstellt, auf der über Moduls [Import](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) Azure ML wie folgt:

![Azureml Leser](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Mithilfe einer Programmiersprache wie Python

Python ähnelt Funktionen generieren, wenn die Daten in SQL Server Daten in Azure Blob mit Python [Prozess Azure BLOB-Daten in Sie Wissenschaft Umgebung](machine-learning-data-science-process-data-blob.md)dokumentiert. Die Daten in ein Panda Daten aus der Datenbank geladen werden müssen und können dann weiter verarbeitet werden. Wir Dokumentieren der Verbindung mit der Datenbank und Laden von Daten in Datenrahmen in diesem Abschnitt.

Format der folgenden Verbindungszeichenfolge kann mit Python mit Pyodbc (ersetzen Servername Dbname, Benutzername und Kennwort mit bestimmten Werten) eine Verbindung zu einer SQL Server-Datenbank verwendet werden:

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Panda-Bibliothek](http://pandas.pydata.org/) in Python bietet zahlreiche Datenstrukturen und Tools zur Datenanalyse zur Datenmanipulation für Python-Programmierung. Der folgende Code liest die Ergebnisse aus einer SQL Server-Datenbank in ein Panda Daten zurückgegeben:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nun können Sie Daten mit Panda arbeiten, wie [KEs für Azure BLOB-Speicherdaten über Panda](machine-learning-data-science-create-features-blob.md)Themen.
