<properties
 pageTitle="Entwicklerhandbuch - jobs | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch - an Ihrem Hub angeschlossen Planen von Aufträgen auf mehreren Geräten"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Planen von Aufträgen auf mehreren Geräten (Vorschau)

## <a name="overview"></a>Übersicht

Wie in früheren Artikeln beschrieben Azure IoT Hub ermöglicht Bausteine ([zwei Eigenschaften und Tags] [ lnk-twin-devguide] und [Cloud-zu-Gerät-Methoden][lnk-dev-methods]).  Normalerweise aktivieren IoT Back-End-Applikationen Geräteadministratoren und Operatoren aktualisieren und IoT Geräte und zu einem geplanten Zeitpunkt.  Aufträge kapseln die Ausführung Twin Geräteupdates und C2D-Methoden für verschiedene Geräte gleichzeitig planen.  Beispielsweise verwenden ein Operator eine Back-End-Anwendung, die initiieren und verfolgen einen Auftrag eine Reihe von Geräten in 43 und Ebene 3, die nicht störend auf den Betrieb des Gebäudes gleichzeitig starten.

### <a name="when-to-use"></a>Verwendung

Berücksichtigen Sie bei Aufträgen mit: Lösung Back-end zu planen und verfolgen die folgenden Aktivitäten auf Gerät:

- Zwei gewünschten Eigenschaften aktualisieren
- Gerät zwei Tags aktualisieren
- C2D-Methoden aufrufen

## <a name="job-lifecycle"></a>Projekt-Lebenszyklus

Aufträge werden von Back-End Lösung und IoT Hub verwaltet.  Einen Auftrag über einen Dienst mit URI initiiert (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) und bei ausgeführten Auftrags durch einen Dienst mit URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Sobald ein Projekt gestartet wird, wird für Projekte Abfragen Back-End-Anwendung aktualisiert den Status der ausgeführten Aufträge aktivieren.

> [AZURE.NOTE] Beim Starten eines Auftrags Eigenschaftennamen und-Werte nur möglich US-ASCII-druckbare alphanumerisch, außer in den folgenden: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Themen:

Die folgenden Themen bieten weitere Informationen über Aufträge.

## <a name="jobs-to-execute-direct-methods"></a>Einzelvorgänge zum direkte Methoden ausführen

Folgendes ist HTTP 1.1 Anforderungsdetails zum Ausführen einer [direkten Methode] [ lnk-dev-methods] auf ein Projekt mit:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Aufträge aktualisieren und Eigenschaften

Im folgenden finden HTTP 1.1 Anforderungsdetails zum Aktualisieren eines Auftrags mit zwei Eigenschaften:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Bei Aufträgen Abfragen

Folgendes ist HTTP 1.1 Anforderungsdetails für [Projekte Abfragen][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
Die ContinuationToken werden aus der Antwort.  

## <a name="jobs-properties"></a>Aufträge-Eigenschaften

Folgendes ist eine Liste der Eigenschaften und entsprechenden Beschreibung für Auftragsergebnisse oder beim Abfragen verwendet werden können.

| Eigenschaft | Beschreibung |
| -------------- | -----------------|
| **Auftrags** | Anwendung bereitgestellte ID für den Auftrag. |
| **Startzeit** | Anwendung bereitgestellten Startzeit (ISO 8601) für den Auftrag. |
| **Endzeit** | IoT Hub bereitgestellten Datum (ISO 8601) Wenn der Auftrag abgeschlossen wurde. Gültig nur, nachdem der Auftrag den Status "abgeschlossen" erreicht. | 
| **Typ** | Arten von Einzelvorgängen: |
| | **ScheduledUpdateTwin**: ein Job eine Reihe von Eigenschaften und Bedarf oder Tags aktualisiert. |
| | **ScheduledDeviceMethod**: ein Projekt für ein Gerät auf zwei Methodenaufruf verwendet. |
| **Status** | Aktuellen Status des Auftrags. Mögliche Werte für Status: |
| | **Ausstehend** : geplant und Empfang durch den Dienst bereit. |
| | **Geplante** : für einen späteren Zeitpunkt geplant. |
| | **Ausführen** : aktiven Auftrag. |
| | **abgebrochen** : Aufgabe wurde abgebrochen. |
| | **Fehler** : Projekt konnte. |
| | **abgeschlossen** : Auftrag wurde abgeschlossen. |
| **deviceJobStatistics** | Statistiken über die Ausführung des Auftrags. |

Während der Vorschau steht das DeviceJobStatistics Objekt erst nach Einzelvorgang abgeschlossen ist.

| Eigenschaft | Beschreibung |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Anzahl der Geräte im Auftrag. |
| **deviceJobStatistics.failedCount** | Anzahl der Geräte, in dem der Auftrag fehlgeschlagen ist. |
| **deviceJobStatistics.succeededCount** | Anzahl der Geräte, in dem der Auftrag war erfolgreich. |
| **deviceJobStatistics.runningCount** | Anzahl der Geräte, die derzeit den Auftrag ausgeführt werden. |
| **deviceJobStatistics.pendingCount** | Anzahl der Geräte, die zum Ausführen des Auftrags ausstehen. |


### <a name="additional-reference-material"></a>Zusätzliches Referenzmaterial

Andere Referenzthemen in Developer Guide enthalten:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschreibt die verschiedenen Endpunkte jedes IoT Hub Runtime und Management macht.
- [Kontingente und Drosselung] [ lnk-quotas] beschreibt die Quoten für den Dienst IoT Hub und Drosselung Verhalten gelten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub-Gerät und SDKs] [ lnk-sdks] Listet die verschiedenen SDKs Sie können Wenn Sie Anwendungsentwicklung Geräte und Dienste, die interagieren mit IoT Hub.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] beschreibt die Abfragesprache IoT Hub über Geräte im Vergleich, Methoden und Aufträge abrufen können.
- [IoT Hub MQTT Unterstützung] [ lnk-devguide-mqtt] MQTT Protokoll IoT Hub unterstützt Weitere Informationen bereit.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie versuchen, einige der in diesem Artikel beschriebenen Konzepte, können Sie die folgende praktische Einführung IoT Hub interessieren:

- [Zeitplan und Broadcast-Aufträge][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
