## <a name="view-device-telemetry-in-the-dashboard"></a>Gerät-Telemetrie im Dashboard anzeigen

Dashboard in remote monitoring-Lösung können Sie Telemetriedaten anzeigen, die Ihre Geräte IoT Hub senden.

1. In Ihrem Browser remote monitoring Lösung Dashboard zurückzukehren Sie, klicken Sie im linken Bereich der **Geräteliste**Navigieren auf **Geräte** .

2. In der **Geräteliste**sollten Sie sehen, dass der Status des Geräts jetzt **ausgeführt**.

    ![][18]

3. Klicken Sie auf **Dashboard** zum Dashboard zurück, wählen das Gerät im **Gerät auf** Dropdown-Anzeigen der Telemetrie. Telemetriedaten aus der beispielanwendung ist 50 Einheiten für Temperatur, 55 Einheiten für externe Temperaturen und 50 Einheiten für Feuchtigkeit. Beachten Sie, dass standardmäßig das Dashboard nur Temperatur und Feuchtigkeit Werte angezeigt.

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>Einen Befehl an das Gerät senden

Das Dashboard in remote monitoring-Lösung können Sie Befehle an Ihre Geräte über IoT Hub zu senden. Das remote monitoring-Lösung können Sie beispielsweise einen Befehl an die interne Temperatur eines Geräts senden.

1. Klicken Sie im remote monitoring lösungsdashboard **Geräte** im linken Bereich der **Geräteliste**navigieren.

2. Klicken Sie auf **Geräte-ID** für das Gerät in der **Geräteliste**.

3. Klicken Sie im Fenster **Gerät** auf **Befehle**.

    ![][13]

4. Wählen Sie in der Dropdownliste **Befehl** **SetTemperature aus**, und geben Sie einen neuen Wert **Temperatur** . Klicken Sie auf **Befehl senden** , um den Befehl an das Gerät senden.

    ![][14]

    > [AZURE.NOTE] Befehlsverlauf zeigt zunächst den Befehlsstatus **offen**. Das Gerät erkennt den Befehl ändert den Status für **Erfolg**.

5. Das Dashboard sicher, dass das Gerät jetzt als der neue Wert 75 sendet.

## <a name="next-steps"></a>Nächste Schritte

Artikel [Anpassen vorkonfigurierte Lösung] [ lnk-customize] beschreibt einige Methoden dieses Beispiel erweitern. Mögliche Erweiterung enthalten echte Sensoren und zusätzliche Befehle implementieren.

Weitere Informationen zu den [Berechtigungen auf der Website azureiotsuite.com][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
