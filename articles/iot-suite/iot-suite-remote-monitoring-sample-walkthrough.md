<properties
 pageTitle="Remoteüberwachung vorkonfigurierte Lösung Walkthrough | Microsoft Azure"
 description="Eine Azure IoT vorkonfigurierte Lösung remote-Überwachung und Architektur."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Remoteüberwachung vorkonfigurierte Lösung Exemplarische Vorgehensweise

## <a name="introduction"></a>Einführung

IoT Suite Remote monitoring [vorkonfigurierte Lösung] [ lnk-preconfigured-solutions] überwacht eine Implementierung einer End-to-End-Lösung für mehrere Computern an Remotestandorten. Die Lösung kombiniert Azure Schlüsseldienste eine generische Implementierung des Business-Szenarios und können Sie sie als Ausgangspunkt für Ihre eigene Implementierung. Sie können [Anpassen] ,[ lnk-customize] Lösung, die Ihren spezifischen Bedürfnissen.

Dieser Artikel führt Sie durch einige Schlüsselelemente remote monitoring-Lösung können Sie verstehen, wie es funktioniert. Dieses Wissen hilft Ihnen:

- Behandeln von Problemen in der Projektmappe.
- Planen Sie die Lösung für Ihren eigenen Bedürfnissen anpassen. 
- Entwerfen Sie Ihre eigene IoT-Lösung, die Azure Services verwendet.

## <a name="logical-architecture"></a>Logische Architektur

Das folgende Diagramm beschreibt die logischen Komponenten der vorkonfigurierte Lösung:

![Logische Architektur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## <a name="simulated-devices"></a>Simulierte Geräte

Vorkonfigurierte Lösung stellt das simulierte Gerät ein Kühlgerät (oder ein Gebäude Klimaanlage Anlage Gerät). Beim Bereitstellen der vorkonfigurierten Lösung bereitstellen Sie automatisch vier simulierte Geräte, ein [Azure Webauftrags]ausführen[lnk-webjobs]. Die simulierten Geräten erleichtern zu Verhalten der Lösung ohne physische Geräte bereitstellen. Um einem realen physischen Gerät bereitstellen, finden Sie unter [Verbinden Sie Ihr Gerät an die entfernte Datenbank überwachen vorkonfigurierte Lösung] [ lnk-connect-rm] Tutorial.

Jedes simulierten Gerät kann folgende Nachricht an IoT Hub senden:

| Nachricht  | Beschreibung |
|----------|-------------|
| Start  | Beginnt das Gerät sendet eine **Geräteinformationen** Meldung mit Informationen zu. Dazu gehören die Geräte-Id, Gerätemetadaten, eine Liste der Befehle das Gerät unterstützt und die aktuelle Konfiguration des Geräts. |
| Präsenz | Ein Gerät sendet regelmäßig eine Nachricht **vorhanden** gemeldet, ob das Gerät das Vorhandensein eines Sensors erkennen kann. |
| Telemetrie | Ein Gerät sendet regelmäßig eine **Telemetrie** -Nachricht, die simulierte für Temperatur und Luftfeuchtigkeit von simulierten Sensoren das Gerät gesammelt Werte. |


Die simulierten Geräten senden die folgenden Eigenschaften in einer **Geräte-Info** Nachricht:

| Eigenschaft               |  Zweck |
|------------------------|--------- |
| Geräte-ID              | ID, die bereitgestellt oder zugewiesen, wenn ein Gerät in der Projektmappe erstellt wird. |
| Hersteller           | Gerätehersteller |
| Modellnummer           | Modellnummer des Geräts |
| Seriennummer          | Seriennummer des Geräts |
| Firmware               | Aktuelle Version der Firmware des Geräts |
| Plattform               | Plattform-Architektur des Geräts |
| Prozessor              | Prozessor-Gerät |
| Installierte RAM          | Auf dem Gerät installierte RAM |
| Hub aktivierter Zustand      | IoT Hub State-Eigenschaft des Geräts |
| Erstellungszeit           | Uhrzeit der Erstellung des Geräts in der Projektmappe |
| Aktualisierungsdatum           | Zeitpunkt der letzten Aktualisierung wurden Eigenschaften für das Gerät |
| Latitude               | Latitude Standort |
| Länge              | Längengradposition des Geräts |

Der Simulator Samen dieser Eigenschaften in simulierten Geräten mit Beispieldaten.  Jedes Mal, wenn der Simulator ein simuliertes Gerät initialisiert stellt das Gerät vordefinierte Metadaten IoT Hub. Beachten Sie, wie Metadaten Aktualisierungen im Portal Gerät überschreibt.


Die simulierten Geräten können die folgenden Befehle aus dem lösungsdashboard IoT Hub gesendet behandeln:

| Befehl                | Beschreibung                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Sendet einen _Ping_ an das Gerät überprüfen aktiv ist   |
| StartTelemetry         | Startet das Gerät Telemetrie senden                 |
| StopTelemetry          | Das Gerät sendet Telemetrie beendet             |
| ChangeSetPointTemp     | Ändert den Sollwert, um den zufälligen Daten generiert wird |
| DiagnosticTelemetry    | Löst Gerätsimulator um eine zusätzliche Telemetrie Wert (ExternalTemp) |
| ChangeDeviceState      | Ändert eine erweiterten Status-Eigenschaft für das Gerät und sendet Gerät Informationen vom Gerät |

Gerät Befehl Bestätigung zu Lösung erfolgt über IoT Hub.

## <a name="iot-hub"></a>IoT Hub

[IoT Hub] [ lnk-iothub] in die Cloud von den Geräten gesendete Daten aufnimmt und Azure Stream Analytics (ASA) Aufträge verfügbar gemacht. IoT Hub sendet Befehle an Geräte für das Gerät Portal. Jeder Stream ASA Auftrag verwendet eine separate IoT Hub Verbrauchergruppe Lesen des Streams Nachrichten von Geräten.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Remote monitoring [Azure Stream Analytics] Solution[ lnk-asa] (ASA) sendet Nachrichten vom IoT Hub zu anderen Back-End-Komponenten zur Verarbeitung oder Lagerung Gerät. ASA Aufgaben führen bestimmte Funktionen basierend auf dem Inhalt der Nachrichten.

**Job 1: Geräteinformationen** filtert Gerät Informationsnachrichten aus der eingehenden Nachricht und sendet sie an einen Endpunkt Event Hub. Ein Gerät sendet Gerät Informationsnachrichten beim Start und einen **SendDeviceInfo** -Befehl. Dieses Projekt verwendet die folgenden Abfragedefinition **Device Info** -Nachrichten identifizieren:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Dieser Auftrag sendet die Ausgabe an einen Hub Ereignis zur weiteren Verarbeitung.

**Job 2: Regeln** eingehende Temperatur und Feuchtigkeit telemetriewerte gegen pro Gerät Schwellenwerte ausgewertet. Schwellenwerte sind in der Regel-Editor im lösungsdashboard verfügbar. Jedes Device-Wert-Paar wird nach dem Zeitstempel im Blob gespeichert, Stream Analytics als **Daten**liest. Der Auftrag vergleicht alle nicht leeren Wert mit festgelegten Schwellenwert für das Gerät. Überschreitet der ' >' Bedingung der Auftrag ausgegeben **, die angibt, dass der Schwellenwert überschritten wird und Gerät, Wert und Timestamp-Werte Ereignis** . Dieses Projekt verwendet die folgenden Abfragedefinition Telemetrie Nachrichten identifizieren, die einen Alarm auslösen soll:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Der Auftrag sendet die Ausgabe an einen Hub Ereignis zur weiteren Verarbeitung und speichert Details jeder Warnung BLOB-Speicher aus dem lösungsdashboard Warnung Informationen lesen können.

**Auftrag 3: Telemetrie** arbeitet mit den eingehenden Gerät Telemetrie Stream auf zwei Arten. Die erste sendet alle Telemetrie Nachrichten von den Geräten permanente Blob-Speicher für die langfristige Speicherung. Die zweite berechnet durchschnittliche, minimale und maximale Feuchtigkeitsgehalt über ein Zeitfenster von 5 Minuten und sendet diese Daten an BLOB-Speicher. Lösungsdashboard liest die Telemetriedaten aus BLOB-Speicher um Diagramme zu füllen. Dieser Job setzt die Definition der folgenden Abfrage:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Ereignis-Hubs

Aufträge ASA **Geräteinformationen** und **Regeln** Ausgabe ihrer Daten Ereignis Hubs zuverlässig auf der **Ereignisprozessor** in der Webauftrag weiterleiten.

## <a name="azure-storage"></a>Azure-Speicher

Die Lösung nutzt Azure BLOB-Speicher alle raw und zusammengefasste Telemetriedaten von Geräten in der Projektmappe beibehalten werden. Das Dashboard liest Telemetriedaten aus dem BLOB-Speicher Diagramme aufgefüllt. Alerts anzeigen liest Dashboard die Daten aus dem BLOB-Speicher, Datensätze telemetriewerte die konfigurierten Schwellenwerte überschritten. Die Lösung verwendet außerdem BLOB-Speicher im Dashboard festgelegten Schwellenwert aufgezeichnet.

## <a name="webjobs"></a>Webaufträge

Neben die Gerät Simulatoren, host Webaufträge in der Lösung der **Ereignisprozessor** in einer Azure Webauftrags, Handles Gerät Informationsnachrichten und Befehl Antworten. Verwendet:

- Gerät Informationsnachrichten die Registrierung des Geräts (gespeichert in der DocumentDB-Datenbank) mit den aktuellen Geräteinformationen aktualisieren.
- Befehl Antwortnachrichten Befehlsverlauf Gerät (gespeichert in der Datenbank DocumentDB) aktualisieren.

## <a name="documentdb"></a>DocumentDB

Die Lösung verwendet eine DocumentDB Datenbank zum Speichern von Informationen über Geräte zur Lösung. Hierzu zählen Gerätemetadaten und der Befehle aus dem Dashboard an Geräte gesendet.

## <a name="web-apps"></a>Webapps

### <a name="remote-monitoring-dashboard"></a>Remote monitoring dashboard
Diese Seite in der Anwendung verwendet PowerBI Javascript Steuerelemente (siehe [Grafik PowerBI Repo](https://www.github.com/Microsoft/PowerBI-visuals)) von den Geräten Telemetriedaten visualisieren. Die Lösung verwendet ASA Telemetrie Auftrag Telemetriedaten BLOB-Speicher.


### <a name="device-administration-portal"></a>Gerät-Verwaltungsportal

Dieses Web app können Sie:

- Bereitstellen Sie ein neues Gerät. Diese Aktion Legt die eindeutigen Geräte-Id und den Authentifizierungsschlüssel generiert. Er schreibt Informationen in Registrierung Identität IoT Hub und die Lösung DocumentDB-Datenbank.
- Verwalten von Eigenschaften. Diese Aktion enthält vorhandene Eigenschaften anzeigen und Aktualisieren mit neuen Eigenschaften.
- Senden Sie Befehle an ein Gerät.
- Zeigt die Chronik Befehl ein.
- Aktivieren und Deaktivieren von Geräten.

## <a name="next-steps"></a>Nächste Schritte

Blogbeiträge von TechNet enthalten weitere Informationen über das remote monitoring vorkonfigurierte Lösung:

- [IoT Suite - Nähkästchen - Überwachung](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [IoT Suite - Remote Monitoring - Live und simulierte Geräte](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Sie können fortfahren, erste Schritte mit IoT Suite durch die folgenden Artikel lesen:

- [Verbinden Sie das Gerät remote monitoring vorkonfigurierte Lösung][lnk-connect-rm]
- [Berechtigungen auf der Website azureiotsuite.com][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md