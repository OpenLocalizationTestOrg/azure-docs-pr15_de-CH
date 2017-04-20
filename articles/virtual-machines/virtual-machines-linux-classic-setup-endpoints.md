<properties
    pageTitle="Einrichten von Endpunkten in einer klassischen Linux VM | Microsoft Azure"
    description="Endpunkte für Linux VM im klassischen Azure-Portal einrichten, die Kommunikation mit einem virtuellen Linux-Maschine in Azure erfahren"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Endpunkte einer klassischen virtuellen Linux-Maschine in Azure einrichten

Alle virtuelle Linux-Computer, die in Azure mit klassischen Bereitstellungsmodell erstellen können automatisch über einen Kanal privates Netzwerk mit anderen virtuellen Computern in der gleichen Cloud-Dienst oder virtuelles Netzwerk kommunizieren. Computer im Internet oder andere virtuelle Netzwerke erfordern jedoch Endpunkte den eingehenden Netzwerkverkehr auf einem virtuellen Computer direkt. Dieser Artikel steht auch für [virtuelle Windows-Maschinen](virtual-machines-windows-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]In der **Ressourcen-Manager** -Bereitstellungsmodell werden Endpunkte konfiguriert **Netzwerk**Sicherheitsgruppen (NSGs). Weitere Informationen finden Sie unter [Öffnen von Ports und Endpunkte] (virtual-Maschinen-Linux-Nsg-quickstart.md).

Beim Erstellen einer virtuellen Linux-Maschine im klassischen Azure-Portal wird ein Endpunkt für Secure Shell (SSH) normalerweise automatisch für Sie erstellt. Sie können beim Erstellen des virtuellen Computers oder nach Bedarf zusätzliche Endpunkte konfigurieren.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Nächste Schritte

* VM-Endpunkt können mithilfe der [Azure-Befehlszeilenschnittstelle](../virtual-machines-command-line-tools.md). Führen Sie den Befehl **Azure Vm erstellen** .

* Wenn Sie einen virtuellen Computer in der Ressourcen-Manager-Bereitstellungsmodell erstellt, können Sie Azure-CLI im Ressourcenmanager Modus Kontrolle des Datenverkehrs für die VM [netzwerksicherheitsgruppen erstellen](../virtual-network/virtual-networks-create-nsg-arm-cli.md) .