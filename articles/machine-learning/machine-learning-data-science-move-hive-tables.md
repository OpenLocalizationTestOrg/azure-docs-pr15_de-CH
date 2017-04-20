<properties
    pageTitle="Erstellen und Laden von Daten in Tabellen Struktur von BLOB-Speicher | Microsoft Azure"
    description="Struktur Tabellen erstellen und die Daten im Blob Tabellen Struktur laden"
    services="machine-learning,storage"
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


#<a name="create-and-load-data-into-hive-tables-from-azure-blob-storage"></a>Erstellen und Laden von Daten in Tabellen Struktur von Azure BLOB-Speicher

Dieses Thema enthält allgemeine Struktur Abfragen, die Hive-Tabellen erstellen und Daten von Azure BLOB-Speicher geladen. Einige Richtlinien erfolgt auch Hive-Tabellen partitionieren und mit der optimierten Zeile Einspaltig (ORK) Formatierung die Abfrageperformance verbessern.

Dieses **Menü** enthält Links zu Themen, die beschreiben, wie Daten in zielumgebungen aufnehmen, wo die Daten gespeichert und verarbeitet während Team Daten Wissenschaft Prozess (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="prerequisites"></a>Erforderliche Komponenten
Dieser Artikel setzt voraus:

* Ein Azure Storage-Konto erstellt. Benötigen Sie Informationen anzeigen Sie [zur Azure-Speicherkonten](../storage/storage-create-storage-account.md) 
* Angepassten Hadoop Cluster mit den HDInsight-Dienst bereitgestellt.  Anleitung, finden Sie unter [Anpassen von Azure HDInsight Hadoop Cluster für erweiterte Analyse](machine-learning-data-science-customize-hadoop-cluster.md).
* Aktiviert remote Zugriff auf den Cluster angemeldet und die Befehlszeilenkonsole Hadoop geöffnet. Anleitung, finden Sie unter [Zugriff die Hadoop Cluster Head Knoten](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-to-azure-blob-storage"></a>Upload von Daten in Azure BLOB-Speicher
Wenn Azure VM erstellen wie [Richten Sie eine Azure Virtual Machine für erweiterte Analyse](machine-learning-data-science-setup-virtual-machine.md)gemäß dieser Datei sollte heruntergeladen wurden die *C:\\Benutzer\\\<Benutzername\>\\Dokumente\\Daten Wissenschaft Skripts* Verzeichnis auf dem virtuellen Computer. Diese Struktur Abfragen müssen nur in eigene Daten Schema- und Azure BLOB-Speicher in die entsprechenden Felder für die Einreichung zu schließen.

Angenommen, dass die Daten für die Hive-Tabellen in eine **nicht komprimierte** Tabellenformat und die Daten auf den Standardwert (oder zusätzlich) hochgeladene Container Speicherkonto Hadoop Cluster eingesetzt wird.

Möchten Sie auf **NYC Taxi Daten**, müssen Sie:

- **herunterladen** der 24 [NYC Taxi Reise](http://www.andresmh.com/nyctaxitrips) Datendateien 12 Reise Dateien und 12 Fahrpreis Dateien
- **Entpacken Sie** alle Dateien in CSV-Dateien und
- **Uploaden** sie die Standard oder entsprechenden Container Azure Storage-Konto, das von der Prozedur erstellt wurde im [Anpassen Azure HDInsight Hadoop-Cluster für die erweiterte Analyse und Technologie](machine-learning-data-science-customize-hadoop-cluster.md) Thema erläutert. Der Prozess zum Standardcontainer für das Speicherkonto CSV-Dateien hochladen finden auf dieser [Seite](machine-learning-data-science-process-hive-walkthrough.md#upload).


## <a name="submit"></a>Struktur Abfragen einreichen

Hive-Abfragen können mit eingereicht werden:

1. [Anfragen Sie Struktur über Hadoop Befehlszeile in Hauptknoten Hadoop Cluster](#headnode)
2. [Anfragen Sie Struktur mit der Struktur](#hive-editor)
3. [Anfragen Sie Struktur mit Azure PowerShell-Befehlen](#ps)

Hive-Abfragen werden SQL-ähnliche. Wenn Sie mit SQL vertraut sind, können Sie [für SQL Benutzer Spickzettel Struktur](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) hilfreich.

Bei einem Hive-Abfrage können Sie das Ziel für die Ausgabe Struktur Abfragen auf dem Bildschirm oder in einer lokalen Datei auf dem Head-Knoten oder ein Azure BLOB auch steuern.


###<a name="headnode"></a>1. senden Sie Struktur Abfragen über Hadoop Befehlszeile Hauptknoten Hadoop Cluster

Die Hive-Abfrage führt komplex Vorlage direkt in dem Head-Knoten der Hadoop Cluster normalerweise zu schneller als mit Hive-Editor oder Azure PowerShell Skripts senden.

Melden Sie sich bei dem Head-Knoten des Clusters Hadoop, Befehlszeile Hadoop auf dem Head-Knoten aufrufen und Befehl `cd %hive_home%\bin`.

Verfügen Sie über drei Struktur Abfragen in Hadoop Befehlszeile senden:

* direkt
* .hql Dateien
* mit der Befehlskonsole Struktur

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Senden Sie Hive-Abfragen direkt in Hadoop Befehlszeile.

Führen Sie Befehl wie `hive -e "<your hive query>;` einfache Struktur Abfragen direkt in Hadoop Befehlszeile senden. Hier ist ein Beispiel, in dem das rote Feld beschreibt den Befehl, der die Hive-Abfrage sendet und das grüne Feld beschreibt die Ausgabe der Hive-Abfrage.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Anfragen Sie Struktur in .hql Dateien

Die Hive-Abfrage ist, besteht aus mehreren Zeilen ist das Bearbeiten von Abfragen in der Befehlszeile oder Struktur Befehlskonsole nicht praktikabel. Eine Alternative ist mit einem Text-Editor im Head-Knoten des Clusters Hadoop um Hive-Abfragen in einer .hql in einem lokalen Verzeichnis des Head-Knotens zu speichern. Dann die Hive-Abfrage in der .hql-Datei mit gesendet werden kann die `-f` Argument wie folgt:

    hive -f "<path to the .hql file>"

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Status Status Bildschirm drucken Struktur Abfragen unterdrücken**

Standardmäßig nach Struktur Abfrage in Hadoop Befehlszeile gesendet wird der Fortschritt des Auftrags Map/Reduce Bildschirm gedruckt. Bildschirm drucken Map/Reduce Job Status unterdrücken, können Sie ein Argument `-S` ("S" in Großbuchstaben) den Befehl Zeile wie folgt:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Senden Sie Struktur Abfragen in Hive-Befehlskonsole.

Sie können auch zuerst eingeben die Befehlskonsole Struktur Befehl ausführen `hive` in Hadoop Befehlszeile und Struktur Abfragen in Hive-Befehlskonsole senden. Hier ist ein Beispiel. In diesem Beispiel markieren zwei rote Felder Menübefehle Befehlskonsole Struktur eingeben und die Hive-Abfrage in Struktur Befehlskonsole übermittelt. Das grüne Feld zeigt die Ausgabe der Hive-Abfrage.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

Die vorherigen Beispiele Ergebnisse direkt die Struktur Abfrage auf dem Bildschirm. Sie können auch die Ausgabe in eine lokale Datei auf dem Head-Knoten oder ein Azure Blob schreiben. Andere Tools können Sie dann um die Ausgabe der Hive-Abfragen weiter zu analysieren.

**Struktur von Abfrageergebnissen in eine lokale Datei ausgeben.**

Ausgabe Struktur Abfrageergebnisse in einem lokalen Verzeichnis auf dem Head-Knoten benötigen Sie die Hive-Abfrage in der Befehlszeile Hadoop wie folgt:

    hive -e "<hive query>" > <local path in the head node>

Im folgenden Beispiel wird die Ausgabe der Hive-Abfrage in eine Datei geschrieben `hivequeryoutput.txt` im Verzeichnis `C:\apps\temp`.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Ausgabe Struktur Abfrageergebnisse in Azure blob**

Sie können auch Hive Abfrageergebnisse in einer Azure Blob in Standardcontainer Hadoop Cluster ausgeben. Die Hive-Abfrage dafür lautet wie folgt:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

Im folgenden Beispiel wird die Ausgabe der Hive-Abfrage ein BLOB-Verzeichnis geschrieben `queryoutputdir` innerhalb des Clusters Hadoop Standardcontainer. Hier müssen Sie nur den Verzeichnisnamen ohne den blobnamen bereitstellen. Ein Fehler wird ausgelöst wenn Sie Verzeichnis und BLOB-Namen wie `wasb:///queryoutputdir/queryoutput.txt`.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Wenn Sie Standardcontainer Hadoop Cluster mit Azure Storage Explorer öffnen, sehen Sie die Ausgabe der Abfrage Struktur wie in der folgenden Abbildung dargestellt. Sie können (rotes Feld hervorgehoben) Filter anwenden, um nur das Blob mit angegebenen Namen abzurufen.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a>2. Geben Sie Struktur Abfragen mit der Struktur

Sie können auch Abfrage-Konsole (Hive-Editor) einen URL der Form *https://&#60; Hadoop Clustername >.azurehdinsight.net/Home/HiveEditor* in einem Webbrowser. Sie muss in der Konsole angemeldet und daher benötigen Ihre Hadoop Cluster Anmeldeinformationen eingeben.

###<a name="ps"></a>3. senden Sie Struktur Abfragen mit Azure PowerShell-Befehle

PowerShell können Sie Struktur Abfragen absenden. Informationen finden Sie unter [Struktur senden Aufträge mit PowerShell](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Struktur-Datenbank und Tabellen erstellen

Hive-Abfragen im [Github Repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) freigegeben und können von dort gedownloadet.

Hier ist die Hive-Abfrage, die eine Tabelle Struktur erstellt.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Hier werden eine Beschreibung der Felder, die Sie schließen möchten und andere Konfigurationen:

- **& #60; Datenbankname >**: der Name der Datenbank, die Sie erstellen möchten. Wenn Sie die Standarddatenbank verwenden möchten, kann die Abfrage *erstellen Datenbank* ausgelassen werden.
- **& #60; Tabellenname >**: der Name der Tabelle, die in der angegebenen Datenbank erstellen möchten. Wenn Sie die Standarddatenbank verwenden möchten, die Tabelle kann verwiesen werden durch *& #60; Tabellenname >* ohne & #60; Datenbankname >.
- **& #60; Feldtrennzeichen >**: Trennzeichen, die Felder in der Datendatei auf Tabelle Struktur begrenzt.
- **& #60; Trennlinie >**: Trennzeichen, die Zeilen in der Datendatei begrenzt.
- **& #60; Speicherort >**: Azure Speicherort Hive-Tabellen speichern. Wenn Sie keinen angeben *Position & #60; Speicherort >*, die Datenbank und die Tabellen werden in *Struktur Lager/* Verzeichnis im Standardcontainer des Clusters Hive standardmäßig. Wenn Sie den Speicherort angeben möchten, muss der Speicherort innerhalb der Standardcontainer für die Datenbank und Tabellen. Diese Position relativ zu den Standardcontainer des Clusters im Format genannt werden muss *' Wasb: / / & #60; Verzeichnis 1 > / "* oder *' Wasb: Verzeichnis 1 / / & #60; > / & Verzeichnis 2; #60 > /'*usw.. Nach Ausführung der Abfrage werden die relativen Verzeichnisse in Standardcontainer erstellt.
- **TBLPROPERTIES("Skip.Header.Line.Count"="1")**: die Datendatei eine Kopfzeile verfügt, haben Sie diese Eigenschaft **am Ende** der *Tabelle erstellen* -Abfrage hinzufügen. Andernfalls wird die Kopfzeile als Datensatz in die Tabelle geladen. Die Datei keinen Kopfzeile, kann diese Konfiguration in der Abfrage weggelassen werden.

## <a name="load-data"></a>Daten Struktur Tabellen laden
Hier ist die Hive-Abfrage, die Daten in einer Tabelle Struktur geladen.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; Pfad für BLOB-Daten >**: BLOB-Datei auf der Tabelle Struktur im Standardcontainer HDInsight Hadoop-Cluster ist die *& #60; Pfad für BLOB-Daten >* sollte im Format *' Wasb: / / & #60; in diesem Container Verzeichnis > / & #60; Blob-Dateiname > "*. BLOB-Datei kann auch in einen zusätzlichen Container HDInsight Hadoop Cluster sein. In diesem Fall *& #60; Pfad für BLOB-Daten >* sollte im Format *' Wasb: / / & #60; Container name>@&#60;storage Kontoname >.blob.core.windows.net/ & #60; Blob-Dateiname > "*.

    >[AZURE.NOTE] BLOB-Daten auf Tabelle Struktur müssen zusätzliche Container das Speicherkonto für Hadoop Cluster oder Standard. Andernfalls schlägt die Abfrage *Laden* beschweren, dass die Daten zugreifen kann.


## <a name="partition-orc"></a>Erweiterte Themen: partitionierte Tabelle und Speicher Struktur Daten ORK-Format

Große Daten ist Partitionierung der Tabelle für Abfragen, die nur einige Partitionen der Tabelle durchsuchen müssen. Beispielsweise ist es sinnvoll, Partitionieren die Daten einer Website nach dem Datum.

Neben Hive-Tabellen partitionieren, ist es auch vorteilhaft, der Struktur Datenspeicher im Format optimiert Zeile Einspaltig (ORK). Weitere Informationen zur Formatierung ORK finden Sie unter <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">verwenden ORK-Dateien verbessert die Leistung beim Struktur lesen, schreiben und Daten</a>.

### <a name="partitioned-table"></a>Partitionierte Tabelle
Hier wird die Struktur eine partitionierte Tabelle erstellt und lädt Daten in dieser Abfrage.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Beim Abfragen von partitionierter Tabellen empfohlen wird am **Anfang** die Partition Bedingung hinzufügen der `where` Klausel wie die Wirksamkeit der Suche erheblich verbessert.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Registrierungsstruktur ORK-Format speichern

Sie können nicht direkt laden Daten von BLOB-Speicher in Hive-Tabellen, die in das ORK-Format gespeichert. Hier sind die Schritte, die Sie um zu laden Daten von Azure-blobs Struktur Tabellen in ORK-Format gespeichert.

Erstellen einer externen Tabelle **Als Textdatei gespeichert** und Laden der Daten aus dem BLOB-Speicher der Tabelle.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Erstellen Sie eine Tabelle mit dem Schema der externen Tabelle in Schritt 1 das gleiche Feldtrennzeichen und Datenspeicher Struktur ORK-Format.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Daten aus der externen Tabelle in Schritt 1 ausgewählt und in die ORK-Tabelle einfügen

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

>[AZURE.NOTE] Wenn Tabelle Textdatei *& #60; Datenbankname >. & #60; Name der externen Textdatei >* Partitionen in Schritt 3 ist der `SELECT * FROM <database name>.<external textfile table name>` Befehl markiert die Partition Variable als Feld in den zurückgegebenen Daten. Einfügen in die *& #60; Datenbankname >. & #60; ORK Tabellenname >* schlägt fehl, da *& #60; Datenbankname >. & #60; ORK Tabellenname >* hat keine Partition-Variable als Feld im Schema. In diesem Fall müssen Sie insbesondere die Felder eingefügt werden sollen *& #60; Datenbankname >. & #60; ORK Tabellenname >* wie folgt:

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Gefahrlos löschen der *& #60; Name der externen Textdatei >* Wenn mithilfe der folgenden Abfrage nach allen Daten eingefügt wurde in *& #60; Datenbankname >. & #60; ORK Tabellenname >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Nach der Durchführung dieses Verfahrens benötigen Sie eine Tabelle mit Daten in dem ORK-Format verwenden.  
