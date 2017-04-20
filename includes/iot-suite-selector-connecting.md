> [AZURE.SELECTOR]
- [C für Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
- [C unter Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [C mbed](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>Das Szenario

In diesem Szenario erstellen Sie ein Gerät, das die folgende Telemetrie [vorkonfigurierte]Lösung Remote sendet[lnk-what-are-preconfig-solutions]:

- Externe Temperaturen
- Temperatur
- Luftfeuchtigkeit

Der Einfachheit halber Code auf dem Gerät erzeugt Werte, aber wir empfehlen Ihnen, das Beispiel erweitern, indem real Sensoren an Ihr Gerät anschließen und real Telemetrie senden.

Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion][lnk-free-trial].

## <a name="before-you-start"></a>Bevor Sie beginnen

Vor dem Schreiben von Code für das Gerät müssen remote monitoring vorkonfigurierte Projektmappe bereitstellen und dann ein neues benutzerdefiniertes Gerät in dieser Projektmappe bereitstellen.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Bereitstellen der remote monitoring vorkonfigurierte Lösung

In diesem Lernprogramm erstellen Gerät sendet Daten an eine Instanz der [Remoteüberwachung] [ lnk-remote-monitoring] vorkonfigurierte Lösung. Wenn Sie bereits remote monitoring vorkonfigurierte Lösung in Azure-Konto bereitgestellt noch nicht, wie folgt:

1. Klicken Sie auf der Seite <https://www.azureiotsuite.com/> **+** eine neue Projektmappe erstellen.

2. Klicken Sie auf **auswählen** , auf die **Remote-Überwachung** die neue Lösung erstellen.

3. Auf der Seite **Erstellen fernüberwachungslösung** geben einen **Projektmappennamen** , wählen Sie die **Region** zu Bereitstellung und Azure Abonnement verwenden möchten. Klicken Sie auf **Projektmappe erstellen**.

4. Warten Sie, bis die Bereitstellung abgeschlossen.

> [AZURE.WARNING] Vorkonfigurierte Lösung verwenden berechenbare Azure Services. Achten Sie darauf, dass Sie Ihr Abonnement vorkonfigurierte Lösung entfernen, wenn dies zu unnötigen Kosten zu vermeiden. Sie können eine vorkonfigurierte Lösung aus Ihrem Abonnement entfernen <https://www.azureiotsuite.com/> Seite.

Nach Abschluss der Bereitstellungsprozess für das remote monitoring-Lösung, klicken Sie auf lösungsdashboard in Ihrem Browser öffnen **Starten** .

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Ihr Gerät remote monitoring-Lösung bereitstellen

> [AZURE.NOTE] Wenn ein Gerät in der Projektmappe bereits bereitgestellt haben, können Sie diesen Schritt überspringen. Sie müssen Anmeldeinformationen des Geräts wissen, wann Sie die Clientanwendung erstellen.

Ein Verbindung mit vorkonfigurierte Lösung muss es sich IoT Hub mit gültigen Anmeldeinformationen identifizieren. Sie können die Anmeldeberechtigungen aus dem lösungsdashboard abrufen. Anmeldeinformationen des Geräts wird in der Clientanwendung später in diesem Lernprogramm enthalten. 

Um ein neues Gerät remote monitoring Projektmappe hinzuzufügen, gehen Sie im lösungsdashboard:

1.  Klicken Sie in der linken unteren Ecke des Dashboards auf **Gerät hinzufügen**.

    ![][1]

2.  Klicken Sie im Bereich **Benutzerdefinierte Geräte** auf **Hinzufügen**.

    ![][2]

3.  Wählen Sie **mich definieren Sie eigene Geräte-ID**, geben Sie Geräte-ID wie **Mydevice**, auf **ID überprüfen** , stellen Sie sicher, dass dieser Name nicht bereits verwendet, und klicken Sie auf **Erstellen** , um das Gerät bereitstellen.

    ![][3]

5. Notieren Sie sich die geräteanmeldeinformationen (Device ID IoT Hub Hostname und Gerät), ist die Clientanwendung remote monitoring-Lösung verbunden. Klicken Sie auf **Fertig**.

    ![][4]

6. Stellen Sie sicher, dass das Gerät im Geräte-Abschnitt angezeigt. **Gerätestatus steht, bis das Gerät eine Verbindung zum remote monitoring-Lösung herstellt.**

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/