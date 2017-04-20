<properties
 pageTitle="Entwicklerhandbuch - direkten Methoden | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch - direkte Methoden zum Aufrufen von Code auf Ihren Geräten verwenden"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Eine direkte Methode auf einem Gerät (Vorschau)

## <a name="overview"></a>Übersicht

IoT Hub ermöglicht das Aufrufen von Methoden auf Geräten aus der Cloud. Methoden stellen eine Anforderung-Antwort-Interaktion mit einem Gerät wie einen HTTP-Aufruf, sie gelingen oder (sobald Benutzer angegebenen Timeout) Fehlschlagen der Benutzer den Status des Anrufs. Dies eignet sich für Szenarien, in denen natürlich sofort abhängig, ob das Gerät SMS-Reaktivierung an ein Gerät senden, ist ein Gerät offline (SMS teurer als einen Methodenaufruf) reagiert, konnte.

Sie können eine Methode als ein Remoteprozeduraufruf direkt an das Gerät vorstellen. Aus der Cloud können nur auf einem Gerät implementierte Methoden aufgerufen werden. Cloud versucht ein Methodenaufruf auf einem Gerät die nicht diese Methode definiert ist, schlägt die Methode fehl.

Jede Methode Gerät abzielt ein Gerät [Aufträge] [ lnk-devguide-jobs] bieten eine Möglichkeit zum Aufrufen von Methoden auf mehreren Geräten und Warteschlange Methodenaufruf für getrennte Geräte.

Jeder Benutzer mit Berechtigungen **Service Verbindung** IoT Hub kann eine Methode auf einem Gerät aufrufen.

### <a name="when-to-use"></a>Verwendung

Gerät Methoden ähneln [Cloud Gerät Nachrichten] [ lnk-devguide-messages] , beide Cloud-Back-End übergeben von Informationen an ein Gerät aktivieren sie grundlegend unterscheiden. Im Prinzip sind Methoden synchron und nicht permanente Cloud Gerät Nachrichten synchron mit bis zu 48 Stunden Haltbarkeit.

Methoden eine Anforderung / Antwort-Muster und sind nicht beständig. Mangelnde Haltbarkeit bietet zwei unmittelbare Vorteile, wenn Sie Geräte Befehle:

- **Sofortiges Feedback bei methodenausführung** bedeutet brauchen Sie die Korrelation zwischen Anforderung und Antwort verwalten.
- **Höherer Durchsatz** bedeutet, dass Vorgänge schneller ausgeführt werden können, da IoT Hub keine Haltbarkeit bereitstellt. IoT Hub ermöglicht mehrere Methodenaufrufe pro Einheit als Cloud-zu-Gerät-Nachrichten.

Cloud-zu-Gerät-Nachrichten sind nicht notwendigerweise Befehle an das Gerät aber eher repräsentieren einen Cloud-Dienst übergeben einige wenig Informationen an das Gerät zu seiner Freizeit Abholen und das Gerät möglicherweise oder reagiert nicht. Cloud-zu-Gerät-Nachrichten haben mehr Timeout (48 Stunden) während Methoden schneller abläuft.

Methoden Sie Gerät für sofortige Befehlsaufruf auf einem Gerät und Aufträge für geplanten Aufruf eines Befehls auf einem Gerät

## <a name="method-lifecycle"></a>Methode-Lebenszyklus

Methoden werden auf dem Gerät und benötigen oder mehrere Eingaben in der Nutzlast Methode korrekt instanziieren. Direkte Methode über einen Dienst mit URI aufrufen (`{iot hub}/twins/{device id}/methods/`). Ein Gerät erhält direkte Methoden eine gerätespezifische MQTT Thema (`$iothub/methods/POST/{method name}/`). Wir unterstützen Methoden auf zusätzliche geräteseitige Netzwerkprotokollen in Zukunft.

> [AZURE.NOTE] Beim Aufrufen einer direkten Methode auf einem Gerät Eigenschaftennamen und-Werte nur möglich US-ASCII-druckbare alphanumerisch, außer in den folgenden: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Methoden sind synchrone und entweder oder nachdem das Zeitlimit (Standard: 30 Sekunden von 3600 Sekunden festgelegt werden). Methoden eignen sich interaktive Szenarios soll ein handeln, wenn das Gerät online und empfangen, wie ein Licht über ein Telefon ist. In diesen Szenarien möchten Sie ein Erfolg oder Fehler anzeigen, damit Cloud-Dienst das Ergebnis so bald wie möglich nutzen kann. Das Gerät möglicherweise einige Nachrichtentext der Methode zurück, aber nicht für das Verfahren. Gibt es keine Garantie auf Bestellung oder eine beliebige Semantik Parallelität Methodenaufrufe.

Gerät Methodenaufrufe sind reine HTTP-Cloud-Seite und MQTT nur aus der Geräteseite.

## <a name="reference-topics"></a>Themen:

Die folgenden Themen bieten weitere Informationen zum direkte Methoden verwenden.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Eine direkte Methode aus einer Back-End-Anwendung

### <a name="method-invocation"></a>Methodenaufruf

Ein direkter Methodenaufrufe sind HTTP-Aufrufe umfassen:

- Der *URI* für das Gerät (`{iot hub}/twins/{device id}/methods/`)
- Der POST- *Methode*
- *Header* enthalten die Berechtigung anfordern, ID, Typ und Codierung
- Ein transparenter JSON *Text* im folgenden Format:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Timeout ist in Sekunden. Wenn kein Timeout festgelegt ist, wird standardmäßig auf 30 Sekunden.
  
### <a name="response"></a>Antwort

Back-End empfängt eine Antwort umfasst:

- *HTTP-Statuscode*, der Fehler von IoT Hub, einschließlich einen 404-Fehler nicht Geräte verwendet
- *Header* enthält das Etag anfordern, ID, Typ und Codierung
- Ein JSON- *Text* im folgenden Format:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Beide `status` und `body` vom Gerät bereitgestellt und Antworten mit Statuscode des Geräts bzw. Beschreibung verwendet.

## <a name="handle-a-direct-method-on-a-devcie"></a>Direkte Methode auf einer Devcie behandeln

### <a name="method-invocation"></a>Methodenaufruf

Geräte Anfragen direkte Methode zum Thema MQTT:`$iothub/methods/POST/{method name}/?$rid={request id}`

Der Text erhält das Gerät ist im folgenden Format:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Anforderungsmethoden sind QoS 0.

### <a name="response"></a>Antwort

Das Gerät sendet Antworten auf `$iothub/methods/res/{status}/?$rid={request id}`, wobei:

 - Die `status` -Eigenschaft ist das Gerät gelieferten methodenausführung.
 - Die `$rid` -Eigenschaft ist die ID aus dem Methodenaufruf IoT Hub empfangen.

Der Körper wird durch das Gerät festgelegt und kann jeder Status.

## <a name="additional-reference-material"></a>Zusätzliches Referenzmaterial

Andere Referenzthemen in Developer Guide enthalten:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschreibt die verschiedenen Endpunkte jedes IoT Hub Runtime und Management macht.
- [Kontingente und Drosselung] [ lnk-quotas] beschreibt die Quoten für den Dienst IoT Hub und Drosselung Verhalten gelten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub-Gerät und SDKs] [ lnk-sdks] Listet die verschiedenen SDKs Sie können Wenn Sie Anwendungsentwicklung Geräte und Dienste, die interagieren mit IoT Hub.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] beschreibt die Abfragesprache IoT Hub über Geräte im Vergleich, Methoden und Aufträge abrufen können.
- [IoT Hub MQTT Unterstützung] [ lnk-devguide-mqtt] MQTT Protokoll IoT Hub unterstützt Weitere Informationen bereit.

## <a name="next-steps"></a>Nächste Schritte

Nun direkten Methoden gelernt haben, können Sie im folgenden Thema Entwicklerhandbuch interessiert sein:

- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Möchten Sie versuchen, einige der in diesem Artikel beschriebenen Konzepte, können Sie die folgende praktische Einführung IoT Hub interessieren:

- [Cloud-zu-Gerät-Methoden][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
