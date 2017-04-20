<properties
    pageTitle="Erstellen einer Linux VM | Microsoft Azure"
    description="So erstellen Sie einen benutzerdefinierten virtuellen Computer mit dem klassischen Bereitstellungsmodell des Linux-Betriebssystems."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-create-a-custom-linux-vm"></a>Erstellen eine benutzerdefinierte Linux VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Version finden Sie [hier](virtual-machines-linux-create-cli-complete.md).

Dieses Thema beschreibt benutzerdefinierten virtuellen Computer (VMs) erstellen mit der Azure-CLI mit klassischen Bereitstellungsmodell. Wir verwenden ein Linux-Image aus verfügbaren **Bilder** in Azure. Azure-CLI-Befehle geben die folgenden Konfigurationsoptionen, unter anderem:

- Die VM Verbinden mit einem virtuellen Netzwerk
- Einen vorhandenen Cloud-Dienst hinzufügen die VM
- Die VM hinzufügen ein Storage-Konto
- Die VM hinzufügen eine Verfügbarkeit oder Speicherort

> [AZURE.IMPORTANT] Soll der virtuelle Computer ein virtuelles Netzwerk verwenden Sie Hostnamen oder einrichten standortübergreifende Verbindung herstellen können, stellen Sie sicher, dass Sie das virtuelle Netzwerk beim Erstellen des virtuellen Computers. Eine virtuelle Maschine kann konfiguriert werden, um ein virtuelles Netzwerk nur beim Erstellen des virtuellen Computers. Ausführliche Informationen über virtuelle Netzwerke Übersicht [Azure virtuelle Netzwerk](http://go.microsoft.com/fwlink/p/?LinkID=294063).


## <a name="how-to-create-a-linux-virtual-machine-using-the-classic-deployment-model"></a>Zum Erstellen einer virtuellen Linux-Maschine mit klassischen Bereitstellungsmodell

[AZURE.INCLUDE [virtual-machines-create-LinuxVM](../../includes/virtual-machines-create-linuxvm.md)]
