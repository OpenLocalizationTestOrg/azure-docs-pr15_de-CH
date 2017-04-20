<properties
 pageTitle="Die Tools (MacOS 10.10) | Microsoft Azure"
 description="Downloaden Sie und installieren Sie die erforderlichen Tools und Software für die erste beispielanwendung für Pi auf MacOS."
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

# <a name="12-get-the-tools-macos-1010"></a>1.2 erhalten Sie Tools (MacOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [MacOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 Was Sie tun

Entwicklungs-Tools und Software für die erste beispielanwendung für Ihre Himbeeren Pi 3 herunterladen Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 Lerninhalte
In diesem Abschnitt lernen Sie:

- Git und Node.js installieren
    - [Git](https://git-scm.com) ist eine verteilte open-Source-Versionskontrollsystem. Beispiel-Anwendung für diese Lektion wird auf Git gespeichert.
    - [Node.js](https://nodejs.org/en/) ist eine JavaScript-Laufzeit ein Ökosystem rich-Paket.
- Wie NPM zusätzliche Node.js-Entwicklungstools installieren.
  - Erforderliche Mindestversion Node.js ist 4,5 LTS.
  - [NPM](https://www.npmjs.com) ist der Paket-Manager für Node.js.

## <a name="123-what-you-need"></a>1.2.3 benötigen Sie

- Eine Verbindung zu den Entwicklungstools und Software herunterladen
- Einen Mac, der MacOS Yosemite (10.10) oder höher

## <a name="124-install-git-and-nodejs"></a>1.2.4 Git und Node.js installieren

Installation von Git und Node.js verwenden Sie [Homebrew](http://brew.sh) Paket-Verwaltungsprogramm folgendermaßen:

1. Installieren Sie Homebrew. Wenn Homebrew bereits installiert haben, fahren Sie mit Schritt 2 fort.
  1. Drücken Sie `Cmd + Space` und `Terminal` um ein Terminalfenster zu öffnen.
  2. Führen Sie den folgenden Befehl ein:

    ```bash
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
2. Installieren Sie Git und Node.js durch Ausführen des folgenden Befehls:

    ```bash
    brew install node git
    ```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 installieren Sie zusätzlicher Node.js-Entwicklungstools

Sie verwenden [gulp.js](http://gulpjs.com) zum Automatisieren der Bereitstellung der beispielanwendung Pi. Sie verwenden auch [Device Discovery Cli](https://github.com/Azure/device-discovery-cli) Netzwerkinformationen zu IoT Geräten abrufen.

Installieren `gulp` und `device-discovery-cli` durch den folgenden Befehl in das Terminal-Fenster ausführen:

```bash
sudo npm install -g device-discovery-cli gulp
```

Treten Probleme beim Installieren von Node.js und diese zusätzliche Entwicklungstools für MacOS finden Sie unter [Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) bei Probleme.

## <a name="126-install-visual-studio-code"></a>1.2.6 installieren von Visual Studio Code

[Downloaden](https://code.visualstudio.com/docs/setup/osx) und installieren Sie Visual Studio Code. Visual Studio-Code ist eine einfache und dennoch leistungsfähige Quellcode-Editor für Windows, Linux und Mac OS. Später im Lernprogramm verwenden Sie diesen Editor, um den Beispielcode zu bearbeiten.

## <a name="127-summary"></a>1.2.7-Zusammenfassung

Sie haben die erforderliche Entwicklungstools und Software für das erste beispielanwendung installiert. Im nächsten Abschnitt erstellen, bereitstellen und beispielanwendung auf die Pi ausführen.

## <a name="next-steps"></a>Nächste Schritte

[1.3 erstellen und Bereitstellen der Anwendung Blink-Beispiel](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
