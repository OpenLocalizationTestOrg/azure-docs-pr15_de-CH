## <a name="create-an-iot-hub"></a>IoT Hub erstellen

Erstellen Sie einen IoT Hub simulierten Gerät herstellen. Die folgenden Schritte zeigen, wie diese Aufgabe mithilfe des Azure-Portals.

1. [Azure-Portal]anmelden[lnk-portal].

2. Klicken Sie in der Indexleiste auf **neue** > **Internet der Dinge** > **Azure IoT Hub**.

    ![Azure-Portal Indexleiste][1]

3. Wählen Sie die Konfiguration für IoT Hub Blatt **IoT Hub** .

    ![IoT Hub blade][2]

    * Geben Sie im Feld **Name** einen Namen für IoT Hub. Wenn der **Name** gültig und verfügbar ist, wird ein grünes Häkchen im Feld **Name** angezeigt.
    * Wählen Sie [Preise und Skalierung Tier][lnk-pricing]. Dieses Lernprogramm erfordert keine bestimmte Ebene. Verwenden Sie für dieses Lernprogramm freie F1-Ebene.
    * In **Ressourcengruppe**eine neue Ressourcengruppe erstellen oder bestehende auswählen. Weitere Informationen finden Sie unter [Using Ressourcengruppen Azure Ressourcen verwalten][lnk-resource-groups].
    * Wählen Sie **Speicherort**den Speicherort IoT Hub hosten. Wählen Sie für dieses Lernprogramm nächstgelegenen Standort aus.

4. Wenn Sie Ihre IoT Hub Optionen ausgewählt haben, klicken Sie auf **Erstellen**.  Es dauert einige Minuten Azure IoT Hub zu erstellen. Zum Überprüfen des Status können Sie den Fortschritt auf das Startmenü oder im Fenster Benachrichtigungen überwachen.

    ![Neuen IoT Hub-status][3]

5. IoT Hub erfolgreich erstellt wurde, klicken Sie auf die neue Kachel für Ihre IoT Hub im Azure-Portal Blade für neue IoT Hub zu öffnen. Notieren Sie den **Hostnamen**und dann auf **Shared-Richtlinien**.

    ![Neue IoT Hub blade][4]

6. **Freigegebene Richtlinien** Blatt klicken Sie auf die Richtlinie **Iothubowner** kopieren Sie und dann Notieren Sie der Verbindungszeichenfolge **Iothubowner** Blatt. Weitere Informationen finden Sie in der [Access Control] [ lnk-access-control] "Azure IoT Hub Developer Guide."

    ![Gemeinsamer Zugriff Richtlinien blade][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
