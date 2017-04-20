<properties
    pageTitle="Einrichten von Endpunkten in einer klassischen Windows-VM | Microsoft Azure"
    description="Informationen Sie zum Endpunkte für einen virtuellen Computer Windows Azure-Verwaltungsportal einrichten, die Kommunikation mit einem Windows-Computer in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Endpunkte auf einem klassischen Windows-Computer in Azure einrichten


Alle Fenster automatisch können virtuelle Computer, die in Azure mit klassischen Bereitstellungsmodell erstellen kommunizieren über ein privates Netzwerkkanal mit anderen virtuellen Computern in der gleichen Cloud-Dienst oder virtuelles Netzwerk. Computer im Internet oder andere virtuelle Netzwerke erfordern jedoch Endpunkte den eingehenden Netzwerkverkehr auf einem virtuellen Computer direkt. Dieser Artikel steht auch für [virtuelle Linux-Computer](virtual-machines-linux-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]In der **Ressourcen-Manager** -Bereitstellungsmodell werden Endpunkte konfiguriert **Netzwerk**Sicherheitsgruppen (NSGs). Weitere Informationen finden Sie unter [externen Zugriff der Azure-Portal mit VM] (virtual-machines-windows-nsg-quickstart-portal.md).

Beim Erstellen einer virtuellen Windows-Maschine im klassischen Azure-Portal werden gemeinsame Endpunkte wie Remotedesktop und Windows PowerShell-Remoting normalerweise automatisch für Sie erstellt. Sie können beim Erstellen des virtuellen Computers oder nach Bedarf zusätzliche Endpunkte konfigurieren.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Nächste Schritte

* Ein Azure PowerShell-Cmdlet zum Einrichten eines VM-Endpunkts finden Sie unter [Hinzufügen AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Ein Azure PowerShell-Cmdlet zu einer ACL für einen Endpunkt finden Sie unter [Verwalten von Zugriffssteuerungslisten (ACLs) für Endpunkte mithilfe von PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Wenn Sie einen virtuellen Computer in der Ressourcen-Manager-Bereitstellungsmodell erstellt, können Sie Azure PowerShell Steuerelement Datenverkehr an die VM [netzwerksicherheitsgruppen erstellen](../virtual-network/virtual-networks-create-nsg-arm-ps.md) .
