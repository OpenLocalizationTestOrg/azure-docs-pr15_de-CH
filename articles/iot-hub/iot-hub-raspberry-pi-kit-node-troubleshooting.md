<properties
 pageTitle="Problembehandlung bei | Microsoft Azure"
 description="Seite "Problembehandlung" Himbeeren Pi Node.js-Erfahrung"
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

# <a name="troubleshooting"></a>Problembehandlung

## <a name="hardware-issues"></a>Hardware-Probleme

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Auch die Anwendung aber die LED nicht blinkt

Dieses Problem bezieht sich immer auf Hardware Circuit-Konnektivität. Gehen Sie folgendermaßen vor, um Probleme zu identifizieren.

1. Überprüfen Sie bei Auswahl der richtigen **GPIO** auf dem Board. Zwei Anschlüsse in dieser Lektion sollte **GPIO-GND (Pin 6)** und **GPIO-04 (Pin 7)**.
2. Überprüfen Sie, ob die Polarität der LED korrekt ist. **Positive**, Anode Pin ist mehr Bein anzugeben.
3. Verwenden Sie **3,3 v Pin** und **GND Pin** auf Himbeeren Pi 3. Pi als Gleichstrom behandelt. Überprüfen Sie, ob die LED funktioniert.

![LED-Spezifikation](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Andere Hardwareprobleme

Informationen zum Beheben von allgemeinen Problemen Himbeeren Pi 3 finden Sie [Problembehandlung Homepage](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Node.js Paket Probleme

### <a name="no-response-during-gulp-tasks"></a>Keine Antwort schlucken Aufgaben

Schlucken Aufgaben Problemen können Sie die `--verbose` Option zum Debuggen. Aktuelle schlucken Aufgaben beenden versuchen `Ctrl + C` und führen Sie dann den folgenden im Konsolenfenster Befehl an Nachrichten zu debuggen. Sie sehen ausführliche Fehlermeldungen in der Ausgabe in der Konsole gedruckt. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Probleme mit Geräten discovery

Behandlung von Problemen mit Hilfe der `devdisco` Befehl, überprüfen Sie die [Readme-Datei](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="other-npm-issues"></a>Andere Probleme NPM

Versuchen Sie, das NPM-Paket mit dem folgenden Befehl aktualisieren:

```bash
npm install -g npm
```

Wenn das Problem weiterhin besteht, Ihre Kommentare am Ende dieses Artikels oder Github Problem in unserem [Beispiel Repository](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started) erstellen

## <a name="remote-debugging"></a>Remotedebuggen

### <a name="run-the-sample-application-in-debug-mode"></a>Beispielanwendung im Debugmodus ausführen

```bash
gulp run --debug
```

Wenn das Debugmodul bereit ist, sehen Sie ```Debugger listening on port 5858``` in der Ausgabe in der Konsole.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>Konfigurieren der VS-Code auf dem entfernten Gerät herstellen

Öffnen Sie das **Debuggen** auf der linken Seite.

Klicken Sie auf die grüne Schaltfläche **Debuggen starten** (F5). VS-Code öffnet eine **launch.json** -Datei, die Sie aktualisieren möchten.

Aktualisieren Sie die **launch.json** -Datei mit folgendem Inhalt. Ersetzen Sie `[device hostname or IP address]` mit dem Gerät IP-Adresse oder Hostname.   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Remotedebuggen – Konfiguration](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Die Remoteanwendung zuordnen

Klicken Sie auf die grüne Schaltfläche **Debuggen starten** (F5) Debuggen. 

Lesen Sie [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) erfahren Sie mehr über den Debugger.

![Interaktive Remotedebuggen](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI-Probleme

Azure ist eine Vorabversion. [Vorschau-Installationshandbuch](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) nach Lösungen suchen finden.

Sollten Fehler mit dem Tool Datei ein [Problem](https://github.com/Azure/azure-cli/issues) im Abschnitt **Probleme** Repo GitHub.

Überprüfen Sie Hilfestellung bei der Behandlung von Problemen der [Readme-Datei](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Python-Installationsprobleme

### <a name="legacy-installation-issues-macos"></a>Ältere Installationsprobleme (MacOS)

Beim Installieren von **Pip**wird ein Berechtigungsfehler bei ältere Paketen **Su** Berechtigungen installiert. Diese Situation tritt auf, weil frühere Installation von Python mit Kaffee (MacOS) nicht vollständig deinstalliert. Einige **Pip** -Pakete von einer vorherigen Installation root, entstanden Berechtigungsfehler verursacht. Die Lösung ist die Root installierten Pakete zu entfernen. Gehen Sie folgendermaßen vor, um diese Aufgabe auszuführen:

1. Gehe zu /usr/local/lib/python2.7/site-packages
2. Pakete auflisten erstellen Stamm:`ls -l | grep root`
3. Deinstallieren Sie Pakete aus Schritt 2:`sudo rm -rf {package name}`
4. Installieren Sie Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT Hub-Probleme

Azure IoT Hub mit erfolgreich bereitgestellt haben `azure-cli`, und Sie benötigen ein Tool zum Verwalten der Geräte Verbindung IoT Hub, versuchen die folgenden Tools:

### <a name="device-explorer"></a>Geräte-Explorer

[Gerät Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) auf dem lokalen Windows-Computer ausgeführt wird und eine Verbindung mit Ihrem IoT Hub in Azure. Er kommuniziert mit den folgenden [IoT Hub Endpunkte](iot-hub-devguide.md):

- *Identitätsmanagement Gerät* bereitstellen und Verwalten von Geräten mit IoT Hub registriert.
- *Empfangen Gerät-zu-Cloud-* Nachrichten von Ihrem Gerät IoT Hub überwachen können.
- *Cloud-Gerät senden* Nachrichten an Ihre Geräte von IoT Hub senden können.

Konfigurieren Sie Ihre `IoT hub connection string` in diesem Tool können Sie alle Funktionen verwenden.

### <a name="iot-hub-explorer"></a>IoT Hub Explorer

[IoT Hub Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) ist ein Beispiel für CLI Tool geräteclients verwalten. Das Tool können Sie Geräte in der identitätsregistrierung verwalten und überwachen Gerät Cloud-Nachrichten Cloud Gerät Befehle.

Um die neueste Version (Vorabversion) des Iothub Explorer-Tools zu installieren, führen Sie den folgenden Befehl in der Befehlszeile Umgebung:

```
npm install -g iothub-explorer@latest
```

Folgendes können Sie zusätzliche Hilfe über die Iothub Explorer-Befehle und deren Parameter:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Verwenden Sie Azure-Portal, um Ihre Ressourcen verwalten

In diesen Lektionen wird CLI erleben zum Erstellen und verwalten Sie Ihre Azure Ressourcen bereitgestellt. Sie möchten auch [Azure-Portal](../azure-portal-overview.md) Hilfe bereitstellen, verwalten und Debuggen Ihrer Azure Ressourcen verwenden.

## <a name="azure-storage-issues"></a>Azure-Speicher Probleme

[Microsoft Azure Storage Explorer (Vorschau)](http://storageexplorer.com) ist eine eigenständige Anwendung von Microsoft, die mit Azure-Speicher unter Windows, Mac OS und Linux ermöglicht. Dieses Tool ermöglicht das Verbinden der Tabelle und Daten. Dieses Tool können Sie Ihre Azure Storage-Problemen.
