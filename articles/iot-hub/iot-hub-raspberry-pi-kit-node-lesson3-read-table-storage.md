<properties
 pageTitle="Gelesene Nachrichten in Azure-Speicher beibehalten | Microsoft Azure"
 description="Schreiben in der Azure-Tabellenspeicher Gerät Cloud-Nachrichten zu überwachen."
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

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 gelesenen Nachrichten beibehalten in Azure-Speicher

## <a name="331-what-will-you-do"></a>3.3.1 Vorgehensweise

Die Gerät Cloud Nachrichten aus Ihrem Himbeeren Pi 3 an IoT Hub Schreiben von Nachrichten in der Azure-Tabellenspeicher zu überwachen. Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="332-what-will-you-learn"></a>3.3.2 Lernziele

- Wie schlucken Nachricht lesen Vorgang zum Lesen von Nachrichten beibehalten in der Azure-Tabellenspeicher.

## <a name="333-what-do-you-need"></a>3.3.3 Was benötigen Sie

- Sie müssen im vorherigen Abschnitt [Azure Blink beispielanwendung auf Ihre Himbeeren Pi 3 ausführen](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)erfolgreich haben.

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 lesen Sie neue Nachrichten von Ihrem Speicher

Im vorherigen Abschnitt haben Sie eine beispielanwendung auf Ihrem Pi ausgeführt. Beispiel Anwendung gesendete Nachrichten an Ihre Azure IoT Hub. Ihre IoT Hub gesendete Nachrichten werden in Ihrem Azure Tabellenspeicher App Azure-Funktion gespeichert. Azure Storage-Verbindungszeichenfolge für Ihre Azure-Tabellenspeicher Nachrichten gelesen werden sollen.

Lesen von Nachrichten in der Azure-Tabellenspeicher folgendermaßen Sie vor:

1. Abrufen der Verbindungszeichenfolge die folgenden Befehle ausführen:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    Der erste Befehl ruft die `storage name` , im zweiten Befehl verwendet wird, um die Verbindungszeichenfolge abzurufen. `iot-sample`Der Standardwert ist `{resource group name}` ändern Sie den Wert in Lektion 2 haben.

2. Öffnen Sie die Datei `config-raspberrypi.json` Datei in Visual Studio Code durch den folgenden Befehl ausführen:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Ersetzen Sie `[Azure Storage connection string]` mit der Verbindungszeichenfolge, die Sie in Schritt 1 erhalten haben.
4. Speichern der `config-raspberrypi.json` Datei.
5. Nachrichten erneut senden und Lesen von Azure Tabellenspeicher durch Ausführen des folgenden Befehls:

    ```bash
    gulp run --read-storage
    ```

    Die Logik zum Lesen von Azure-Tabellenspeicher ist der `azure-table.js` Datei.

    ![schlucken ausführen - Lese-Speicher](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 Zusammenfassung

Sie haben erfolgreich Pi mit der IoT Hub in der Cloud verbunden und Blink beispielanwendung Gerät Cloud-Nachrichten senden. App Azure-Funktion kann auch verwendet werden, um Ihre Azure-Tabellenspeicher IoT Hub Nachrichten speichern. Sie können mit der nächsten Lektion zum Cloud-Gerät von Ihrem IoT Hub Pi schicken übergehen.

## <a name="next-steps"></a>Nächste Schritte

[Lektion 4: Cloud Gerät senden](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
