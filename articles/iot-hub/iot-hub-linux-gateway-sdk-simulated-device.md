<properties
    pageTitle="Simuliert ein SDK Gateway IoT | Microsoft Azure"
    description="Azure IoT Gateway SDK Exemplarische Vorgehensweise Linux senden Telemetriedaten aus einem simulierten Gerät Azure IoT Gateway SDK erläutern."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-linux"></a>Azure IoT Gateway SDK (Beta) – Nachrichten Gerät Cloud mit einem simulierten Gerät mit Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Bevor Sie beginnen, müssen Sie:

- [Einrichten der Umgebung] [ lnk-setupdevbox] für die Arbeit mit dem SDK unter Linux.
- [Erstellen einen IoT Hub] [ lnk-create-hub] in der Azure-Abonnements benötigen Sie den Namen des Haupt-diese exemplarische Vorgehensweise. Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.
- IoT Hub zwei Geräte hinzu, und ihre Ids und Gerät Schlüssel notieren. Können [Geräte Explorer oder Iothub Explorer] [ lnk-explorer-tools] Tool IoT Hub im vorherigen Schritt erstellten Geräte hinzufügen und ihre Schlüssel abzurufen.

Zum Beispiel:

1. Öffnen Sie eine Shell.
2. Navigieren Sie zum Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** -Repository.
3. Führen Sie das Skript **tools/build.sh** . Dieses Skript verwendet das Dienstprogramm **Cmake** Ordner **Erstellen** im Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository erstellen und generieren eine Makefile. Das Skript erstellt die Projektmappe und die Tests ausführt.

> [AZURE.NOTE]  Jedes Mal, wenn das Skript **build.sh** löscht und dann neu **Erstellen** Ordner im Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** -Repository.

Um das Beispiel auszuführen:

Öffnen Sie in einem Texteditor die Datei **samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json** in Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** -Repository. Diese Datei konfiguriert die Beispielgateway Module:

- Das Modul **IoTHub** Verbindung IoT Hub. Sie müssen Daten IoT Hub senden konfigurieren. Legen Sie den Wert **IoTHubName** auf den Namen des Haupt-IoT insbesondere auf **Azure devices.net** **IoTHubSuffix** eingestellt. Legen Sie **den Wert** eines: "HTTP", "AMQP" oder "MQTT". Beachten Sie, dass derzeit nur "HTTP" eine TCP-Verbindung für alle Geräte Nachrichten freigeben. Wenn Sie den Wert für "AMQP" oder "MQTT" festgelegt, behält das Gateway eine separate TCP-Verbindung IoT Hub für jedes Gerät.
- Modul **Zuordnung** der IoT Hub-Geräte-Ids die MAC-Adressen des simulierten Geräten zugeordnet. Sicher **DeviceId** Werte die zwei Geräte-Ids übereinstimmen IoT Hub hinzugefügt und die Werte **DeviceKey** die Taste zwei Geräte enthalten.
- Die **BLE1** und **BLE2** sind den simulierten Geräten. Beachten Sie, wie die MAC-Adressen im Modul **Zuordnung** übereinstimmen.
- Das Modul **Protokollierung** protokolliert Gateway-Aktivitäten in einer Datei.
- **Modulpfad** Werte unterhalb davon ausgehen, dass das Beispiel aus dem Stammverzeichnis Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository ausgeführt wird.
- Array **Links** unten die JSON-Datei verbunden mit der **Zuordnung** Modulen, und **Zuordnung** mit dem **IoTHub** -Modul Module **BLE1** und **BLE2** . Es wird sichergestellt, dass alle Nachrichten vom Modul **Protokollierung** protokolliert werden.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "./build/modules/iothub/libiothub_hl.so",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "./build/modules/identitymap/libidentity_map_hl.so",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "./build/modules/logger/liblogger_hl.so",
            "args":
            {
                "filename":"./deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}

```

Speichern Sie Änderungen in der Konfigurationsdatei.

Um das Beispiel auszuführen:

1. Navigieren Sie in der Shell der Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** -Repository.
2. Führen Sie den folgenden Befehl ein:

    ```
    ./build/samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ./samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```

3. Können [Geräte Explorer oder Iothub Explorer] [ lnk-explorer-tools] Tool Nachrichten überwachen, die vom Gateway IoT Hub empfängt.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie IoT Gateway SDK komplexere Verständnis und probieren einige Codebeispiele, besuchen Sie die folgenden Developer Tutorials und Ressourcen:

- [Gerät Cloud-Nachrichten von einem echten Gerät IoT Gateway SDK][lnk-physical-device]
- [Azure IoT Gateway SDK][lnk-gateway-sdk]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Sichern Sie Ihre IoT Lösung von Grund auf][lnk-securing]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-create-hub]: iot-hub-create-through-portal.md