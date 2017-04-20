<properties
     pageTitle="Verwenden das Azure-Portal Dateiupload konfigurieren | Microsoft Azure"
     description="Überblick Dateiupload mit Azure-Portal konfigurieren"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>Konfigurieren Sie Upload von Dateien mit Azure-portal

## <a name="file-upload"></a>Datei hochladen

[Dateiupload Funktionalität IoT Hub]mit[lnk-upload], müssen Sie zunächst ein Azure Storage-Konto mit Ihrem Hub. Wählen der **Datei hochladen** Upload Eigenschaften IoT Hub aufgelistet, die geändert wird.

![][13]

**Behälter**: verwenden das Azure-Portal auf BLOB-Container in einem Azure-Speicher Ihres aktuellen Azure Abonnements IoT Hub zugeordnet. Bei Bedarf können Sie ein Azure Storage-Konto für den **Speicherkonten** Blade und BLOB-Container auf den **Container** erstellen. IoT Hub generiert automatisch SAS-URIs mit Schreibberechtigungen für diesen Container BLOB-Geräte verwenden, wenn sie Dateien hochladen.

![][14]

**Benachrichtigung für hochgeladene Dateien empfangen**: Aktivieren oder deaktivieren Sie Datei hochladen Benachrichtigung über das umschalten.

**SAS-TTL**: Diese Einstellung wird die Time-to-live SAS-URIs zurückgegebene IoT Hub für das Gerät. Standardmäßig eine Stunde, aber andere Werte mit dem Schieberegler angepasst werden.

**Benachrichtigung über, TTL Standardeinstellungen**: Benachrichtigung der Time-to-live einer Datei hochladen, bevor sie abgelaufen ist. Standardmäßig ein Tag aber auf andere Werte mit dem Schieberegler angepasst werden.

**Benachrichtigung Lieferung maximale Anzahl**: Anzahl der IoT Hub versucht, eine Datei hochzuladen Benachrichtigung. Standardmäßig auf 10 festgelegt, aber auf andere Werte mit dem Schieberegler angepasst werden.

![][15]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über die Datei-Upload-Funktionen IoT Hub finden Sie [Hochladen von Dateien von einem Gerät] [ lnk-upload] in das Entwicklerhandbuch.

Folgen Sie diesen Links, um weitere Informationen zum Verwalten von Azure IoT Hub:

- [BULK IoT-Geräte verwalten][lnk-bulk]
- [Verwendung Metriken][lnk-metrics]
- [Überwachung von Vorgängen][lnk-monitor]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway]
- [Sichern Sie Ihre IoT Lösung von Grund auf][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md