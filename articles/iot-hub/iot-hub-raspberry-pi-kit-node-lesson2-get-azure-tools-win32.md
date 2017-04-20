<properties
 pageTitle="Abrufen von Azure Tools (Windows 7 +) | Microsoft Azure"
 description="Installieren Sie Python und Azure-Befehlszeilenschnittstelle (CLI Azure) unter Windows 7 und höher."
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

# <a name="21-get-azure-tools-windows-7-"></a>2.1 erhalten Azure Tools (Windows 7 +)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [MacOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 Was Sie tun

Installation von Python und Befehlszeile Azure Schnittstelle (Azure CLI). Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 Lerninhalte

- Python installieren
- Azure-CLI installieren

## <a name="213-what-you-need"></a>2.1.3 benötigen Sie

- Ein Windows-Computer mit Internetzugang
- Ein aktives Azure-Abonnement. Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) in wenigen Minuten erstellen.

## <a name="214-install-python"></a>2.1.4 Python installieren

Installieren Sie Python auf Ihren Computer. Sie können Python 2.7, 3.4 oder 3.5. Dieses Lernprogramm basiert auf Python 2.7. Wenn Python bereits installiert haben, fahren Sie mit Abschnitt 2.1.5.

[Abrufen von Python für Windows](https://www.python.org/downloads/)

Sie müssen auch den Pfad Ordner hinzufügen, python.exe und pip.exe auf dem System installiert `PATH` -Umgebungsvariable. Befindet sich python.exe in `C:\Python27` und pip.exe installiert ist `C:\Python27\Scripts`.

## <a name="215-install-the-azure-cli"></a>2.1.5 Azure CLI installieren

Azure-CLI bietet für Befehlszeilen für Azure können direkt von der Befehlszeile aus bereitstellen und Verwalten von Ressourcen.

Um Azure CLI installieren, gehen Sie folgendermaßen vor:

1. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
2. Führen Sie die folgenden Befehle:

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```
3. Überprüfen Sie die Installation durch Ausführen des folgenden Befehls:

    ```bash
    az iot -h
    ```

Die folgende Ausgabe wird angezeigt, wenn die Installation erfolgreich war.

![AZ Iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="216-summary"></a>2.1.6 Zusammenfassung

Sie haben Azure CLI installiert. Weiter zum nächsten Abschnitt zu Ihrer Identität Azure IoT Hub und Gerät mithilfe der Azure-CLI.

## <a name="next-steps"></a>Nächste Schritte

[2.2 Erstellen Sie IoT Hub und registrieren Sie Ihre Himbeeren Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
