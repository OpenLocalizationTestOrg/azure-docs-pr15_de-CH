<properties
 pageTitle="Führen Sie beispielanwendung Gerät Cloud-Nachrichten | Microsoft Azure"
 description="Bereitstellen und Ausführen einer beispielanwendung auf Ihrem Himbeeren Pi 3, die Nachrichten an IoT Hub gesendet und blinkt die LED."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3.2 führen Sie beispielanwendung Gerät Cloud-Nachrichten aus

## <a name="321-what-you-will-do"></a>3.2.1 Was Sie tun

Bereitstellen und Ausführen einer beispielanwendung Ihre Himbeeren Pi 3, die Nachrichten an IoT Hub sendet. Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="322-what-you-will-learn"></a>3.2.2 Lerninhalte

- Wie Sie schlucken Tool zum Bereitstellen und Pi Beispiel Node.js-Anwendung ausführen.

## <a name="323-what-you-need"></a>3.2.3 benötigen Sie

- Sie müssen erfolgreich abgeschlossen haben im vorherigen Abschnitt in dieser Lektion: [Azure Funktion app und Azure Storage-Konto zum Verarbeiten und Speichern von IoT Hub Nachrichten erstellen](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 Zeichenfolgen Verbindung IoT Hub und Gerät abrufen

Das Gerät wird verwendet die Pi Verbindung IoT Hub. IoT Hub-connect-Zeichenfolge Verbindung IoT Hub mit Identität des Geräts, die Pi im IoT Hub verwendet.

- Abrufen der Verbindungszeichenfolge IoT Hub folgenden Azure CLI-Befehl ausführen:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`ist der Name, den Sie in Lektion 2 angegeben. Mit `iot-sample` als Wert der `{resource group name}` ändern Sie den Wert in Lektion 2 haben.

- Abrufen der Verbindungszeichenfolge Gerät den folgenden Befehl ausführen:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`hat denselben Wert wie die mit dem vorherigen Befehl. Verwenden Sie `iot-sample` als Wert der `{resource group name}` und `myraspberrypi` als Wert der `{device id}` ändern Sie den Wert in Lektion 2 haben.

## <a name="325-configure-the-device-connection"></a>3.2.5 Konfigurieren der Verbindungs

1. Initialisieren der Konfigurationsdatei durch Ausführen der folgenden Befehle:

    ```bash
    npm install
    gulp init
    ```

2. Öffnen Sie die Gerätekonfigurationsdatei `config-raspberrypi.json` in Visual Studio Code durch den folgenden Befehl ausführen:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Stellen Sie die folgenden Ersatzteile in der `config-raspberrypi.json` Datei:

  - Ersetzen **[Gerät Hostnamen oder IP-Adresse]** Gerät IP-Adresse oder Hostname von wurde `device-discovery-cli` oder der Wert geerbt, was Sie in Lektion 1 konfiguriert.
  - Ersetzen Sie **[IoT Gerät Verbindungszeichenfolge]** mit den `device connection string` Sie erhalten.
  - Ersetzen Sie **[IoT Hub Verbindungszeichenfolge]** mit den `iot hub connection string` Sie erhalten.

Aktualisierung der `config-raspberrypi.json` Datei, damit Sie die beispielanwendung auf Ihrem Computer bereitstellen können.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 Bereitstellen und Ausführen der beispielanwendung

Bereitstellen und Ausführen der beispielanwendung Pi durch Ausführen des folgenden Befehls:

```bash
gulp
```

> [AZURE.NOTE] Schlucken Standardtask führt `install-tools`, `deploy`, und `run` Aufgaben nacheinander. In [Lektion 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)wird diese Aufgaben nacheinander einzeln ausgeführt.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 Überprüfen der Funktionsfähigkeit des Beispiels Anwendung

Pi blinken alle zwei Sekunden an sollte die LED angezeigt werden. Jedes Mal, wenn die LED blinkt beispielanwendung sendet eine Nachricht an Ihre IoT Hub und stellt sicher, dass die Nachricht erfolgreich an IoT Hub gesendet wurden. Darüber hinaus wird die Nachricht für jede Meldung IoT Hub im Konsolenfenster gedruckt. Beispiel-Anwendung endet automatisch nach 20 Nachrichten senden.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 Zusammenfassung

Sie haben bereitgestellt und auf Pi IoT Hub Gerät Cloud-Nachrichten an neue Blink beispielanwendung ausführen. Sie können mit dem nächsten Abschnitt Nachrichten überwacht, beim Schreiben in Azure Storage-Konto verschieben.

## <a name="next-steps"></a>Nächste Schritte

[3.3 gelesenen Nachrichten beibehalten in Azure-Speicher](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)