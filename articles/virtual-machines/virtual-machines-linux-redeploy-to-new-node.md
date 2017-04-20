<properties 
    pageTitle="Virtuelle Linux-Computer erneut | Microsoft Azure" 
    description="Beschreibt, wie virtuelle Linux-Computer so SSH-Verbindungsprobleme erneut bereitstellen." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Virtuelle Computer neue Azure Knoten erneut

Haben Sie wurden schwierigkeiten Problembehandlung SSH oder Anwendung auf ein Azure Virtual Machine (VM) einbringt VM kann helfen. Wenn eine VM erneut die VM auf einen neuen Knoten in der Azure-Infrastruktur bewegt und dann wieder auf die Konfigurationsoptionen und zugehörigen Ressourcen beibehalten wird. Dieser Artikel veranschaulicht das eine VM mit Azure-CLI oder Azure-Portal erneut.

> [AZURE.NOTE] Nachdem eine VM erneut die temporäre Diskette verloren und dynamische IP-Adressen virtuelle Netzwerkschnittstelle zugeordnet werden. 


## <a name="using-azure-cli"></a>Mithilfe von Azure CLI

Vergewissern Sie sich die [Neuesten Azure CLI installiert](../xplat-cli-install.md) auf Ihrem Computer haben und Ressourcen-Manager-Modus (`azure config mode arm`).

Verwenden Sie folgenden Azure CLI-Befehl auf dem virtuellen Computer erneut:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Sie sehen den Status der VM-Änderung geht schrittweise bereitstellen. Die `PowerState` der VM wechselt von 'Ausführen' auf 'Aktualisieren', 'Starten' und schließlich 'Running' geht schrittweise auf einen neuen Host einbringt. Überprüfen Sie den Status der VMs innerhalb einer Ressourcengruppe mit:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Nächste Schritte
Bei Problemen mit Ihrer VM finden Sie Hilfe auf [Problembehandlung SSH-Verbindungen](virtual-machines-linux-troubleshoot-ssh-connection.md) oder [detaillierte Problembehandlungsschritte SSH](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md). Wenn Sie eine Anwendung auf Ihrem virtuellen Computer zugreifen können, können auch [Probleme der Anwendung](virtual-machines-linux-troubleshoot-app-connection.md)lesen.