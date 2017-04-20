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
* **SetDesiredConfigurationAndQuery.js**, eine Node.js-Anwendung vom Back-End ausgeführt werden, die Konfiguration auf einem Gerät und fragt den Konfigurationsprozess aktualisieren.

> [AZURE.NOTE] Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen über die verschiedenen SDKs, mit Gerät und Back-End-Anwendung.

Um dieses Lernprogramm benötigen Sie Folgendes:

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

In diesem Abschnitt erstellen Sie eine app Node.js-Konsole, die *gewünschten Eigenschaften* auf den zugeordneten **MyDeviceId** mit neuen Telemetrie-Konfigurationsobjekt aktualisiert. Dann fragt gespeichert im Hub Gerät im Vergleich und zeigt die Differenz zwischen den gewünschten und gemeldeten Konfigurationen des Geräts.

1. Erstellen Sie einen neuen Ordner namens **Setdesiredandqueryapp**. Erstellen Sie eine neue package.json-Datei mit dem folgenden Befehl in die Befehlszeile im Ordner **Setdesiredandqueryapp** . Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Setdesiredandqueryapp** führen Sie den folgenden Befehl **Azure Iothub** installiert:

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. Mit einem Texteditor eine neue Datei **SetDesiredAndQuery.js** im Ordner " **Addtagsandqueryapp** " erstellen.

4. Fügen Sie folgenden Code in die Datei **SetDesiredAndQuery.js** und Ersetzen Sie **{-Verbindungszeichenfolge}** Platzhalter mit der Erstellung den Hub kopierten Verbindungszeichenfolge:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    **Registrierung** -Objekt enthält alle Methoden mit Gerät im Vergleich aus dem Dienst interagieren. Der vorherige Code nach dem **Registry** -Objekt initialisiert ruft zwei für **MyDeviceId**und aktualisiert die gewünschten Eigenschaften mit ein neues Konfigurationsobjekt Telemetrie. Danach wird das Ereignis **QueryTwins** Funktion 10 Sekunden.

    > [AZURE.IMPORTANT] Diese Anwendung fragt IoT Hub alle 10 Sekunden zu Illustrationszwecken. Verwenden Sie Abfragen für Benutzer Berichte über viele Geräte und nicht für Änderungen. Wenn Ihre Lösung erfordert Echtzeit-Benachrichtigungen Ereignisse verwenden [Gerät Cloud-Nachrichten][lnk-d2c].

5. Fügen Sie den folgenden Code direkt vor der `registry.getDeviceTwin()` Aufruf die **QueryTwins** -Funktion implementiert:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    Die vorherige Code Abfragen der im Vergleich im Hub gespeichert und druckt Konfigurationen gewünschten und gemeldete Telemetrie. [IoT Hub-Abfragesprache] finden Sie unter[ lnk-query] lernen, umfassende Berichte über alle Ihre Geräte.


8. Führen Sie mit **SimulateDeviceConfiguration.js** die Anwendung:

        node SetDesiredAndQuery.js 5m

    Müsste der Konfiguration sich **Erfolg** **Ausstehend** , **erfolgreich** mit dem neuen aktiven senden fünf Minuten statt Stunden.

    > [AZURE.IMPORTANT] Es gibt eine Verzögerung von einer Minute zwischen Bericht Gerätevorgang und Abfrage. Dies ist die Abfrage Infrastruktur zu sehr hohen Ebene aktivieren. Rufen Sie verwenden konsistent auf eine zwei **GetDeviceTwin** -Methode in der **Registrierung** .

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm Sie gewünschte Konfiguration als *Eigenschaften* von Back-End-Anwendung und schrieb eine Ausgabegerät app erkennen, ändern und simuliert einen mehrstufigen Aktualisierungsvorgang als *Eigenschaften gemeldet* die Statusberichte Twin.

Die folgenden Ressourcen verwenden, um zu erfahren, wie:

- Telemetriedaten von Geräten mit dem [Einstieg mit IoT Hub] senden[ lnk-iothub-getstarted] Lernprogramm
- Planen oder Durchführen von Operationen für große Geräte Siehe [verwendet Aufträge planen und Gerätevorgänge] [ lnk-schedule-jobs] Tutorial.
- Geräte interaktiv (z. B. einschalten Fan benutzergesteuerten App), [direkte Methoden] steuern[ lnk-methods-tutorial] Tutorial.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

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
