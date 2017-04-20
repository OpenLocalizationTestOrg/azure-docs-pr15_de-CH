## <a name="create-a-device-management-enabled-iot-hub"></a>Erstellen Sie ein Gerät Management aktiviert IoT Hub

Da Vorschau IoT Hub Gerätemanagement ist, müssen Sie Gerät aktiviert IoT Verwaltungszentrale erstellen. IoT Hub Gerätemanagement allgemeine Verfügbarkeit erreicht, wird in diesem Lernprogramm aktualisiert. Die folgenden Schritte zeigen, wie Azure-Portal mit dieser Aufgabe.

1.  Mit der [Azure-Portal]anmelden.
2.  Klicken Sie in der Indexleiste auf **neu**, dann auf **Internet der Dinge**und klicken Sie dann auf **Azure IoT Hub**.

    ![][img-new-hub]

3.  Wählen Sie die Konfiguration für IoT Hub Blatt **IoT Hub** .

    ![][img-configure-hub]

  -   Geben Sie im Feld **Name** einen Namen für IoT Hub. Wenn der **Name** gültig und verfügbar ist, wird ein grünes Häkchen im Feld **Name** angezeigt.
  -   **Preise und Skalierung Ebene**auswählen Dieses Lernprogramm erfordert keine bestimmte Ebene.
  -   In **Ressourcengruppe**eine neue Ressourcengruppe erstellen oder bestehende auswählen. Weitere Informationen finden Sie unter [Using Ressourcengruppen Azure Ressourcen verwalten].
  -   Aktivieren Sie das Kontrollkästchen **Aktivieren Device Management - Vorschau**.
  -   Wählen Sie **Speicherort**den Speicherort IoT Hub hosten. IoT Hub Device-Management steht nur in ostasiatischen USA, Nordeuropa und Ostasien während public Preview-Version.

    > [AZURE.NOTE]Wenn Sie nicht das Feld **Device Management aktivieren**aktivieren, funktionieren die Beispiele nicht.<br/>Überprüfen Sie **Device Management aktivieren**, erstellen Sie eine Vorschau, die IOT Hub unterstützt nur in ostasiatischen USA, Nordeuropa und Ostasien und nicht Produktionsszenarien. Sie können keine Geräte in und aus Device Management aktiviert Hubs migrieren.

4.  Wenn Sie Ihre IoT Hub Optionen ausgewählt haben, klicken Sie auf **Erstellen**. Es dauert einige Minuten Azure IoT Hub zu erstellen. Zum Überprüfen des Status können Sie dem **Startmenü** oder im Fenster **Benachrichtigungen** überwachen.

    ![][img-monitor]

5.  Wenn IoT Hub erfolgreich erstellt wurde, öffnet das Blatt für den Hub. Notieren Sie den **Hostnamen**und dann auf **Shared-Richtlinien**.

    ![][img-keys]

6.  Klicken Sie auf die Richtlinie **Iothubowner** kopieren Sie und notieren Sie die Verbindungszeichenfolge in **Iothubowner** Blade. Kopieren sie auf eine Position, die Sie später zugreifen können, da Sie dieses Lernprogramm benötigt.

    > [AZURE.NOTE] Achten Sie darauf, Produktionsszenarien Anmeldeinformationen **Iothubowner** verzichten.

    ![][img-connection]

Sie haben nun ein Verwaltung aktiviert IoT Hub. Benötigen Sie die Verbindungszeichenfolge für diese praktische Einführung abgeschlossen.

## <a name="create-a-device-identity"></a>Erstellen Sie eine geräteidentität

In diesem Abschnitt verwenden Sie das Tool Knoten [IoT Hub Explorer] [ iot-hub-explorer] geräteidentität in diesem Lernprogramm erstellen.

1. Führen Sie Folgendes in der Befehlszeile Umgebung:

    Npm installieren -giothub-explorer@latest

2. Führen Sie folgenden Befehl zum Anmelden bei Ihrem Hub ersetzen an `{service connection string}` mit zuvor IoT Hub-Verbindungszeichenfolge:

    Iothub Explorer-Login "{Service Connection String}"

3. Abschließend erstellen eine neue geräteidentität aufgerufen `myDeviceId` mit dem Befehl:

    Iothub-Explorer erstellen MyDeviceId - Verbindungszeichenfolge

Notieren Sie die Verbindungszeichenfolge Gerät aus dem Ergebnis. Diese Verbindungszeichenfolge app Gerät dient der IoT Hub als Gerät verbinden.

![][img-identity]

Finden Sie unter [Erste Schritte mit IoT Hub] [ lnk-getstarted] für Gerät Identitäten programmgesteuert erstellen.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure-portal]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Verwenden von Ressourcengruppen Azure Ressourcen verwalten]: ../articles/azure-portal/resource-group-portal.md
