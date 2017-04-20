<properties
    pageTitle="Azure IoT Hub für Einsteiger Node.js | Microsoft Azure"
    description="Azure IoT Hub mit Node.js Schritte. Verwenden Sie Azure IoT Hub und Node.js mit Microsoft Azure IoT SDKs Internet der Dinge-Lösung implementieren."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Erste Schritte mit Azure IoT Hub für Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Lernprogramms haben Sie Node.js Konsole Applikationen:

* **CreateDeviceIdentity.js**, ein geräteidentität zugeordneten Sicherheitsschlüssel simulierte Gerät verbunden werden.
* **ReadDeviceToCloudMessages.js**, dem simulierten Gerät per Telemetrie angezeigt.
* **SimulatedDevice.js**, die Verbindung IoT Hub mit der zuvor erstellten Gerät und sendet eine Nachricht Telemetrie jedes zweite mit AMQP-Protokoll.

> [AZURE.NOTE] Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen zu den verschiedenen SDKs, mit denen Sie sowohl Anwendung auf Geräte und Ihre Lösung Back-End.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Node.js-Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sie haben jetzt Ihre IoT Hub. Sie haben den Hostnamen IoT Hub und IoT Hub-Verbindungszeichenfolge, die Sie den Rest dieses Lernprogramms abgeschlossen.

## <a name="create-a-device-identity"></a>Erstellen Sie eine geräteidentität

In diesem Abschnitt erstellen Sie eine Node.js-Konsole app, die in der identitätsregistrierung IoT Hub geräteidentität erstellt. Ein Gerät Verbindung keine IoT Hub, wenn einen Eintrag in die Registrierung des Geräts Identität verfügt. Siehe Abschnitt **Geräteidentitätsregistrierung** [IoT Hub-Entwicklerhandbuch] [ lnk-devguide-identity] Weitere Informationen. Beim Ausführen dieser Konsolenanwendung generiert er einen eindeutigen Geräte-ID und Schlüssel, mit dem das Gerät selbst ermitteln, wenn Gerät Cloud-Nachrichten IoT Hub sendet.

1. Erstellen Sie einen neuen Ordner namens **Createdeviceidentity**. Erstellen Sie im Ordner **Createdeviceidentity** eine package.json mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Createdeviceidentity** führen Sie den folgenden Befehl **Iothub Azure** Service SDK installiert:

    ```
    npm install azure-iothub --save
    ```

3. Mit einem Texteditor eine Datei **CreateDeviceIdentity.js** im Ordner " **Createdeviceidentity** " erstellen.

4. Fügen Sie den folgenden `require` -Anweisung am Anfang der Datei **CreateDeviceIdentity.js** :

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Fügen Sie folgenden Code in die Datei **CreateDeviceIdentity.js** , und Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für den IoT Hub im vorherigen Abschnitt erstellten: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Fügen Sie den folgenden Code, um eine Gerät Identität im IoT Hub erstellt. Dieser Code erstellt ein, wenn die Geräte-Id in der Registrierung nicht vorhanden ist, andernfalls wird des Schlüssels von dem vorhandenen Gerät:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Speichern Sie und schließen Sie die Datei **CreateDeviceIdentity.js** .

8. Um die **Createdeviceidentity** -Anwendung ausführen, führen Sie den folgenden Befehl an der Befehlszeile im Ordner "Createdeviceidentity":

    ```
    node CreateDeviceIdentity.js 
    ```

9. Notieren Sie sich die **Geräte-Id** und **Gerät**. Sie benötigen diese später beim Erstellen einer Anwendung Verbindung IoT Hub ein.

> [AZURE.NOTE] IoT Hub identitätsregistrierung speichert nur Identitäten Gerät an den Hub sicheren Zugriff zu ermöglichen. Es speichert Geräte-IDs und Schlüssel als Anmeldeinformationen und ein Flag aktiviert/deaktiviert, mit denen Sie Zugriff für ein einzelnes Gerät deaktivieren. Wenn die Anwendung andere gerätespezifische Metadaten speichern muss, sollte einen anwendungsspezifische Speicher verwendet werden. [IoT Hub Developer Guide] finden Sie unter[ lnk-devguide-identity] Weitere Informationen.

## <a name="receive-device-to-cloud-messages"></a>Gerät-zu-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Node.js Konsole app, die Gerät zu Cloud-Nachrichten IoT Hub liest. IoT Hub macht ein [Ereignis Hubs][lnk-event-hubs-overview]-kompatiblen Endpunkt Gerät Cloud-Nachrichten lesen können. In diesem Lernprogramm erstellt zur Vereinfachung, einen grundlegenden Leser, der nicht für einen hohen Durchsatz geeignet ist. [Gerät-zu-Cloud-Nachrichten verarbeiten] [ lnk-process-d2c-tutorial] Lernprogramm veranschaulicht das Gerät Cloud-Nachrichten Ebene verarbeiten. [Erste Schritte mit Event Hubs] [ lnk-eventhubs-tutorial] Tutorial enthält weitere Informationen zum Verarbeiten von Nachrichten von Ereignis und IoT Hub Ereignis Hub-kompatible Endpunkte für.

> [AZURE.NOTE] Ereignis Hub-kompatiblen Endpunkt zum Lesen von Gerät Cloud-Nachrichten immer verwendet das AMQP-Protokoll.

1. Erstellen Sie einen neuen Ordner namens **Readdevicetocloudmessages**. Erstellen Sie im Ordner **Readdevicetocloudmessages** eine package.json mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Readdevicetocloudmessages** führen Sie den folgenden Befehl **Azure Ereignis Hubs** installiert:

    ```
    npm install azure-event-hubs --save
    ```

3. Mit einem Texteditor eine Datei **ReadDeviceToCloudMessages.js** im Ordner " **Readdevicetocloudmessages** " erstellen.

4. Fügen Sie den folgenden `require` -Anweisung am Anfang der Datei **ReadDeviceToCloudMessages.js** :

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Fügen Sie die folgende Variablendeklaration hinzu, und Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge IoT Hub:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Fügen Sie die folgenden zwei Funktionen, die Ausgabe in der Konsole:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Fügen Sie folgenden Code zum **EventHubClient**erstellen, öffnen Sie die Verbindung IoT Hub und Empfänger für jede Partition erstellen. Diese Anwendung verwendet einen Filter, wenn Empfänger erstellt, damit der Empfänger liest nur Nachrichten IoT Hub nach Empfänger gestartet. Dieser Filter eignet sich in einer Umgebung für die aktuelle Gruppe von Nachrichten sehen. In einer produktiven Umgebung Code sollten sicherstellen, dass alle Nachrichten - Prozesse [wie IoT Hub Gerät Cloud-Nachrichten] finden Sie unter[ lnk-process-d2c-tutorial] mehr zum Tutorial:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Speichern Sie und schließen Sie die Datei **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Gerät

In diesem Abschnitt erstellen Sie eine Node.js-Konsole app, die ein Gerät simuliert, das Gerät zu Cloud an IoT Hub sendet.

1. Erstellen Sie einen neuen Ordner namens **Simulateddevice**. Erstellen Sie im Ordner **Simulateddevice** eine package.json mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Simulateddevice** führen Sie den folgenden Befehl **Azure Iot Gerät** Gerät SDK-Paket und **Azure Iot-Gerät Amqp** Paket installieren:

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Mit einem Texteditor eine neue Datei **SimulatedDevice.js** im Ordner " **Simulateddevice** " erstellen.

4. Fügen Sie den folgenden `require` -Anweisung am Anfang der Datei **SimulatedDevice.js** :

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Hinzufügen einer Variable **ConnectionString** und Gerät Client erstellen. Ersetzen Sie **{Youriothostname}** mit dem Namen IoT Hub im Abschnitt *IoT Hub* erstellt. Ersetzen Sie **{Yourdevicekey}** mit dem Gerät Schlüsselwert im Abschnitt *Erstellen einer geräteidentität* generiert:

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Fügen Sie die folgende Funktion zum Anzeigen von Ausgaben der Anwendung:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Erstellen Sie einen Rückruf und Funktion **SetInterval** eine neue Nachricht IoT Hub pro Sekunde an:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. Öffnen Sie die Verbindung IoT Hub und senden Sie Nachrichten:

    ```
    client.open(connectCallback);
    ```

9. Speichern Sie und schließen Sie die Datei **SimulatedDevice.js** .

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine wiederholen Richtlinien in diesem Lernprogramm. In Produktionscode sollten Sie wiederholungsrichtlinien (z. B. eine exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]implementieren[lnk-transient-faults].


## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun die Anwendung ausführen.

1. Eine Befehlszeile im Ordner **Readdevicetocloudmessages** zum Überwachen des IoT Hubs den folgenden Befehl ausführen:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Node.js IoT Hub-Clientanwendung Gerät Cloud-Nachrichten überwachen][7]

2. Eine Befehlszeile im Ordner **Simulateddevice** führen Sie den folgenden Befehl Senden von Daten an IoT Hub beginnen:

    ```
    node SimulatedDevice.js
    ```

    ![Node.js IoT Hub Gerät Clientanwendung Gerät Cloud-Nachrichten][8]

3. Die **Verwendung** Kachel in [Azure-Portal] [ lnk-portal] zeigt die Anzahl der Nachrichten an den Hub gesendet:

    ![Azure Portal Verwendung nebeneinander anzeigen der Anzahl der Nachrichten an IoT Hub][43]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm einen neuen IoT Hub in Azure-Portal konfiguriert und eine geräteidentität in den Hub identitätsregistrierung erstellt. Mithilfe dieses geräteidentität der simulierten Gerät App Gerät Cloud-Nachrichten an den Hub senden. Sie erstellt auch eine Anwendung, die Nachrichten vom Hub angezeigt. 

Zum Einstieg IoT Hub und andere Szenarien IoT zu:

- [Verbinden Ihr Gerät][lnk-connect-device]
- [Erste Schritte mit Device-management][lnk-device-management]
- [Erste Schritte mit IoT Gateway SDK][lnk-gateway-SDK]

Die Projektmappe IoT Erweiterung Gerät Cloud-Nachrichten Ebene verarbeiten finden Sie unter [Device Cloud-Nachrichten verarbeiten] [ lnk-process-d2c-tutorial] Tutorial.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
