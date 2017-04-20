<properties
 pageTitle="Informationen Gerätemetadaten remote monitoring Lösung | Microsoft Azure"
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
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Gerät Informationen Metadaten in die Überwachung vorkonfigurierte Lösung

Azure IoT Suite remote vorkonfigurierte Lösung zeigt einen Ansatz zur Verwaltung von Gerätemetadaten. Dieser Artikel beschreibt den Ansatz dieser Lösung ermöglicht zu verstehen:

- Welche Gerätemetadaten speichert die Projektmappe.
- Wie die Lösung die Gerätemetadaten verwaltet.

## <a name="context"></a>Kontext

Remote monitoring vorkonfigurierte Lösung verwendet [Azure IoT Hub] [ lnk-iot-hub] Geräte zum Senden von Daten in die Cloud zu aktivieren. IoT Hub enthält eine [geräteidentitätsregistrierung] [ lnk-identity-registry] zum Steuern des Zugriffs auf IoT Hub. IoT Hub geräteidentitätsregistrierung ist unabhängig von Remote monitoring lösungsspezifische *geräteregistrierung* , die Geräte Informationen Metadaten gespeichert. Die remote monitoring-Lösung verwendet eine [DocumentDB] [ lnk-docdb] Datenbank in seiner geräteregistrierung zum Speichern von Informationen Gerätemetadaten implementieren. [Microsoft Azure IoT Referenzarchitektur] [ lnk-ref-arch] beschreibt die Rolle der Registrierung des Geräts in einer normalen IoT-Lösung.

> [AZURE.NOTE] Remote monitoring vorkonfigurierte Lösung hält die Registrierung des Geräts Identität mit Registrierung des Geräts. Beide verwenden die gleichen Geräte-Id zur eindeutigen Identifizierung jedes Gerät IoT Hub angeschlossen.

[IoT Hub Device Management Vorschau] [ lnk-dm-preview] IoT Hub, der in diesem Artikel beschriebenen Gerät Informationen Verwaltungsfunktionen ähneln Funktionen hinzugefügt. Remote monitoring-Lösung verwendet derzeit nur allgemein verfügbare (GA) Funktionen in IoT Hub.

## <a name="device-information-metadata"></a>Gerätemetadaten Informationen

Ein Gerät Informationen Metadaten JSON gespeichertes Dokument in der Registrierungsdatenbank DocumentDB Gerät hat folgende Struktur:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: das Gerät schreibt diese Eigenschaften und die Autorität für diese Daten. Andere Geräte Beispiel enthalten Hersteller, Modell und Seriennummer. 
- **DeviceID**: eindeutige Gerätenummer. Dieser Wert ist in der IoT Hub Gerät identitätsregistrierung.
- **HubEnabledState**: der Status des Geräts IoT Hub. Dieser Wert wird erst ersten Herstellen einer Verbindung zunächst auf **null** festgelegt. Im lösungsportal wird ein **null** -Wert als das Gerät "registrierte jedoch nicht vorhanden." dargestellt.
- **CreatedTime**: das Gerät wurde erstellt.
- **DeviceState**: der Zustand vom Gerät gemeldet.
- **UpdatedTime**: die Zeit das Gerät über das lösungsportal zuletzt aktualisiert wurde.
- **SystemProperties**: lösungsportal schreibt die Systemeigenschaften und das Gerät sind keine dieser Eigenschaften. Eine Systemeigenschaft Beispiel ermächtigt die Lösung mit der **ICCID** ist und an einen Dienst SIM-Geräte verwalten.
- **Befehle**: eine Liste der Befehle das Gerät unterstützt. Das Gerät stellt diese Informationen zur Lösung.
- **CommandHistory**: eine Liste der Befehle per remote monitoring-Lösung und den Status dieser Befehle.
- **IsSimulatedDevice**: ein Flag, das Gerät als simulierten Gerät an.
- **ID**: die Kennung DocumentDB für dieses Gerät.

> [AZURE.NOTE] Geräteinformationen kann auch Metadaten zur Beschreibung der Telemetrie Gerät IoT Hub sendet. Die remote monitoring-Lösung mithilfe dieser Metadaten Telemetrie anpassen, wie das Dashboard [dynamische Telemetrie]angezeigt[lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Lebenszyklus

Wenn ein Gerät im lösungsportal erstellen, erstellt die Projektmappe einen Eintrag in der Registrierung des Geräts wie zuvor gezeigt. Der Großteil dieser Informationen zunächst Stub und **HubEnabledState** auf **null**festgelegt. Zu diesem Zeitpunkt erstellt die Projektmappe auch einen Eintrag für das Gerät in der Geräte identitätsregistrierung Schlüssel generiert, wenn, die das Gerät mit IoT Hub authentifizieren.

Wenn ein Gerät zunächst der Projektmappe eine Verbindung herstellt, sendet ein Gerät Informationen angezeigt. Dieses Gerät Informationsnachricht enthält Eigenschaften wie Hersteller, Modell und Seriennummer. Eine Gerät Informationsnachricht enthält auch eine Liste der Befehle, die das Gerät einschließlich Informationen zum Befehlsparameter unterstützt. Bei die Lösung diese Nachricht empfängt, aktualisiert die Gerätemetadaten Informationen in die Registrierung des Geräts.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Anzeigen und Bearbeiten von Informationen im lösungsportal

Die Geräteliste im lösungsportal die folgenden Eigenschaften als Spalten angezeigt: **Status** **DeviceId**, **Hersteller**, **Modell**, **Seriennummer**, **Firmware**, **Plattform**, **Prozessoren**und **RAM installiert**. Die Geräteeigenschaften **breiten-** und **Längengrad** Laufwerk Position im Bing-Karte auf dem Dashboard. 

![Geräteliste][img-device-list]

Wenn Sie im **Gerät** im lösungsportal auf **Bearbeiten** klicken, können Sie diese Eigenschaften bearbeiten. Diese Eigenschaften bearbeiten aktualisiert den Datensatz für das Gerät in der DocumentDB-Datenbank. Wenn ein Gerät eine aktualisierte Gerätetreiber Info-Nachricht sendet, überschreibt es Änderungen im lösungsportal. **DeviceId**, **Hostname**, **HubEnabledState**, **CreatedTime**, **DeviceState**und **UpdatedTime** Eigenschaften im lösungsportal Sie können nicht bearbeitet das Gerät Autorität über diese Eigenschaften verfügt.

![Gerät bearbeiten][img-device-edit]

Lösungsportal können Sie ein Gerät aus der Projektmappe entfernen. Wenn Sie ein Gerät entfernen, Lösung entfernt Informationen Gerätemetadaten Lösung Gerät Registrierung und Eintrags in der Registrierung IoT Hub Gerät Identität. Bevor Sie ein Gerät entfernen können, müssen Sie ihn deaktivieren.

![Gerät entfernen][img-device-remove]

## <a name="device-information-message-processing"></a>Gerät die Meldung Verarbeitung

Gerät Informationsnachrichten vom Gerät unterscheiden sich von Telemetrie Nachrichten. Gerät Informationsnachrichten enthalten Informationen wie Eigenschaften, die Befehle können ein und alle Befehlsverlauf. IoT Hub selbst keine Kenntnis von einem Gerät Informationen Nachricht enthaltenen Metadaten und verarbeitet die Nachricht dasselbe Gerät Cloud Nachrichten verarbeitet. Remote monitoring ein [Azure Stream Analytics] Solution[ lnk-stream-analytics] (ASA) Auftrag liest Nachrichten IoT Hub. **DeviceInfo** Stream Analytics job Filter für Nachrichten mit **"Objekttyp": "DeviceInfo"** und leitet sie an die Hostinstanz **EventProcessorHost** , die in einem Webauftrag ausgeführt wird. Logik in der **EventProcessorHost** -Instanz verwendet die Geräte-Id DocumentDB-Datensatz für das Gerät und der Datensatz aktualisiert. Der Registrierungsdatensatz enthält jetzt Geräteeigenschaften, Befehle und Befehle.

> [AZURE.NOTE] Ein Gerät Informationen wird eine Standardnachricht Gerät Cloud. Die Lösung unterscheidet zwischen Gerät Informationen und Telemetrie Nachrichten mithilfe von ASA Abfragen.

## <a name="example-device-information-records"></a>Beispiel-Gerätedatensätze

Remote monitoring vorkonfigurierte Lösung verwendet zwei Arten von Gerätedatensätze: Datensätze für den simulierten Geräten Lösung und Datensätze für benutzerdefinierte Geräte mit der Projektmappe verbinden bereitgestellt.

### <a name="simulated-device"></a>Ausgabegerät

Das folgende Beispiel zeigt den JSON-Informationsdatensatz ein simulierter. Dieser Eintrag hat einen Wert für **UpdatedTime**, das angibt, dass das Gerät eine Nachricht **DeviceInfo** IoT Hub gesendet hat. Der Datensatz enthält einige allgemeine Geräteeigenschaften, definiert sechs Befehle die simulierten Geräten unterstützen und das **IsSimulatedDevice** -Flag auf **1**festgelegt.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Benutzerdefiniertes Gerät

Das folgende Beispiel zeigt den JSON-Informationsdatensatz ein benutzerdefiniertes und hat das **IsSimulatedDevice** -Flag auf **0**gesetzt. Sie sehen, dass dieses benutzerdefinierte zwei Befehle unterstützt und lösungsportal **SetTemperature** -Befehl an das Gerät gesendet wurde:

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

Hier der Meldung JSON **DeviceInfo** Gerät gesendeten Informationen Gerätemetadaten aktualisieren:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie lernen, wie die vorkonfigurierten Lösungen anpassen haben, Erkunden Sie die Features und Funktionen IoT Suite vorkonfigurierte Lösung:

- [Vorbeugende Wartung vorkonfiguriert Lösungsübersicht][lnk-predictive-overview]
- [Häufig gestellte Fragen zum IoT Suite][lnk-faq]
- [IoT Sicherheit von Grund auf][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
