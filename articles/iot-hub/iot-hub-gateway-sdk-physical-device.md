<properties
    pageTitle="Ein reales Gerät IoT Gateway SDK verwenden | Microsoft Azure"
    description="Azure IoT Gateway SDK Exemplarische Vorgehensweise Texas Instruments SensorTag Gerät zum Senden von Daten an IoT Hub über ein Gateway auf ein Intel Edison Compute-Modul"
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure IoT Gateway SDK (Beta) – Nachrichten Gerät Cloud mit einem echten Gerät mit Linux

In dieser exemplarischen Vorgehensweise [Bluetooth geringem Beispiel] [ lnk-ble-samplecode] veranschaulicht die [Azure IoT Gateway SDK] verwenden[ lnk-sdk] , vorwärts Gerät Cloud Telemetrie IoT Hub ein physisches Gerät und Befehle IoT Hub zu einem physischen Gerät weiterleiten.

Diese exemplarische Vorgehensweise umfasst:

* **Architektur**: Wissenswertes Bluetooth geringem Beispiel Architektur.

* **Erstellen und Ausführen**: die erforderlichen Schritte zum Erstellen und Ausführen des Beispiels.

## <a name="architecture"></a>Architektur

Der exemplarischen wird Vorgehensweise erstellen und führen ein IoT Gateway für ein Intel Edison Compute-Modul, das Linux ausgeführt. Das Gateway basiert IoT Gateway SDK. Das Beispiel verwendet ein Texas Instruments SensorTag Bluetooth niedriger Energie (BLE) Daten sammeln.

Wenn Sie das Gateway ausführen es:

- Verbindet ein SensorTag Gerät mit Bluetooth niedriger Energie (BLE)-Protokoll.
- Verbindung IoT Hub über HTTP.
- Leitet Telemetrie IoT Hub von Gerät SensorTag.
- Routen Befehle IoT Hub mit dem Gerät SensorTag.

Das Gateway enthält die folgenden Module:

- Ein *BLE Modul* , das mit einem BLE Gerät Gerät und sendet Befehle an das Gerät Daten erhalten.
- *BLE Cloud Modul* , die JSON-Nachrichten aus der Cloud in BLE Anweisungen für *BLE Modul*übersetzt.
- Eine *Protokollierung Modul* , das alle Gateway Nachrichten protokolliert.
- Ein *Modul Zuordnung* , die zwischen BLE Gerät MAC-Adressen und Azure IoT Hub Gerät Identitäten übersetzt.
- Ein *IoT Hub-Modul* , das Daten an einen Hub IoT uploads und IoT Hub gerätebefehle empfängt.
- *BLE Drucker Module* , die vom Gerät BLE Telemetrie interpretiert und druckt formatierter Daten an die Konsole zur Problembehandlung und Debuggen.

### <a name="how-data-flows-through-the-gateway"></a>Datenflusses über das Gateway

Das folgenden Blockdiagramm zeigt Telemetrie Upload Fluss Datenpipeline:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

Die Schritte, die ein Element der Telemetrie akzeptiert unterwegs von einem Gerät BLE IoT Hub sind:

1. BLE Gerät generiert ein Beispiel Temperatur und über Bluetooth Modul BLE im Gateway sendet.
2. BLE Modul im Beispiel empfängt und Broker mit der MAC-Adresse des Geräts veröffentlicht.
3. Zuordnung Zugriffszeichenfolge nimmt diese Nachricht und verwendet eine interne Tabelle, die MAC-Adresse des Geräts in IoT Hub geräteidentität (Geräte-ID und Gerät) übersetzen. Anschließend wird eine neue Nachricht mit Beispieldaten Temperatur, die MAC-Adresse des Geräts, die Geräte-ID und den Geräteschlüssel veröffentlicht.
4. Modul IoT Hub empfängt die neue Nachricht (generiert durch die Zugriffszeichenfolge Zuordnung) und IoT Hub veröffentlicht.
5. Das Modul Protokollierung protokolliert alle Nachrichten vom Broker in eine Datei.

Das folgenden Blockdiagramm zeigt Gerät Befehl Fluss Datenpipeline:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. Das Modul IoT Hub fragt regelmäßig IoT Hub Nachrichteneingang Befehl.
2. Empfängt das Modul IoT Hub eine neue Befehlsnachricht veröffentlicht es Broker.
3. Zuordnung Zugriffszeichenfolge nimmt die Befehlsnachricht und verwendet eine interne Tabelle Gerätenummer IoT Hub, ein MAC-Adresse übersetzt. Anschließend wird eine neue Nachricht, die MAC-Adresse des Zielgeräts in der Eigenschaften der Nachricht enthält, veröffentlicht.
4. BLE Cloud Modul diese Nachricht nimmt und die richtige Anweisung BLE BLE Modul übersetzt. Anschließend wird eine neue Nachricht veröffentlicht.
5. Das Modul BLE nimmt diese Nachricht und führt e/a-Anweisung BLE Gerät kommunizieren.
6. Das Modul Protokollierung protokolliert alle Nachrichten vom Broker in eine Datei.

## <a name="prepare-your-hardware"></a>Vorbereiten der hardware

In diesem Lernprogramm wird angenommen, dass mithilfe von [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) Geräte eine Platine von Intel.

### <a name="set-up-the-edison-board"></a>Richten Sie die Platine

Bevor Sie beginnen, sollten Sie sicherstellen, dass Ihr Gerät Edison zu Ihrem drahtlosen Netzwerk herstellen können. Um Ihr Gerät Edison einzurichten, müssen Sie für die Verbindung zu einem Host-Computer. Intel bietet Einstieg Handbücher für die folgenden Betriebssysteme:

- [Erste Schritte mit der Intel-Platine Entwicklung unter Windows 64-Bit-][lnk-setup-win64].
- [Erste Schritte mit Intel Edison Development Board auf Windows 32-Bit-][lnk-setup-win32].
- [Erste Schritte mit Intel Edison Development Board unter Mac OS X][lnk-setup-osx].
- [Erste Schritte mit Intel® Platine unter Linux][lnk-setup-linux].

Geräts Edison und damit vertraut, Sie sollten führen Sie die Schritte in diesen "Erste Schritte" Artikel außer dem letzten Schritt "IDE auswählen" aktuelle Lernprogramm nicht erforderlich ist. Am Ende des Installationsvorgangs Edison müssen Sie:

- Aktualisiert die Edison mit der neuesten Firmware.
- Eine serielle Verbindung auf dem Server die Edison hergestellt.
- Führen Sie das Skript **Configure_edison** ein Kennwort und Ihre Edison WiFi aktivieren.

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>SensorTag-Gerät von der Platine ermöglichen Sie Konnektivität

Vor dem Ausführen des Beispiels müssen Sie überprüfen, ob die Platine SensorTag Gerät herstellen kann.

Zunächst müssen Sie überprüfen, ob Ihre Edison SensorTag Gerät herstellen kann.

1. Bluetooth-Edison aufheben und überprüfen Sie, ob die Versionsnummer **5,37**.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. Führen Sie den Befehl **Bluetoothctl** . Sie können nun in einer interaktiven Bluetooth-Shell. 

3. Geben Sie Befehl **Einschalten** schalten Sie die Bluetooth-Controller. Ähnliche Ausgabe sollte angezeigt werden:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Geben Sie in interaktiven Bluetooth-Shell den Befehl **scan** nach Bluetooth-Geräten suchen. Ähnliche Ausgabe sollte angezeigt werden:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Stellen Sie das Gerät SensorTag sichtbar, kleine Taste (grüne LED blinkt). Edison sollten SensorTag Gerät erkannt:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    In diesem Beispiel sehen Sie, dass die MAC-Adresse des Geräts SensorTag **A0:E6:F8:B5:F6:00**ist.

6. **Überprüfen Sie** den Befehl Scan deaktivieren.
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. Schließen Sie an das SensorTag Gerät die MAC-Adresse eingeben **Verbinden \<MAC-Adresse >**. Beachten Sie, dass die folgende Beispielausgabe abgekürzt:
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    Hinweis: Sie können GATT Merkmale des Geräts erneut die **Liste der Attribute** Befehl auflisten.

8. Sie können nun den Befehl **Trennen** Gerät trennen und Beenden von Bluetooth-Shell mit dem Befehl **Beenden** :
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Sie nun können zum Ausführen des Beispiels BLE Gateway auf Ihrem Gerät Edison.

## <a name="run-the-ble-gateway-sample"></a>Führen Sie das Beispiel BLE Gateway

Zum Ausführen des Beispiels BLE auf Ihre Edison müssen Sie drei Aufgaben:

- Konfigurieren Sie zwei Beispiel-Geräte im IoT Hub.
- Erstellen Sie IoT Gateway SDK auf Ihrem Gerät Edison.
- Konfigurieren Sie und führen Sie des Beispiels BLE Geräts Edison aus.

Gleichzeitig schreiben unterstützt IoT Gateway SDK nur Gateways, die BLE Module auf Linux verwenden.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Konfigurieren Sie zwei Beispiel-Geräte in IoT Hub

- [Erstellen einen IoT Hub] [ lnk-create-hub] in der Azure-Abonnements benötigen Sie den Namen des Haupt-diese exemplarische Vorgehensweise. Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.
- Ein Gerät IoT Hub **SensorTag_01** aufgefordert, und notieren Sie sich den Schlüssel-Id und das Gerät. Können [Geräte Explorer oder Iothub Explorer] [ lnk-explorer-tools] Tools dieses Gerät IoT Hub hinzufügen im vorherigen Schritt erstellt und den Schlüssel abgerufen werden. Sie ordnen das Gerät SensorTag Gerät das Gateway konfigurieren.

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>Erstellen Sie IoT Gateway SDK auf Ihrem Gerät Edison

Die Version der **Git** auf der Edsion unterstützt Untermodule. Der vollständige Quellcode für das SDK IoT Gateway Edison herunterladen haben Sie zwei Optionen:

- Option #1: Klonen [Azure IoT Gateway SDK] [ lnk-sdk] Repository auf Ihrem Edison und das Repository für jedes Untermodul manuell kopieren.
- Option #2: Klonen [Azure IoT Gateway SDK] [ lnk-sdk] Repository auf einem, **Git** Untermodule unterstützt, und kopieren Sie das vollständige Untermodule auf die Edison-Repository.

Bei Auswahl der Option #2 Befehlen Sie folgenden **Git** IoT Gateway SDK und die Untermodule Klonen:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

Sie sollten dann das gesamte lokale Repository in einer einzigen Archivdatei zip-vor dem Kopieren der Edison. Sie können ein Dienstprogramm wie **Pscp** enthaltenen **kitten** Archivdatei Edison kopieren. Zum Beispiel:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Wenn Sie eine vollständige Kopie des Repository IoT Gateway SDK auf Ihrem Edison, bauen Sie es mit dem folgenden Befehl aus dem Ordner, der das SDK enthält:

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>Konfigurieren Sie und führen Sie des Beispiels BLE Geräts Edison aus

Starten und Ausführen des Beispiels müssen Sie jedes nimmt im Gateway. Diese Konfiguration bietet eine JSON und müssen alle fünf teilnehmenden Module konfiguriert. Es gibt eine JSON-Beispieldatei im **gateway_sample.json** , die Sie als Ausgangspunkt verwenden können, eine eigene Konfigurationsdatei erstellen-Repository bereitgestellt. Diese Datei wird im Ordner " **Samples/Ble_gateway_hl/Src** " lokale Kopie des IoT Gateway SDK.

Die folgenden Abschnitte beschreiben zu bearbeiten diese Konfigurationsdatei BLE Beispiel davon aus, dass das Repository IoT Gateway SDK in die **/home/root/azure-iot-gateway-sdk /** auf Ihrem Gerät Edison. Wenn das Repository an anderer Stelle, sollten Sie die Pfade entsprechend anpassen:

#### <a name="logger-configuration"></a>Protokollierung-Konfiguration

Vorausgesetzt, das Gateway-Repository befindet sich im Ordner **/home/root/azure-iot-gateway-sdk /**, Protokollierung Modul wie folgt konfigurieren:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>BLE Modulkonfiguration

Beispielkonfiguration für BLE Gerät übernimmt ein Texas Instruments SensorTag. Alle BLE Standardgerät, die eine GATT Peripheriegeräte funktionieren sollte ausgeführt werden kann, aber Sie müssen die charakteristische GATT-IDs und Daten (für Write-Anweisungen) aktualisieren. Die MAC-Adresse des Geräts SensorTag hinzufügen: 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>IoT Hub-Modul

Den Namen des Haupt-IoT hinzufügen. Suffixwert ist normalerweise **Azure-devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Identität Zuordnungskonfiguration Modul

Fügen Sie die MAC-Adresse des Geräts SensorTag und Geräte-Id und Schlüssel des **SensorTag_01** Geräts IoT Hub hinzugefügt:

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>BLE Modul Druckerkonfiguration

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>Routing-Konfiguration

Die folgende Konfiguration wird Folgendes sichergestellt:
- **Protokollierung** -Modul empfängt und alle Nachrichten protokollieren.
- **SensorTag** -Modul sendet Nachrichten an die **Zuordnung** und **BLE Drucker** Module.
- **Mapping** -Modul sendet Nachrichten an das Modul **IoTHub** an IoT Hub gesendet werden.
- **IoTHub** -Modul sendet Nachrichten an das Modul **zuordnen** .
- Das **Mapping** -Modul sendet Nachrichten an das **SensorTag** -Modul.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

Um das Beispiel auszuführen ausführen und den Pfad zur Konfigurationsdatei JSON binäre **Ble_gateway_hl** . Wenn Sie die Datei **gateway_sample.json** verwendet, sieht der auszuführende Befehl:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Sie müssen kleine Taste auf SensorTag Gerät, es sichtbar, bevor Sie das Beispiel ausführen.

Wenn Sie das Beispiel ausführen, können Sie [Geräte Explorer oder Iothub Explorer] [ lnk-explorer-tools] Tool Nachrichten überwacht das Gateway SensorTag Gerät weiterleitet.

## <a name="send-cloud-to-device-messages"></a>Cloud-Gerät senden

BLE Modul unterstützt auch Anleitung Azure IoT Hub an das Gerät senden. Sie können [Azure IoT Hub Gerät Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) oder [IoT Hub Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) JSON-Nachrichten senden, die das Gateway-Modul BLE BLE Gerät gibt. Beispielsweise bei Verwendung von Texas Instruments SensorTag Gerät können Sie die folgenden JSON-Nachrichten an das Gerät IoT Hub senden.

- Alle LEDs und Summer zurücksetzen (deaktivieren)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- I/o "Remote" Konfigurieren

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Die LED einschalten

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Die grüne LED einschalten

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- Summer aktivieren

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

Das Standardverhalten für ein Gerät, über HTTP Verbindung IoT Hub ist 25 Minuten für einen neuen Befehl zu überprüfen. Wenn Sie mehrere separate Befehle senden müssen Sie daher 25 Minuten für das Gerät auf jeden Befehl.

> [AZURE.NOTE] Das Gateway überprüft auch neue Befehle bei jedem Start so zu einem Befehl durch Beenden und starten das Gateway zu erzwingen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie IoT Gateway SDK komplexere Verständnis und probieren einige Codebeispiele, besuchen Sie die folgenden Developer Tutorials und Ressourcen:

- [Azure IoT Gateway SDK][lnk-sdk]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
