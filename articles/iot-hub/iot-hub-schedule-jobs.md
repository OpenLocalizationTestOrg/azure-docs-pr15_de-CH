<properties
 pageTitle="Einzelvorgänge planen | Microsoft Azure"
 description="In diesem Lernprogramm wird gezeigt, wie Aufträge planen"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Lernprogramm: Planen und broadcast-Aufträge (Vorschau)

## <a name="introduction"></a>Einführung
Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine Anwendung Back-End erstellen und Nachverfolgen von Aufträgen, die planen und Millionen von Geräten ermöglicht.  Aufträge können für Folgendes verwendet werden:

- Zwei gewünschten Eigenschaften aktualisieren
- Gerät zwei Tags aktualisieren
- Cloud-zu-Gerät-Methoden aufrufen

Im Prinzip ein Auftrag schließt eine der folgenden Aktionen und zeigt den Verlauf der Ausführung für verschiedene Geräte, eine zwei-Abfrage definiert.  Beispielsweise kann mithilfe eines Auftrags eine Back-End der Anwendung ein Neustart auf 10.000 Geräten durch eine Abfrage zwei und zu einem späteren Zeitpunkt geplant Methodenaufruf.  Die Anwendung kann dann verfolgen, und diese Geräte erhalten die Neustart-Methode ausführen.

Erfahren Sie mehr über diese Funktionen in diesen Artikeln:

- Zwei Geräte und Eigenschaften: [Erste Schritte mit] [ lnk-get-started-twin] und [Tutorial: Verwendung und Eigenschaften][lnk-twin-props]
- Cloud-zu-Gerät-Methoden: [Developer Guide - direkten Methoden] [ lnk-dev-methods] und [Tutorial: C2D Methoden][lnk-c2d-methods]

Dieses Lernprogramm zeigt Ihnen, wie an:

- Erstellen eines simulierten Geräts eine direkte Methode LockDoor ermöglicht die Anwendung wieder End aufgerufen werden können.
- Erstellen Sie eine, die Methodenaufrufe der LockDoor direkt auf dem simulierten Gerät mithilfe eines Auftrags aktualisiert und gewünschten Eigenschaften mithilfe einer Gerät.

Am Ende dieses Lernprogramms haben Sie gegenseitige Node.js-Konsole:

**simDevice.js**, mit der IoT Hub mit dem Gerät verbunden und LockDoor direkte Methode empfängt.

**scheduleJobService.js**, direkter Methodenaufrufe auf dem simulierten Gerät und die zwei Nachlauf Eigenschaften mithilfe eines Auftrags aktualisieren.

Um dieses Lernprogramm benötigen Sie Folgendes:

Node.js-Version 0.12.x oder höher <br/>  [Vorbereiten der Umgebung] [ lnk-dev-setup] Node.js für dieses Lernprogramm unter Windows oder Linux Installation beschrieben.

Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Gerät

In diesem Abschnitt Node.js Konsole app, die eine direkte Methode namens Cloud simulierten Gerät einen Neustart auslöst entspricht erstellen und verwendet zwei Gerät gemeldet Eigenschaften Gerät zwei Abfragen Geräte und beim letzten Neustart aktivieren.

1. Erstellen Sie einen neuen Ordner namens **SimDevice**.  Erstellen Sie im Ordner **SimDevice** eine package.json mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```
    
2. Die Befehlszeile im Ordner **SimDevice** führen Sie den folgenden Befehl **Azure Iot Gerät** Gerät SDK-Paket und **Azure Iot-Gerät Mqtt** Paket installieren:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Mit einem Texteditor eine neue Datei **simDevice.js** im Ordner " **SimDevice** " erstellen.

4. Fügen Sie die folgenden "-Anweisung am Anfang der Datei **simDevice.js** erforderlich":

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Hinzufügen einer Variable **ConnectionString** und Gerät Client erstellen.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Fügen Sie die folgende Funktion LockDoor Methode behandeln.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Registrieren Sie den Handler für die LockDoor-Methode den folgenden Code hinzufügen.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Speichern Sie und schließen Sie die Datei **simDevice.js** .

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine wiederholen Richtlinien in diesem Lernprogramm. In Produktionscode sollten Sie wiederholungsrichtlinien (z. B. eine exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]implementieren[lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Planen von Aufträgen für direkten aufrufen und eine zwei Eigenschaften aktualisieren

In diesem Abschnitt erstellen eine Node.js-Konsole, die ein remote-LockDoor auf einem Gerät mit einer direkten Methode initiiert und die zwei Eigenschaften aktualisieren.

1. Erstellen Sie einen neuen Ordner namens **ScheduleJobService**.  Erstellen Sie im Ordner **ScheduleJobService** eine package.json mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```
    
2. Die Befehlszeile im Ordner **ScheduleJobService** führen Sie den folgenden Befehl **Azure Iothub** Gerät SDK-Paket und **Azure Iot-Gerät Mqtt** Paket installieren:

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. Mit einem Texteditor eine neue Datei **scheduleJobService.js** im Ordner " **ScheduleJobService** " erstellen.

4. Fügen Sie die folgenden "-Anweisung am Anfang der Datei **dmpatterns_gscheduleJobServiceetstarted_service.js** erforderlich":

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Fügen Sie die folgenden Variablendeklarationen, und Ersetzen Sie die Platzhalterwerte:

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Fügen Sie die folgende Funktion, mit der die Ausführung des Einzelvorgangs überwachen:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Fügen Sie den folgenden Code, um den Auftrag zu planen, der Gerät-Methode aufruft:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Fügen Sie den folgenden Code dem Auftrag Twin aktualisieren:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Speichern Sie und schließen Sie die Datei **scheduleJobService.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun die Anwendung ausführen.

1. Führen Sie in der Befehlszeile in den Ordner **SimDevice** den folgenden Befehl warten auf Neustart direkte Methode beginnen.

    ```
    node simDevice.js
    ```

2. Führen Sie in der Befehlszeile in den Ordner **ScheduleJobService** den folgenden Befehl Trigger RRU und Abfrage für Gerät und zu dem letzten Mal neu starten.

    ```
    node scheduleJobService.js
    ```

3. Sie sehen die Ausgabe von Gerät und Back-End-Anwendung.


## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm wird einen Auftrag so planen Sie eine direkte Methode eines Geräts und der Aktualisierung der Geräte und Eigenschaften verwendet.

Dazu Einstieg IoT Hub und Device Management Muster wie Remote über eine Firmware-Aktualisierung finden Sie unter:

[Exemplarische Vorgehensweise: So führen Sie eine Firmware-Aktualisierung][lnk-fwupdate]

Weiter mit IoT Hub Einstieg finden Sie unter [Erste Schritte mit dem SDK Gateway IoT][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx