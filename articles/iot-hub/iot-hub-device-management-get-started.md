<properties
 pageTitle="Erste Schritte mit Device-Management | Microsoft Azure"
 description="Dieses Lernprogramm veranschaulicht das Device Management auf Azure IoT Hub beginnen"
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

# <a name="tutorial-get-started-with-device-management-preview"></a>Lernprogramm: Erste Schritte mit Device-Management (Vorschau)

## <a name="introduction"></a>Einführung
IoT Cloudanwendungen können primitive in Azure IoT Hub nämlich Gerät Twin und direkten Methoden zu Remote Management Aktionen auf Geräten zu überwachen.  Dieser Artikel enthält Hinweise und Code für eine IoT Cloud-Anwendung und ein Funktionsweise zu initiieren und Überwachen von einem Remotegerät Neustart IoT Hub verwenden.

Verwenden Sie zum Starten und überwachen Management Aktionen auf den Geräten von Cloud-basierten, Back-End-Anwendung, Azure IoT Hub Grundelemente wie [Gerät zwei] [ lnk-devtwin] und [Cloud-zu-Gerät (C2D) Methoden][lnk-c2dmethod]. Dieses Lernprogramm zeigt Ihnen wie ein Back-End-Anwendung und ein Gerät zusammenarbeiten können können initiieren und überwachen Remotegerät Neustart IoT Hub.

Sie verwenden eine C2D Methode Device Management-Aktionen (z. B. Neustart, Funktion und Firmware-Aktualisierung) aus einer Backend-app in die Cloud zu initiieren. Das Gerät ist verantwortlich für:

- Behandeln den Methodenaufruf IoT Hub gesendet.
- Initiiert entsprechende Maßnahme Gerät auf das Gerät.
- Bereitstellen von Statusupdates über das Gerät zwei Eigenschaften IoT Hub gemeldet.

Eine Back-End-Anwendung können in der Cloud Gerät zwei Abfragen zum Bericht über den Fortschritt der Device Management-Aktionen ausführen.

Dieses Lernprogramm zeigt Ihnen, wie an:

- Verwenden Sie Azure-Portal IoT Hub und Erstellen einer geräteidentität im IoT Hub.
- Erstellen eines simulierten Geräts eine direkte Methode Neustart ermöglicht Cloud genannt.
- Erstellen Sie eine, die auf dem simulierten Gerät über IoT Hub Neustart direkte Methode aufruft.

Am Ende dieses Lernprogramms haben Sie gegenseitige Node.js-Konsole:

**dmpatterns_getstarted_device.js**, die IoT Hub mit der Identität des Geräts zuvor erstellte Verbindung empfängt Neustart direkte Methode, simuliert einen physischen Neustart und meldet bei dem letzten Neustart.

**dmpatterns_getstarted_service.js**, direkter Methodenaufrufe auf dem simulierten Gerät zeigt die Antwort und zeigt aktualisierte Gerätetreiber Twin gemeldet Eigenschaften.

Um dieses Lernprogramm benötigen Sie Folgendes:

Node.js-Version 0.12.x oder höher <br/>  [Vorbereiten der Umgebung] [ lnk-dev-setup] Node.js für dieses Lernprogramm unter Windows oder Linux Installation beschrieben.

Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Gerät

In diesem Abschnitt Node.js Konsole app, die eine direkte Methode namens Cloud simulierten Gerät einen Neustart auslöst entspricht erstellen und verwendet zwei Gerät gemeldet Eigenschaften Gerät zwei Abfragen Geräte und beim letzten Neustart aktivieren.

1. Erstellen Sie einen neuen Ordner namens **Manageddevice**.  Erstellen Sie im Ordner **Manageddevice** eine package.json mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```
    
2. In der Befehlszeile in den Ordner **Manageddevice** , führen Sie den folgenden Befehl zum Installieren der **azure-iot-device@dtpreview** Gerät SDK-Paket und **azure-iot-device-mqtt@dtpreview** Paket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Mit einem Texteditor eine neue Datei **dmpatterns_getstarted_device.js** im Ordner " **Manageddevice** " erstellen.

4. Fügen Sie die folgenden "-Anweisung am Anfang der Datei **dmpatterns_getstarted_device.js** erforderlich":

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Hinzufügen einer Variable **ConnectionString** und Gerät Client erstellen.  Ersetzen Sie die Verbindungszeichenfolge mit Ihrem Gerät Verbindungszeichenfolge.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Fügen Sie die folgende Funktion zum Implementieren der direkten Methode auf dem Gerät

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Öffnen Sie die Verbindung IoT Hub und starten Sie direkte Methode Listener:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Speichern Sie und schließen Sie die Datei **dmpatterns_getstarted_device.js** .

 [AZURE.NOTE] Zur Vereinfachung, implementiert keine wiederholen Richtlinien in diesem Lernprogramm. In Produktionscode sollten Sie wiederholungsrichtlinien (z. B. eine exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]implementieren[lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Trigger RRU auf dem Gerät eine direkte Methode 

In diesem Abschnitt erstellen Sie eine Node.js Konsole app, die eine RRU auf einem Gerät mit einer direkten Methode initiiert und Gerät zwei Abfragen der letzten Neustart für dieses Gerät gefunden.

1. Erstellen Sie einen neuen Ordner namens **Triggerrebootondevice**.  Erstellen Sie im Ordner **Triggerrebootondevice** eine package.json mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```
    
2. In der Befehlszeile in den Ordner **Triggerrebootondevice** , führen Sie den folgenden Befehl zum Installieren der **azure-iothub@dtpreview** Gerät SDK-Paket und **azure-iot-device-mqtt@dtpreview** Paket:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. Mit einem Texteditor eine neue Datei **dmpatterns_getstarted_service.js** im Ordner " **Triggerrebootondevice** " erstellen.

4. Fügen Sie die folgenden "-Anweisung am Anfang der Datei **dmpatterns_getstarted_service.js** erforderlich":

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Fügen Sie die folgenden Variablendeklarationen, und Ersetzen Sie die Platzhalterwerte:

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Fügen Sie die folgende Funktion zum Aufrufen der Methode Gerät um das Zielgerät neu zu starten:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Fügen Sie die folgende Funktion des Geräts abzufragen und dem letzten Neustart:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Fügen Sie folgenden Code zum Aufrufen von Funktionen, die direkte Methode Neustart und Abfrage zuletzt einen Neustart auslöst:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Speichern Sie und schließen Sie die Datei **dmpatterns_getstarted_service.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun die Anwendung ausführen.

1. Führen Sie in der Befehlszeile in den Ordner **Manageddevice** den folgenden Befehl warten auf Neustart direkte Methode beginnen.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Triggerrebootondevice** den folgenden Befehl Trigger RRU und Abfrage für Gerät und zu dem letzten Mal neu starten.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Reagieren auf direkte Methode sehen Sie die Meldung drucken

## <a name="customize-and-extend-the-device-management-actions"></a>Passen Sie an und erweitern Sie der Device Management-Aktionen

IoT-Lösungen können definierte Device Management Muster erweitern oder benutzerdefinierte Muster Gerät Twin und C2D Methode primitive aktivieren. Andere Geräte Maßnahmen gehören Factory zurücksetzen, Firmware-Aktualisierung Softwareupdate, Powermanagement, Verwaltung von Netzwerk und Konnektivität und Verschlüsselung.

## <a name="device-maintenance-windows"></a>Gerät Wartungsfenster

In der Regel konfigurieren Sie Geräte für Aktionen ausführen, die Unterbrechung und Ausfallzeiten minimiert.  Gerät Wartungsfenster sind häufig verwendete Zeit definieren, wenn ein Gerät die Konfiguration aktualisiert werden soll. Die Back-End-Lösung können die gewünschten Eigenschaften des Geräts und definieren und Aktivieren einer Richtlinie auf dem Gerät, das ein Wartungsfenster ermöglicht. Erhält ein Fenster Wartungsrichtlinien können gemeldete Eigenschaft von Gerät und den Status der Richtlinie gemeldet. Die Back-End-Anwendung können Sie Gerät zwei Abfragen der Geräte und Richtlinien bestätigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm verwendet Sie eine direkte Methode auslösen RRU auf einem Gerät verwendet zwei Geräte abgefragt und Geräte zu dem letzten Neustart des Geräts aus der Cloud und Eigenschaften der letzten Neustart vom Gerät gemeldet gemeldet.

Dazu Einstieg IoT Hub und Device Management Muster wie Remote über eine Firmware-Aktualisierung finden Sie unter:

[Exemplarische Vorgehensweise: So führen Sie eine Firmware-Aktualisierung][lnk-fwupdate]

Die IoT erweitern Lösung und Zeitplan auf mehreren Geräten Methodenaufrufe finden Sie unter [Zeitplan und broadcast Aufträge] [ lnk-tutorial-jobs] Tutorial.

Weiter mit IoT Hub Einstieg finden Sie unter [Erste Schritte mit dem SDK Gateway IoT][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx