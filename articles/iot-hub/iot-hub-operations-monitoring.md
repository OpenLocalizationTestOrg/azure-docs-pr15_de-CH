<properties
 pageTitle="IoT Hub Vorgänge überwachen"
 description="Eine Übersicht über Azure IoT Hub Vorgänge überwachen, den Status von Vorgängen auf Ihrem Haupt-IoT in Echtzeit überwachen"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-operations-monitoring"></a>Einführung in die Überwachung von Vorgängen

IoT Hub Vorgänge überwachen können Sie den Status von Vorgängen auf Ihrem Haupt-IoT in Echtzeit überwachen. IoT Hub verfolgt Ereignisse in verschiedene Kategorien von Vorgängen, und abonnieren Sie können Ereignisse aus einer oder mehreren Kategorien zu einem Endpunkt der IoT Hub zur Verarbeitung gesendet. Sie können Daten auf Fehler überwachen oder komplexere Verarbeitung auf Datenmuster.

IoT Hub überwacht fünf Kategorien von Ereignissen:

- Identität Gerätevorgänge
- Gerät Telemetrie
- Cloud-zu-Gerät-Befehle
- Anschlüsse
- Datei-uploads

## <a name="how-to-enable-operations-monitoring"></a>Aktivieren der Überwachung von Vorgängen

1. IoT Hub zu erstellen. Finden Sie Informationen zum Erstellen von IoT Hub im [Einstieg] [ lnk-get-started] Guide.

2. Öffnen Sie das Blade IoT Hub. Klicken Sie dort auf **Vorgänge überwachen**.

    ![][1]

3. Wählen Sie die Überwachung überwachen, und klicken Sie dann auf **Speichern**möchten. Die Ereignisse sind zum Lesen aus dem Ereignis Hub-kompatiblen Endpunkt **Überwachung Einstellungen**aufgeführt. IoT Hub Endpunkt heißt `messages/operationsmonitoringevents`.

    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>Kategorien und ihre Verwendung

Jede Kategorie Spuren Überwachung von Vorgängen eine andere Art von Interaktion mit IoT Hub und jede Überwachung Kategorie ein Schema verfügt Strukturierung der Ereignisse in dieser Kategorie.

### <a name="device-identity-operations"></a>Identität Gerätevorgänge

Die Gerätekategorie Identität Operationen verfolgt beim Erstellen, aktualisieren oder Löschen eines Eintrags in der IoT Hub identitätsregistrierung auftretenden Fehler. Überwachung dieser Kategorie eignet sich für die Bereitstellung von Szenarien.

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>Gerät Telemetrie

Die Gerätekategorie Telemetrie verfolgt Fehler, die IOT Hub und beziehen sich auf die Pipeline Telemetrie. Zu dieser Kategorie gehören Fehler beim Telemetrie-Ereignisse (z. B. die Einschränkung) senden und empfangen Telemetrie-Ereignisse (z. B. nicht autorisierte Leser). Beachten Sie, dass diese Kategorie Fehler Code auf dem Gerät selbst fangen kann.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>Cloud-zu-Gerät-Befehle

Kategorie Cloud Gerät Befehle protokolliert Fehler, die IOT Hub und beziehen sich auf Gerät Befehlspipeline. Diese Kategorie enthält Fehler beim Senden von Befehlen (z. B. unbefugte Absender), Befehle (z. B. Lieferung Anzahl überschritten) und Befehl Feedback (z. B. Feedback abgelaufen) empfangen. Diese Kategorie fängt keine Fehler ein, die einen Befehl falsch behandelt, wenn der Befehl erfolgreich übermittelt wurde.

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>Anschlüsse

Kategorie Verbindung protokolliert Fehler beim Verbinden oder von IoT Hub trennen. Nachverfolgen dieser Kategorie ist nützlich zum Identifizieren von nicht autorisierten Verbindungsversuche und zum Nachverfolgen, wenn eine Verbindung für Geräte in Bereichen schlechte Verbindung unterbrochen wird.

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>Datei-uploads

Datei hochladen Kategorie protokolliert Fehler, die IOT Hub und beziehen sich auf die Funktion zum Hochladen. Diese Kategorie umfasst auftretenden mit SAS-URI (z. B. wenn es abläuft, bevor ein Gerät den Hub Upload abgeschlossen benachrichtigt), fehlgeschlagene Uploads gemeldet vom Gerät und eine Datei während IoT Hub Benachrichtigung Nachricht erstellen nicht im Speicher gefunden wird. Beachten Sie, dass diese Kategorie kann nicht Fehler aufzufangen, die direkt ausgeführt werden, während das Gerät eine Datei in den Speicher hochladen.

    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>Nächste Schritte

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md