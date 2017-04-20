<properties
 pageTitle="Direkte Methoden | Microsoft Azure"
 description="In diesem Lernprogramm wird gezeigt, wie direkte Methoden"
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
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Praktische Einführung: Direkte Methoden

## <a name="introduction"></a>Einführung

Azure IoT Hub einen komplett verwalteten Service, zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und eine Anwendung beenden. Vorherigen Lernprogramme ([IoT Hub Einstieg] und [Cloud-zu-Gerät-Nachrichten mit IoT Hub]) veranschaulichen die grundlegende Gerät Cloud und Cloud Gerät Messagingfunktionen IoT Hub. IoT Hub können Sie auch ohne permanente Methoden auf Geräten aus der Cloud. Methoden stellen eine Anforderung-Antwort-Interaktion mit einem Gerät wie einen HTTP-Aufruf, sie gelingen oder (sobald Benutzer angegebenen Timeout) Fehlschlagen der Benutzer den Status des Anrufs. [Eine direkte Methode auf einem Gerät] [ lnk-devguide-methods] beschreibt Methoden ausführlicher und bietet Hinweise zu Methoden und Cloud-zu-Gerät-Nachrichten verwenden.

Dieses Lernprogramm zeigt Ihnen, wie an:

- Verwenden Sie Azure-Portal IoT Hub und Erstellen einer geräteidentität im IoT Hub.
- Erstellen eines simulierten Geräts eine direkte Methode Cloud genannt.
- Erstellen Sie eine, die auf dem simulierten Gerät über IoT Hub eine direkte Methode aufruft.

> [AZURE.NOTE] Zu diesem Zeitpunkt sind direkte Methoden nur Verbindung IoT Hub mit dem Protokoll MQTT Geräte zugegriffen werden. Siehe [MQTT unterstützen] [ lnk-devguide-mqtt] Artikel Hinweise vorhandenen Gerät app konvertiert MQTT verwenden.

Am Ende dieses Lernprogramms haben Sie gegenseitige Node.js-Konsole:

* **CallMethodOnDevice.js**, ruft eine Methode auf dem simulierten Gerät und die Antwort angezeigt.
* **SimulatedDevice.js**, Verbindung IoT Hub mit der zuvor erstellten Geräte und die Cloud aufgerufene Methode entspricht.

> [AZURE.NOTE] Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen zu den verschiedenen SDKs, mit denen Sie sowohl Anwendung auf Geräte und Ihre Lösung Back-End.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Node.js-Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Gerät

In diesem Abschnitt erstellen Sie eine Node.js-Konsole app, die eine Methode namens Cloud entspricht.

1. Erstellen Sie einen neuen Ordner namens **Simulateddevice**. Erstellen Sie im Ordner **Simulateddevice** eine package.json mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Simulateddevice** führen Sie den folgenden Befehl **Azure Iot Gerät** Gerät SDK-Paket und **Azure Iot-Gerät Mqtt** Paket installieren:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Mit einem Texteditor eine neue Datei **SimulatedDevice.js** im Ordner " **Simulateddevice** " erstellen.

4. Fügen Sie den folgenden `require` -Anweisung am Anfang der Datei **SimulatedDevice.js** :

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Hinzufügen einer Variable **ConnectionString** und Gerät Client erstellen. Ersetzen Sie **{Gerät Verbindungszeichenfolge}** mit der Verbindungszeichenfolge im Abschnitt *Erstellen einer geräteidentität* generiert:

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Fügen Sie die folgende Funktion zum Implementieren der Methode auf dem Gerät:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Öffnen die Verbindung IoT Hub und Ihre initialisieren Methode Listener:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Speichern Sie und schließen Sie die Datei **SimulatedDevice.js** .

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine wiederholen Richtlinien in diesem Lernprogramm. In Produktionscode sollten Sie wiederholungsrichtlinien (z. B. Verbindungsversuch) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]implementieren[lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Aufrufen einer Methode für ein Gerät

In diesem Abschnitt erstellen Sie eine Node.js-Konsole app, die Ruft eine Methode auf dem simulierten Gerät und die Antwort angezeigt.

1. Erstellen Sie einen neuen Ordner namens **Callmethodondevice**. Erstellen Sie im Ordner **Callmethodondevice** eine package.json mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Callmethodondevice** führen Sie den folgenden Befehl **Azure Iothub** installiert:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Mit einem Texteditor eine Datei **CallMethodOnDevice.js** im Ordner " **Callmethodondevice** " erstellen.

4. Fügen Sie den folgenden `require` -Anweisung am Anfang der Datei **CallMethodOnDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Fügen Sie die folgende Variablendeklaration hinzu, und Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge IoT Hub:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. Erstellen des Clients die Verbindung IoT Hub zu öffnen.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Fügen Sie die folgende Funktion zum Gerät-Methode aufgerufen und das Gerät auf der Konsole gedruckt:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Speichern Sie und schließen Sie die Datei **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun die Anwendung ausführen.

1. Eine Befehlszeile im Ordner **Simulateddevice** führen Sie den folgenden Befehl empfängt Methodenaufrufe von IoT Hub:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. Eine Befehlszeile im Ordner **Callmethodondevice** zum Überwachen des IoT Hubs den folgenden Befehl ausführen:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Das Gerät reagiert der Methode durch Drucken der Nachricht und die Anwendung, die Methode Anzeige der Antwort vom Gerät aufgerufen wird angezeigt:

    ![][9]
    
## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm einen neuen IoT Hub in Azure-Portal konfiguriert und eine geräteidentität in den Hub identitätsregistrierung erstellt. Mithilfe dieses geräteidentität der simulierten Gerät App Cloud aufgerufenen Methoden reagieren. Sie erstellt auch eine app, die ruft Methoden auf dem Gerät und zeigt die Antwort vom Gerät. 

Zum Einstieg IoT Hub und andere Szenarien IoT zu:

- [Erste Schritte mit IoT Hub]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Die IoT erweitern Lösung und Zeitplan auf mehreren Geräten Methodenaufrufe finden Sie unter [Zeitplan und broadcast Aufträge] [ lnk-tutorial-jobs] Tutorial.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Nachrichten Sie Cloud-zu-Gerät-mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Erste Schritte mit IoT Hub]: iot-hub-node-node-getstarted.md
