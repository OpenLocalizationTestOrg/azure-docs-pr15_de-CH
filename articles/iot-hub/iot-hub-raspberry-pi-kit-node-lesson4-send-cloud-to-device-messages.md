<properties
 pageTitle="Führen Sie die beispielanwendung Cloud Gerät Nachrichten empfangen | Microsoft Azure"
 description="Beispiel-Anwendung in Lektion 4 auf Pi und überwacht eingehende Nachrichten aus Ihrem IoT Hub. Eine neue Aufgabe schlucken sendet Nachrichten an Pi Ihre IoT Hub die LED blinkt."
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

# <a name="41-run-the-sample-application-to-receive-cloud-to-device-messages"></a>4.1 Führen Sie beispielanwendung Cloud-zu-Gerät-Nachrichten empfangen aus

In diesem Abschnitt stellen Sie eine beispielanwendung auf Ihrem Himbeeren Pi 3. Beispiel-Anwendung überwacht eingehende Nachrichten von IoT Hub. Sie auch einen Schluck Vorgang auf Ihrem Computer Pi von IoT Hub Nachrichten an. Nach dem Empfang der Nachrichten, blinkt die LED Beispiel-Anwendung. Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="411-what-you-will-do"></a>4.1.1 Was Sie tun

- Verbinden Sie die beispielanwendung IoT Hub.
- Bereitstellen und Ausführen der beispielanwendung.
- Nachrichten von IoT Hub in Pi die LED blinkt.

## <a name="412-what-you-will-learn"></a>4.1.2 Lerninhalte

- Wie Sie eingehende Nachrichten von IoT Hub überwachen.
- Wie Pi Cloud Gerät Nachrichten IoT Hub an. 

## <a name="413-what-do-you-need"></a>4.1.3 Was benötigen Sie

- Himbeeren Pi 3, die eingerichtet ist. Zum Einrichten der Pi finden Sie unter [Lektion 1: Erste Schritte mit Ihrem Gerät Himbeeren Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md)
- Ein IoT-Hub, der in der Azure-Abonnement erstellt wird. Erstellen Sie Ihre Azure IoT Hub finden Sie unter [Lektion 2: Erstellen der Azure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="414-connect-the-sample-application-to-your-iot-hub"></a>4.1.4 Verbinden Sie beispielanwendung mit IoT hub

1. Stellen Sie sicher, Sie im Ordner "Repo" `iot-hub-node-raspberrypi-getting-started`. Öffnen Sie die beispielanwendung in Visual Studio Code durch Ausführen der folgenden Befehle:

    ```bash
    cd Lesson4
    code .
    ```

    Beachten Sie die `app.js` in der Datei der `app` Unterordner. Die `app.js` Datei ist der Schlüssel mit dem Code IoT Hub Nachrichten überwachen. Die `blinkLED` Funktion blinkt die LED.

    ![REPO-Struktur](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)

2. Initialisieren Sie die Konfigurationsdatei mit den folgenden Befehlen:

    ```bash
    npm install
    gulp init
    ```

    Wenn Sie auf diesem Computer Lektion 3 abgeschlossen haben, werden alle Konfigurationen geerbt, sodass 4.1.5 Schritt überspringen können. Wenn Sie Lektion 3 auf einem anderen Computer ausgeführt, müssen Sie ersetzen Sie die Platzhalter in der `config-raspberrypi.json` Datei. Die `config-raspberrypi.json` Datei befindet sich im Unterverzeichnis der Basisordner.

    ![Konfiguration](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

- Ersetzen Sie **[Gerät Hostnamen oder IP-Adresse]** mit IP-Adresse oder Hostname durch Ausführen des Befehls erhalten Ihre Pi`devdisco list --eth`
- Gerät-Verbindungszeichenfolge, die Sie erhalten mit dem Befehl Ersetzen **[IoT Gerät Verbindungszeichenfolge]** `az iot hub show-connection-string --name {my hub name} --resource-group {resource group name}`.
- Ersetzen Sie **[IoT Hub Verbindungszeichenfolge]** mit der Verbindungszeichenfolge IoT Hub, der mit dem Befehl get `az iot device show-connection-string --hub {my hub name} --device-id {device id} --resource-group {resource group name}`.

## <a name="415-deploy-and-run-the-sample-application"></a>4.1.5 bereitstellen und Ausführen der beispielanwendung

Bereitstellen und Ausführen der beispielanwendung Pi durch Ausführen der folgenden Befehle:
  
```
gulp
```

Schlucken Befehl führt den Task Tools installieren zuerst. Die beispielanwendung Pi bereitstellen. Schließlich wird die Anwendung ausgeführt, Pi und einer separaten Aufgabe auf dem Hostcomputer 20 Blink Pi von IoT Hub schicken.

Nach Anwendung der Probe Nachrichten IoT Hub Empfangsbereitschaft. In der Zwischenzeit sendet die schlucken Aufgabe mehrere "blinken" Nachrichten von IoT Hub an Pi. Für jede Meldung blinken Ruft die beispielanwendung die BlinkLED-Funktion, um die LED zu blinken.

Die LED blinken alle zwei Sekunden schlucken Vorgang 20 Nachrichten von IoT Hub an Pi senden wird angezeigt. Zuletzt wird eine "stop" angezeigt, die die Anwendung nicht mehr ausgeführt wird.

![Schlucken](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="416-summary"></a>4.1.6 Zusammenfassung

Sie haben Nachrichten aus IoT Hub, Pi blinkt die LED gesendet. Weiter wird Optionaler Abschnitt, der wie ein- und Ausschalten der LED-Verhalten zu ändern.

## <a name="next-steps"></a>Nächste Schritte

[Optionaler Abschnitt: ein- und Ausschalten der LED-Verhalten zu ändern](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)