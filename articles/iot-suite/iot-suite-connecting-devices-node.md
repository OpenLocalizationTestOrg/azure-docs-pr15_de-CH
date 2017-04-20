<properties
   pageTitle="Gerät mit Node.js | Microsoft Azure"
   description="Beschreibt, wie ein Gerät der Azure IoT Suite vorkonfiguriert remote monitoring-Lösung mit einer Anwendung Node.js geschrieben."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Verbinden Sie das Gerät remote monitoring vorkonfigurierte Lösung (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Erstellen und Ausführen der Beispielprojektmappe node.js

1. *Microsoft Azure IoT SDKs* GitHub Repository klonen und *Microsoft Azure IoT Device SDK Node.js* in Ihre desktop-Umgebung installieren, führen Sie die [Umgebung vorbereiten] [ lnk-github-prepare] Informationen.

2. Aus der lokalen Kopie der [Azure-Iot-Sdks] [ lnk-github-repo] Repository kopieren die folgenden Dateien aus dem Knoten/Gerätebeispiele Ordner in einen Ordner auf Ihrem Gerät:

  - Packages.JSON
  - remote_monitoring.js

3. Öffnen Sie die Datei remote_monitoring.js, und suchen Sie die folgende Variable:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. Die Verbindungszeichenfolge Gerät **[IoT Hub Gerät Verbindungszeichenfolge]** ersetzen. Die Werte entnehmen für IoT Hub Hostname, Geräte-Id und Gerät remote monitoring lösungsdashboard Sie. Eine Verbindungszeichenfolge Gerät hat das folgende Format:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Wenn der Hostname IoT Hub **Contoso** und die Geräte-Id **Mydevice**, sieht aus wie die Verbindungszeichenfolge:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Speichern Sie die Datei. Führen Sie die folgenden Befehle an der Befehlszeile in den Ordner mit diesen Dateien erforderlichen Pakete installieren und führen Sie die beispielanwendung:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md