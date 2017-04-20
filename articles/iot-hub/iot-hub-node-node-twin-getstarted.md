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

Am Ende dieses Lernprogramms haben Sie gegenseitige Node.js-Konsole:

* **AddTagsAndQuery.js**, eine Node.js-Anwendung vom Back-End ausgeführt werden soll, fügt Tags Abfragen Gerät im Vergleich.
* **TwinSimulatedDevice.js**, eine Node.js-Anwendung simuliert ein Gerät, das mit der Identität des Geräts zuvor erstellte IoT Hub Verbindung und meldet den Zustand der Verbindung.

> [AZURE.NOTE] Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen über die verschiedenen SDKs, mit Gerät und Back-End-Anwendung.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Node.js-Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Die Dienst-Anwendung erstellen

In diesem Abschnitt erstellen Sie eine Node.js-Konsole app, die zwei **MyDeviceId**zugeordneten Speicherort Metadaten hinzufügt. Anschließend fragt es die Geräte auswählen Hub gespeichert im Vergleich in den USA und die liegt, eine Mobilfunkverbindung reporting.

1. Erstellen Sie einen neuen Ordner namens **Addtagsandqueryapp**. Erstellen Sie eine neue package.json-Datei mit dem folgenden Befehl in die Befehlszeile im Ordner **Addtagsandqueryapp** . Übernehmen Sie die Standardwerte:

    ```
    npm init
    ```

2. Die Befehlszeile im Ordner **Addtagsandqueryapp** führen Sie den folgenden Befehl **Azure Iothub** installiert:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Mit einem Texteditor eine neue Datei **AddTagsAndQuery.js** im Ordner " **Addtagsandqueryapp** " erstellen.

4. Fügen Sie folgenden Code in die Datei **AddTagsAndQuery.js** und Ersetzen Sie **{-Verbindungszeichenfolge}** Platzhalter mit der Erstellung den Hub kopierten Verbindungszeichenfolge:

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    **Registrierung** -Objekt enthält alle Methoden mit Gerät im Vergleich aus dem Dienst interagieren. Der vorherige Code zuerst initialisiert **das Registrierungsobjekt** dann abgerufen und für **MyDeviceId**und schließlich die Tags mit den gewünschten Informationen aktualisiert.

    Nach der Aktualisierung der Tags wird die Funktion **QueryTwins** .

7. Fügen Sie den folgenden Code am Ende des **AddTagsAndQuery.js** die **QueryTwins** -Funktion implementiert:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    Der vorherige Code werden zwei Abfragen ausgeführt: die erste Anweisung wählt nur die Geräte im Vergleich der Geräte in der **Redmond43** und die zweite passt die Abfrage nur die Geräte aus, die auch über Mobilfunknetz verbunden sind.

    Beachten Sie, dass der vorherige Code, wenn **das Abfrageobjekt** erstellt eine maximale Anzahl von zurückgegebenen Dokumente angibt. **Das Abfrageobjekt** enthält eine boolesche Eigenschaft **HasMoreResults** , mit denen Sie **NextAsTwin** Methoden mehrmals aufrufen, um Ergebnisse abzurufen. **Nächste** Methode steht für Ergebnisse nicht Gerät im Vergleich zum Beispiel Abfrageergebnisse Aggregation.

8. Führen Sie die Anwendung:

        node AddTagsAndQuery.js

    Sie sollten sehen, dass ein Gerät in die Ergebnisse für die Abfrage nach allen Geräten in **Redmond43** und für die Abfrage, die die Ergebnisse auf Geräte beschränkt, die ein Mobilfunknetz verwenden.

    ![][1]

Im nächsten Abschnitt erstellen Sie eine Geräte-app, die meldet die Verbindungsinformationen und ändert das Ergebnis der Abfrage im vorherigen Abschnitt.

## <a name="create-the-device-app"></a>Das Gerät erstellen

In diesem Abschnitt erstellen Sie eine Node.js gemeldet Konsole app, die eine Verbindung mit Ihrem Hub **MyDeviceId**und aktualisiert dann die zwei Eigenschaften, die Informationen enthalten, mit einem Mobilfunknetz verbunden ist.

> [AZURE.NOTE] Zu diesem Zeitpunkt sind Geräte im Vergleich nur Verbindung IoT Hub mit dem Protokoll MQTT Geräte zugegriffen werden. Siehe [MQTT unterstützen] [ lnk-devguide-mqtt] Artikel Hinweise vorhandenen Gerät app konvertiert MQTT verwenden.

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

6. Da das Gerät die Konnektivitätsinformationen gemeldet, erscheint es in beiden Abfragen. Im Ordner " **Addtagsandqueryapp** " zurück und die Abfragen:

        node AddTagsAndQuery.js

    Diese Zeit **MyDeviceId** sollten beide Abfrageergebnisse angezeigt.

    ![][3]

## <a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm einen neuen IoT Hub in Azure-Portal konfiguriert und eine geräteidentität in den Hub identitätsregistrierung erstellt. Sie Gerät Metadaten aus einer Back-End-Anwendung als Tags hinzugefügt und schrieb eine Ausgabegerät app Device Connectivity Informationen in zwei Geräte. Sie haben gelernt, wie Abfragen mithilfe der IoT Hub SQL-ähnliche Sprache.

Die folgenden Ressourcen verwenden, um zu erfahren, wie:

- Telemetriedaten von Geräten mit dem [Einstieg mit IoT Hub] senden[ lnk-iothub-getstarted] Lernprogramm
- Geräte und die gewünschten Eigenschaften die [gewünschten Eigenschaften konfiguriert Geräte] konfigurieren[ lnk-twin-how-to-configure] Lernprogramm
- Geräte interaktiv (z. B. einschalten Fan benutzergesteuerten App), [direkte Methoden] steuern[ lnk-methods-tutorial] Tutorial.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md