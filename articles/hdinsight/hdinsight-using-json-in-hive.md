<properties
   pageTitle="Analysieren und Prozess JSON Dokumente mit Struktur in HDInsight | Microsoft Azure"
   description="Informationen zu JSON-Dokumente analysieren mit Struktur in HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Verarbeiten und Analysieren von JSON-Dokumente mit Struktur in HDInsight

Informationen Sie zum Verarbeiten und Analysieren von JSON-Dateien mit Struktur in HDInsight. Im Lernprogramm werden folgende JSON-Dokument verwendet werden

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Die Datei finden Sie unter wasbs://processjson@hditutorialdata.blob.core.windows.net/. Weitere Informationen zum Verwenden von Azure BLOB-Speicher mit HDInsight finden Sie unter [verwenden bietet kompatible Azure BLOB-Speicher Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md). Wenn Sie möchten, können Sie die Datei zum Standardcontainer des Clusters kopieren.

In diesem Lernprogramm verwenden Sie Struktur-Konsole.  Informationen die Hive-Konsole öffnen finden Sie in der [Struktur mit Hadoop auf HDInsight mit Remotedesktop](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>JSON-Dokumente zusammenfassen

Im nächsten Abschnitt beschriebenen Methoden erfordern das JSON-Dokument in einer einzelnen Zeile. So müssen Sie das JSON-Dokument in eine Zeichenfolge zusammenfassen. Wenn JSON Dokument bereits vereinfacht wird, können Sie diesen Schritt überspringen und direkt zum nächsten Abschnitt weitergehen Analyse von JSON-Daten.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Unformatierte JSON-Datei befindet sich unter **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. Die Tabelle *StudentsRaw* Struktur verweist auf raw nicht reduzierte JSON-Dokument.

Die Tabelle *StudentsOneLine* Struktur speichert Daten in HDInsight Standard-Dateisystem unter dem Pfad */json/Studenten* .

Die INSERT-Anweisung Füllen der StudentOneLine mit vereinfachten JSON-Daten.

Die SELECT-Anweisung wird nur 1 Zeile zurück.

Hier ist die Ausgabe der SELECT-Anweisung:

![JSON-Dokuments reduzieren.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>JSON-Dokumente Struktur analysieren

Struktur bietet drei unterschiedliche Mechanismen JSON-Dokumente Abfragen:

- Verwenden Sie die GET\_JSON\_Objekt UDF (benutzerdefinierte Funktion)
- Verwenden Sie das UDF JSON_TUPLE
- Verwenden von benutzerdefinierten SerDe
- Schreiben Sie eigene UDF Python oder in anderen Sprachen. [Artikel] [ hdinsight-python] Struktur unter Python-Code ausführen.

### <a name="use-the-getjsonobject-udf"></a>Verwenden Sie die GET\_JSON_OBJECT UDF
Struktur bietet die integrierte UDF [Json Objekt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) die JSON Abfragen zur Laufzeit durchführen können. Diese Methode nimmt zwei Argumente – Tabelle und Methodennamens zusammengefasste JSON-Dokument und JSON-Feld, das analysiert werden soll. Betrachten wir ein Beispiel, wie diese UDF funktioniert.

Abrufen von vor- und Nachnamen für Studenten

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Hier ist die Ausgabe dieser Abfrage im Konsolenfenster.

![Get_json_object UDF][image-hdi-hivejson-getjsonobject]

Es gibt einige Einschränkungen UDF Get-Json_object.

- Da jedes Feld in der Abfrage erneut analysieren der Abfrage muss, beeinträchtigt die Leistung.
- Abrufen\_JSON_OBJECT() gibt die Zeichenfolge eines Arrays. Zum Konvertieren in ein Array Struktur müssen reguläre Ausdrücke verwenden, um die Klammern ersetzen ' [' und ']' und dann auch teilen, um das Array abzurufen.


Deshalb Struktur Wiki empfiehlt Json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Verwenden Sie das UDF JSON_TUPLE

Ein anderes UDF bereitgestellten Struktur wird [Json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) aufgerufen, die besser [Get_ Json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object)führt. Diese Methode akzeptiert eine Reihe von Schlüsseln und eine JSON-Zeichenfolge und gibt ein Tupel mithilfe einer Funktion. Die folgende Abfrage gibt die Studenten-Id und die Klasse aus dem JSON-Dokument:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Die Ausgabe dieses Skripts in der Hive-Konsole:

![Json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_TUPEL verwendet die [Seitenansicht](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) Syntax im Hive Json ermöglicht\_Tupel erstellt eine virtuelle Tabelle durch Anwenden der Funktion UDT in jeder Zeile der ursprünglichen Tabelle.  Komplexe JSONs werden durch die wiederholte Verwendung der SEITENANSICHT zu schwerfällig. Außerdem kann JSON_TUPLE nicht geschachtelten JSONs behandeln.


###<a name="use-custom-serde"></a>Verwenden von benutzerdefinierten SerDe

SerDe ist die beste Wahl für verschachtelte JSON-Dokumente analysiert, können definieren JSON-Schema und das Schema der Dokumente analysieren. In dieser praktischen Einführung verwenden eines der beliebtesten SerDe Sie, die von [Rcongiu](https://github.com/rcongiu)entwickelt wurde.

**Mithilfe von benutzerdefinierten SerDe**

1. Installieren Sie [Java SE Development Kit 7u55 JDK-1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Entscheiden Sie Windows X64 Version des JDK, ob Sie die Windows-Bereitstellung von HDInsight verwenden

    >[AZURE.WARNING] JDK 1,8 funktioniert nicht mit diesem SerDe.

    Fügen Sie nach der Installation neue Umgebungsvariablen:

    1. Öffnen Sie Windows-Bildschirm **Erweiterte Systemeinstellungen anzeigen** .
    2. Klicken Sie auf **Umgebungsvariablen**.  
    3. Fügen Sie eine neue Umgebungsvariable **JAVA_HOME** zeigen **C:\Program Files\Java\jdk1.7.0_55** oder wo die JDK installiert ist.

    ![Richtig konfigurierte Werte einrichten für JDK][image-hdi-hivejson-jdk]

2. [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip) installieren

    Pfad zu Steuerelement Ordner Bin hinzufügen Bereich--> Bearbeiten Systemvariablen für Variablen Environment Konto. Der folgende Screenshot zeigt dazu.

    ![Einrichten von Maven][image-hdi-hivejson-maven]

3. Klonen Sie das Projekt [Struktur JSON SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) Github Website. Hierzu können Sie auf die Schaltfläche "Download Zip", wie im folgenden Screenshot gezeigt.

    ![Klonen des Projekts][image-hdi-hivejson-serde]

4: Wechseln zu dem Ordner, in dem dieses Paket und geben "Mvn Package" heruntergeladen haben. Dies sollte erforderlichen JAR-Dateien erstellen, die dann über den Cluster kopiert.

5: Ziel Ordner im Stammordner, in dem das Paket heruntergeladen. Laden Sie die json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar Datei Head-Knoten des Clusters. In der Regel die binäre Struktur-Ordner abgelegt: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin oder ähnliches.

6: Aufforderung Struktur geben "add JAR-/path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar". Da in meinem Fall die JAR-Datei im Ordner "C:\apps\dist\hive-0.13.x\bin" ist, kann ich die JAR-Datei mit dem Namen direkt hinzufügen, wie unten dargestellt:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

Jetzt können Sie die SerDe verwenden, um Abfragen für JSON-Dokument ausführen.

Die folgende Anweisung erstellt eine Tabelle mit einem definierten schema

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Vor- und Nachnamen des Studenten aufgelistet

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Hier ist das Ergebnis der Hive-Konsole.

![SerDe Abfrage 1][image-hdi-hivejson-serde_query1]

Berechnet die Summe der Ergebnisse das JSON-Dokument

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Die obige Abfrage verwendet [seitlicher Ansicht explodieren](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF, erweitern das Array der Punkte zusammengefasst werden können.

Hier ist die Ausgabe der Hive-Konsole.

![SerDe Abfrage 2][image-hdi-hivejson-serde_query2]

Wählen zu bestimmten Student mehr als 80 Punkte hat Fächer  
      JT StudentClassCollection.ClassId von Json_table Jt seitlicher Ansicht explodieren (JT StudentClassCollection.Score) Auflistung wie dem Faktor > 80.

Die obige Abfrage gibt ein Array der Struktur im Gegensatz zu erhalten\_Json\_Objekt gibt eine Zeichenfolge zurück.

![SerDe Abfrage 3][image-hdi-hivejson-serde_query3]

Möchten Sie Skil fehlerhafte JSON dann wie in der [Wiki-Seite](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) dieses SerDe Sie dafür können durch den folgenden Code eingeben:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Zusammenfassung
Abschließend hängt Szenario die Art der JSON-Operator in der Registrierungsstruktur, die Sie auswählen. Einen einfachen JSON-Dokument und Sie nur ein Feld Suchen – Sie können auch die Struktur UDF Get\_Json\_Objekt. Haben Sie mehrere Tasten auf nachschlagen können Sie Json_tuple verwenden. Haben Sie ein verschachteltes Dokument sollten Sie das JSON-SerDe verwenden.

Andere verwandte Artikel finden Sie unter

- [Mit der Struktur und HiveQL in HDInsight Hadoop eine Beispieldatei Apache log4j analysieren](hdinsight-use-hive.md)
- [Analysieren Sie Verzögerung Daten mit Struktur in HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Analysieren Sie Twitter-Daten mithilfe von Struktur in HDInsight](hdinsight-analyze-twitter-data.md)
- [Eine Stapelverarbeitung Hadoop mit DocumentDB und HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
