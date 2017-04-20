<properties
 pageTitle="Erste Schritte mit Himbeeren Pi 3 | Microsoft Azure"
 description="Erste Schritte mit Himbeeren Pi 3 Ihre Azure IoT Hub erstellen und Pi Verbindung IoT hub"
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

# <a name="get-started-with-raspberry-pi-3"></a>Erste Schritte mit Himbeeren Pi 3

Sie beginnen in diesem Lernprogramm lernen Sie die Grundlagen der Verwendung von Himbeeren Pi 3, ausgeführten Raspbian. Sie lernen dann Geräte nahtlos in die Cloud mit [Azure IoT Hub](iot-hub-what-is-iot-hub.md)verbinden. Windows 10 IoT Proben besuchen Sie [windowsondevices.com](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>Lektion 1: Konfigurieren Sie das Gerät

![Lektion 1 E2E-Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

In dieser Lektion Konfigurieren Sie das Gerät Himbeeren Pi 3 mit einem Betriebssystem, Umgebung einrichten und Bereitstellen einer Anwendung auf die Pi.

### <a name="configure-your-device"></a>Konfigurieren Sie das Gerät

Konfigurieren Sie Ihre Himbeeren Pi 3 für die erstmalige Verwendung und installieren Sie des Betriebssystems Raspbian ein freies Betriebssystem Himbeeren Pi Hardware optimiert ist.

*Geschätzte Zeit: 30 Minuten* 

[Gehe zu 'die Konfiguration'](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>Die Tools
Herunterladen Sie Tools und Software erstellen und Bereitstellen der ersten Anwendung Himbeeren Pi 3

*Geschätzte Zeit: 20 Minuten* 

[Gehe zu "Tools"](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Erstellen und Bereitstellen der Anwendungdes blinken

Klonen Sie Beispiel Node.js Anwendung von Github und Bereitstellung dieser Anwendung auf Ihr Mainboard Himbeeren Pi 3 schlucken. Diese Anwendung blinkt die LED alle zwei Sekunden an das Board verbunden.

*Geschätzte Zeit: 5 Minuten* 

[Klicken Sie auf ' erstellen und Bereitstellen der Anwendung blinken "](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>Lektion 2: Erstellen des IoT Hubs

![Lektion 2 E2E-Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

In dieser Lektion erstellen Sie ein kostenlose Azure-Konto Sie, Ihre Azure IoT Hub bereitstellen und das erste Gerät in Azure IoT Hub erstellen.

Abzuschließen Sie Lektion 1, bevor Sie diese Lektion starten.

### <a name="get-the-azure-tools"></a>Die Azure Tools

Installieren von Azure Befehlszeilenschnittstelle (CLI Azure).

*Geschätzte Zeit: 10 Minuten* 

[Gehe zu 'Get Azure tools](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>Erstellen Sie IoT Hub und registrieren Sie Ihre Himbeeren Pi 3

Die Ressourcengruppe erstellen, Ihre ersten Azure IoT Hub bereitstellen und Azure IoT Hub Azure CLI mit das erste Gerät hinzufügen. 

*Geschätzte Zeit: 10 Minuten* 

[Gehe zu 'IoT Hub erstellen und registrieren Sie Ihre Himbeeren Pi 3'](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>Lektion 3: Nachrichten Sie Gerät Cloud-

![Lektion 3 E2E-Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

In dieser Lektion senden Sie Nachrichten an IoT Hub von Pi. Sie erstellen außerdem eine Azure Funktion app, die nimmt Nachrichten aus Ihrem IoT Hub und schreibt sie in Azure-Tabellenspeicher.

Führen Sie vor Beginn dieser Lektion Lektionen 1 und 2.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Erstellen Sie ein Azure-Funktion app und Azure Storage-Konto

Verwenden einer Vorlage Ressourcenmanager Azure Azure Funktion app und Azure Storage-Konto erstellen.

*Geschätzte Zeit: 10 Minuten* 

["Erstellen Sie eine Azure Funktion app und Azure Storage-Konto"](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Führen Sie beispielanwendung Gerät Cloud-Nachrichten aus

Bereitstellen und Ausführen einer beispielanwendung Geräts Himbeeren Pi 3, die Nachrichten an IoT Hub sendet.

*Geschätzte Zeit: 10 Minuten* 

[Gehe zu "beispielanwendung Gerät Cloud-Nachrichten ausführen"](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Gelesene Nachrichten beibehalten in Azure-Speicher
Schreiben in Azure Storage Gerät Cloud-Nachrichten zu überwachen.

*Geschätzte Zeit: 5 Minuten* 

[Gehe zu "gelesene Nachrichten in Azure-Speicher beibehalten"](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>Lektion 4: Cloud Gerät senden

![Lektion 4 E2E-Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

In dieser Lektion demos wie Ihre Himbeeren Pi 3 Nachrichten Ihre Azure IoT Hub an. Nachrichten auf und Steuern von Verhalten der LED an der Pi. Eine Beispiel-Anwendung wird diese Aufgabe vorbereitet.

Vollständige Lektionen 1, 2 und 3, bevor Sie diese Lektion starten.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Führen Sie die beispielanwendung Cloud-zu-Gerät-Nachrichten empfangen

Beispiel-Anwendung in Lektion 4 auf Pi und überwacht eingehende Nachrichten aus Ihrem IoT Hub. Eine neue Aufgabe schlucken sendet Nachrichten an Pi Ihre IoT Hub die LED blinkt.

*Geschätzte Zeit: 10 Minuten* 

[Gehe zum beispielanwendung Cloud-zu-Gerät-Nachrichten empfangen "ausführen"](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Optionaler Abschnitt: ein- und Ausschalten der LED-Verhalten zu ändern

Anpassen der Nachrichten der LEDS ein- und ausschalten Verhalten ändern.

*Geschätzte Zeit: 10 Minuten* 

[Klicken Sie auf ' Optionaler Abschnitt: ein- und Ausschalten der LED-Verhalten ändern "](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie Probleme im Unterricht entsprechen, können Sie Lösungen auf dieser Seite suchen.

[Gehen Sie zu "Fehlerbehebung"](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
