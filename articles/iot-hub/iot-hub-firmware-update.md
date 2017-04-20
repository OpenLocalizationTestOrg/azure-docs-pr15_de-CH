<properties
 pageTitle="Wie Sie eine Firmware-Aktualisierung | Microsoft Azure"
 description="Dieses Lernprogramm wird gezeigt, wie eine Aktualisierung der firmware"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Exemplarische Vorgehensweise: So führen Sie eine Firmware Update (Vorschau)

## <a name="introduction"></a>Einführung
[Erste Schritte mit Device-Management] [ lnk-dm-getstarted] Tutorial sah wie das [Gerät zwei] [ lnk-devtwin] und [Cloud-zu-Gerät (C2D) Methoden] [ lnk-c2dmethod] primitive ein Remote-Neustart. In diesem Lernprogramm verwendet die gleichen IoT Hub primitive Anleitung und zeigt, wie ein End-to-End simulierte Firmware Update.  Dieses Muster wird in der Firmware-Update-Implementierung für Intel Edison Gerät Beispiel verwendet.

Dieses Lernprogramm zeigt Ihnen, wie an:

- Erstellen Sie eine, die auf dem simulierten Gerät über IoT Hub FirmwareUpdate direkte Methode aufruft.
- Erstellen eines simulierten Geräts, das Firmwareupdate direkte Methode der durchläuft einen mehrstufigen Prozess, der wartet, downloaden Sie das Firmware-Image, das Firmware-Image downloads implementiert und schließlich gilt th Firmware-Image.  In jeder Phase das Gerät anhand der zwei Eigenschaften aktualisieren gemeldet ausführen ausgeführt.

Am Ende dieses Lernprogramms haben Sie gegenseitige Node.js-Konsole:

**dmpatterns_fwupdate_service.js**, die direkte Methode auf dem simulierten Gerät aufruft, zeigt die Antwort und regelmäßig (alle 500 ms) zeigt zwei Gerät aktualisiert gemeldet Eigenschaften.

**dmpatterns_fwupdate_device.js**, die IoT Hub mit der Identität des Geräts zuvor erstellte Verbindung FirmwareUpdate direkte Methode empfängt, durchläuft eine mehrstufige Prozess für die Simulation ein Firmware-Update einschließlich: Warten auf Bild herunterladen, das neue Bild herunterladen und schließlich das Abbild angewendet.


Um dieses Lernprogramm benötigen Sie Folgendes:

Node.js-Version 0.12.x oder höher <br/>  [Vorbereiten der Umgebung] [ lnk-dev-setup] Node.js für dieses Lernprogramm unter Windows oder Linux Installation beschrieben.

Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

Folgen Sie den [Einstieg Device Management](iot-hub-device-management-get-started.md) Artikel IoT Hub erstellen und die Verbindungszeichenfolge.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Gerät

In diesem Abschnitt Node.js Konsole app, die eine direkte Methode namens Cloud ein Firmwareupdate simulierten Gerät auslöst reagiert erstellen und verwendet zwei Gerät gemeldet Eigenschaften Gerät zwei Abfragen Geräte und beim letzten Neustart aktivieren.

1. Erstellen Sie einen neuen Ordner namens **Manageddevice**.  Erstellen Sie im Ordner **Manageddevice** eine package.json mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```
    
2. In der Befehlszeile in den Ordner **Manageddevice** , führen Sie den folgenden Befehl zum Installieren der **azure-iot-device@dtpreview** Gerät SDK-Paket und **azure-iot-device-mqtt@dtpreview** Paket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Mit einem Texteditor eine neue Datei **dmpatterns_fwupdate_device.js** im Ordner " **Manageddevice** " erstellen.

4. Fügen Sie die folgenden "-Anweisung am Anfang der Datei **dmpatterns_fwupdate_device.js** erforderlich":

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Hinzufügen einer Variable **ConnectionString** und Gerät Client erstellen.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Fügen Sie die folgende Funktion mit zwei aktualisiert Eigenschaften gemeldet

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Fügen Sie die folgenden Funktionen den Download simulieren und das Firmware-Image gelten.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Fügen Sie die folgende Funktion den Firmware-Aktualisierungsstatus über die zwei aktualisieren Eigenschaften auf download gemeldet.  In der Regel Geräte verfügbaren Updates informiert und Administrator definierte Richtlinie wird das Gerät herunterladen und Anwenden der Aktualisierung.  Dies ist, wo die Logik zum Aktivieren dieser Richtlinie ausgeführt würde.  Der Einfachheit halber für 4 Sekunden verzögert und das Firmware-Image herunterladen. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Fügen Sie die folgende Funktion den Firmware-Aktualisierungsstatus über die zwei aktualisieren Eigenschaften, downloaden das Firmware-Image gemeldet.  Es folgt durch einen Firmware-Download simulieren und schließlich aktualisiert den Firmware-Aktualisierungsstatus Download Erfolg oder Fehler mit.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Fügen Sie die folgende Funktion den Firmware-Aktualisierungsstatus über die zwei aktualisieren Eigenschaften, das Firmware-Image anwenden gemeldet.  Es folgt durch Anwenden des Firmware-Images simulieren und schließlich aktualisiert den Firmware-Aktualisierungsstatus übernehmen Erfolg oder Fehler mit.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Fügen Sie folgenden Functoin die Methode FirmwareUpdate und Aktualisierungsvorgang mehrstufigen initiieren.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Schließlich fügen Sie den folgenden Code die Verbindung IoT Hub als Gerät, 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine wiederholen Richtlinien in diesem Lernprogramm. In Produktionscode sollten Sie wiederholungsrichtlinien (z. B. eine exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]implementieren[lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Lösen Sie remote Firmware-Aktualisierung auf dem Gerät eine direkte Methode aus 

In diesem Abschnitt erstellen Sie eine Node.js Konsole app, die remote Firmware-Aktualisierung auf einem Gerät mit einer direkten Methode initiiert und Gerät zwei Abfragen regelmäßig den Status der aktiven Firmware-Aktualisierung auf diesem Gerät abrufen.


1. Erstellen Sie einen neuen Ordner namens **Triggerfwupdateondevice**.  Erstellen Sie im Ordner **Triggerfwupdateondevice** eine package.json mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```
    
2. Die Befehlszeile im Ordner **Triggerfwupdateondevice** führen Sie den folgenden Befehl **Azure Iothub** Gerät SDK-Paket und **Azure Iot-Gerät Mqtt** Paket installieren:

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Mit einem Texteditor eine neue Datei **dmpatterns_getstarted_service.js** im Ordner " **Triggerfwupdateondevice** " erstellen.

4. Fügen Sie die folgenden "-Anweisung am Anfang der Datei **dmpatterns_getstarted_service.js** erforderlich":

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Fügen Sie die folgenden Variablendeklarationen, und Ersetzen Sie die Platzhalterwerte:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Fügen Sie die folgende Funktion zu und zeigt den Wert der FirmwareUpdate Eigenschaft gemeldet.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Fügen Sie die folgende Funktion aufrufen FirmwareUpdate-Methode, um das Gerät neu starten:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Schließlich berichtet hinzufügen die folgende Funktion Code zu Firmware aktualisieren Sequenz und regelmäßig zeigen die beiden Eigenschaften:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Speichern Sie und schließen Sie die Datei **dmpatterns_fwupdate_service.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun die Anwendung ausführen.

1. Führen Sie in der Befehlszeile in den Ordner **Manageddevice** den folgenden Befehl warten auf Neustart direkte Methode beginnen.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Triggerfwupdateondevice** den folgenden Befehl Trigger RRU und Abfrage für Gerät und zu dem letzten Mal neu starten.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Reagieren auf direkte Methode sehen Sie die Meldung drucken

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm Sie direkte Methode auslösen remote Firmware-Aktualisierung auf einem Gerät und regelmäßige Twin gemeldet Eigenschaften den Fortschritt der Firmware aktualisieren Process.  

Die IoT erweitern Lösung und Zeitplan auf mehreren Geräten Methodenaufrufe finden Sie unter [Zeitplan und broadcast Aufträge] [ lnk-tutorial-jobs] Tutorial.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
