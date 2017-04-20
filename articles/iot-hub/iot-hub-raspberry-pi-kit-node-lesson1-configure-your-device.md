<properties
 pageTitle="Konfigurieren Sie das Gerät | Microsoft Azure"
 description="Konfigurieren Sie Ihre Himbeeren Pi 3 für die erstmalige Verwendung und installieren Sie des Betriebssystems Raspbian ein freies Betriebssystem Himbeeren Pi Hardware optimiert ist."
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

# <a name="11-configure-your-device"></a>1.1 Konfigurieren Sie 1.1 das Gerät

## <a name="111-what-you-will-do"></a>1.1.1 Was Sie tun

Konfigurieren Sie Pi für die erstmalige Verwendung und installieren Sie des Betriebssystems Raspbian ein freies Betriebssystem Himbeeren Pi Hardware optimiert ist. Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="112-what-you-will-learn"></a>1.1.2 Lerninhalte

In diesem Abschnitt lernen Sie:

- Installieren Sie Raspbian auf Pi
- So schalten Sie ein USB-Kabel mit Pi
- Pi Netzwerk über ein Ethernet-Kabel oder WLAN-Verbindung
- Eine LED auf dem steckbrett hinzufügen und Verbinden mit Pi

## <a name="113-what-you-need"></a>1.1.3 benötigen Sie

Zum Abschluss dieses Abschnitts benötigen Sie folgende Teile aus der Himbeeren Pi 3 Starter Kit:

- Himbeeren Pi 3 board
- 16GB MicroSD Karte
- 5V 2A Stromversorgung mit sechs Fuß micro USB-Kabel
- Die steckbrett
- Connector-Kabel
- 560 Ohm Widerstand
- Ein Diffuses 10mm LED
- Das Ethernet-Kabel

![Bezüglich des Starter Kits](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Außerdem müssen Sie:

- Verkabelte oder drahtlose Verbindung für die Verbindung mit Pi
- Ein USB-Adapter oder Mini-SD-Speicherkarte Betriebssystemabbild in die MicroSD-Karte zu brennen.
- Ein Computer unter Windows, Mac und Linux. Der Computer wird zum Raspbian auf der MicroSD-Karte installieren.
- Eine Internet-Verbindung die erforderlichen Tools und Software herunterladen

## <a name="114-install-raspbian-on-the-microsd-card"></a>1.1.4 Raspbian auf die MicroSD-Karte installieren

Vorbereiten der MicroSD Karte Raspbian Bild zu schreiben.

1. Raspbian herunterladen
  1. [Download](https://www.raspberrypi.org/downloads/raspbian/) der Zip-Datei für Raspbian Jessie Pixel.
  2. Raspbian Bild in einen Ordner auf Ihrem Computer zu extrahieren.
2. Installieren Sie Raspbian MicroSD-Karte.
  1. [Herunterladen](https://www.etcher.io) und Installieren der Radierer SD-Brenner Dienstprogramm.
  2. Führen Sie Radierer und wählen Sie in Schritt 1 Raspbian-Bild, das Sie extrahiert.
  3. Wählen Sie das MicroSD kartenlaufwerk.
    Hinweis: Radierer möglicherweise bereits das richtige Laufwerk ausgewählt.
  4. Klicken Sie auf Flash Raspbian MicroSD-Karte installieren.
  5. Entfernen Sie die Karte MicroSD von Ihrem Computer abgeschlossen.
    Hinweis: Es ist sicher MicroSD Karte direkt entfernen, da Radierer automatisch ausgeworfen oder die MicroSD-Karte nach Abschluss überschreibt.
  6. Legen Sie die MicroSD-Karte in Pi.

![Die SD-Karte](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="115-power-on-your-pi"></a>1.1.5 schalten Pi

Schalten Sie die Pi mit micro USB-Kabel und das Netzteil.

![Einschalten](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [AZURE.NOTE] Muss das Netzteil im Kit verwenden mindestens 2A sicherstellen, dass Ihre Himbeeren mit ausreichend Strom ordnungsgemäß eingezogen wird.

## <a name="116-connect-your-raspberry-pi-3-to-the-network"></a>1.1.6 verbinden Sie die Himbeeren Pi 3 mit dem Netzwerk

Sie können Ihre Pi an ein verkabeltes Netzwerk oder einem drahtlosen Netzwerk verbinden. Stellen Sie sicher, dass Pi mit demselben Netzwerk wie Ihr Computer verbunden ist. Beispielsweise können Sie die Pi mit demselben Switch verbinden, denen mit Ihrem Computer verbunden ist.

### <a name="1161-connect-to-a-wired-network"></a>1.1.6.1 mit einem verkabelten Netzwerk verbinden

Verwenden Sie das Ethernet-Kabel an Ihr drahtgebundenes Netzwerk Pi Verbindung. Zwei LEDs auf Pi aktivieren, wenn die Verbindung hergestellt wird.

![Verbindung über Ethernet-Kabel](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="1162-connect-to-a-wireless-network"></a>1.1.6.2 mit einem drahtlosen Netzwerk verbinden

Folgen Sie der [Anleitung](https://www.raspberrypi.org/learning/software-guide/wifi/) Himbeeren Pi Foundation Pi zu Ihrem drahtlosen Netzwerk herstellen. Lernen müssen Sie zuerst einen Monitor und eine Tastatur mit Pi verbinden.

## <a name="117-connect-the-led-to-your-pi"></a>1.1.7 verbinden Sie die LED mit Pi

Verwenden Sie diese Aufgabe [steckbrett](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), Connector Drähte, die LED und der Widerstand. Hierfür verbinden sie [Allgemeine Eingabe/Ausgabe](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) Ports Pi. 

![Steckbrett-LED und Widerstand](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Kürzere Bein der LED **GPIO**-Gnd (Pin 6) anschließen
2. Längere Bein der LED an einem Bein Widerstand anschließen
3. Verbinden des Abschnitts Widerstand **GPIO**4 (Pin 7).

Beachten Sie, dass die LED Polarität wichtig. Diese Einstellung Polarität ist bekannt als aktiv niedrig.

![Belegung](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Herzlichen Glückwunsch! Sie haben erfolgreich Ihre Pi konfiguriert.

## <a name="118-summary"></a>1.1.8 Zusammenfassung

In diesem Abschnitt haben Sie Pi konfigurieren Raspbian installieren, Pi mit einem Netzwerk verbinden und Pi LED mit erfahren. Beachten Sie, dass die LED noch nicht leuchtet. Im nächsten Abschnitt Installieren Sie die erforderlichen Tools und Software zum Ausführen einer beispielanwendung auf Ihrem Pi.

![Hardware ist bereit](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Nächste Schritte

[1.2 erhalten Sie tools](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
