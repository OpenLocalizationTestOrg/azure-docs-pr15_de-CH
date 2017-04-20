> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>Einführung

Gerät im Vergleich sind JSON-Dokumente, die Gerätestatusinformationen (Metadaten, Konfigurationen und Zustände) zu speichern. IoT Hub besteht ein Gerät und für jede IoT Hub Verbindung Gerät.

Gerät im Vergleich zu verwenden:

* Gerät Metadaten vom Back-End zu speichern.
* Ihre geräteanwendung aktuellen Zustandsinformationen verfügbaren Funktionen wie Bedingung (z. B. die Konnektivität Verfahren) Bericht.
* Synchronisieren Sie den Zustand des lang andauernde Workflows (z. B. Firmware und Konfigurationsdateien Updates) Gerät app und Back-End.
* Abfrage der Geräte-Metadaten, Konfiguration oder Zustand.

> [AZURE.NOTE] Gerät im Vergleich dienen für die Synchronisierung und für Gerätekonfigurationen und Geschäftsbedingungen Abfragen. [Gerät-zu-Cloud] -Nachrichten[ lnk-d2c] Zeichenfolgen Zeitstempel Ereignisse (z. B. Telemetrie Datenströme zeitbasierte Sensor) und [Cloud-zu-Gerät-Methoden] [ lnk-methods] für die interaktive Steuerung von Geräten wie ein Fan von benutzergesteuerten app einschalten.

Gerät im Vergleich in ein IoT Hub gespeichert und enthalten:

* *Tags*Gerät Metadaten mit dem Back-End;
* *Eigenschaften*, JSON-Objekte zurück und beobachten von der app Gerät geändert werden; und
* *gemeldete Eigenschaften*JSON-Objekten in app Gerät geändert werden und von der Rückseite beenden. Tags und Eigenschaften Arrays enthalten, aber Objekte geschachtelt werden.

![][img-twin]

Darüber hinaus kann app-Back-End Gerät im Vergleich anhand der obigen Daten Abfragen.
Finden Sie unter [Grundlegendes zu Gerät im Vergleich] [ lnk-twins] Weitere Informationen zu Gerät im Vergleich und [IoT Hub Abfragesprache] [ lnk-query] Referenz für Abfragen.

> [AZURE.NOTE] Zu diesem Zeitpunkt sind Geräte im Vergleich nur Verbindung IoT Hub mit dem Protokoll MQTT Geräte zugegriffen werden. Siehe [MQTT unterstützen] [ lnk-devguide-mqtt] Artikel Hinweise vorhandenen Gerät app konvertiert MQTT verwenden.

Dieses Lernprogramm zeigt Ihnen, wie an:

- Erstellen Sie eine Back-End-Anwendung, die ein Gerät zwei und einer simulierten Gerät, das den Kanal Konnektivität als eine *Eigenschaft gemeldet* auf das Gerät meldet *Tags* hinzugefügt.
- Abfrage Geräte Ihre Tags und Eigenschaften, die zuvor erstellte Filter über Back-End-Anwendung.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md