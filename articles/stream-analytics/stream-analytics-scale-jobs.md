<properties
    pageTitle="Stream Analytics-Aufträge zur Erhöhung des Durchsatzes skalieren | Microsoft Azure"
    description="Erfahren Sie, wie Stream Analytics-Aufträge input Partitionen konfigurieren und Optimieren der Abfragedefinition Auftrag streaming Einheiten festlegen."
    keywords="Data streaming optimieren Analytics streaming-Datenverarbeitung"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Skalieren von Azure Stream Analytics Arbeitsplätze Stream Datenverarbeitung Durchsatz erhöhen

Lernen Analytics Aufträge und Stream Analytics berechnen *streaming-Einheiten* wie Stream Analytics-Aufträge input Partitionen konfigurieren und optimieren die Abfragedefinition Analytics Auftrag streaming Einheiten festlegen. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Was sind die Teile ein Stream Analytics-Auftrag?
Eine Auftragsdefinition Stream Analytics enthält Eingaben, Abfrage und Ausgabe. Eingaben sind, in dem der Auftrag den Datenstrom liest und ausgegeben, in dem der Auftrag Auftragsergebnisse, sendet die Abfrage wird verwendet, um dem Dateneingabestream transformieren.  

Ein Auftrag muss mindestens eine Eingabequelle für Datenstreaming. Stream input Datenquelle kann in einem Azure Service Bus Event Hub oder Azure BLOB-Speicher gespeichert werden. Weitere Informationen finden Sie unter [Einführung in Azure Stream Analytics](stream-analytics-introduction.md) und [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md).

## <a name="configuring-streaming-units"></a>Konfigurieren der Streaming-Einheiten
Ressourcen und Leistung für die Ausführung eines Auftrags Azure Stream Analytics darstellen Streaming-Einheiten (SUs). SUs bieten eine Möglichkeit, die relative Verarbeitungskapazität anhand von CPU, Speicher, eine kombinierte Veranstaltung lesen und Schreibraten. Jeder Stream entspricht ungefähr 1MB/s Durchsatz. 

Wie viele SUs auswählen für ein bestimmten Auftrag hängt von der Partitionskonfiguration für Eingaben und für das Projekt definierte Abfrage erforderlich sind. Sie können Ihre Kontingents im streaming-Einheiten für einen Auftrag mithilfe der Azure-Verwaltungsportal auswählen. Jede Azure-Abonnement standardmäßig hat ein Kontingent von bis zu 50 streaming für alle Analytics-Aufträge in einer bestimmten Region. Wenden Sie sich an [Microsoft Support](http://support.microsoft.com)um Streaming-Einheiten für Ihre Abonnements zu erhöhen.

Die Streaming-Einheiten ein Auftrags verwenden kann hängt der Partitionskonfiguration für Eingaben und die Abfrage für das Projekt definiert. Beachten Sie, dass ein gültiger Wert für den Stream Einheiten verwendet werden muss. Die gültigen Werte beginnen bei 1, 3, 6 und in Schritten von 6, wie unten dargestellt.

![Azure Stream Analytics Stream Einheiten skalieren][img.stream.analytics.streaming.units.scale]

Dieser Artikel zeigt das berechnen und Optimieren Sie die Abfrage zur Erhöhung des Durchsatzes Analytics Aufträge.

## <a name="embarrassingly-parallel-job"></a>Sehr parallel Auftrag
Sehr parallele Auftrag ist das skalierbarste Szenario haben wir in Azure Stream Analytics. Er verbindet eine Partition der Eingabe einer Instanz der Abfrage auf eine Partition der Ausgabe. Diese Parallelität erfordert Folgendes:

1.  Ist die Abfragelogik von demselben Schlüssel von derselben Abfrageinstanz verarbeitet werden, müssen Sie sicherstellen, dass Ereignisse auf dieselbe Partition der Eingabe wechseln. Bei Ereignis-Hubs bedeutet dies, dass die Ereignisdaten **PartitionKey** festgelegt müssen oder partitionierte Absender verwenden. BLOB bedeutet dies, dass die Ereignisse in der gleichen Partition Ordner gesendet werden. Wenn die Abfragelogik nicht erfordert denselben Schlüssel von derselben Abfrageinstanz verarbeitet werden, können Sie diese Anforderung ignorieren. Ein Beispiel hierfür wäre eine einfache Abfrage Projekt wählen Sie Filter.  
2.  Sobald Daten wie es auf der muss angeordnet ist, müssen wir sicherstellen, dass die Abfrage partitioniert werden. Dazu müssen Sie alle Schritte **Partition By** verwenden. Mehrere Schritte können jedoch alle partitioniert werden mit demselben Schlüssel. Zu beachten ist, dass derzeit der Partitionierungsschlüssel muss vollständig parallele Arbeit auf **IDs** festgelegt werden.  
3.  Derzeit unterstützen nur Ereignis Hubs und Blob partitionierte Ausgabe. Für Ereignis Hubs Ausgabe müssen Sie Feld **PartitionKey** **IDs**zu. BLOB müssen Sie nichts.  
4.  Ein weiterer Aspekt beachten, die Anzahl der Eingabe Partitionen muss die Anzahl der Partitionen Ausgabe gleich. BLOB-Ausgabe Partitionen unterstützt derzeit nicht, aber dies ist zulässig, da das Partitionierungsschema upstream Abfrage erbt. Beispiele für Partitionswerte, die einen Auftrag vollständig parallelen ermöglichen:  
    1.  8 Ereignis Hubs Partitionen und 8 Ereignis Hubs-Ausgabe Partitionen
    2.  8 Ereignis Hubs geben Partitionen und BLOB-Ausgabe  
    3.  8 Blob input Partitionen und BLOB-Ausgabe  
    4.  8 Blob input Partitionen und 8 Ereignis Hubs Ausgabe Partitionen  

Hier sind einige Beispielszenarien peinlich parallel verlaufen.

### <a name="simple-query"></a>Einfache Abfrage
Eingabe-Ereignis Hubs mit 8 Partitionen Ausgabe-Ereignis Hub mit 8 Partitionen

**Abfrage:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Diese Abfrage ist ein einfacher Filter und so wir brauchen Partitionierung der Eingabe senden wir Ereignis Hubs sorgen. Sie werden feststellen, dass die Abfrage- **Partition von** **IDs**, damit wir #2 von oben Bedarfs. Für die Ausgabe müssen wir Ereignis Hubs Ausgabe im Auftrag der **PartitionKey** Feld **PartitionId**festgelegt. Ein Letzter Scheck, input Partitionen == Ausgabe Partitionen. Diese Topologie ist sehr parallel.

### <a name="query-with-grouping-key"></a>Abfrage mit Gruppierungsschlüssel
Eingabe-Ereignis Hubs mit 8 Partitionen Ausgabe-Blob

**Abfrage:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Diese Abfrage hat einen Gruppierungsschlüssel und denselben Schlüssel muss von derselben Abfrageinstanz verarbeitet werden. Dies bedeutet, dass wir unsere Ereignisse partitionierte an Ereignisse Hubs übermitteln müssen. Kümmern welcher wir? **IDs** ist ein Auftrag Logik ist entscheidend wichtig **TollBoothId**. Daher sollten wir die **PartitionKey** Festlegen der Ereignisdaten wir senden an Event Hubs zu **TollBoothId** des Ereignisses. Die Abfrage hat **Partition By** **PartitionId**wir gut. Für die Ausgabe ist Blob, müssen wir nicht konfigurieren **PartitionKey**. Für Anforderung #4 ist das Blob müssen wir nicht kümmern. Diese Topologie ist sehr parallel.

### <a name="multi-step-query-with-grouping-key"></a>Abfrage mit Gruppierungsschlüssel Multi-Schritt ###
Eingabe-Ereignis Hub mit 8 Partitionen Ausgabe-Ereignis Hub mit 8 Partitionen

**Abfrage:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Diese Abfrage hat einen Gruppierungsschlüssel und denselben Schlüssel muss von derselben Abfrageinstanz verarbeitet werden. Wir können dieselbe Strategie wie die vorherige Abfrage verwenden. Die Abfrage umfasst mehrere Schritte. Muss jeder Schritt **Partition By** **PartitionId**? Ja, wir sind. Für die Ausgabe wir müssen **PartitionKey** , **IDs** wie oben beschriebenen und wir sehen auch hat dieselbe Anzahl an Partitionen als Eingabe. Diese Topologie ist sehr parallel.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Beispielszenarien nicht sehr parallel

### <a name="mismatched-partition-count"></a>Nicht übereinstimmende Anzahl von Partitionen ###
Eingabe-Ereignis Hubs mit 8 Partitionen Ausgabe-Ereignis Hub mit 32 Partitionen

Es spielt keine Rolle, welche die Abfrage in diesem Fall liegt der Eingaben partitionieren Anzahl! = Partitionsanzahl Ausgabe.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Ereignis-Hubs oder Blobs verwenden nicht als Ausgabe
Eingabe-Ereignis Hubs mit 8 Partitionen Ausgabe – PowerBI

PowerBI Ausgabe unterstützen nicht aktuell Partitionierung.

### <a name="multi-step-query-with-different-partition-by-values"></a>Mehrere Schritt Abfrage mit unterschiedlichen Werten Partition By
Eingabe-Ereignis Hub mit 8 Partitionen Ausgabe-Ereignis Hub mit 8 Partitionen

**Abfrage:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Wie Sie sehen können, verwendet der zweite Schritt **TollBoothId** als Partitionierungsschlüssel dar. Dies entspricht nicht der erste Schritt und erfordert daher uns eine zufällige Wiedergabe. 

Dies sind einige Beispiele und gegenbeispiele Stream Analytics-Aufträge, die zu einer sehr parallel Topologie und der höchste mögliche. Für Aufträge, die nicht eines dieser Profile, zukünftige Updates Details wie maximal die kanonische Stream Analytics Szenarien erfolgen.

Jetzt machen verwenden die folgende Faustregel:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Berechnet die maximale Einheiten eines Auftrags streaming
Die Gesamtzahl der Streaming-Einheiten, die durch einen Stream Analytics-Auftrag verwendet werden hängt die Anzahl der Schritte in der Abfrage für das Projekt und die Anzahl der Partitionen für jeden Schritt definiert.

### <a name="steps-in-a-query"></a>Schritte in einer Abfrage
Eine Abfrage kann eine oder mehrere Schritte haben. Jeder Schritt besteht aus einer Unterabfrage mit dem **WITH** -Schlüsselwort definiert. Nur Abfrage außerhalb das **WITH** -Schlüsselwort zählt auch als Schritt, z. B. die **SELECT** -Anweisung in der folgenden Abfrage:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Die vorherige Abfrage umfasst zwei Schritte.

> [AZURE.NOTE] Diese Beispielabfrage werden später in diesem Artikel erläutert.

### <a name="partition-a-step"></a>Partition ein

Partitionierung Schritt ist Folgendes erforderlich:

- Die Eingabequelle partitioniert. Weitere Informationen finden Sie unter [Event Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).
- Die **SELECT** -Anweisung der Abfrage muss aus einer partitionierten Datenquelle lesen.
- Die Abfrage im Schritt müssen das **Partition By** -Schlüsselwort

Wenn eine Abfrage partitioniert, die Eingabeereignisse werden verarbeitet und aggregierte in separaten Partitionsgruppen und Ausgaben Ereignisse werden generiert für jede der Gruppen. Kombinierte Aggregat erwünscht ist, muss erstellt einen zweiten Schritt nicht partitionierten zusammenfassen.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Berechnen der Max streaming-Einheiten für einen Auftrag

Alle nicht partitionierten Schritte können bis zu sechs Streaming-Einheiten für einen Auftrag für Stream Analytics zusammen skalieren. Dazu muss zusätzliche streaming-Einheiten ein Schritt partitioniert werden. Jede Partition haben sechs Streaming-Einheiten.

<table border="1">
<tr><th>Abfrage eines Auftrags</th><th>Max. Einheiten für den Auftrag streaming</th></td>

<tr><td>
<ul>
<li>Die Abfrage enthält einen Schritt.</li>
<li>Der Schritt ist nicht partitioniert.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Der Eingabedatenstrom ist in 3 partitioniert.</li>
<li>Die Abfrage enthält einen Schritt.</li>
<li>Der Schritt ist partitioniert.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Die Abfrage enthält zwei Schritte.</li>
<li>Weder die Schritte unterteilt ist.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>Die Dateneingabe Stream ist in 3 partitioniert.</li>
<li>Die Abfrage enthält zwei Schritte. Input Schritt partitioniert und im zweite Schritt nicht.</li>
<li>Die SELECT-Anweisung liest der partitionierten Eingabe.</li>
</ul>
</td>
<td>24 (18 partitionierte Schritte) + 6 nicht partitionierten Schritte</td></tr>
</table>

### <a name="examples-of-scale"></a>Beispiele für Skalierung
Die folgende Abfrage berechnet die Anzahl der durch eine Mautstation mit drei Mautstationen innerhalb von drei Minuten ein. Diese Abfrage kann bis zu sechs streaming skaliert werden.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Um weitere Streaming-Einheiten für die Abfrage verwenden, sowohl der Datenstrom Eingabe und Abfrage partitioniert werden. Da Stream Datenpartition auf 3 festgelegt ist, kann die folgende geänderte Abfrage bis 18 Streaming-Einheiten skaliert werden.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Wenn eine Abfrage partitioniert, die Eingabeereignisse verarbeitet und in separaten Partitionsgruppen aggregiert. Für jede dieser Gruppen werden ebenfalls Ausgabeereignisse generiert. Partitionieren kann unerwartete Ergebnisse, wenn Feld **Group by** nicht Partitionsschlüssel im Stream Dateneingabe. Beispielsweise ist das **TollBoothId** -Feld in der vorherigen Beispielabfrage nicht Partition Schlüssel Input1. Die Daten aus der Mautstation 1 können in mehrere Partitionen verteilt werden.

Alle Partitionen Input1 wird die Verarbeitung von Stream Analytics und mehrere Datensätze Anzahl Auto-Pass-through für dieselbe Mautstation im selben Tumbling Fenster erstellt. Wenn input Partitionsschlüssel geändert werden kann, kann dieses Problem behoben werden, beispielsweise einen zusätzlichen Schritt nicht Partition hinzufügen:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Diese Abfrage kann auf 24 streaming-Einheiten skaliert.

>[AZURE.NOTE] Verknüpfen von zwei Streams sicher, dass der Partitionsschlüssel der Spalte, dass Sie Verknüpfungen, und die gleiche Anzahl von Partitionen in beiden Streams haben Streams partitioniert werden.


## <a name="configure-stream-analytics-job-partition"></a>Stream Analytics Auftrag Partition konfigurieren

**Streaming-Einheit für ein Projekt anpassen**

1. Melden Sie sich im [Verwaltungsportal](https://manage.windowsazure.com)an.
2. Klicken Sie im linken Bereich auf **Stream Analytics** .
3. Klicken Sie auf Stream Analytics-Projekt, das Sie skalieren möchten.
4. Klicken Sie am oberen Rand der Seite **Skalieren** .

![Azure Stream Analytics Stream Einheiten skalieren][img.stream.analytics.streaming.units.scale]

Im Portal Azure Einstellungen unter Einstellungen möglich:

![Azure-Portal Stream Analytics Konfiguration][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Leistung überwachen

Mit Verwaltungsportal können Sie den Durchsatz eines Auftrags in Ereignissen pro Sekunde verfolgen:

![Azure Stream Analytics Überwachen von Einzelvorgängen][img.stream.analytics.monitor.job]

Berechnen des erwarteten Durchsatzes der Arbeitslast in Ereignissen pro Sekunde. Ist der Durchsatz geringer sein optimieren Sie Eingabe Partition optimieren Sie die Abfrage und Ihre Arbeit fügen Sie zusätzliche Streaming-Einheiten hinzu.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Stream Analytics Durchsatz Ebene - Himbeeren Pi-Szenario


Um zu verstehen, wie Stream Analytics-Aufträge in einer Situation im Hinblick auf Verarbeitung Durchsatz über mehrere Streaming-Einheiten skaliert, ist hier ein Experiment, die Sensordaten (Clients) an Event Hub sendet, verarbeitet und Warnung oder Statistiken als Ausgabe mit einem anderen Ereignis Hub sendet.

Der Client sendet synthetisierte Sensordaten an Event Hubs im JSON-Format Stream Analytics und die Datenausgabe wird auch im JSON-Format.  Die Beispieldaten aussehen wie – würde  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Abfrage: "Alarm senden ausgeschalteten Lichts"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Durchsatz messen: Durchsatz in diesem Zusammenhang ist die Eingabedaten in eine feste Zeitspanne (10 Minuten) von Stream Analytics verarbeitet. Um optimale Verarbeitung Durchsatz für die Eingabedaten sowohl der Datenstrom Eingabe und Abfrage partitioniert werden. **COUNT()**steht auch in der Abfrage zu messen, wie viele Eingabeereignisse verarbeitet wurden. Um sicherzustellen, dass der Auftrag nicht einfach input Ereignisse wartet, wurde jeder Partition des Eingabe-Ereignis mit ausreichend Eingabedaten (ca. 300MB) geladen.  

Im folgenden sind die Ergebnisse mit zunehmender Anzahl der Streaming-Einheiten und entsprechende Partition im Ereignis Hubs zählt.  

<table border="1">
<tr><th>Input-Partitionen</th><th>Ausgabe-Partitionen</th><th>Streaming-Einheiten</th><th>Konstanter Datendurchsatz
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8,06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Hilfe
Versuchen Sie [Azure Stream Analytics Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)für weitere Unterstützung.


## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
