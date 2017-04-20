<properties
    pageTitle="Verwenden Sie Daten und Lookup Tabellen in Stream Analytics | Microsoft Azure"
    description="Verwenden von Daten in einer Abfrage Stream Analytics"
    keywords="Nachschlagetabelle Referenzdaten"
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

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Daten oder Lookup Tabellen verwenden in einen Stream Analytics Eingabestream

Daten (auch bekannt als Nachschlagetabelle) ist eine begrenzte statischen Datensatz oder verlangsamen Natur ändern eine Suche durchführen oder Ihre Datenstrom zuordnen. Zum Nutzen von Referenzdaten in Azure Stream Analytics-Auftrag werden im Allgemeinen verwenden ein [Verweis Daten verknüpfen](https://msdn.microsoft.com/library/azure/dn949258.aspx) in Ihrer Abfrage. Stream Analytics verwendet Azure BLOB-Speicher als die Speicherschicht für Referenzdaten und Azure Data Factory Bezug Daten transformiert oder [eine beliebige Anzahl von Cloud basiert](../data-factory/data-factory-data-movement-activities.md)und lokalen Datenspeicher in Azure BLOB-Speicher für die Verwendung als Verweis kopiert werden können. Referenzdaten werden als eine Folge von Blobs (in der Konfiguration definiert) in aufsteigender Reihenfolge der BLOB-Name angegebenen Zeit modelliert. Es unterstützt **nur** am Ende der Sequenz mit Zeitpunkt **größer** als der von der letzten Blob in der Sequenz hinzufügen.

## <a name="configuring-reference-data"></a>Konfigurieren von Daten

Konfigurieren Sie die Referenzdaten müssen Sie zu Typ **Daten**ein. In der folgenden Tabelle wird jede Eigenschaft, die Sie beim Erstellen der Referenz Dateneingabe Beschreibung bereitstellen müssen erläutert:

<table>
<tbody>
<tr>
<td>Eigenschaftenname</td>
<td>Beschreibung</td>
</tr>
<tr>
<td>Input Alias</td>
<td>Ein Anzeigename, die auf diese Eingabe in der Job-Abfrage verwendet werden.</td>
</tr>
<tr>
<td>Konto</td>
<td>Der Name des Speicherkontos befinden sich die BLOB-Dateien. Ist in den Abonnement Beruf Analytics Stream, können Sie sie aus der Dropdown unten auswählen.</td>
</tr>
<tr>
<td>Speicherkontoschlüssel</td>
<td>Der geheime Schlüssel Storage-Konto zugeordnet. Dies wird automatisch ausgefüllt, wenn das Speicherkonto in dieselbe Abonnement als Stream Analytics-Auftrag ist.</td>
</tr>
<tr>
<td>Behälter</td>
<td>Container stellen eine logische Gruppierung für Blobs in Microsoft Azure Blob-Dienst gespeichert. Beim Hochladen eines BLOBs auf BLOB-Dienst müssen Sie einen Container für dieses Blob angeben.</td>
</tr>
<tr>
<td>Pfad-Muster</td>
<td>Der Pfad zu Ihrem Blobs im angegebenen Container verwendet. In diesem Pfad können Sie eine oder mehrere Instanzen der 2 folgenden Variablen festlegen:<BR>{Datum} {Time}<BR>Beispiel 1: products/{date}/{time}/product-list.csv<BR>Beispiel 2: products/{date}/product-list.csv
</tr>
<tr>
<td>[Optional] Datumsformat</td>
<td>Wenn Sie {Datum} innerhalb der Pfad, die Sie angegeben verwendet haben, können wählen Sie das Datumsformat in dem Organisation Ihrer Dateien aus der Liste der unterstützten Formate. Beispiel: JJJJ/MM/TT</td>
</tr>
<tr>
<td>[Optional] Zeitformat</td>
<td>Wenn Sie {Time} innerhalb der Pfad, die Sie angegeben verwendet haben, können wählen Sie das Format in dem Organisation Ihrer Dateien aus der Liste der unterstützten Formate. Beispiel: HH</td>
</tr>
<tr>
<td>Ereignis-Serialisierungsformat</td>
<td>Um sicherzustellen, dass Ihre Abfragen funktionieren wie erwartet, Stream Analytics muss wissen, welche Serialisierungsformat verwenden Sie für eingehende Datenströme. Für Daten werden die Formate CSV und JSON.</td>
</tr>
<tr>
<td>Codierung</td>
<td>UTF-8 ist derzeit das einzige unterstützte Codierungsformat</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Referenzdaten nach einem Zeitplan generieren

Wenn die Referenzdaten langsam veränderliche Daten besteht, Unterstützung zum Aktualisieren von Verweis aktiviert, Daten durch Angabe eines Musters Pfad in der {Datum} und {Uhrzeit} Substitutionstoken. Stream Analytics übernehmen die Definitionen der aktualisierten Verweis basierend auf diesem Pfad. Z. B. ein Muster der `sample/{date}/{time}/products.csv` mit einem Datumsformat **"YYYY-MM-DD"** und weist Format **"Hh: mm"** Stream Analytics aktualisierte Blob abholen `sample/2015-04-16/17:30/products.csv` 5:30 Uhr am 16. April-2015 UTC-Zeitzone.

> [AZURE.NOTE] Stream Analytics-Aufträge suchen derzeit Blob aktualisieren nur, wenn die Systemzeit im BLOB-Namen codiert Zeit erhöht. Der Auftrag sieht beispielsweise für `sample/2015-04-16/17:30/products.csv` wie möglich, aber nicht vor 17:30 Uhr am 16. April-2015 UTC Zone Zeit. Es wird *niemals* Suchen nach einer Datei mit einer codierten Zeit früher als die letzte erkannt wird.
> 
> Z.b. Sobald der Auftrag Blob findet `sample/2015-04-16/17:30/products.csv` vor 17:30 Uhr 16. April 2015 mit codierten Dateien ignoriert Wenn spät ankommen `sample/2015-04-16/17:25/products.csv` Blob erstellt im selben Container der Auftrag nicht verwendet.
> 
> Auch wenn `sample/2015-04-16/17:30/products.csv` nur 16. April 2015 um 10:03 Uhr erstellt jedoch keine Blob mit einem älteren Datum im Container vorhanden ist, der Auftrag diese Datei ab 16. April 2015 um 10:03 Uhr und die früheren Daten erst dann verwenden.
> 
> Eine Ausnahme wird gestartet, wenn der Auftrag erneut verarbeitet Daten zurück oder der Auftrag wird. Startzeitpunkt sucht der Auftrag für das aktuelle Blob vor den Einzelvorgang produziert die Startzeit angegeben. Dies gibt ein Dataset **nicht leeren** Referenz der Einzelvorgang sicherzustellen. Wenn gefunden wird, zeigt das Projekt folgende Diagnose: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) kann aktualisierte Blobs musste von Stream Analytics Datendefinitionen Verweis erstellen orchestrieren verwendet werden. Data Factory ist ein Cloud-basierte Integrationsdienst, der koordiniert und automatisiert die Verlagerung und Transformation von Daten. Data Factory unterstützt [eine große Anzahl von Cloud-basierten und lokalen Datenspeicher verbinden](../data-factory/data-factory-data-movement-activities.md) und Daten einfach nach einem regelmäßigen Zeitplan, den Sie angeben. Weitere Informationen und schrittweise Anleitung Data Factory-Pipeline einrichten, um Referenzdaten für Stream Analytics generieren, die nach einem vordefinierten Zeitplan aktualisiert Anzeigen dieser [GitHub-Beispiel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs)

## <a name="tips-on-refreshing-your-reference-data"></a>Tipps zur Aktualisierung der Referenzdaten ##

1. Überschreiben von Daten-Blobs Verweis verursacht Stream Analytics Blob laden und in einigen Fällen wird des Auftrags nicht. Das empfohlene Verfahren zum Ändern von Daten ein neues Blob mit demselben Container hinzufügen und Pfad Muster gemäß Auftrag Eingabe Zeitpunkt **größer** als der letzte Blob in der Reihenfolge angegeben.
2.  Referenzdaten Blobs nicht dem Blob "Letzte Änderung" Zeit aber nur von Uhrzeit und Datum im Blob bestellt werden Namen {Datum} und {time} ersetzen.
3.  Mehrmals, ein Projekt zurückkehren muss, müssen daher Verweis Daten-Blobs nicht verändert oder gelöscht werden.

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
Sie haben einen verwalteten Service für streaming-Daten über das Internet der Dinge Analytics Stream Analytics eingeführt. Weitere Informationen zu diesem Dienst finden Sie unter:

- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
