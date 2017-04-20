<properties
 pageTitle="Entwicklerhandbuch - messaging | Microsoft Azure"
 description="Entwicklerhandbuch für IoT Hub Azure - Gerät Cloud und Cloud-zu-Gerät-messaging"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="send-and-receive-messages-with-iot-hub"></a>Senden und Empfangen von Nachrichten mit IoT Hub

## <a name="overview"></a>Übersicht

IoT Hub bietet die folgenden messaging primitive zur Kommunikation mit einem Gerät:

- [Gerät Cloud] [ lnk-d2c] aus einer Anwendung ein back-End.
- [Cloud-zu-Gerät-] [ lnk-c2d] einer Anwendung back-End (*Service* oder *Cloud*).

Haupteigenschaften des IoT Hub Messagingfunktionen sind Zuverlässigkeit und Haltbarkeit von Nachrichten. Diese Eigenschaften ermöglichen gegenüber instabile Konnektivität auf Gerät und Spitzen in Verarbeitung auf Cloud laden. IoT Hub implementiert *mindestens* Lieferung Garantien für Gerät Cloud und Cloud-zu-Gerät-messaging.

IoT Hub unterstützt mehrere [Protokolle Gerät mit] [ lnk-protocols] (wie MQTT, AMQP und HTTP). Um nahtlose Zusammenarbeit über Protokolle unterstützen IoT Hub definiert [Allgemeine Nachrichtenformat] [ lnk-message-format] , die alle verbundenen Geräte Protokolle unterstützen.

IoT Hub macht ein [Ereignis Hub-kompatiblen Endpunkt] [ lnk-compatible-endpoint] Back-End-Applikationen Gerät Cloud-Nachrichten vom Hub Lesen aktivieren.

### <a name="when-to-use"></a>Verwendung

Messaging ist eine Hauptfunktion von IoT Hub. Verwenden sie, wenn Sie Nachrichten von Ihrem Gerät auf Ihre Back-End oder Nachrichten vom Back-End an ein Gerät senden müssen.

Ein Vergleich IoT Hub und Ereignis Hubs finden Sie unter [Vergleich der IoT Hub und Ereignis Hubs][lnk-compare].

## <a name="device-to-cloud-messages"></a>Gerät-zu-Cloud-Nachrichten

Gerät-zu-Cloud-Nachrichten über ein Gerät verbundenen Endpunkt (**/devices/ {DeviceId} Nachrichten/Ereignisse**) senden. Back-End-Dienst empfängt Gerät Cloud-Nachrichten über einen Dienst verbundenen Endpunkt (**/messages/events**), [Ereignis Hubs]mit[lnk-event-hubs]. Daher können Sie [Event Hubs Integration und SDKs] [ lnk-compatible-endpoint] Gerät Cloud-Nachrichten empfangen.

IoT Hub implementiert Gerät Cloud messaging im [Ereignis Hubs]ähnlich ist[lnk-event-hubs]. IoT Hub Gerät Cloud-Nachrichten sind eher Ereignis Hubs *Ereignisse* [Service Bus] [ lnk-servicebus] *Nachrichten*.

Diese Implementierung hat die folgenden Konsequenzen:

* Ebenso Ereignis Hubs Ereignisse Gerät Cloud-Nachrichten sind langlebig und beibehaltenen in IoT Hub bis zu sieben Tage ( [Gerät Cloud-Konfigurationsoptionen]finden Sie unter[lnk-d2c-configuration]).
* Gerät-zu-Cloud-Nachrichten über einen festen Satz von Partitionen, die zum Zeitpunkt der Erstellung festgelegt ist partitioniert ( [Gerät Cloud-Konfigurationsoptionen]finden Sie unter[lnk-d2c-configuration]).
* An Ereignis Hubs müssen Clients Gerät Cloud-Nachrichten lesen Analog Partitionen und Prüfpunkte behandeln. [Event Hubs - Verwenden von Ereignissen]finden Sie unter[lnk-event-hubs-consuming-events].
* Wie Ereignis Hubs Gerät Cloud-Nachrichten können höchstens 256 KB und stapelweise sendet optimieren gruppiert werden. Batches können höchstens 256 KB und höchstens 500 Nachrichten sein.

Es gibt jedoch einige wichtige Unterschiede zwischen IoT Hub Gerät Cloud messaging und Ereignis-Hubs:

* [Steuern des Zugriffs auf IoT Hub] wie[ lnk-devguide-security] Abschnitt IoT Hub pro-Authentifizierung und Steuerung ermöglicht.
* IoT Hub ermöglicht Millionen von Geräten gleichzeitig (siehe [Vorgaben und Drosselung][lnk-quotas]), Ereignis Hubs 5000 AMQP Verbindungen pro Namespace auf.
* IoT Hub erlaubt keine beliebigen Partitionierung mithilfe einer **PartitionKey**. Gerät-zu-Cloud-Nachrichten werden basierend auf ihrer ursprünglichen **DeviceId**partitioniert.
* Skalierung IoT Hub unterscheidet Ereignis Hubs skalieren. Weitere Informationen finden Sie unter [Skalierung IoT Hub][lnk-guidance-scale].

> [AZURE.NOTE] Sie können nicht in allen Szenarios IoT Hub für Ereignis Hubs ersetzen. Beispielsweise könnte in einigen Berechnungen Ereignis verarbeitet Ereignisse in Bezug auf eine andere Eigenschaft oder ein Feld vor der Datenströme neu partitionieren erforderlich. In diesem Szenario können Sie Event Hub um zwei Teile des Streams Verarbeitungspipeline zu entkoppeln. Weitere Informationen finden Sie unter *Partitionen* in [Azure Ereignisübersicht Hubs][lnk-eventhub-partitions].

Details zur Verwendung von messaging Gerät Cloud [IoT Hub APIs und SDKs]finden Sie[lnk-sdks].

> [AZURE.NOTE] Bei Verwendung von HTTP Gerät Cloud-Nachrichten Eigenschaftsnamen und-Werte können nur ASCII alphanumerische Zeichen enthalten, und ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Nicht-Telemetrie Datenverkehr

Geräte senden häufig neben Telemetrie Daten, auch Nachrichten und Anfragen, die Ausführung und Behandlung von der Geschäftslogikschicht Anwendung benötigen. Beispielsweise kritische Alarme, die eine bestimmte Aktion im Back-End oder Gerät Antworten vom Back-End gesendeten Befehle auslösen müssen.

Weitere Informationen über die beste Möglichkeit, diese Art von Nachricht finden Sie die [Tutorial: IoT Hub Gerät Cloud-Nachrichten verarbeitet] [ lnk-d2c-tutorial] Tutorial.

### <a name="device-to-cloud-configuration-options"></a>Gerät-zu-Cloud-Konfigurationsoptionen

IoT Hub enthält die folgenden Eigenschaften steuern Gerät Cloud messaging ermöglicht.

* **Anzahl von Partitionen**. Legen Sie diese Eigenschaft bei der Erstellung die Anzahl der Partitionen Gerät Cloud Ereignis Ingestion.
* **Aufbewahrungszeit**. Diese Eigenschaft gibt die Retentionszeit für Gerät Cloud-Nachrichten. Der Standardwert ist ein Tag auf sieben Tage erhöht werden

Außerdem analog zum Ereignis Hubs IoT Hub ermöglicht Ihnen, Consumer in der Cloud Gerät erhalten Endpunkt.

Sie können alle diese Eigenschaften programmgesteuert über [IoT Hub Ressourcenanbieter REST-APIs][lnk-resource-provider-apis], oder mithilfe der [Azure-Portal][lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Anti-spoofing-Eigenschaften

Gerät Gerät Cloud Nachrichten IoT Hub spoofing zu Stempeln aller Nachrichten mit den folgenden Eigenschaften:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Die ersten beiden **DeviceId** und **GenerationId** des ursprünglichen Geräts nach [Gerät Identitätseigenschaften]enthalten[lnk-device-properties].

**ConnectionAuthMethod** -Eigenschaft enthält eine serialisierte JSON-Objekt mit den folgenden Eigenschaften:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Cloud-zu-Gerät-Nachrichten

Cloud-zu-Gerät-Nachrichten über einen Dienst verbundenen Endpunkt (**/messages/devicebound**) senden. Ein Gerät erhält durch einen gerätespezifischen Endpunkt (**/devices/ {DeviceId} Nachrichten/Devicebound**).

Jede Nachricht Cloud Gerät zielt auf ein einzelnes Gerät indem die **to** -Eigenschaft auf **/devices/ {DeviceId} / Nachrichten Devicebound**.

>[AZURE.IMPORTANT] Jede Warteschlange enthält höchstens 50 Cloud-zu-Gerät-Nachrichten. Möchten weitere Nachrichten an denselben Gerät führt zu einem Fehler.

> [AZURE.NOTE] Beim Senden von Nachrichten Cloud Gerät Eigenschaftsnamen und-Werte können nur ASCII alphanumerische Zeichen enthalten, und ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Nachrichten-lifecycle

Um mindestens Nachrichtenübermittlung sicherzustellen, behält IoT Hub Cloud Gerät Nachrichten in Warteschlangen pro Gerät. Geräte müssen explizit *Abschluss* IoT Hub aus der Warteschlange entfernen bestätigen. Konnektivität und Gerätefehler Stabilität garantiert.

Das folgende Diagramm zeigt die Grafik Lebenszyklus Zustand für eine Cloud-zu-Gerät-Nachricht.

![Cloud-zu-Gerät-Nachricht Lebenszyklus][img-lifecycle]

Wenn der Dienst eine Nachricht sendet, gilt die *Warteschlange*. Wenn ein Gerät *empfangen* möchte eine Nachricht IoT Hub *Sperren* (setzt den Status **unsichtbar**) Nachricht auf demselben Gerät zu anderen Nachrichten anderer Threads zuzulassen. Abschluss ein Geräts Threads die Verarbeitung einer Nachricht benachrichtigt er IoT Hub durch *abschließen* der Nachricht.

Ein Gerät kann auch:

- *Ablehnen* die Nachricht, wodurch IoT Hub **Zustellversuchen** Zustand festgelegt. Hinweis: Geräte mit MQTT können nicht C2D Nachrichten ablehnen.
- *Verlassen* **Warteschlange**die Nachricht, wodurch IoT Hub zu der Nachricht in der Warteschlange mit dem Status gesetzt.

Ein Thread kann eine Nachricht verarbeiten, ohne IoT Hub fehlschlagen. In diesem Fall Übergang Nachrichten automatisch von **unsichtbaren** Zustand der **Warteschlange** Zustand nach einem *Timeout Sichtbarkeit (oder Sperren)*. Der Standardwert dieses Zeitlimit beträgt eine Minute.

Eine Nachricht kann **in die Warteschlange eingereiht** und **unsichtbare** Zustände für maximal Anzahl **max Lieferung Count** -Eigenschaft auf IoT Hub gemäß Übergang zwischen. Nach dieser Anzahl der Übergänge legt IoT Hub auf **Zustellversuchen**Status der Nachricht. Ebenso IoT Hub wird der Status einer Nachricht auf **Zustellversuchen** nach den Ablaufzeitpunkt ( [Gültigkeitsdauer]Siehe[lnk-ttl]).

Ein Cloud-zu-Gerät-Nachrichten finden Sie unter [Tutorial: wie Cloud-Gerät mit IoT Hub senden][lnk-c2d-tutorial]. Themen wie verschiedene APIs und SDKs Cloud-zu-Gerät-Funktionen verfügbar machen, finden Sie unter [IoT Hub APIs und SDKs][lnk-sdks].

> [AZURE.NOTE] Cloud-zu-Gerät-Nachrichten führen normalerweise immer der Verlust der Nachricht nicht die Anwendungslogik auswirkt. Beispielsweise der Nachrichteninhalt wurde erfolgreich im lokalen Speicher beibehalten oder ein Vorgang erfolgreich ausgeführt wurde. Die Nachricht kann auch vorübergehend Informationen tragen, deren Verlust die Funktionalität der Anwendung nicht beeinträchtigt. Manchmal können Sie Cloud-zu-Gerät-Nachricht für lang andauernde Vorgänge ausführen, nach der Task-Beschreibung im lokalen Speicher speichern. Dann können Sie die Anwendung Backend mit Gerät Cloud-Nachrichten in verschiedenen Stadien der Bearbeitung der Aufgabe benachrichtigen.

### <a name="message-expiration-time-to-live"></a>Ablaufzeit (Zeit Live)

Jede Nachricht Cloud-Gerät hat eine Ablaufzeit. Dieses Mal wird vom Dienst (in der **ExpiryTimeUtc** -Eigenschaft) oder durch IoT Hub mit als IoT Hub-Eigenschaft angegebene Standard- *Gültigkeitsdauer* festgelegt. [Cloud-zu-Gerät-Konfigurationsoptionen]finden Sie unter[lnk-c2d-configuration].

> [AZURE.NOTE] Zu nutzen Ablaufzeit wird getrennte Geräte senden Nachrichten kurzfristig Werte festlegen. Dieser Ansatz erzielt dasselbe Ergebnis wie die Verbindung des Geräts und effizienter verwalten. Wenn Sie Nachricht Empfangsbestätigungen anfordern, benachrichtigt IoT Hub welche Geräte Nachrichten empfangen werden, und welche Geräte nicht online oder fehlgeschlagen sind.

### <a name="message-feedback"></a>Nachricht feedback

Sendet eine Nachricht an das Gerät Cloud kann der Dienst die Übermittlung von Nachrichten Feedback bezüglich der endgültige Zustand der Nachricht anfordern.

- Festlegen die **Ack** -Eigenschaft auf **positive**generiert IoT Hub eine Feedback-Nachricht, wenn und nur wenn die Cloud-zu-Gerät-Nachricht den Status " **abgeschlossen** " erreicht.
- Festlegen die **Ack** -Eigenschaft auf **negative**generiert IoT Hub eine Feedback-Nachricht Wenn, die Cloud-zu-Gerät-Nachricht **Zustellversuchen** Zustand erreicht.
- Festlegen die Eigenschaft **Ack** **zu**generiert IoT Hub in jedem Fall eine Feedback-Nachricht.

> [AZURE.NOTE] ** **Ack** ist**und keine Meldung Feedback bedeutet, dass die Feedback-Nachricht abgelaufen. Der Dienst kann nicht, was mit der ursprünglichen Nachricht. In der Praxis sollten ein Dienstes die Bewertung verarbeitet werden können, bevor es abläuft. Maximale Ablaufzeit wird zwei Tage Zeit, den Dienst ermöglicht erneut ausgeführt, wenn ein Fehler auftritt.

Als [Endpunkte]erklärt[lnk-endpoints], IoT Hub liefert Feedback über Service verbundenen Endpunkt (**/messages/servicebound/feedback**) Nachrichten. Die Semantik zum Empfangen von Feedback entsprechen Cloud Gerät Nachrichten und die gleiche [Nachricht Lebenszyklus][lnk-lifecycle]. Wenn möglich, wird Nachricht Feedback in einer einzelnen Nachricht mit dem folgenden Format zusammengefasst:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| EnqueuedTime | Zeitstempel, der angibt, wann die Nachricht erstellt wurde. |
| Benutzer-ID | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

Der Text ist ein JSON serialisierten Array aller Datensätze mit folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| EnqueuedTimeUtc | Zeitstempel, der angibt, wenn das Ergebnis der Nachricht passiert. Z. B. abgelaufen Gerät abgeschlossen oder die Nachricht. |
| OriginalMessageId | **MessageId** Cloud-zu-Gerät-Nachricht, dazu Feedback bezieht. |
| StatusCode | Erforderlicher Integer-Wert. Feedback Nachrichten IoT Hub verwendet. <br/> 0 = Erfolg <br/> 1 = Nachricht abgelaufen <br/> 2 = Lieferung maximale Anzahl überschritten <br/> 3 = Nachricht abgelehnt |
| Beschreibung | Zeichenfolgenwerte für **StatusCode**. |
| Geräte-ID | **Geräte-ID** des Zielgeräts, diese Information Feedback bezieht sich, Cloud-zu-Gerät-Nachricht. |
| DeviceGenerationId | **DeviceGenerationId** des Zielgeräts, diese Information Feedback bezieht sich, Cloud-zu-Gerät-Nachricht. |


>[AZURE.IMPORTANT] Der Dienst muss eine **MessageId** Cloud-zu-Gerät-Nachricht zu korrelieren Feedback mit der ursprünglichen Nachricht angeben.

Das folgende Beispiel zeigt den Nachrichtentext Feedback.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Cloud-zu-Gerät-Konfigurationsoptionen

Jede IoT Hub stellt die folgenden Konfigurationsoptionen für Cloud-zu-Gerät-messaging.

| Eigenschaft | Beschreibung | Bereich und Standard |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | TTL-Standardwert für Cloud-zu-Gerät-Nachrichten. | ISO_8601 Intervall bis 2D (mindestens 1 Minute). Standard: 1 Stunde. |
| maxDeliveryCount | Maximale Lieferung Anzahl für Cloud-Gerät pro-Warteschlangen. | 1 bis 100. Standard: 10. |
| feedback.ttlAsIso8601 | Aufbewahrung Bewertungsnachrichten Dienst gebunden. | ISO_8601 Intervall bis 2D (mindestens 1 Minute). Standard: 1 Stunde. |
| feedback.maxDeliveryCount | Lieferung maximale Anzahl für Feedback-Warteschlange. | 1 bis 100. Standard: 100. |

Weitere Informationen finden Sie unter [Erstellen IoT Hubs][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Gerät-zu-Cloud-Nachrichten

IoT Hub stellt einen Endpunkt für die Back-End-Dienste Gerät Cloud-Nachrichten durch den Hub lesen. Der Endpunkt ist Ereignis Hub-kompatibel ist, ermöglicht Sie die Mechanismen verwenden Service Event Hubs zum Lesen von Nachrichten unterstützt.

Bei Verwendung von [Azure Service Bus SDK für .NET] [ lnk-servicebus-sdk] oder [Ereignis Hubs - Prozessor Veranstalter][lnk-eventprocessorhost], können Zeichenfolgen Verbindung IoT Hub mit den richtigen Berechtigungen. Verwenden Sie als Ereignis-Hubnamen **Nachrichten-Ereignisse** .

Verwendung SDKs (oder Produktintegrationen) nicht IoT Hub, ein Ereignis Hub-kompatiblen Endpunkt und Ereignis Hub-kompatiblen Namen müssen aus IoT Hub Einstellungen im [Azure-Portal]abrufen[lnk-management-portal]:

1. Klicken Sie auf **Messaging**IoT Hub Blatt.
2. Im Abschnitt **Gerät Cloud Einstellungen** finden Sie die folgenden Werte: **Ereignis Hub-kompatiblen Endpunkt** **Ereignis Hub-kompatible Namen**und **Partitionen**.

    ![Gerät-zu-Cloud-Einstellung][img-eventhubcompatible]

> [AZURE.NOTE] Das SDK einen **Hostname** oder **Namespace** -Wert benötigt, entfernen Sie das Schema aus dem **Ereignis Hub-kompatiblen Endpunkt** Wenn beispielsweise Ihr Ereignis Hub-kompatiblen Endpunkt ist **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, der **Hostname** wäre **Iothub ns-Myiothub 1234.servicebus.windows.net**und **Namespace** wäre **Iothub-ns-Myiothub-1234**.

Anschließend können Sie alle freigegebenen Codezugriffssicherheits-Richtlinie mit den **ServiceConnect** Berechtigungen für die Verbindung mit dem angegebenen Ereignis-Hub.

Benötigen Sie eine Verbindungszeichenfolge Event Hub mithilfe der vorherigen erstellen, verwenden Sie das folgende Muster:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Folgendes ist eine Liste von SDKs und Integrationen mit Ereignis Hub-kompatible Endpunkte verwenden können, die IoT Hub verfügbar macht:

* [Java-Ereignis Hubs client](https://github.com/hdinsight/eventhubs-client)
* [Apache Storm Auslauf](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Sie können die [Quelle Auslauf](https://github.com/apache/storm/tree/master/external/storm-eventhubs) GitHub anzeigen.
* [Apache Spark-integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Themen:

Die folgenden Themen bieten weitere Informationen zum Austausch von Nachrichten mit IoT Hub.

## <a name="message-format"></a>Nachrichtenformat

IoT Hub Nachrichten umfassen:

* Ein Satz von *Systemeigenschaften*. Eigenschaften, die IoT Hub interpretiert, oder legt diesen fest. Dieser Satz ist vorgegeben.
* Ein Satz von *Anwendungseigenschaften*. Ein Wörterbuch von Eigenschaften, die die Anwendung definieren und Zugriff ohne Textkörper der Meldung deserialisiert. IoT Hub ändert nie diese Eigenschaften.
* Eine nicht transparente binärer Text.

Weitere Informationen zum Codieren der Nachricht in verschiedenen Protokollen finden Sie unter [IoT Hub APIs und SDKs][lnk-sdks].

Die folgende Tabelle enthält verschiedene Systemeigenschaften IoT Hub Nachrichten.

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| MessageId | Ein Benutzerdefinierbar Bezeichner für die Meldung für Anforderung-Antwort-Muster verwendet. Format: Groß-/Kleinschreibung String (bis zu 128 Zeichen) ASCII-7-Bit alphanumerischen Zeichen + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Laufende Nummer | Eine Zahl (pro Gerät-Warteschlange eindeutig) IoT Hub jeder Cloud-zu-Gerät-Nachricht. |
| An | [Cloud -] Gerät angegebenen Ziel[ lnk-c2d] Nachrichten. |
| ExpiryTimeUtc | Datum und Uhrzeit der Nachricht abläuft. |
| EnqueuedTime | Datum und Zeit des Nachrichtenempfangs IoT Hub. |
| CorrelationId | Eine Eigenschaft in einer Antwortnachricht, die normalerweise die MessageId Anforderung im Anforderung-Antwort-Muster enthält. |
| Benutzer-ID | Eine ID verwendet, um den Ursprung der Nachrichten angeben. Wenn Nachrichten von IoT Hub generiert, es soll `{iot hub name}`. |
| ACK | Ein Generator Feedback angezeigt. Diese Eigenschaft wird in Cloud Gerät Nachrichten zur IoT Hub zu Feedback-Nachrichten durch den Verbrauch der Nachricht anfordern vom Gerät. Mögliche Werte: **keine** (Standard): kein Feedback-Meldung wird generiert, **positive**: eine Feedback-Nachricht erhalten, wenn die Nachricht abgeschlossen wurde, **negative**: eine Feedback-Nachricht empfangen, wenn die Nachricht abgelaufen (oder maximale Lieferung erreicht wurde), ohne vom Gerät oder **vollständige**abgeschlossen: positive und negative. Weitere Informationen finden Sie unter [Nachricht Feedback][lnk-feedback]. |
| ConnectionDeviceId | Eine ID IoT Hub Gerät Cloud-Nachrichten festlegen. **Geräte-ID** des Geräts, das die Nachricht enthält. |
| ConnectionDeviceGenerationId | Eine ID IoT Hub Gerät Cloud-Nachrichten festlegen. Enthält **GenerationId** (nach [Gerät Identitätseigenschaften][lnk-device-properties]) des Geräts, das die Nachricht gesendet. |
| ConnectionAuthMethod | Eine Authentifizierungsmethode IoT Hub Gerät Cloud-Nachrichten festlegen. Diese Eigenschaft enthält Informationen über die Authentifizierungsmethode verwendet das Gerät die Nachricht authentifiziert. Weitere Informationen finden Sie unter [Device cloud Anti-spoofing][lnk-antispoofing].|

## <a name="communication-protocols"></a>Kommunikationsprotokolle

IoT Hub ermöglicht Geräte [MQTT][lnk-mqtt], MQTT über WebSockets [AMQP][lnk-amqp], AMQP WebSockets und HTTP-Protokolle für die geräteseitige Kommunikation. Die folgende Tabelle enthält allgemeine Vorschläge zur Auswahl des Protokolls:

| Protokoll | Wenn dieses Protokoll wählen Sie |
| -------- | ------------------------------------ |
| MQTT <br> MQTT über WebSocket     | Auf allen Geräten, die keine Verbindung mehrere Geräte (jeder mit eigenen Anmeldeinformationen pro Gerät) über dieselbe TLS-Verbindung verwenden. |
| AMQP <br> AMQP über WebSocket    | Feld und Cloud Gateways zu Verbindung multiplexing auf Geräten verwenden. |
| HTTP    | Verwenden Sie für Geräte, die andere Protokolle unterstützen kann. |

Berücksichtigen Sie wählen Sie das Protokoll für die Kommunikation geräteseitige sollten Sie folgende Punkte:

* **Cloud-zu-Gerät-Muster**. HTTP nicht effizient Serverpush implementieren. Bei Verwendung von HTTP Abfragen Geräte so IoT Hub für Cloud-zu-Gerät-Nachrichten. Dieser Ansatz ist für das Gerät und IoT Hub ineffizient. Unter der aktuellen HTTP-Richtlinien sollte jedes Nachrichten alle 25 Minuten abrufen. MQTT und AMQP unterstützt Serverpush auf der anderen Seite beim Cloud-zu-Gerät-Nachrichten empfangen. Sie ermöglichen sofortige Push Nachrichten IoT Hub für das Gerät. Wartezeit bei der Nachrichtenübermittlung sind von Belang, MQTT oder AMQP die besten Protokolle verwenden. Für selten angeschlossenen Geräte funktioniert HTTP.
* **Feld-Gateways**. MQTT und HTTP kann nicht mehrere Geräte (jeder mit eigenen Anmeldeinformationen pro Gerät) Verbinden über die TLS-Verbindung. Für [Feld Gateway Szenarien][lnk-azure-gateway-guidance], diese Protokolle sind suboptimale eine TLS-Verbindung zwischen dem Feld Gateway und IoT Hub für jedes Gerät mit dem Feld Gateway erforderlich.
* **Geräte mit geringen Ressourcen**. Die Bibliotheken MQTT und HTTP haben weniger Platz als AMQP-Bibliotheken. So Gerät Ressourcen (z. B. weniger als 1 MB RAM) zur Verfügung hat, diese Protokolle das einzige protokollimplementierung möglicherweise.
* **Netzwerk**. AMQP-Standardprotokoll verwendet Port 5671 und MQTT auf Port 8883, die in Netzwerken auftreten könnten, die nicht-HTTP-Protokolle geschlossen sind. MQTT über WebSockets AMQP WebSockets und HTTP sind in diesem Szenario verwendet werden.
* **Nutzlastgröße**. MQTT und AMQP sind binäre Protokolle als HTTP kompakter Nutzlasten führen.

> [AZURE.NOTE] Bei Verwendung von HTTP sollte jedes Cloud Gerät Nachrichten alle 25 Minuten abrufen. Während der Entwicklung ist es jedoch häufiger als alle 25 Minuten Abfragen zulässig.

## <a name="port-numbers"></a>Port-Nummern

Geräte können mit IoT Hub in Azure über verschiedene Protokolle kommunizieren. Die Wahl des Protokolls wird in der Regel von Anforderungen der Lösung gesteuert. Die folgende Tabelle listet die ausgehende Ports für ein Gerät an ein bestimmtes Protokoll verwendet werden müssen:

| Protokoll | Anschlüsse |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT über WebSockets | 443    |
| AMQP     | 5671    |
| AMQP über WebSockets | 443    |
| HTTP     | 443     |
| LWM2M (Device Management) | 5684 |

Nach dem Erstellen eines IoT Hubs in Azure-Region hält Hub die IP-Adresse für die Lebensdauer dieses Hub. Jedoch ist zum Dienst halten bewegt Microsoft IoT Hub auf einen anderen Maßstab dann er eine neue IP-Adresse zugewiesen.

## <a name="notes-on-mqtt-support"></a>Hinweise zum MQTT support

IoT Hub implementiert das Protokoll MQTT v3.1.1 mit folgenden Einschränkungen bestimmte Verhalten:

  * **QoS-2 nicht unterstützt**. Wenn ein Geräte-Client eine Nachricht mit **QoS-2**veröffentlicht, schließt IoT Hub des Netzwerks. Wenn ein Thema mit **QoS-2**ein Gerät Client abonniert gewährt IoT Hub maximale QoS-1 im Paket **SUBACK** .
  * **Nachrichten beibehalten nicht beibehalten**. Gerät-Client eine Nachricht mit dem beibehalten-Flag auf 1 festgelegt veröffentlicht, IoT Hub fügt der **X-opt-behalten** Application-Eigenschaft der Nachricht. In diesem Fall IoT Hub beibehalten Nachricht nicht beibehalten, sondern leitet es Back-End-Anwendung.

Weitere Informationen finden Sie unter [IoT Hub MQTT unterstützen][lnk-devguide-mqtt].

Als letzte Überlegung sollten Sie [Azure IoT Protokollgateway] [ lnk-azure-protocol-gateway] , ermöglicht leistungsfähige benutzerdefinierte Protokollgateway bereitstellen, die direkt mit IoT Hub. Azure IoT Protokollgateway können Sie das Gerät Protokoll Brache MQTT Bereitstellung aufnehmen oder andere benutzerdefinierte Protokolle anpassen. Dieser Ansatz ist jedoch erforderlich: ausführen und benutzerdefinierte Protokollgateway betreiben.

## <a name="additional-reference-material"></a>Zusätzliches Referenzmaterial

Andere Referenzthemen in Developer Guide enthalten:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschreibt die verschiedenen Endpunkte jedes IoT Hub Runtime und Management macht.
- [Kontingente und Drosselung] [ lnk-quotas] beschreibt die Quoten für den Dienst IoT Hub und Drosselung Verhalten gelten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub-Gerät und SDKs] [ lnk-sdks] Listet die verschiedenen SDKs Sie können Wenn Sie Anwendungsentwicklung Geräte und Dienste, die interagieren mit IoT Hub.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] beschreibt die Abfragesprache IoT Hub über Geräte im Vergleich, Methoden und Aufträge abrufen können.
- [IoT Hub MQTT Unterstützung] [ lnk-devguide-mqtt] MQTT Protokoll IoT Hub unterstützt Weitere Informationen bereit.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie senden und Empfangen von Nachrichten mit IoT Hub gelernt haben, können Sie in den folgenden Themen Entwicklerhandbuch interessiert sein:

- [Hochladen von Dateien von einem Gerät][lnk-devguide-upload]
- [Verwalten von Identitäten in IoT Hub Gerät][lnk-devguide-identities]
- [Steuern des Zugriffs auf IoT Hub][lnk-devguide-security]
- [Gerät im Vergleich mit dem Zustand und Konfigurationen synchronisiert][lnk-devguide-device-twins]
- [Eine direkte Methode auf einem Gerät][lnk-devguide-directmethods]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Möchten Sie einige der in diesem Artikel beschriebenen Konzepte zu testen, können Sie die folgenden Lernprogramme IoT Hub interessiert.

- [Erste Schritte mit Azure IoT Hub][lnk-getstarted-tutorial]
- [Wie Cloud-Gerät mit IoT Hub senden][lnk-c2d-tutorial]
- [IoT Hub Gerät Cloud-Nachrichten verarbeitet][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
