> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>Einführung

In [Erste Schritte mit IoT Hub im Vergleich][lnk-twin-tutorial], gelernt Gerät Metadaten aus der Lösung Backend mit *Tags*Bericht Gerät Startbedingungen ein Gerät App mit *gemeldeten Eigenschaften*festlegen und diese Informationen mithilfe einer SQL-ähnlichen Sprache Abfragen.

In diesem Lernprogramm erfahren Sie, wie Sie die die zwei *Eigenschaften* zusammen mit *Eigenschaften gemeldet*, Gerät apps Remote konfigurieren. Genauer gesagt, in diesem Lernprogramm wird wie Twin gemeldet und Eigenschaften ermöglichen eine mehrstufige Konfiguration ein Gerät Anwendung festlegen, und die Sichtbarkeit zu Lösung den Status dieses Vorgangs auf allen Geräten.

Dieses Lernprogramm folgt auf hohem Niveau *gewünschten Muster* für Device-Management. Die grundlegende Idee dieses Muster soll das Backend Lösung den gewünschten Zustand für verwaltete Geräte anstatt Befehle angeben. Dadurch wird das Gerät für die beste Möglichkeit, die gewünschten Zustand (wichtiger IoT Szenarien, Gerätetyp Bedingung wirkt auf die Befehle sofort ausführen), einrichten und den aktuellen Status und potenziellen Fehlerzustände kontinuierlich zu reporting. Das gewünschten Muster ist für die Verwaltung großer Mengen von Geräten, Back-End zu volle Transparenz des Status des Konfigurationsvorgangs auf allen Geräten.
Finden Sie weitere Informationen zur Rolle des gewünschten Musters Device Management [Overview Azure IoT Hub Device]Management[lnk-dm-overview].

> [AZURE.NOTE] In Szenarien, in denen Geräte auf interaktive Weise (Fan benutzergesteuerten App einschalten) werden, erwägen [Cloud Gerät Methoden][lnk-methods].

In diesem Lernprogramm folgt Anwendung Backend Telemetrie Konfiguration des Zielgeräts und dadurch die geräteanwendung mehreren Schritten um eine Konfiguration (z. B. Neustart Software-Modul), Updates in diesem Lernprogramm mit einem einfachen simuliert).

Backend-speichert die Konfiguration in das Gerät zwei Eigenschaften wie folgt:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] Da Konfigurationen komplexer Objekte sind, werden normalerweise eindeutige Ids zugewiesen (Hashes oder [GUIDs][lnk-guid]) ihre Vergleiche zu vereinfachen.

App Gerät meldet die aktuelle Konfiguration spiegeln die gewünschte Eigenschaft **TelemetryConfig** gemeldeten Eigenschaften:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Hinweis wie gemeldete **TelemetryConfig** eine zusätzliche Eigenschaft **Status**verwendet, um den Status der Aktualisierung der Konfiguration.

Beim Empfang einer neuen gewünschten Konfigurations meldet Gerät app eine ausstehende Konfiguration durch Ändern der Informationen:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Dann zu einem späteren Zeitpunkt meldet Gerät app Erfolg Fehler bei diesem Vorgang die obige Eigenschaft aktualisieren.
Beachten Sie, wie die Back-End können jederzeit den Status des Konfigurationsvorgangs auf allen Geräten abgefragt.

Dieses Lernprogramm zeigt Ihnen, wie an:

- Erstellen Sie ein simuliertes Gerät, das Back-End konfigurationsupdates empfängt und meldet mehrere Updates als *gemeldete Eigenschaften* auf die Konfiguration aktualisieren.
- Backend-app aktualisiert die gewünschte Konfiguration eines Geräts und fragt dann den Konfigurationsprozess Update zu erstellen.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier