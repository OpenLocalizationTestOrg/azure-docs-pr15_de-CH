<properties
    pageTitle="Erste Schritte mit im Vergleich | Microsoft Azure"
    description="Dieses Lernprogramm zeigt Ihnen wie im Vergleich"
    services="iot-hub"
    documentationCenter="node"
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

# <a name="tutorial-get-started-with-device-twins-preview"></a>Lernprogramm: Erste Schritte mit Gerät im Vergleich (Vorschau)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Am Ende dieser praktischen Einführung haben Sie eine und einem Konsolenanwendungsprojekt Node.js:

* **AddTagsAndQuery.sln**, eine .NET app vom Back-End ausgeführt werden soll, fügt Tags Abfragen Gerät im Vergleich.
* **TwinSimulatedDevice.js**, eine Node.js-Anwendung simuliert ein Gerät, das mit der Identität des Geräts zuvor erstellte IoT Hub Verbindung und meldet den Zustand der Verbindung.

> [AZURE.NOTE] Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen über die verschiedenen SDKs, mit Gerät und Back-End-Anwendung.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015.

+ Node.js-Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Die Dienst-Anwendung erstellen

In diesem Abschnitt erstellen Sie eine Node.js-Konsole app, die zwei **MyDeviceId**zugeordneten Speicherort Metadaten hinzufügt. Anschließend fragt es die Geräte auswählen Hub gespeichert im Vergleich in den USA und die liegt, eine Mobilfunkverbindung reporting.

1. In Visual Studio ein Visual C# herkömmlichen Windows-Desktop Projekt hinzufügen der aktuellen Projektmappe mit der Projektvorlage **Konsolenanwendungsprojekt** . Nennen Sie das Projekt **AddTagsAndQuery**.

    ![Neue Visual C# herkömmlichen Windows-Desktop-Projekt][img-createapp]

2. Im Projektmappen-Explorer mit der rechten Maustaste des **AddTagsAndQuery** Projekts und dann auf **Nuget-Pakete verwalten**.

3. Im Fenster **Nuget Paket-Manager** unbedingt **Include Prerelease** aktiviert ist, **microsoft.azure.devices**suchen, wählen Sie installieren **Installieren** der *Vorabversion **Microsoft.Azure.Devices** * Verpacken und die Vereinbarung akzeptieren. Dieses Verfahren heruntergeladen, installiert und ein Verweis auf die [Microsoft Azure IoT Service SDK] [ lnk-nuget-service-sdk] Nuget-Paket und die zugehörigen Dateien.

    ![NuGet Paket-Manager Fenster][img-servicenuget]

4. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei **"Program.cs"** :

        using Microsoft.Azure.Devices;

5. Fügen Sie die folgenden Felder der Klasse **Anwendung** . Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für den IoT Hub, den Sie im vorherigen Abschnitt erstellt haben.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Der **Program** -Klasse die folgende Methode hinzugefügt:

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    Die **RegistryManager** -Klasse stellt alle Methoden Gerät im Vergleich aus dem Dienst zu interagieren. Der vorherige Code zuerst initialisiert das **RegistryManager** -Objekt abgerufen und für **MyDeviceId**, und schließlich die Tags mit den gewünschten Informationen aktualisiert.

    Nach der Aktualisierung zwei Abfragen ausgeführt: die erste Anweisung wählt nur die Geräte im Vergleich der Geräte in der **Redmond43** und die zweite passt die Abfrage nur die Geräte aus, die auch über Mobilfunknetz verbunden sind.

    Beachten Sie, dass der vorherige Code, wenn **das Abfrageobjekt** erstellt eine maximale Anzahl von zurückgegebenen Dokumente angibt. **Das Abfrageobjekt** enthält eine boolesche Eigenschaft **HasMoreResults** , mit denen Sie **GetNextAsTwinAsync** Methoden mehrmals aufrufen, um Ergebnisse abzurufen. Eine Methode namens **GetNextAsJson** steht für Ergebnisse nicht Gerät im Vergleich zum Beispiel Abfrageergebnisse Aggregation.

7. Die **Main** -Methode schließlich folgenden Zeilen hinzufügen:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Führen Sie diese Anwendung, und ein Gerät in die Ergebnisse für die Abfrage nach allen Geräten in **Redmond43** und für die Abfrage, die die Ergebnisse auf Geräte beschränkt, mit denen ein Mobilfunknetz sehen.

    ![Abfrageergebnisse im Fenster][img-addtagapp]

Im nächsten Abschnitt erstellen Sie eine Geräte-app, die meldet die Verbindungsinformationen und ändert das Ergebnis der Abfrage im vorherigen Abschnitt.

## <a name="create-the-device-app"></a>Das Gerät erstellen

In diesem Abschnitt erstellen Sie eine Node.js gemeldet Konsole app, die eine Verbindung mit Ihrem Hub **MyDeviceId**und aktualisiert dann die zwei Eigenschaften, die Informationen enthalten, mit einem Mobilfunknetz verbunden ist.

1. Erstellen Sie einen neuen Ordner namens **Reportconnectivity**. Erstellen Sie eine neue package.json-Datei mit dem folgenden Befehl in die Befehlszeile im Ordner **Reportconnectivity** . Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Reportconnectivity** führen Sie den folgenden Befehl **Azure-Iot-Gerät**und **Azure Iot-Gerät Mqtt** Paket installieren:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Mit einem Texteditor eine neue Datei **ReportConnectivity.js** im Ordner " **Reportconnectivity** " erstellen.

4. Fügen Sie folgenden Code in die Datei **ReportConnectivity.js** und Ersetzen Sie **{Gerät Verbindungszeichenfolge}** Platzhalter mit der Erstellung der Identität des **MyDeviceId** Geräts kopierten Verbindungszeichenfolge:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    Das **Client** -Objekt macht alle Methoden benötigen Sie interagieren mit Gerät im Vergleich vom Gerät. Der vorherige Code nach **Client** -Objekt initialisiert abgerufen und für **MyDeviceId** und aktualisiert die gemeldete Eigenschaft mit den Verbindungsinformationen.

5. Führen Sie die geräteanwendung

        node ReportConnectivity.js

    Sollte die Meldung `twin state reported`.

6. Da das Gerät die Konnektivitätsinformationen gemeldet, erscheint es in beiden Abfragen. Führen Sie die Anwendung auf .NET **AddTagsAndQuery** erneut abfragen. Diese Zeit **MyDeviceId** sollten beide Abfrageergebnisse angezeigt.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm einen neuen IoT Hub in Azure-Portal konfiguriert und eine geräteidentität in den Hub identitätsregistrierung erstellt. Sie Gerät Metadaten aus einer Back-End-Anwendung als Tags hinzugefügt und schrieb eine Ausgabegerät app Device Connectivity Informationen in zwei Geräte. Sie haben gelernt, wie Abfragen mithilfe der IoT Hub SQL-ähnliche Sprache.

Die folgenden Ressourcen verwenden, um zu erfahren, wie:

- Telemetriedaten von Geräten mit dem [Einstieg mit IoT Hub] senden[ lnk-iothub-getstarted] Lernprogramm
- Geräte und die gewünschten Eigenschaften die [gewünschten Eigenschaften konfiguriert Geräte] konfigurieren[ lnk-twin-how-to-configure] Lernprogramm
- Geräte interaktiv (z. B. einschalten Fan benutzergesteuerten App) mit [direkten Methoden] steuern[ lnk-methods-tutorial] Tutorial.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

