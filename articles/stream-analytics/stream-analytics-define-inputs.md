<properties
    pageTitle="Verbindung: Eingaben aus einem Datenstrom | Microsoft Azure"
    description="Informationen Sie zum Einrichten von auf Stream Analytics 'Eingänge' aufgerufen. Eingaben enthalten einen Datenstrom von Ereignissen und auch auf Daten verweisen."
    keywords="Datenstrom, Verbindung, Ereignisstream"
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

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Verbindung: erfahren Sie mehr über Data Stream Eingaben von Ereignissen Stream Analytics

Die Verbindung mit Stream Analytics ist ein Datenstrom von Ereignissen aus einer Datenquelle. Dies heißt "Eingabe". Stream Analytics bietet erstklassige Integration mit Azure Stream Quellen Event Hub IoT Hub und BLOB-Speicherung von Daten, die aus denselben oder unterschiedlichen Azure-Abonnement als Beruf Analytics kann.

## <a name="data-input-types-data-stream-and-reference-data"></a>Dateneingabe Typen: Daten-Stream und Referenz
Daten an eine Datenquelle gesendet werden, ist von Stream Analytics-Auftrags genutzt und in Echtzeit verarbeitet. Eingaben sind in zwei Typen unterteilt: Data stream Eingaben und Daten verweisen.

### <a name="data-stream-inputs"></a>Datenstrom Eingaben
Ein Datenstrom ist ungebundene Sequenz von Ereignissen über Zeit. Stream Analytics-Aufträge sind mindestens ein Stream Dateneingabe verbraucht und dem Auftrag umgewandelt. BLOB-Speicher, Ereignis-Hubs und IoT Hubs sind als input Stream Datenquellen unterstützt. Ereignis-Hubs wird Ereignisstreams mehrere Geräte und Dienste, wie soziale Medien Aktivitätsfeeds auf Informationen oder den Sensoren erfassen. IoT Hubs sind zum Sammeln von Daten von Geräten im Internet der Dinge (IoT) Szenarien optimiert.  BLOB-Speicher kann als eine Eingabequelle für Aufnahme Datenmengen als Stream verwendet werden.  

### <a name="reference-data"></a>Referenzdaten
Stream Analytics unterstützt einen zweiten Eingabe als Daten bezeichnet. Diese werden zusätzliche statische oder langsam veränderliche langfristig und r. für Korrelation und suchen. Azure BLOB-Speicher ist derzeit die einzige unterstützte Eingabequelle für Referenzdaten. Reference Data Source Blobs sind auf 100MB Größe.
Erstellen von verweisen Daten finden Sie unter [Daten verwenden](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>Erstellen Sie eine Dateneingabe Stream mit einem Ereignis-Hub

[Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/) sind hochgradig skalierbare Ereignis Ingestor veröffentlichen-abonnieren. Sie können Millionen Ereignisse pro Sekunde sammeln, damit bearbeiten und große Mengen von Daten der angeschlossenen Geräte und Programme analysiert. Es ist eine der am häufigsten verwendeten Eingaben für Stream Analytics. Ereignis-Hubs und Stream Analytics bieten gemeinsam eine End-to-End-Lösung für Echtzeit-Analysen. Ereignis-Hubs ermöglichen Kunden, Ereignisse in Azure in Echtzeit und Stream Analytics-Aufträge können in Echtzeit verarbeiten. Kunden können z. B. Web klickt, Sensorwerte online Protokollereignisse an Event Hubs senden und Arbeitsplätze Stream Analytics um Ereignis Hubs wie die Eingabedaten für Echtzeit filtern, sammeln und Korrelation verwenden.

Es ist wichtig ist Default Timestamp Ereignisse Ereignis Hubs in Stream Analytics aus den Zeitstempel, den das Ereignis im Ereignis eingetroffen ist EventEnqueuedUtcTime. Zum Verarbeiten der Daten als Stream über einen Zeitstempel in die Ereignisnutzlast muss das Schlüsselwort [Zeitstempel von](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwendet werden.

### <a name="consumer-groups"></a>Verbraucherorganisationen

Jede Eingabe Stream Analytics Ereignis Hub sollte einen eigenen Consumer Gruppe konfiguriert werden. Einen selbst-join oder mehrere Eingaben umfasst, kann Eingaben mehrere Leser, gelesen werden, die Anzahl der Leser in einer einzigen Gruppe auswirkt. Vermeidung der Überschreitung Event Hub 5 Leser pro Partition pro Verbraucher ist es empfehlenswert, ein Consumer Gruppe für jede Stream Analytics. Beachten Sie, dass auch maximal 20 Verbrauchergruppen pro Ereignis Hub. Weitere Informationen finden Sie im [Ereignis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-event-hub-as-an-input-data-stream"></a>Konfigurieren von Event Hub als Eingabedaten stream

In der folgenden Tabelle erläutert jede Eigenschaft bei Hub mit Beschreibung eingeben:

| EIGENSCHAFTENNAME | BESCHREIBUNG |
|------|------|
| Input Alias | Einen angezeigten Namen, der auf diese Eingabe in der Job-Abfrage verwendet werden |
| Service Bus-Namespace | Service Bus-Namespace ist ein Container für eine Gruppe von Entitäten messaging. Bei der Erstellung neuer Ereignis-Hub wird außerdem einen Service Bus-Namespace erstellt. |
| Ereignis-Hub | Der Name des Haupt-Ereignis eingeben. |
| Ereignisname Hub-Richtlinie | Die SAS-Richtlinie auf der Registerkarte Ereignis Hub konfigurieren. Jede freigegebene Richtlinie einen Namen, Berechtigungen festlegen und Zugriffstasten. |
| Ereignis-Hub Richtlinienschlüssel | Shared Zugriffstaste Zugriffsberechtigung Servicebus-Namespace. |
| Hub Consumer Ereignisgruppe (Optional) | Verbrauchergruppe Daten vom Hub Ereignis aufnehmen. Wenn nicht angegeben, verwendet Stream Analytics-Aufträge Consumer-Standardgruppe Daten aus dem Ereignis Hub aufnehmen. Es wird empfohlen, jeden Stream Analytics verschiedenen Consumer Gruppe verwenden. |
| Ereignis-Serialisierungsformat | Um sicherzustellen, dass Ihre Abfragen funktionieren wie erwartet, Stream Analytics muss wissen, welche Serialisierungsformat (JSON, CSV oder Avro) verwenden Sie für eingehende Datenströme. |
| Codierung | UTF-8 ist derzeit das einzige unterstützte Codierungsformat. |

Wenn Ihre Daten eine Event Hub Quelle stammt, können Sie in Ihrer Abfrage Stream Analytics einige Metadatenfelder zugreifen. Die folgende Tabelle enthält die Felder und deren Beschreibung.

| EIGENSCHAFT | BESCHREIBUNG |
|------|------|
| EventProcessedUtcTime | Das Datum und die Uhrzeit, zu der das Ereignis von Stream Analytics verarbeitet wurde. |
| EventEnqueuedUtcTime | Datum und Uhrzeit, die das Ereignis vom Ereignis Hubs empfangen wurde. |
| IDs | Die nullbasierte Partitions-ID für den Eingabeadapter. |

Beispielsweise können Sie eine Abfrage wie die folgende schreiben:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Erstellen Sie ein Stream IoT Hub Dateneingabe

Azure Iot Hub ist eine hochgradig skalierbare Veröffentlichen-Abonnieren Ereignis Ingestor IoT Szenarien optimiert.
Es ist wichtig ist Default Timestamp Ereignisse IoT Hubs in Stream Analytics aus den Zeitstempel, den das Ereignis in IoT Hub eingegangen ist EventEnqueuedUtcTime. Zum Verarbeiten der Daten als Stream über einen Zeitstempel in die Ereignisnutzlast muss das Schlüsselwort [Zeitstempel von](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwendet werden.

> [AZURE.NOTE] Nur Nachrichten mit einer Eigenschaft DeviceClient können verarbeitet werden.

### <a name="consumer-groups"></a>Verbraucherorganisationen

Jede Eingabe Stream Analytics IoT Hub sollte einen eigenen Consumer Gruppe konfiguriert werden. Einen selbst-join oder mehrere Eingaben umfasst, kann Eingaben mehrere Leser, gelesen werden, die Anzahl der Leser in einer einzigen Gruppe auswirkt. Vermeidung der Überschreitung IoT Hub 5 Leser pro Partition pro Verbraucher ist es empfehlenswert, ein Consumer Gruppe für jede Stream Analytics.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>Konfigurieren Sie IoT Hub als einen Datenstream Eingabe

In der folgenden Tabelle wird jede Eigenschaft input IoT Hub-Registerkarte mit Beschreibung erläutert:

| EIGENSCHAFTENNAME | BESCHREIBUNG |
|------|------|
| Input Alias | Ein Anzeigename, die auf diese Eingabe in der Job-Abfrage verwendet werden. |
| IoT Hub | IoT Hub ist ein Container für eine Gruppe von Messaging-Entitäten. |
| Endpunkt | Der Name des IoT Hub-Endpunkt. |
| Richtlinienname gemeinsamer Zugriff | Die freigegebene Richtlinie IoT Hub zugreifen. Jede freigegebene Richtlinie einen Namen, Berechtigungen festlegen und Zugriffstasten. |
| Gemeinsamer Zugriff Richtlinienschlüssel | Shared Zugriffstaste auf IoT Hub authentifizieren. |
| Verbrauchergruppe (Optional) | Verbrauchergruppe IoT Hub Daten aufnehmen. Wenn nicht angegeben, verwendet Stream Analytics-Aufträge Consumer-Standardgruppe Daten aus IoT Hub aufnehmen. Es wird empfohlen, jeden Stream Analytics verschiedenen Consumer Gruppe verwenden. |
| Ereignis-Serialisierungsformat | Um sicherzustellen, dass Ihre Abfragen funktionieren wie erwartet, Stream Analytics muss wissen, welche Serialisierungsformat (JSON, CSV oder Avro) verwenden Sie für eingehende Datenströme. |
| Codierung | UTF-8 ist derzeit das einzige unterstützte Codierungsformat. |

Wenn Ihre Daten aus einer Quelle IoT Hub kommt, können Sie in Ihrer Abfrage Stream Analytics einige Metadatenfelder zugreifen. Die folgende Tabelle enthält die Felder und deren Beschreibung.

| EIGENSCHAFT | BESCHREIBUNG |
|------|------|
| EventProcessedUtcTime | Das Datum und die Uhrzeit, zu der das Ereignis verarbeitet wurde. |
| EventEnqueuedUtcTime | Datum und Uhrzeit, die das Ereignis vom IoT Hub empfangen wurde. |
| IDs | Die nullbasierte Partitions-ID für den Eingabeadapter. |
| IoTHub.MessageId | Bidirektionalen Kommunikation IoT Hub Korrelation verwendet. |
| IoTHub.CorrelationId | Antworten auf Nachrichten und Feedback IoT Hub verwendet. |
| IoTHub.ConnectionDeviceId | Die authentifizierte Id dieser Nachricht Abstempeln IoT Hub Servicebound Nachrichten. |
| IoTHub.ConnectionDeviceGenerationId | Die GenerationId der authentifizierten Gerät zum Senden dieser Nachricht Abstempeln IoT Hub Servicebound Nachrichten. |
| IoTHub.EnqueuedTime | Zeit, wenn die Nachricht von IoT Hub empfangen wurde. |
| IoTHub.StreamId | Benutzerdefiniertes Ereigniseigenschaft Sender-Gerät hinzugefügt. |

## <a name="create-a-blob-storage-data-stream-input"></a>Erstellen Sie eine BLOB-Speicher Stream Dateneingabe

BLOB-Speicher bietet für Szenarios mit großen Mengen an unstrukturierten Daten in der Cloud speichern kostengünstige und skalierbare Lösung. Daten im [BLOB-Speicher](https://azure.microsoft.com/services/storage/blobs/) werden im Allgemeinen als "ruhende" Daten aber als Datenstrom von Stream Analytics verarbeitet werden. Ein häufiges Szenario für BLOB-Speicher Eingaben mit Stream Analytics ist Verarbeitung, wo Telemetrie aus einem System erfasst und analysiert und verarbeitet werden, um aussagekräftige Daten extrahieren soll.

Es ist wichtig zu beachten, dass Default Timestamp der BLOB-Speicherereignisse im Stream Analytics den Zeitstempel Blob die *IsBlobLastModifiedUtcTime*zuletzt geändert wurde. Zum Verarbeiten der Daten als Stream über einen Zeitstempel in die Ereignisnutzlast muss das Schlüsselwort [Zeitstempel von](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwendet werden.

Beachten Sie, dass CSV Eingaben **erfordern** eine Kopfzeile formatierte können Sie Felder für die Daten festlegen. Weitere Header Zeilenfelder müssen alle **eindeutig**sein.

> [AZURE.NOTE] Hinzufügen von Inhalten zu einer vorhandenen BLOBs unterstützt Analytics Stream nicht. Stream Analytics sehen nur einen Blob einmal und alle Änderungen nach dieser Lesevorgang nicht verarbeitet werden. Empfiehlt die Daten einmal hochladen und nicht keine weiteren Ereignisse im BLOB-Speicher hinzufügen.

In der folgenden Tabelle wird jede Eigenschaft im BLOB-Speicher input Registerkarte mit Beschreibung erläutert:

<table>
<tbody>
<tr>
<td>EIGENSCHAFTENNAME</td>
<td>BESCHREIBUNG</td>
</tr>
<tr>
<td>Input Alias</td>
<td>Ein Anzeigename, die auf diese Eingabe in der Job-Abfrage verwendet werden.</td>
</tr>
<tr>
<td>Konto</td>
<td>Der Name des Speicherkontos befinden sich die BLOB-Dateien.</td>
</tr>
<tr>
<td>Speicherkontoschlüssel</td>
<td>Der geheime Schlüssel Storage-Konto zugeordnet.</td>
</tr>
<tr>
<td>Behälter
</td>
<td>Container stellen eine logische Gruppierung für Blobs in Microsoft Azure Blob-Dienst gespeichert. Beim Hochladen eines BLOBs auf BLOB-Dienst müssen Sie einen Container für dieses Blob angeben.</td>
</tr>
<tr>
<td>Pfad-Präfix Muster [optional]</td>
<td>Der Pfad zu Ihrem Blobs im angegebenen Container verwendet.
In diesem Pfad können Sie eine oder mehrere Instanzen der folgenden 3 Variablen angeben:<BR>{Datum}, {Uhrzeit}<BR>{Partition}<BR>Beispiel 1: cluster1/Protokolle / {Datum} und {Uhrzeit} / {partition}<BR>Beispiel 2: cluster1/Protokolle / {Datum}<P>Beachten Sie, dass "*" ist kein zulässiger Wert für Pathprefix. Nur gültige <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure BLOB-Zeichen</a> sind zulässig.</td>
</tr>
<tr>
<td>[Optional] Datumsformat</td>
<td>Wenn Datum Token im Präfix Pfad verwendet wird, können Sie das Datumsformat in dem Organisation Ihrer Dateien. Beispiel: JJJJ/MM/TT</td>
</tr>
<tr>
<td>[Optional] Zeitformat</td>
<td>Wenn Zeit Token im Präfix Pfad verwendet wird, geben Sie das Format in dem Organisation Ihrer Dateien an Derzeit ist der einzige unterstützte Wert HH.</td>
</tr>
<tr>
<td>Ereignis-Serialisierungsformat</td>
<td>Um sicherzustellen, dass Ihre Abfragen funktionieren wie erwartet, Stream Analytics muss wissen, welche Serialisierungsformat (JSON, CSV oder Avro) verwenden Sie für eingehende Datenströme.</td>
</tr>
<tr>
<td>Codierung</td>
<td>Für CSV- und JSON ist UTF-8 zu diesem Zeitpunkt nur unterstütztes Codierungsformat.</td>
</tr>
<tr>
<td>Trennzeichen</td>
<td>Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen für Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Registerkarte und Balken.</td>
</tr>
</tbody>
</table>

Wenn Ihre Daten-BLOB-Speicherquelle stammt, können Sie einige Metadatenfelder in Ihrer Abfrage Stream Analytics zugreifen. Die folgende Tabelle enthält die Felder und deren Beschreibung.

| EIGENSCHAFT | BESCHREIBUNG |
|------|------|
| BlobName | Der Name der Eingabe Blob, dem das Ereignis stammt. |
| EventProcessedUtcTime | Das Datum und die Uhrzeit, zu der das Ereignis von Stream Analytics verarbeitet wurde. |
| BlobLastModifiedUtcTime | Datum und Uhrzeit der letzten Änderung das Blob. |
| IDs | Die nullbasierte Partitions-ID für den Eingabeadapter. |

Beispielsweise können Sie eine Abfrage wie die folgende schreiben:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
Sie haben für Ihre Aufträge Stream Analytics Datenverbindungsoptionen in Azure erfahren. Erfahren Sie mehr über Stream Analytics finden Sie unter:

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
