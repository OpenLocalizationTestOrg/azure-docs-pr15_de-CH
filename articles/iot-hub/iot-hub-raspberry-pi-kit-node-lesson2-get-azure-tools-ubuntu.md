<properties
 pageTitle="Abrufen von Azure Tools (Ubuntu 16.04) | Microsoft Azure"
 description="Installieren Sie Python und Azure Befehlszeilen-Schnittstelle (Azure CLI) on Ubuntu."
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

# <a name="21-get-azure-tools-ubuntu-1604"></a>2.1 erhalten Sie Azure Tools (Ubuntu 16.04)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [MacOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 Was Sie tun

Installieren die Azure Befehlszeilenschnittstelle (CLI Azure). Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 Lerninhalte

- Azure-CLI installieren
- IoT Untergruppe der Azure-CLI hinzufügen

## <a name="213-what-you-need"></a>2.1.3 benötigen Sie

- Ubuntu-Rechner mit Internetzugang
- Ein aktives Azure-Abonnement. Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) in wenigen Minuten erstellen.

## <a name="214-install-the-azure-cli"></a>2.1.4 Azure CLI installieren

Azure-CLI bietet für Befehlszeilen für Azure können direkt von der Befehlszeile aus bereitstellen und Verwalten von Ressourcen. 

Gehen Sie folgendermaßen vor, um die neuesten Azure CLI installieren:

1. Führen Sie die folgenden Befehle in einem Terminalfenster. Es dauert fünf Minuten Azure CLI installieren.

    ```bash
    sudo apt-get update
    sudo apt-get install -y libssl-dev libffi-dev
    sudo apt-get install -y python-dev
    sudo apt-get install -y build-essential
    sudo apt-get install -y python-pip
    sudo pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. Überprüfen Sie die Installation durch Ausführen des folgenden Befehls:

    ```bash
    az iot -h
    ```

Die folgende Ausgabe sollte angezeigt werden, wenn die Installation erfolgreich war.

![AZ Iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="215-summary"></a>2.1.5 Zusammenfassung

Sie haben Azure CLI installiert. Weiter zum nächsten Abschnitt zu Ihrer Identität Azure IoT Hub und Gerät mithilfe der Azure-CLI.

## <a name="next-steps"></a>Nächste Schritte

[2.2 Erstellen Sie IoT Hub und registrieren Sie Ihre Himbeeren Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
