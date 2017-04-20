<properties
   pageTitle="Gerät mit C auf Mbed | Microsoft Azure"
   description="Beschreibt, wie ein Gerät der Azure IoT Suite vorkonfiguriert remote monitoring-Lösung mit einer Anwendung auf einem Gerät Mbed c geschrieben."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Verbinden Sie das Gerät remote monitoring vorkonfigurierte Lösung (Mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Erstellen und Ausführen der Beispielprojektmappe C

Die folgende Anleitung beschreibt die Schritte zum Herstellen einer Verbindung eine [Mbed-aktiviert Freescale FRDM-K64F] [ lnk-mbed-home] Gerät remote monitoring-Lösung.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Verbinden Sie Mbed mit dem Netzwerk und Desktop-Computer

1. Verbinden Sie Mbed-Gerät über ein Ethernet-Kabel mit Ihrem Netzwerk. Dieser Schritt ist erforderlich, da die beispielanwendung Internetzugriff benötigt.

2. Finden Sie unter [Erste Schritte mit Mbed] [ lnk-mbed-getstarted] Geräts Mbed Computer herstellen.

3. Wenn Ihr Computer Windows ausgeführt wird, finden Sie unter [Computerkonfiguration] [ lnk-mbed-pcconnect] Geräts Mbed Zugriff auf serielle Ports konfigurieren.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Ein Mbed-Projekt erstellen und Importieren des Beispielcodes

1. Gehen Sie in Ihrem Webbrowser mbed [Developer-Website](https://developer.mbed.org/). Wenn Sie sich angemeldet haben, sehen Sie eine Option zum Erstellen eines Kontos (es ist kostenlos). Andernfalls melden Sie sich mit Ihren Anmeldeinformationen. Klicken Sie rechts oben auf der Seite **Compiler** . Diese Aktion ruft die *Workspace* -Benutzeroberfläche.

2. Sicherstellen Sie, dass die Hardwareplattform verwendeten oben rechts im Fenster angezeigt wird, oder klicken Sie rechts auf Hardware-Plattform.

3. Klicken Sie im Hauptmenü auf **Importieren** . Klicken Sie dann auf Importieren von URL-Verknüpfung neben dem Logo Mbed Welt **hier** .

    ![][6]

4. Das Popup-Fenster geben Sie den Link für die https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ Probe Code klicken Sie **Importieren**.

    ![][7]

5. Im Mbed-Compiler-Fenster finden Sie, dass verschiedene Bibliotheken Importieren dieses Projekts auch importiert werden. Einige bereitgestellt und verwaltet von Azure IoT-Team ([Azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [Iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [Iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [Azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), während andere Drittanbieter-Bibliotheken im Katalog Bibliotheken Mbed.

    ![][8]

6. Öffnen Sie die Datei remote_monitoring\remote_monitoring.c, und suchen Sie den folgenden Code in der Datei:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Replace [Geräte-Id] und [Geräteschlüssel] mit den Gerätedaten Beispielprogramm Verbindung IoT Hub aktivieren. Verwenden der IoT Hub Hostname [IoTHub Name] ersetzen und Platzhalter [IoTHub Suffix, d. h. Azure devices.net]. Z. B. ist Hostname Ihrer IoT Hub **contoso.azure devices.net**, **Contoso** **HubName** und alles **HubSuffix**ist:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Der Code

Wenn Sie wie die Anwendung funktioniert, beschreibt dieser Abschnitt Hauptbestandteile des Beispielcodes. Wenn Sie den Code ausführen möchten, fahren Sie mit [das Programm erstellen und Ausführen](#buildandrun).

#### <a name="defining-the-model"></a>Definieren des Modells

In diesem Beispiel wird das [Serialisierungsprogramm] [ lnk-serializer] Bibliothek ein Modell definieren, das Gerät IoT Hub senden und Empfangen von IoT Hub kann Nachrichten angibt. In diesem Beispiel definiert den **Contoso** -Namespace ein **Thermostat** -Modell, das **ExternalTemperature**, **Temperatur**und **Feuchtigkeit** Telemetriedaten mit Metadaten wie die Geräte-Id, Eigenschaften und Befehle, die auf das Gerät gibt:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Um die Modelldefinition gehören die Definitionen für die Befehle **SetTemperature** und **SetHumidity** , die auf das Gerät:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Verbinden das Modell zur Bibliothek

Die Funktionen **SendMessage** und **IoTHubMessage** sind Standardcode für Telemetrie vom Gerät senden und Nachrichten an den Befehlshandler IoT Hub verbinden.

#### <a name="the-remotemonitoringrun-function"></a>Die Remote_monitoring_run-Funktion

**Die Hauptfunktion des Programms** Ruft die Funktion **Remote_monitoring_run** beim Anwendungsstart das Gerät Verhalten als Client IoT Hub-Gerät ausführen. Diese Funktion **Remote_monitoring_run** besteht hauptsächlich aus verschachtelten Funktionen:

- **Plattform\_Init** und **Plattform\_Deinit** führen plattformspezifische Initialisierung und Herunterfahren.
- **Serialisierungsprogramm\_Init** und **Serialisierungsprogramm\_Deinit** initialisieren und Deduplizierung initialisieren Bibliothek des Serialisierungsprogramms.
- **IoTHubClient\_erstellen** und **IoTHubClient\_Destroy** ein Clienthandle **IotHubClientHandle**, mit den geräteanmeldeinformationen für die Verbindung IoT Hub zu erstellen.

Der Hauptteil der Funktion **Remote_monitoring_run** führt das Programm die folgenden Operationen mit Hilfe des Griffs **IotHubClientHandle** :

- Erstellt eine Instanz des Modells Thermostat Contoso und richtet die Nachrichten-Callbacks zwei Befehle.
- Sendet Informationen, einschließlich der Befehle, die Bibliothek des Serialisierungsprogramms mit IoT Hub unterstützt. Wenn Hub diese Nachricht empfängt, wird den Gerätestatus im Dashboard von **Ausstehend** **ausgeführt**.
- Startet eine **while** -Schleife, die Temperatur, Temperatur und feuchtigkeitswerte IoT Hub pro Sekunde sendet.

Zu Referenzzwecken hier ist ein Beispiel **DeviceInfo** Nachricht an IoT Hub beim Start:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Zu Referenzzwecken hier ist ein Beispiel **Telemetrie** Nachricht IoT Hub:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Zu Referenzzwecken ist hier ein Beispiel von IoT Hub erhaltenen **Befehl** :

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Das Programm erstellen und ausführen

1. Klicken Sie auf **Erstellen** , um die Anwendung zu erstellen. Sie können sicher Warnungen ignorieren, wenn der Build Fehler generiert vor dem Fortfahren beheben.

2. Wenn der Build erfolgreich ist, Mbed-Compiler Website generiert eine bin-Datei mit dem Namen des Projekts und auf Ihren Computer heruntergeladen. Kopieren Sie die bin-Datei auf das Gerät. Speichern die bin-Datei auf das Gerät wird das Gerät neu starten und das Programm in die bin-Datei ausführen. Sie können das Programm jederzeit starten durch Drücken der Reset-Taste auf dem Gerät Mbed.

3. SSH-Client, z. B. über kitten Gerät verbinden. Sie bestimmen den seriellen Anschluss das Gerät verwendet Windows Geräte-Manager überprüfen.

    ![][11]

4. Klicken Sie in kitten auf die **serielle** Verbindung. In der Regel Herstellen einer Verbindung mit 9600 Baud, so geben Sie 9600 im Feld **Geschwindigkeit** . Klicken Sie auf **Öffnen**.

5. Das Programm startet die Ausführung. Möglicherweise das Motherboard zurücksetzen (drücken Sie STRG + UNTBR oder Platinen Reset-Taste), wenn die Anwendung die Verbindung nicht automatisch gestartet wird.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
