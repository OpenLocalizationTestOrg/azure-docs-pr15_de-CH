<properties 
    pageTitle="Virtuelle Windows-Maschinen erneut | Microsoft Azure" 
    description="Beschreibt, wie Windows virtuelle Maschinen RDP-Verbindung Probleme erneut bereitstellen." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Virtuelle Computer neue Azure Knoten erneut

Wenn Sie Problemen konfrontiert sind kann die Problembehandlung Remote Desktop (RDP) Verbindung oder Anwendung Zugriff auf Windows-basierten Azure Virtual Machine (VM) Erneutes VM helfen. Wenn eine VM erneut die VM auf einen neuen Knoten in der Azure-Infrastruktur bewegt und dann wieder auf die Konfigurationsoptionen und zugehörigen Ressourcen beibehalten wird. Dieser Artikel veranschaulicht das eine VM mit Azure PowerShell oder Azure-Portal erneut.

> [AZURE.NOTE] Nachdem eine VM erneut die temporäre Diskette verloren und dynamische IP-Adressen virtuelle Netzwerkschnittstelle zugeordnet werden. 

## <a name="using-azure-powershell"></a>Mithilfe von Azure PowerShell

Stellen Sie sicher, dass die neuesten Azure PowerShell 1.x auf dem Computer installiert. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Verwenden Sie diesen Befehl Azure PowerShell auf dem virtuellen Computer erneut:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Nächste Schritte
Bei Problemen mit Ihrer VM finden Sie Hilfe [zur Problembehandlung RDP-Verbindungen](virtual-machines-windows-troubleshoot-rdp-connection.md) oder [detaillierte Problembehandlungsschritte RDP](virtual-machines-windows-detailed-troubleshoot-rdp.md). Wenn Sie eine Anwendung auf Ihrem virtuellen Computer zugreifen können, können auch [Probleme der Anwendung](virtual-machines-windows-troubleshoot-app-connection.md)lesen.