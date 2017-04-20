<properties 
    pageTitle="Computer lernen app: Anomalie Erkennungsdienst | Microsoft Azure" 
    description="Anomalien erkennen API ist ein Beispiel erstellt mit Microsoft Azure Maschine, die Anomalien in Serie Daten mit numerischen Werten entdeckt, die rechtzeitig einheitlich verteilt sind." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Computer lernen Anomalie Erkennungsdienst#

##<a name="overview"></a>Übersicht

[Anomalien erkennen API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) zeigt integriert Azure maschinelles lernen, die Anomalien in Serie Daten mit numerischen Werten entdeckt, die rechtzeitig einheitlich verteilt sind. 

Diese API erkennt folgende ungewöhnliche Muster Serie Daten:

* **Positive und negative Entwicklungen**: Z. B. beim Überwachen der Speicherverwendung Aufwärtstrend Computing von Interesse sein kann so auf einen Speicherverlust

* **Änderung der dynamischen Wertebereich**: Z. B. bei einem Clouddienst ausgelöste Ausnahmen Änderungen in der dynamischen Wertebereich deutet Instabilität der Zustand des Dienstes, und

* **Leistungsspitzen und taucht**: Z. B. die Anzahl der fehlgeschlagenen Anmeldeversuchen in einem Dienst oder Anzahl der Auscheckvorgänge in einer e-Commerce-Site überwachen Spitzen oder Dips deutet abnormales Verhalten.

Diese Computer lernen Detektoren überarbeiten diese Werte über Zeit und Bericht Veränderungen ihre Werte als Anomalie Faktoren. Sie benötigen keine Ad-hoc-Schwellenwert optimiert und ihre Ergebnisse können Fehlalarmrate steuern verwendet werden. API nützlich in Szenarien ist wie-Überwachung von KPIs im Laufe der Zeit verfolgen Normalbetriebswerte Überwachung der Verwendung durch wie Anzahl der Suchvorgänge, Anzahl der Klicks, Performance-Überwachung über Indikatoren wie Speicher, CPU, Datei usw. mit der Zeit.

Erkennen von Netzwerkanomalien Angebot enthält nützliche Tools zum Einstieg. 

* Die [Web-Anwendung](http://anomalydetection-aml.azurewebsites.net/) können Sie auswerten und die Ergebnisse der Normalbetriebswerte APIs Daten darstellen. 

* Der [Beispielcode](http://adresultparser.codeplex.com/) veranschaulicht die programmgesteuerten Zugriff auf die API und analysieren die Ergebnisse in C#.

>[AZURE.NOTE]
>Versuchen Sie **es Anomalie Einblicke Lösung** , ausgestattet mit [dieser API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection)
>
>Dieses Ende Lösung zum Azure Abonnement <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank">bereitgestellt **Hier starten >**</a>


##<a name="api-definition"></a>API-Definition

Der Service bietet eine REST-basierten API usw. über HTTPS auf verschiedene Arten einschließlich Web oder mobilen Anwendung R, Python, Excel verwendet werden kann.  Senden zeitreihendaten an diesen Dienst über einen REST-API-Aufruf, und führt eine Kombination der oben beschriebenen drei Anomalie. Der Dienst führt auf Azure Machine Learning Plattform skaliert nahtlos an Ihre geschäftlichen Bedürfnisse und SLAs.

###<a name="headers"></a>Kopfzeilen
Sicherstellen Sie, dass enthalten die richtigen Header in der Anforderung die wie folgt:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Ihr kontoschlüssel finden von Ihrem Konto auf dem [Markt Azure](https://datamarket.azure.com/account/keys)Sie. 

###<a name="score-api"></a>Bewertung-API

Bewertung API zum Normalbetriebswerte auf Datenreihen nicht saisonale Zeit ausführen. Die API führt eine Reihe von Anomalien müssen Daten und ihre Anomalie Ergebnisse zurückgibt. Die folgende Abbildung zeigt ein Beispiel Anomalien die Bewertung API erkennen. Diese Zeitreihen hat 2 verschiedene ändert und 3 Spitzen. Die roten Punkte zeigen die Zeit mit der Änderung die erkannt wird, schwarze Punkte erkannten Spitzen angezeigt.

![Bewertung-API][1]
    
**URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Beispiel-Anfragetext**

In der Anforderung senden wir eine Time Series (Siehe abgeschnitten) mit Datenpunkten 3/1/2016 bis 3/10/2016 und Parameter der Sammlung müssen die API zum Ermitteln, ob diese Datenpunkte außergewöhnlicher sind. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Als Reaktion darauf werden: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality API

ScoreWithSeasonality API zum Erkennen von Netzwerkanomalien auf Zeitreihe, die saisonale Muster ausführen. Diese API ist für Abweichung saisonale Muster erkennen.  

Die folgende Abbildung zeigt ein Beispiel Anomalien in saisonale Zeitreihe. Die Zeitreihe ist eine Sammlung (die 1. schwarzen Punkt), zwei Dips (2. schwarzen Punkt und am Ende) und eine Änderung (roter Punkt). Beachten Sie, dass Dip Zeitreihe und Änderung der sind nur erkennbar, wenn saisonale Komponenten aus der Reihe entfernt werden.

![Saisonbedingten API][2]

**URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Beispiel-Anfragetext**

In dem Antrag senden eine Time Series (Siehe abgeschnitten) mit Datenpunkten 3/1/2016 bis 3/10/2016 und Parameter an die API zum Ermitteln, ob diese Datenpunkte außergewöhnlicher sind. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Als Reaktion darauf werden: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Detektoren

Das Erkennen von Netzwerkanomalien API unterstützt Detektoren 3 Kategorien. Informationen über bestimmte Eingabeparameter und Ausgaben fernübertragbare finden in der folgenden Tabelle.

|Detektor Kategorie|Detektor|Beschreibung|Eingabeparameter|Ausgaben
|---|---|---|---|---|
|Spitze-Detektoren|TSpike-Erkennung|Spitzen und Dips anhand weit sind erste und dritte QUARTILE erkennen|*tspikedetector.sensitivity:* dauert Ganzzahlwert im Bereich von 1 bis 10, Standard: 3; Höhere Werte fängt mehr Extremwerte dadurch weniger|TSpike: Binärwerte – '1' Sammlung/Dip erkennt, '0' sonst|
||ZSpike-Erkennung|Spitzen und Dips abhängig, wie weit die Datenpunkten von ihrem Mittelwert werden erkannt|*zspikedetector.sensitivity:* nehmen Ganzzahlwert im Bereich von 1 bis 10, Standard: 3; Höhere Werte fängt damit weniger mehr extreme Werte|ZSpike: Binärwerte – '1' Sammlung/Dip erkennt, '0' sonst|
|Langsame Trend-Erkennung|Langsame Trend-Erkennung|Erkennen Sie langsame positiv nach Gruppe Empfindlichkeit|*trenddetector.sensitivity:* Schwellenwert auf Detektor (Standard: 3,25, 3,25 – 5 ist ein angemessener Bereich auswählen. Je weniger empfindlich)|TScore: schwebendes Anzahl Anomalie Bewertung Trend darstellt|
|Änderung Detektoren|Unidirektionale Änderung Detektors|Erkennen aufwärts nach Empfindlichkeit Satz ändern|*upleveldetector.sensitivity:* Schwellenwert auf Detektor (Standard: 3,25, 3,25 – 5 ist ein angemessener Bereich auswählen. Je weniger empfindlich)|PScore: schwebendes Zahl Anomalie Bewertung auf Aufwärts Ebene ändern|
||Bidirektionale Ebene ändern Detektors|Erkennen Sie nach oben und unten nach Gruppe Empfindlichkeit Änderung|*bileveldetector.sensitivity:* Schwellenwert auf Detektor (Standard: 3,25, 3,25 – 5 ist ein angemessener Bereich auswählen. Je weniger empfindlich)|RPScore: schwebendes Zahl die Anomalie auf nach oben und nach unten Ebene ändern

###<a name="parameters"></a>Parameter

Weitere Informationen zu diesen Eingabeparametern wird in der folgenden Tabelle aufgeführt:

|Eingabeparameter|Beschreibung|Standardeinstellung|Typ|Gültiger Bereich|Vorgeschlagene Bereich|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Intervall in Sekunden für das Aggregieren Aggregation Eingabe Zeitreihe|0 (keine Aggregation durchgeführt)|ganze Zahl|0: Aggregation > 0 andernfalls überspringen|1 Tag Zeitreihen abhängige 5 Minuten
|preprocess.aggregationFunc|Funktion zum Aggregieren von Daten in der angegebenen AggregationInterval|Mittel|Aufzählung|Mittelwert, Summe, Länge|N/A|
|preprocess.replaceMissing|-Werte werden fehlende Daten schreiben|Lkv (letzten bekannten Wert)|Aufzählung|NULL Lkv, Mittel|N/A|
|detectors.historyWindow|Geschichte (in Anzahl von Datenpunkten) Anomalie Bewertung Berechnung|500|ganze Zahl|10-2000|Zeitreihen abhängig|
|upleveldetector.Sensitivity|Empfindlichkeit für Ebene aufwärts ändern Detektors. |3,25|Doppelte|Keine|3,25 5(Lesser values mean more sensitive)|
|bileveldetector.Sensitivity|Empfindlichkeit für bidirektionale Änderung Detektor. |3,25|Doppelte|Keine|3,25 5(Lesser values mean more sensitive)|
|trenddetector.Sensitivity|Empfindlichkeit für Detektor positiv. |3,25|Doppelte|Keine|3,25 5(Lesser values mean more sensitive)|
|tspikedetector.Sensitivity |Empfindlichkeit für TSpike-Erkennung|3|ganze Zahl|1 bis 10|3 5(Lesser values mean more sensitive)|
|zspikedetector.Sensitivity |Empfindlichkeit für ZSpike-Erkennung|3|ganze Zahl|1 bis 10 |3 5(Lesser values mean more sensitive)|
|seasonality.Enable|Ob saisonbedingten Analyse durchgeführt werden soll|true|boolescher Wert|True, false|Zeitreihen abhängig|
|seasonality.numSeasonality |Maximale Anzahl von periodischen Zyklen erkannt werden|1|ganze Zahl|1, 2|1 2|
|seasonality.Transform |Ob saisonale (und) Trend Komponenten vor Normalbetriebswerte gestrichen|deseason|Aufzählung|None, deseason, deseasontrend|N/A|
|postprocess.tailRows |Anzahl der neuesten Datenpunkte zu der Ergebnisse|0|ganze Zahl|0 (alle Datenpunkte halten), oder geben Sie Anzahl der Punkte, Ergebnisse|N/A|

###<a name="output"></a>Ausgabe
Die API führt alle selbsttätigen Feuermelder müssen auf Ihrer Zeit Datenreihe und Anomalien Faktoren und binäre Sammlung Indikatoren für jeden Zeitpunkt zurückgegeben. Die folgende Tabelle listet Ausgaben der API. 

|Ausgaben|Beschreibung|
|---|---|
|Zeit|Timestamps von Rohdaten oder aggregierte (oder) Zeichen kalkulatorischen Daten Wenn Aggregation (bzw.) fehlende Daten Zuschreibung angewendet|
|OriginalData|Werte von Rohdaten oder aggregierte (oder) Zeichen kalkulatorischen Daten, wenn Aggregation (bzw.) fehlende Daten Zuschreibung angewendet|
|ProcessedData|Eines der folgenden: <ul><li>Saisonbereinigte Reihen erhebliche saisonbedingten festgestellt und deseason aktiviert;</li><li>saisonal angepasst und trendbereinigte Zeitreihe erhebliche saisonbedingten festgestellt wurde und Deseasontrend aktiviert</li><li>Andernfalls entspricht das OriginalData</li>|
|TSpike|Binärer Indikator an, ob eine TSpike Erkennung erkannt wird|
|ZSpike|Binärer Indikator an, ob eine ZSpike Erkennung erkannt wird|
|Pscore|Bewegliche Zahl Anomalie Punktzahl nach oben auf Ändern|
|Palert|1/0-Wert, der angibt, ist eine aufwärts ändern Anomalie basierend auf der Empfindlichkeit|
|RPScore|Eine bewegliche Zahl die Anomalie Punkten bidirektionale Änderung|
|RPAlert|1/0, dass ist bidirektional Wert basierend auf der Empfindlichkeit Anomalie|
|TScore|Eine bewegliche Zahl die Anomalie Punkten positiv|
|TAlert|1/0-Wert enthält, ist eine positive Entwicklung Anomalie basierend auf der Empfindlichkeit|


Diese Ausgabe kann mit einem [einfachen Parser](https://adresultparser.codeplex.com/) analysiert - Beispielcode, der zeigt, wie die API und die Ausgabe analysiert wird. Die Anomalien können auf einem Dashboard dargestellt oder menschliche Experten für Maßnahmen weitergegeben oder ticketing-Systeme integriert.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
