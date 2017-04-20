<properties
    pageTitle="Zwei Eigenschaften | Microsoft Azure"
    description="In diesem Lernprogramm wird gezeigt, wie zwei Eigenschaften"
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Lernprogramm: Verwenden Sie Eigenschaften zum Konfigurieren von Geräten (Vorschau)

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Am Ende dieses Lernprogramms haben Sie gegenseitige Node.js-Konsole:

* **SimulateDeviceConfiguration.js**, ein Ausgabegerät app, das wartet, bis ein Update Konfiguration und den Status der Aktualisierung einer simulierten Konfiguration.
* **SetDesiredConfigurationAndQuery**, eine .NET Konsole app vom Back-End setzt der Konfiguration auf einem Gerät und Abfragen des Aktualisierungsvorgangs Konfiguration ausgeführt werden soll.

> [AZURE.NOTE] Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen über die verschiedenen SDKs, mit Gerät und Back-End-Anwendung.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015.

+ Node.js-Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

Wenn Sie [Erste Schritte mit Gerät im Vergleich] ausgeführt[ lnk-twin-tutorial] Lernprogramm bereits aktiviert Verwaltungszentrale Gerät und geräteidentität aufgerufen **MyDeviceId**; und [erstellen die app simulierten Gerät] überspringen[ lnk-how-to-configure-createapp] Abschnitt.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>Erstellen der simulierten Gerät

In diesem Abschnitt erstellen Sie eine Node.js-Konsole app, die eine Verbindung mit Ihrem Hub **MyDeviceId**, wartet, bis ein Update Konfiguration und Updates auf den simulierten Konfigurationsprozess Update meldet.

1. Erstellen Sie einen neuen Ordner namens **Simulatedeviceconfiguration**. Erstellen Sie eine neue package.json-Datei mit dem folgenden Befehl in die Befehlszeile im Ordner **Simulatedeviceconfiguration** . Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Simulatedeviceconfiguration** führen Sie den folgenden Befehl **Azure-Iot-Gerät**und **Azure Iot-Gerät Mqtt** Paket installieren:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Mit einem Texteditor eine neue Datei **SimulateDeviceConfiguration.js** im Ordner " **Simulatedeviceconfiguration** " erstellen.

4. Fügen Sie folgenden Code in die Datei **SimulateDeviceConfiguration.js** und Ersetzen Sie **{Gerät Verbindungszeichenfolge}** Platzhalter mit der Erstellung der Identität des **MyDeviceId** Geräts kopierten Verbindungszeichenfolge:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    **Client** -Objekt enthält alle Methoden mit Gerät im Vergleich vom Gerät interagieren. Der vorherige Code nach **dem Clientobjekt** initialisiert ruft zwei für **MyDeviceId**und fügt einen Handler für das Update auf die gewünschten Eigenschaften. Der Handler überprüft, wird einem Änderungsantrag Konfiguration durch Vergleich der ConfigIds und ruft eine Methode, die die Änderung der Konfiguration wird gestartet.

    Beachten Sie, dass der Einfachheit halber der Programmcode einen hartcodierten Standardwert für die Erstkonfiguration. Eine echte Anwendung würden wahrscheinlich die Konfiguration aus dem lokalen Speicher geladen werden.
    
    > [AZURE.IMPORTANT] Gewünschte Eigenschaftenänderungsereignisse werden bei Verbindung immer einmal ausgegeben, stellen Sie sicher, dass eine tatsächliche die gewünschten Eigenschaften Änderung vor dem Ausführen einer Aktion.

5. Fügen Sie die folgenden Methoden vor der `client.open()` aufrufen:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    Die **InitConfigChange** -Methode Aktualisierungsanfrage Config gemeldete Eigenschaften im lokalen Twin Objekt aktualisiert und setzt den Status auf **Pending**und zwei Geräte vom Dienst aktualisiert. Nach dem erfolgreichen Update Twin, simuliert einen langer Prozess, der bei der Ausführung der **CompleteConfigChange**beendet. Diese Methode aktualisiert die lokalen und gemeldete Eigenschaften festlegen des Status für **Erfolg** und Entfernen des **PendingConfig** -Objekts. Anschließend werden zwei vom Dienst aktualisiert.

    Beachten, Bandbreite, speichern gemeldet Eigenschaften werden durch Festlegen der Eigenschaften um (benannte **Patch** im obigen Code), statt Ersetzen das gesamte Dokument geändert werden.

    > [AZURE.NOTE] In diesem Lernprogramm simuliert das Verhalten für gleichzeitige konfigurationsupdates nicht. Einige Konfigurationsprozesse Update möglicherweise ändert Zielkonfiguration aufzunehmen, und die Aktualisierung wird ausgeführt, andere müssen diese Warteschlange andere konnte mit einem Fehlerzustand ablehnen. Stellen Sie sicher das gewünschte Verhalten für den Prozess der Konfiguration und fügen Sie entsprechende Logik hinzu, bevor Sie mit der Änderung der Konfiguration.

6. Führen Sie die geräteanwendung:

        node SimulateDeviceConfiguration.js

    Sollte die Meldung `retrieved device twin`. Halten Sie die app.

## <a name="create-the-service-app"></a>Die Dienst-Anwendung erstellen

In diesem Abschnitt erstellen Sie eine .NET Konsole app, die *gewünschten Eigenschaften* auf den zugeordneten **MyDeviceId** mit neuen Telemetrie-Konfigurationsobjekt aktualisiert. Dann fragt gespeichert im Hub Gerät im Vergleich und zeigt die Differenz zwischen den gewünschten und gemeldeten Konfigurationen des Geräts.

1. In Visual Studio ein Visual C# herkömmlichen Windows-Desktop Projekt hinzufügen der aktuellen Projektmappe mit der Projektvorlage **Konsolenanwendungsprojekt** . Nennen Sie das Projekt **SetDesiredConfigurationAndQuery**.

    ![Neue Visual C# herkömmlichen Windows-Desktop-Projekt][img-createapp]

2. Im Projektmappen-Explorer mit der rechten Maustaste des **SetDesiredConfigurationAndQuery** Projekts und dann auf **Nuget-Pakete verwalten**.

3. Im Fenster **Nuget Paket-Manager** unbedingt **Include Prerelease** aktiviert ist, **microsoft.azure.devices**suchen, wählen Sie installieren **Installieren** der *Vorabversion **Microsoft.Azure.Devices** * Verpacken und die Vereinbarung akzeptieren. Dieses Verfahren heruntergeladen, installiert und ein Verweis auf die [Microsoft Azure IoT Service SDK] [ lnk-nuget-service-sdk] Nuget-Paket und die zugehörigen Dateien.

    ![NuGet Paket-Manager Fenster][img-servicenuget]

4. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei **"Program.cs"** :

        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;

5. Fügen Sie die folgenden Felder der Klasse **Anwendung** . Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für den IoT Hub, den Sie im vorherigen Abschnitt erstellt haben.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Der **Program** -Klasse die folgende Methode hinzugefügt:

        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
            
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired state");

            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }

    **Registrierung** -Objekt enthält alle Methoden mit Gerät im Vergleich aus dem Dienst interagieren. Der vorherige Code nach dem **Registry** -Objekt initialisiert ruft zwei für **MyDeviceId**und aktualisiert die gewünschten Eigenschaften mit ein neues Konfigurationsobjekt Telemetrie.
    Danach alle 10 Sekunden dabei fragt den im Vergleich im Hub gespeichert und Konfigurationen gewünschten und gemeldete Telemetrie. [IoT Hub-Abfragesprache] finden Sie unter[ lnk-query] lernen, umfassende Berichte über alle Ihre Geräte.

    > [AZURE.IMPORTANT] Diese Anwendung fragt IoT Hub alle 10 Sekunden zu Illustrationszwecken. Verwenden Sie Abfragen für Benutzer Berichte über viele Geräte und nicht für Änderungen. Wenn Ihre Lösung erfordert Echtzeit-Benachrichtigungen Ereignisse verwenden [Gerät Cloud-Nachrichten][lnk-d2c].

7. Die **Main** -Methode schließlich folgenden Zeilen hinzufügen:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();

8. Mit **SimulateDeviceConfiguration.js** senden ausführen .NET Anwendung von Visual Studio mit **F5** gemeldete Konfiguration **Erfolg** **Ausstehend** , **erfolgreich** erneut mit dem neuen Änderung sehen sollten fünf Minuten statt Stunden.

    > [AZURE.IMPORTANT] Es gibt eine Verzögerung von einer Minute zwischen Bericht Gerätevorgang und Abfrage. Dies ist die Abfrage Infrastruktur zu sehr hohen Ebene aktivieren. Rufen Sie verwenden konsistent auf eine zwei **GetDeviceTwin** -Methode in der **Registrierung** .

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm gewünschte Konfiguration als *gewünschter* vom Back-End festgelegt und schrieb eine Gerät app erkennt diese Änderung und einen mehrstufigen Aktualisierungsvorgang als *gemeldete Eigenschaften* die Statusberichte Twin simulieren.

Die folgenden Ressourcen verwenden, um zu erfahren, wie:

- Telemetriedaten von Geräten mit dem [Einstieg mit IoT Hub] senden[ lnk-iothub-getstarted] Lernprogramm
- Planen oder Durchführen von Operationen für große Geräte Siehe [verwendet Aufträge planen und Gerätevorgänge] [ lnk-schedule-jobs] Tutorial.
- Geräte interaktiv (z. B. einschalten Fan benutzergesteuerten App), [direkte Methoden] steuern[ lnk-methods-tutorial] Tutorial.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
