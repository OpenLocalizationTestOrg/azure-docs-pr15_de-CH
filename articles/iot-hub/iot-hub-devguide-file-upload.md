<properties
 pageTitle="Entwicklerhandbuch - Dateiupload | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch - Hochladen von Dateien von einem Gerät an IoT Hub"
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

# <a name="upload-files-from-a-device"></a>Hochladen von Dateien von einem Gerät

## <a name="overview"></a>Übersicht

Im [IoT Hub Endpunkte] [ lnk-endpoints] Artikel Geräte können initiieren Dateiuploads durch Senden einer Benachrichtigung über einen Endpunkt verbundenen Geräte (**/devices/ {DeviceId} / Dateien**).  Wenn ein Gerät IoT Hub der Upload abgeschlossen informiert, generiert IoT Hub Datei hochladen Benachrichtigungen, die durch einen Dienst verbundenen Endpunkt (**/messages/servicebound/filenotifications**) als Nachrichten empfangen werden können.

Statt Vermittlung Nachrichten über IoT Hub selbst fungiert IoT Hub stattdessen als Verteiler zugeordneten Azure Storage-Konto. Ein Gerät fordert einen Token Speicher IoT Hub, die für die Datei das Gerät laden möchte. Das Gerät verwendet SAS-URI in den Speicher hochladen und nach Abschluss des Uploads sendet das Gerät eine Benachrichtigung der Fertigstellung IoT Hub. IoT Hub stellt sicher, dass die Datei hochgeladen wurde und dann der neue Dienst mit Datei Benachrichtigung messaging Endpunkt eine Datei hochladen Benachrichtigung hinzugefügt.

IoT Hub von einem Gerät hochladen, müssen konfigurieren den Hub indem [Azure Storage] [ lnk-associate-storage] zu berücksichtigen.

Ihr Gerät kann [einen Upload initialisieren] [ lnk-initialize] und [Benachrichtigen IoT Hub] [ lnk-notify] Abschluss des Uploads. Optional, wenn ein Gerät IoT Hub informiert, dass der Upload abgeschlossen ist, der Dienst erzeugen eine [Benachrichtigung][lnk-service-notification].

### <a name="when-to-use"></a>Verwendung

Verwenden Sie diese Funktion IoT Hub, wenn Sie eine Datei von einem Gerät auf Ihre Back-End-Dienst, anstatt eine Nachricht über IoT Hub hochladen müssen.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>IoT Hub ein Azure Storage-Konto zuordnen

Um Datei-Upload-Funktionalität zu verwenden, müssen Sie zuerst IoT Hub Azure Storage-Konto verknüpfen. Sie erreichen dies entweder über das [Azure-Portal][lnk-management-portal], oder programmgesteuert über [IoT Hub Ressourcenanbieter REST-APIs][lnk-resource-provider-apis]. Nachdem Sie IoT Hub Azure Storage-Konto zugeordnet ist, gibt der Dienst SAS-URI mit einem Gerät, wenn das Gerät eine Datei-Upload-Anforderung initiiert.

> [AZURE.NOTE] [Azure IoT Hub SDKs] [ lnk-sdks] SAS-URI abgerufen und Hochladen der Datei informiert IoT Hub der Upload abgeschlossen automatisch behandelt.

## <a name="initialize-a-file-upload"></a>Initialisieren eines Datei-Uploads

IoT Hub hat einen Endpunkt speziell für Geräte ein SAS-URI für Speicher hochladen eine Datei anfordern. Initiiert das Gerät den Dateiupload durch Senden eines BEITRAGS an IoT Hub an `{iot hub}.azure-devices.net/devices/{deviceId}/files` mit folgenden JSON-Text:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT Hub gibt das Gerät verwendet die Datei folgenden zurück:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Veraltete: Initialisieren Sie Dateiupload mit GET

> [AZURE.NOTE] Dieser Abschnitt beschreibt veraltete Funktionen für SAS-URI IoT Hub empfangen. Verwenden Sie die oben beschriebene buchen.

IoT Hub verfügt über zwei REST-Endpunkte Datei hochladen SAS-URI für Speicher und IoT Hub der Upload abgeschlossen benachrichtigen zu unterstützen. Initiiert das Gerät der Hochladevorgang per ein IoT Hub unter `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Hub gibt zurück SAS-URI für die Datei hochgeladen werden und eine Korrelations-ID nach Abschluss des Uploads verwendet werden.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>IoT Hub abgeschlossenen Dateiupload benachrichtigen

Das Gerät ist verantwortlich für das Hochladen der Datei auf Speicher mit Azure Storage SDKs. Nach Abschluss des Uploads sendet das Gerät ein IoT Hub an `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` mit folgenden JSON-Text:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Der Wert der `isSuccess` ist ein Boolean, ob die Datei erfolgreich hochgeladen wurde. Der Statuscode für `statusCode` ist der Status für den Upload der Datei in den Speicher, und die `statusDescription` entspricht der `statusCode`.

## <a name="reference-topics"></a>Themen:

Die folgenden Themen bieten weitere Informationen zum Hochladen von Dateien von einem Gerät.

## <a name="file-upload-notifications"></a>Datei-Upload-Benachrichtigung

Als ein Gerät eine Datei IoT Hub Upload Abschluss benachrichtigt, erstellt der Dienst optional eine Benachrichtigung, die den Namen und den Speicherort der Datei enthält.

Als [Endpunkte]erklärt[lnk-endpoints], IoT Hub liefert Datei hochladen Benachrichtigung über einen Dienst verbundenen Endpunkt (**/messages/servicebound/fileuploadnotifications**) als Nachrichten. Empfangen-Semantik für Datei-Uploads Benachrichtigung entsprechen Cloud Gerät Nachrichten und die gleiche [Nachricht Lebenszyklus][lnk-lifecycle]. Jede Datei hochladen Benachrichtigung Endpunkt entnommen wird ein JSON-Datensatz mit folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| EnqueuedTimeUtc | Zeitstempel, der angibt, wann die Benachrichtigung erstellt wurde. |
| Geräte-ID | **Geräte-ID** des Geräts die Datei hochgeladen. |
| BlobUri | Der URI der hochgeladenen Datei. |
| BlobName | Name der hochgeladenen Datei. |
| LastUpdatedTime | Zeitstempel, der angibt, wann die Datei zuletzt aktualisiert wurde. |
| BlobSizeInBytes | Die Größe der hochgeladenen Datei. |

**Beispiel**. Dies ist ein Beispiel einer Datei-Upload-Benachrichtigung.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Upload Notification-Optionen

Jede IoT Hub stellt die folgenden Konfigurationsoptionen für Datei-Upload-Benachrichtigung:

| Eigenschaft | Beschreibung | Bereich und Standard |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Steuert, ob Datei hochladen Benachrichtigungen Endpunkt Benachrichtigungen Datei geschrieben werden. | Bool. Standard: True. |
| **fileNotifications.ttlAsIso8601** | Standard-TTL für Datei-Upload-Benachrichtigung. | ISO_8601 Intervall bis zu 48 H (mindestens 1 Minute). Standard: 1 Stunde. |
| **fileNotifications.lockDuration** | Dauer der Datei Benachrichtigungen Warteschlange zu sperren. | 5 bis 300 Sekunden (mindestens 5 Sekunden). Standardwert: 60 Sekunden. |
| **fileNotifications.maxDeliveryCount** | Lieferung maximale Anzahl für die Datei hochladen Warteschlange. | 1 bis 100. Standard: 100. |

## <a name="additional-reference-material"></a>Zusätzliches Referenzmaterial

Andere Referenzthemen in Developer Guide enthalten:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschreibt die verschiedenen Endpunkte jedes IoT Hub Runtime und Management macht.
- [Kontingente und Drosselung] [ lnk-quotas] beschreibt die Quoten für den Dienst IoT Hub und Drosselung Verhalten gelten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub-Gerät und SDKs] [ lnk-sdks] Listet die verschiedenen SDKs Sie können Wenn Sie Anwendungsentwicklung Geräte und Dienste, die interagieren mit IoT Hub.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] beschreibt die Abfragesprache IoT Hub über Geräte im Vergleich, Methoden und Aufträge abrufen können.
- [IoT Hub MQTT Unterstützung] [ lnk-devguide-mqtt] MQTT Protokoll IoT Hub unterstützt Weitere Informationen bereit.

## <a name="next-steps"></a>Nächste Schritte

Jetzt zum Hochladen von Dateien von Geräten mit IoT Hub gelernt haben, können Sie in den folgenden Themen Entwicklerhandbuch interessiert sein:

- [Verwalten von Identitäten in IoT Hub Gerät][lnk-devguide-identities]
- [Steuern des Zugriffs auf IoT Hub][lnk-devguide-security]
- [Gerät im Vergleich mit dem Zustand und Konfigurationen synchronisiert][lnk-devguide-device-twins]
- [Eine direkte Methode auf einem Gerät][lnk-devguide-directmethods]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Möchten Sie versuchen, einige der in diesem Artikel beschriebenen Konzepte, können Sie die folgende praktische Einführung IoT Hub interessieren:

- [Wie Sie Dateien von Geräten mit IoT Hub in die Cloud hochladen][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
