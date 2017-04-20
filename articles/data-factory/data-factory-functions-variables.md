<properties 
    pageTitle="Factory Funktionen und Variablen | Microsoft Azure" 
    description="Enthält eine Liste von Azure Data Factory Funktionen und Variablen" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Data Factory - Funktionen und Variablen
Dieser Artikel enthält Informationen über Funktionen und Variablen von Azure Data Factory unterstützt.
  
## <a name="data-factory-system-variables"></a>Data Factory Systemvariablen

Name der Variablen | Beschreibung | Objektbereich | JSON-Bereich und Anwendungsfälle
------------- | ----------- | ------------ | ------------------------
WindowStart | Anfang des Zeitintervalls für aktuelle Aktivität ausführen | Aktivität | <ol><li>Geben Sie Auswahl Abfragen. Anzeigen Sie Connector Artikeln in Artikel [Datenaktivitäten](data-factory-data-movement-activities.md)</li><li>Übergeben von Parametern an Hive-Skript (Beispiel oben).</li>
WindowEnd | Ende des Zeitintervalls für aktuelle Aktivität ausführen | Aktivität | wie oben
SliceStart | Anfang des Zeitintervalls für Datenslice hergestellten | Aktivität<br/>DataSet | <ol><li>Geben Sie dynamische Pfade und Dateinamen [Azure Blob](data-factory-azure-blob-connector.md) und [Dateisystem Datasets an](data-factory-onprem-file-system-connector.md).</li><li>Geben Sie input Abhängigkeiten mit Data Factory Aktivität Inputs-Auflistung.</li></ol>
SliceEnd | Ende des Intervalls für aktuelle Datenslice hergestellten | Aktivität<br/>DataSet | dasselbe wie oben. 

> [AZURE.NOTE] Data Factory muss derzeit mit der Aktivität genau festgelegten Zeitplan Verfügbarkeit von ausgabedataset festgelegten Zeitplan übereinstimmen. Dies bedeutet, dass WindowStart, WindowEnd und SliceStart und SliceEnd immer denselben Zeitraum und eine einzelne Ausgabe Segment zuordnen.
 
## <a name="data-factory-functions"></a>Data Factory-Funktionen 

Funktionen können in Data Factory mit oben genannten Systemvariablen für folgende Zwecke Sie:

1.  Datenabfragen Auswahl angeben (Siehe Anschluss Artikeln Artikel [Datenaktivitäten](data-factory-data-movement-activities.md) .

    Die Syntax zum Aufrufen einer Data Factory-Funktion ist: ** $$ ** Auswahl Abfragen und andere Aktivitäten und Datensätze.  
2. Auflistung Eingaben input Abhängigkeiten mit Data Factory Aktivität angeben (siehe Beispiel oben).

    $$ ist nicht erforderlich für input Abhängigkeit Ausdrücke angeben.   

Im folgenden Beispiel wird **SqlReaderQuery** -Eigenschaft in einer JSON-Datei von der Funktion **Text.Format** zurückgegebene Wert zugewiesen. Dieses Beispiel verwendet auch eine Systemvariable namens **WindowStart**, die Startzeit der Aktivität Fenster darstellt.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Funktionen

Die folgende Tabelle enthält alle Funktionen in Azure Data Factory:

Kategorie | Funktion | Parameter | Beschreibung
-------- | -------- | ---------- | ----------- 
Zeit | AddHours(X,Y) | X: DateTime <br/><br/>Y: int | Zeitpunkt X hinzugefügt Y Stunden. <br/><br/>Beispiel: 9/5/2013 12:00 Uhr + 2 Stunden = 9/5/2013 2:00 Uhr
Zeit | AddMinutes(X,Y) | X: DateTime <br/><br/>Y: int | X hinzugefügt Y Minuten.<br/><br/>Beispiel: 9/15/2013 12:00 Uhr + 15 Minuten = 9/15/2013 12:15 Uhr
Zeit | StartOfHour(X) | X: Datetime | Ruft die Startzeit für die Stundenkomponente X dargestellt Stunden ab. <br/><br/>Beispiel: StartOfHour 15/9/2013 10:23 Uhr ist 15/9/2013 05:00 Uhr
Datum | AddDays(X,Y) | X: DateTime<br/><br/>Y: int | X hinzugefügt Y Tage.<br/><br/>Beispiel: 9/15/2013 12:00 Uhr + 2 Tage = 9/17/2013 12:00 Uhr
Datum | AddMonths(X,Y) | X: DateTime<br/><br/>Y: int | X hinzugefügt Y Monate.<br/><br/>Beispiel: 9/15/2013 12:00 Uhr + 1 Monat = 10/15/2013 12:00 Uhr 
Datum | AddQuarters(X,Y) | X: DateTime <br/><br/>Y: int | Fügt Y * X 3 Monate.<br/><br/>Beispiel: 9/15/2013 12:00 Uhr + 1 Quartal = 12/15/2013 12:00 Uhr
Datum | AddWeeks(X,Y) | X: DateTime<br/><br/>Y: int | Fügt Y * X 7 Tage<br/><br/>Beispiel: 9/15/2013 12:00 Uhr + 1 Woche = 9/22/2013 12:00 Uhr
Datum | AddYears(X,Y) | X: DateTime<br/><br/>Y: int | X hinzugefügt Y Jahre.<br/><br/>Beispiel: 9/15/2013 12:00 Uhr + 1 Jahr = 9/15/2014 12:00 Uhr
Datum | Day(X) | X: DateTime | Ruft die Komponente für Tage x.<br/><br/>Beispiel: Tag 9/15/2013 12:00 Uhr ist 9. 
Datum | DayOfWeek(X) | X: DateTime | Ruft die Wochentagkomponente x.<br/><br/>Beispiel: DayOfWeek 15/9/2013 ist 12:00 Uhr Sonntag.
Datum | DayOfYear(X) | X: DateTime | Ruft den Tag des Jahres die Jahreskomponente X dargestellt.<br/><br/>Beispiele:<br/>12/1/2015: Tag 335 2015<br/>12/31/2015: Tag 365 2015<br/>12/31/2016: Tag 366 2016 (Schaltjahr)
Datum | DaysInMonth(X) | X: DateTime | Ruft die Tage im Monat die Monatskomponente Parameter X dargestellt.<br/><br/>Beispiel: DaysInMonth 15/9/2013 sind 30 da 30 Tage im Monat September.
Datum | EndOfDay(X) | X: DateTime | Ruft Datum und Uhrzeit, die das Ende (Tagkomponente) x darstellt.<br/><br/>Beispiel: EndOfDay 15/9/2013 10:23 Uhr ist 15/9/2013 11:59:59 Uhr.
Datum | EndOfMonth(X) | X: DateTime | Ruft das Ende des Monats, dargestellt durch Monatskomponente Parameter X. <br/><br/>Beispiel: EndOfMonth 15/9/2013 ist 10:23 Uhr 30/9/2013 11:59:59 Uhr (Zeitpunkt, die das Ende des Monats September)
Datum | StartOfDay(X) | X: DateTime | Ruft den Beginn des Tages durch die Tagkomponente Parameter X dargestellt.<br/><br/>Beispiel: StartOfDay 15/9/2013 10:23 Uhr ist 15/9/2013 12:00 Uhr.
DateTime | FROM(X) | X: Zeichenfolge | Zeichenfolge X zu einem Zeitpunkt zu analysieren.
DateTime | Ticks(X) | X: DateTime | Ruft die Ticks-Eigenschaft des Parameters X. Ein Tick entspricht 100 Nanosekunden. Der Wert dieser Eigenschaft stellt die Anzahl der Ticks, die seit 12:00:00 Uhr, 1. Januar 0001 verstrichen sind. 
Text | Format(X) | X: String-Variablen | Formatiert den Text.

#### <a name="textformat-example"></a>Text.Format-Beispiel

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

Siehe [benutzerdefinierte Datums- und Uhrzeit-Formatzeichenfolgen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) Thema, unterschiedliche Formatierungsoptionen beschrieben, die Sie verwenden (z. B.: Yy vs. JJJJ). 

> [AZURE.NOTE] Beim Verwenden einer Funktion in eine andere Funktion, Sie müssen nicht mit **$$** für die innere Funktion Präfix. Beispiel: $$Text.Format ('PartitionKey Eq \\' My_pkey_filter_value\\"und RowKey \\" {0:yyyy-MM-Dd hh: mm:}\\'', Time.AddHours (SliceStart -6)). Beachten Sie dabei, dass **$$** Präfix wird nicht für die **Time.AddHours** -Funktion verwendet. 
  

