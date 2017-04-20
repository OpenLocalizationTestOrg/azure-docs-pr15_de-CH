<properties
 pageTitle="Erstellen und Bereitstellen die Anwendung Blink | Microsoft Azure"
 description="Klonen Sie Beispiel Node.js Anwendung von Github und Bereitstellung dieser Anwendung auf Ihr Mainboard Himbeeren Pi 3 schlucken. Diese Anwendung blinkt die LED alle zwei Sekunden an das Board verbunden."
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

# <a name="13-create-and-deploy-the-blink-application"></a>1.3 erstellen und Bereitstellen der Anwendungdes blinken

## <a name="131-what-you-will-do"></a>1.3.1 Was Sie tun

Klonen Sie Beispiel Node.js Anwendung von Github und verwenden Sie schlucken Tool zum Bereitstellen der beispielanwendung die Himbeeren Pi 3. Beispiel-Anwendung blinkt die LED alle zwei Sekunden an das Board verbunden. Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="132-what-you-will-learn"></a>1.3.2 Lerninhalte

- Verwendung der `device-discover-cli` Tool Netzwerkinformationen zu Pi abrufen.
- Zum Bereitstellen und Ausführen der beispielanwendung Pi.
- Bereitstellung und Debuganwendungen auf Pi Remote ausführen.

## <a name="133-what-you-need"></a>1.3.3 benötigen Sie

Sie müssen die folgenden Abschnitte in Lektion 1 erfolgreich haben:

- [Konfigurieren Sie das Gerät](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
- [Die Tools](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="134-obtain-the-ip-address-and-host-name-of-your-pi"></a>1.3.4 beziehen der IP-Adresse und Name des Pi

Öffnen Sie ein Eingabeaufforderungsfenster in Windows oder ein Terminalfenster MacOS oder Ubuntu, und führen Sie den folgenden Befehl:

```bash
devdisco list --eth
```

Wie die folgende sollte Ausgabe angezeigt werden:

![Gerätesuche](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Beachten Sie die `IP address` und `hostname` von Pi. Sie benötigen diese Informationen in diesem Abschnitt.

> [AZURE.NOTE] Stellen Sie sicher, dass Pi mit demselben Netzwerk wie Ihr Computer verbunden ist. Beispielsweise wenn Ihr Computer mit einem drahtlosen Netzwerk verbunden ist, während der Pi an ein verkabeltes Netzwerk verbunden ist, kann nicht die IP-Adresse in der Devdisco-Ausgabe angezeigt.

## <a name="135-clone-the-sample-application"></a>1.3.5 Klonen der beispielanwendung

Gehen Sie folgendermaßen vor, um den Beispielcode zu öffnen:

1. Klonen von Github Repository Beispiel durch Ausführen des folgenden Befehls:

    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
    ```

2. Öffnen Sie die beispielanwendung in Visual Studio Code durch Ausführen der folgenden Befehle:

    ```bash
    cd iot-hub-node-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![REPO-Struktur](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

Die `app.js` Datei der `app` Unterordner ist die Schlüssel, die Kontrolle der LED-Code enthält.

### <a name="136-install-application-dependencies"></a>1.3.6 anwendungsabhängigkeiten installieren

Installieren Sie die Bibliotheken und andere Module müssen Sie die Beispiel-Anwendung durch Ausführen des folgenden Befehls:

```bash
npm install
```

## <a name="137-configure-the-device-connection"></a>1.3.7 die Verbindung zum Gerät konfigurieren

Gehen Sie folgendermaßen vor, um die Verbindung zu konfigurieren:

1. Generieren der Gerätekonfigurationsdatei durch den folgenden Befehl ausführen:

    ```bash
    gulp init
    ```

    Die Konfigurationsdatei `config-raspberrypi.json` Pi Anmeldung Anmeldeinformationen enthält. Vermeiden Sie den Verlust von Benutzeranmeldeinformationen die Konfigurationsdatei im Unterordner generiert `.iot-hub-getting-started` der Basisordner auf dem Computer.

2. Öffnen Sie die Gerätekonfigurationsdatei im Visual Studio-Code durch den folgenden Befehl ausführen:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Ersetzen Sie den Platzhalter `[device hostname or IP address]` mit der IP-Adresse oder den Hostnamen im Abschnitt 1.3.4 erhalten.

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

Herzlichen Glückwunsch! Sie haben die erste beispielanwendung für Pi erstellt.

## <a name="138-deploy-and-run-the-sample-application"></a>1.3.8 bereitstellen und Ausführen der beispielanwendung

### <a name="1381-install-nodejs-and-npm-on-your-pi"></a>1.3.8.1 Installieren von Node.js und NPM auf Pi

Installieren Sie auf Pi Node.js und NPM durch Ausführen des folgenden Befehls:

```bash
gulp install-tools
```

Es dauert 10 Minuten beim ersten Ausführen dieser Aufgabe.

### <a name="1382-deploy-and-run-the-sample-app"></a>1.3.8.2 bereitstellen und Ausführen der Beispiel-app

Bereitstellen und Ausführen der beispielanwendung durch Ausführen des folgenden Befehls:

```bash
gulp deploy && gulp run
```

### <a name="1383-verify-the-app-works"></a>1.3.8.3 Überprüfen Sie, ob die Anwendung funktioniert

Die LED sollte jetzt auf Ihre Pi blinken alle zwei Sekunden angezeigt werden.  Wenn Sie nicht sehen, die LED blinkt, finden Sie die [Fehlerbehebung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) für Probleme.
![Blinkt](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

> [AZURE.NOTE] Mit `Ctrl + C` zum Beenden der Anwendung.

## <a name="139-summary"></a>1.3.9-Zusammenfassung

Sie haben die erforderlichen Tools zum Arbeiten mit Pi installiert und eine beispielanwendung Pi blinkt die LED bereitgestellt. Sie können nun mit der nächsten Lektion erstellen, bereitstellen und einem anderen beispielanwendung ausführen, die mit Pi Azure IoT Hub senden und Empfangen von Nachrichten.

## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt zu Lektion 2, die beginnt mit [Azure tools](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
