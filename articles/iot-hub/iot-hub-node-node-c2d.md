<properties
    pageTitle="Cloud-zu-Gerät-Nachrichten mit IoT Hub | Microsoft Azure"
    description="Dieses Lernprogramm lernen Java Azure IoT Hub mit Cloud-zu-Gerät-Nachrichten senden."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Exemplarische Vorgehensweise: Cloud Gerät IoT Hub und Node.js Nachrichten

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von Geräten IoT aktivieren und eine Anwendung zurück. Das Lernprogramm [Erste Schritte mit IoT Hub] veranschaulicht IoT Hub erstellen und Bereitstellen einer geräteidentität code eines simulierten Geräts, das Gerät zu Cloud-Nachrichten sendet.

Dieses Lernprogramm baut auf [Erste Schritte mit IoT Hub]. Es veranschaulicht an:

- Nachrichten Sie von der Anwendung Cloud Back-End Cloud-Gerät an einem einzigen Gerät IoT Hub.
- Nachrichten Sie Cloud-zu-Gerät-auf einem Gerät.
- Ihre Anwendung Cloud back-End, Nachrichten an ein Gerät von IoT Hub Übermittlung Bestätigung (*Feedback*) anfordern.

Finden Sie mehr Cloud-zu-Gerät-Nachrichten [IoT Hub-Entwicklerhandbuch][IoT Hub Developer Guide - C2D].

Am Ende dieser praktischen Einführung führen Sie gegenseitige Node.js-Konsole:

* **SimulatedDevice**, eine geänderte Version der app in [Schritten mit IoT Hub], Verbindung IoT Hub und empfängt Nachrichten, Cloud-Gerät erstellt.
* **SendCloudToDeviceMessage**, die Cloud-zu-Gerät-Nachricht mit dem simulierten Gerät IoT Hub sendet und erhält die Lieferung Bestätigung.

> [AZURE.NOTE] IoT Hub unterstützt SDK für zahlreiche Plattformen und Sprachen (z. B. C, Java und Javascript) durch Azure IoT Geräte-SDKs. Eine ausführliche Anleitung zum Verbinden Sie das Gerät in diesem Lernprogramm Code und sehen im Allgemeinen Azure IoT Hub [Azure IoT Developer Center].

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Node.js-Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Empfangen von Nachrichten auf dem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierten geräteanwendung in Cloud Gerät IoT Hub Nachrichten [beginnen mit IoT Hub] erstellt.

1. Mit einem Texteditor die Datei SimulatedDevice.js.

2. Ändern Sie die **ConnectCallback** -Funktion, um Nachrichten von IoT Hub gesendet. In diesem Beispiel ruft das Gerät immer die **vollständige** Funktion IoT Hub darauf, dass die Meldung verarbeitet wurde. Die neue Version der **ConnectCallback** -Funktion sieht folgendermaßen aus:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
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

    > [AZURE.NOTE] Wenn Sie HTTP statt MQTT oder AMQP als Transportprotokoll verwenden, überprüft die Instanz **DeviceClient** Nachrichten selten IoT Hub (weniger als 25 Minuten). Finden Sie weitere Informationen zu den Unterschieden zwischen MQTT, AMQP und HTTP-Unterstützung und IoT Hub Drosselung [IoT Hub-Entwicklerhandbuch][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Cloud-Gerät senden

In diesem Abschnitt erstellen Sie eine Node.js-Konsole app, die Cloud-Gerät an die app simulierten Gerät sendet. Sie benötigen die Geräte-Id des Geräts, das im Lernprogramm [Erste Schritte mit IoT Hub] hinzugefügt. Sie benötigen die Verbindungszeichenfolge für Ihre IoT Hub, den in der [Azure-Portal]finden.

1. Erstellen Sie einen leeren Ordner namens **Sendcloudtodevicemessage**. Erstellen Sie im Ordner **Sendcloudtodevicemessage** eine package.json mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Sendcloudtodevicemessage** führen Sie den folgenden Befehl **Azure Iothub** installiert:

    ```
    npm install azure-iothub --save
    ```

3. Mit einem Texteditor eine neue Datei **SendCloudToDeviceMessage.js** im Ordner " **Sendcloudtodevicemessage** " erstellen.

4. Fügen Sie den folgenden `require` -Anweisung am Anfang der Datei **SendCloudToDeviceMessage.js** :

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Fügen Sie folgenden Code zur **SendCloudToDeviceMessage.js** -Datei. Ersetzen Sie den Wert der Verbindungszeichenfolge Platzhalter mit der Verbindungszeichenfolge für den IoT Hub im Lernprogramm [Erste Schritte mit IoT Hub] erstellte. Die Geräte-Id des Geräts im Lernprogramm [Erste Schritte mit IoT Hub] hinzugefügten ersetzen Sie Ziel Gerät Platzhalter:

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Fügen Sie die folgende Funktion zum Drucken der Ergebnisse in der Konsole:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Fügen Sie die folgende Funktion um Übermittlung Feedback Nachrichten auf der Konsole gedruckt:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Fügen Sie folgenden Code zum Senden einer Nachricht an Ihr Gerät und die Feedback-Meldung zu behandeln, wenn das Gerät die Cloud-zu-Gerät-Nachricht bestätigt:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Speichern Sie und schließen Sie die Datei **SendCloudToDeviceMessage.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun die Anwendung ausführen.

1. Führen Sie in der Befehlszeile in den Ordner **Simulateddevice** den folgenden Befehl Telemetrie IoT Hub senden und Cloud-zu-Gerät-Nachrichten:

    ```
    node SimulatedDevice.js 
    ```

    ![Führen Sie die Anwendung simulierten Gerät][img-simulated-device]

2. Eine Befehlszeile im Ordner **Sendcloudtodevicemessage** führen Sie den folgenden Befehl Cloud Gerät senden und Warten auf Bestätigung Feedback:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Führen Sie die Anwendung auf den c2d-Befehl senden][img-send-command]

    > [AZURE.NOTE] Der Einfachheit halber implementiert dieses Lernprogramm keine Richtlinien wiederholen. Im Produktionscode sollten Sie wiederholen (z.B. exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]Maßnahmen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie Cloud-zu-Gerät-Nachrichten senden und empfangen. 

Beispiele für umfassende End-to-End-Lösung, IoT Hub verwenden, finden Sie unter [Azure IoT Suite].

Weitere Informationen zur Entwicklung mit IoT Hub finden Sie unter [IoT Hub-Entwicklerhandbuch].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Erste Schritte mit IoT Hub]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Entwicklerhandbuch IoT Hub]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Vorübergehender Fehler behandeln]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/