<properties
    pageTitle="Skalieren der Stream Analytics-Auftrag mit Azure Machine Learning | Microsoft Azure"
    description="Informationen zum Stream Analytics-Aufträge (Partitionierung, SU Menge usw.) richtig skaliert bei Azure Machine Learning Funktionen."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Skalieren der Stream Analytics-Auftrag mit Azure maschinelles lernen

Oft ist es ganz einfach einen Stream Analytics-Auftrag und Beispieldaten durchlaufen. Was tun wir, wenn wir die gleiche Aufgabe mit höheren Datenvolumen müssen? Es erfordert Kenntnisse im Stream Analytics-Auftrags so konfigurieren, dass skaliert werden. In diesem Dokument liegt der Schwerpunkt auf die Besonderheiten der Skalierung Stream Analytics-Aufträge mit Computerlernen. Informationen wie Stream Analytics-Aufträge im Allgemeinen finden Sie unter [Skalierung Aufträge](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Was ist eine Funktion Azure Machine Learning Stream Analytics?

Computer Lernfunktion in Stream Analytics kann wie einem regulären Funktionsaufruf in der Abfragesprache Stream Analytics verwendet werden. Allerdings sind die Funktionsaufrufe hinter der Szene tatsächlich Azure Machine Learning Web Service Requests. Computer lernen WebServices unterstützen "Batchverarbeitung" mehrere Zeilen Mini-Stapel in der gleichen API Webdienstaufrufs Gesamtdurchsatz zu heißt. Finden Sie in folgenden Artikeln Weitere Informationen; [Azure Machine Learning-Funktionen in Stream Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) und [Azure Machine Learning Web Services](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Konfigurieren eines Auftrags Stream Analytics mit Computerlernen

Beim Konfigurieren einer Maschine Lernfunktion Stream Analytics-Auftrags werden zwei Parameter müssen die Batchgröße von Funktionsaufrufen maschinelles lernen und Streaming-Einheiten (SUs) für den Auftrag Stream Analytics bereitgestellt. Zum Ermitteln der entsprechenden Werte für diese muss zuerst eine Entscheidung Latenz und der Durchsatz, also Wartezeit der Stream Analytics und Durchsatz der einzelnen SU unterscheiden. SUs können immer einen Auftrag zur Erhöhung des Durchsatzes einer gut partitionierten Abfrage Stream Analytics zusätzliche SUs erhöht die Kosten für das Projekt hinzugefügt.

Daher ist es wichtig festzustellen, die *Toleranz* der Latenz in einen Stream Analytics-Auftrag ausgeführt. Zusätzlicher Latenz Azure Machine Learning Serviceanfragen ausgeführt wird natürlich mit Batchgröße Wartezeit Stream Analytics-Auftrags zusammengesetzte erhöhen. Auf der anderen Seite Stapelgröße erhöhen kann Stream Analytics-Auftrags zu *mehr Ereignisse mit der *dieselbe Anzahl * der Computerlernen Web Service-Requests. Häufig ist der Anstieg der Computerlernen Web Service Wartezeit Sub linear zu Batchgröße so ist es wichtig, die kostengünstigste Batchgröße für einen Webdienst maschinelles lernen in jeder Situation. Stapel Standardgröße für Web Service-Requests 1000 und kann entweder mit der [Stream Analytics REST API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST-API") oder [PowerShell Client für Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShell Client für Stream Analytics")geändert werden.

Eine Batchgröße festgelegt, der Betrag von Streaming-Einheiten (SUs) bestimmt werden, basierend auf der Anzahl der Ereignisse, die die Funktion muss pro Sekunde verarbeiten. Weitere Informationen zu Streaming-Einheiten finden Sie im Artikel [Stream Analytics Skalierung Aufträge](stream-analytics-scale-jobs.md#configuring-streaming-units).

Im Allgemeinen werden 20 gleichzeitige Verbindungen zum Webdienst maschinelles lernen für alle SUs 6, 1 SU und 3 SU Einzelvorgänge 20 gerätebasierte auch erhalten  Wenn die Eingabedaten Rate 200.000 Ereignisse pro Sekunde und die Batchgröße der Standardwert 1000 ist die resultierende Web Service Wartezeit mit 1000 Ereignisse Mini-200 ms. Daher, jede Verbindung 5 Anfragen zu Machine Learning-Webdienst in einer Sekunde. Mit 20 verarbeiten Stream Analytics-Auftrags 20.000 in 200 ms und daher 100.000 Ereignisse in einer Sekunde. So benötigt zum Verarbeiten von 200.000 Ereignisse pro Sekunde Stream Analytics-Auftrags 40 gerätebasierte 12 SUs kommt. Das folgende Diagramm zeigt Anfragen von Stream Analytics-Auftrag an den Webdienst-Endpunkt maschinelles lernen – alle 6 SUs hat 20 gleichzeitige Zugriffe auf Computer Learning-Webdienst Max.

![Skalierung Stream Analytics Machine Learning Funktionen 2 Job-Beispiel] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Skalierung Stream Analytics Machine Learning Funktionen 2 Job-Beispiel")

Im Allgemeinen ist ***B*** für die Batchgröße, ***L*** für Web Service-Wartezeit zu Batchgröße B in Millisekunden Durchsatz Stream Analytics-Auftrag mit ***N*** SUs:

![Stream Analytics mit Funktionen Formel lernen skalieren] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Stream Analytics mit Funktionen Formel lernen skalieren")

Eine zusätzliche Überlegung kann die 'Max. gleichzeitige Aufrufe"Web Service auf Computerlernen, empfahl Maximalwert (derzeit 200) festlegen.

Weitere Informationen zu dieser Einstellung finden Sie [Skalierung Artikel Computer Learning Web Services](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Beispiel – Stimmungsanalyse

Das folgende Beispiel enthält einen Druckauftrag Stream Analytics stimmungsanalyse Computer Lernfunktion im [Stream Analytics maschinelles lernen Integration Lernprogramm](stream-analytics-machine-learning-integration-tutorial.md)beschriebenen.

Die Abfrage ist eine einfache Funktion **Stimmung** gefolgt vollständig partitionierten Abfrage wie folgt:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Das folgende Szenario; Ein Stream Analytics-Auftrag muss mit einem Durchsatz von 10.000 Tweets pro Sekunde um Stimmung Analyse TWEETS (Ereignisse) erstellt werden. Mit 1 SU, konnte dieser Stream Analytics-Auftrag den Datenverkehr verarbeiten können? Verwenden die standardmäßige Stapelgröße von 1000 sollte der Auftrag können mit der Eingabe. Weitere sollte hinzugefügte Computer Learning-Funktion generieren nicht mehr als eine Sekunde Latenz ist Standard Wartezeit Stimmung Analyse Machine Learning-Webdienst (mit Batch Standardgröße von 1000). Stream Analytics-Auftrag **insgesamt** oder End-to-End-Latenz wäre normalerweise einige Sekunden. Sehen Sie detailliertere in diesen Stream Analytics-Auftrag *besonders* Computer lernen Funktionsaufrufe. Haben die Stapelgröße 1000, dauert ein Durchsatz von 10.000 Ereignisse zu 10 Anfragen an WebService. Selbst bei 1 SU sind genügend gerätebasierte eingehender Verkehr aufzunehmen.

Aber was ist, wenn das Eingabeereignis steigt von 100 X und Stream Analytics-Auftrags muss nun 1.000.000 Tweets pro Sekunde verarbeiten? Es gibt zwei Optionen:

1.  Erhöhen Sie die Batchgröße oder
2.  Partition Eingabestream Ereignisse parallel verarbeiten

Mit der ersten Option wird Auftrag **Latenz** erhöht.

Mit der zweiten Option müssten mehrere SUs bereitgestellt werden und daher mehr gleichzeitige maschinelles lernen Web Service-Requests. Dies bedeutet, dass die Job- **Kosten** steigen.


Angenommen Sie, die Latenz stimmungsanalyse Machine Learning-Webdienst 200 ms für 1000 Ereignisbatches oder unter 250ms für 5.000-Ereignisbatches, 300ms für 10.000 Ereignisbatches oder 500 ms für 25.000 Ereignisbatches.

1. Mit der ersten Option (**nicht** mehr SUs-Bereitstellung) werden die Batchgröße **25.000**gesteigert. Dies würde wiederum Auftrag 1.000.000 Ereignisse mit 20 gleichzeitige Machine Learning-Webdienst (mit einer Wartezeit von 500 ms pro Aufruf) zu ermöglichen. So würde die zusätzliche Latenz Stream Analytics-Auftrags durch Stimmung Funktion Anfragen an Computer Learning Web Service-Requests auf **500 ms**von **200 ms** erhöht. Hinweis Die batch-Größe **kann nicht** jedoch unendlich mehr als Webdienste maschinelles lernen erfordert die Nutzlastgröße einer Anforderung 4 MB oder kleiner Webdienst Timeout nach 100 Sekunden des Vorgangs anfordert.
2. Verwenden die zweite Option, die Batchgröße bleibt bei 1000, mit 200 ms Web Service Wartezeit, alle 20 aktiven Verbindungen mit dem Webdienst 1000 *20* 5 Ereignisse verarbeiten = 100.000 pro Sekunde. So um 1.000.000 Ereignisse pro Sekunde verarbeitet müssten der Auftrag 60 SUs. Im Vergleich zu der ersten Option, würde Stream Analytics-Auftrag mehrere Web Service Batchanforderungen wiederum höhere Kosten generieren.

Unten ist eine Tabelle für den Durchsatz des Auftrags Stream Analytics für andere SUs und Batchgrößen (in Anzahl von Ereignissen pro Sekunde).

| Stapelgröße (ML Wartezeit)  | 500 (200 ms) | 1.000 (200 ms) | 5.000 (250ms) | 10.000 (300ms) | 25.000 (500 ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SU** | 2.500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **3 SUs** | 2.500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **6 SUs** | 2.500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **12 SUs** | 5.000 | 10.000 | 40.000 | 60.000 | 100.000 |
| **18 SUs** | 7.500 | 15.000 | 60.000 | 90.000 | 150.000 |
| **24 SUs** | 10.000 | 20.000 | 80.000 | 120.000 | 200.000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25.000 | 50.000 | 200.000 | 300.000 | 500.000 |

Jetzt haben Sie bereits verstanden wie Computerlernen im Stream Analytics funktioniert. Sie wissen wahrscheinlich auch, dass Stream Analytics Aufträge 'Pull' Daten aus Datenquellen und jede 'Pull' gibt einen Batch von Ereignissen für den Stream Analytics-Auftrag zu. Wie web diese Pull Model Auswirkung der Serviceanfragen?

Normalerweise wird nicht die Batchgröße für maschinelles lernen Funktionen setzen wir genau durch die Anzahl der Ereignisse zurückgegebene Stream Analytics Auftrags 'Pull' teilbar sein. In diesem Fall "partial" Stapel Machine Learning-Webdienst aufgerufen wird. Dies entstehen keine zusätzliche Arbeit Wartezeit Overhead zusammenfügende Ereignisse ziehen zu ziehen.

## <a name="new-function-related-monitoring-metrics"></a>Neue Funktion bezogenen Überwachung Kennzahlen

Im Bereich Überwachung eines Auftrags Stream Analytics wurden drei zusätzliche Funktion bezogene Statistiken hinzugefügt. Sie sind Funktion Anfragen, Funktion Ereignisse und Fehler Funktion Anfragen wie in der nachfolgenden Grafik dargestellt.

![Stream Analytics mit Funktionen Metriken lernen skalieren] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Stream Analytics mit Funktionen Metriken lernen skalieren")

Sind wie folgt definiert:

**Funktion Anfragen**: Anzahl der Funktion Abfragen.

**Funktion Ereignisse**: die Anzahl der Ereignisse Funktion Anfragen.

**Fehler bei Funktion Anfragen**: die Anzahl der fehlgeschlagenen Funktion Anfragen.

## <a name="key-takeaways"></a>Hauptargumente  

Die wichtigsten Punkte zusammengefasst um ein Stream Analytics-Auftrag mit Computerlernen skalieren müssen Folgendes berücksichtigt werden:

1.  Das Eingabeereignis rate
2.  Die zulässigen Wartezeit der ausgeführte Auftrag für Stream Analytics (und die Batchgröße maschinelles lernen Web Service Requests)
3.  Bereitgestellte Stream Analytics SUs und die Anzahl der Computerlernen Web Service-Requests (zusätzlichen Funktion bezogenen Kosten)

Eine vollständig partitionierte Abfrage Stream Analytics wurde als Beispiel verwendet. Eine komplexe Abfrage ggf. [Azure Stream Analytics Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) ist eine großartige Ressource für weitere Hilfe aus dem Stream Analytics-Team.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Stream Analytics finden Sie unter:

- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
