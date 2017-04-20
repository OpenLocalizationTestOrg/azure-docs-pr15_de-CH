<properties
    pageTitle="Klassische Windows-VM Größe | Microsoft Azure"
    description="Die Größe einer virtuellen Windows-Maschine im klassischen Bereitstellungsmodell mit Azure Powershell erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Ändern der Größe einer klassischen Bereitstellungsmodell erstellt Windows VM

Dieser Artikel beschreibt, wie ein VM Windows Azure Powershell mit klassischen Bereitstellungsmodell erstellt Größe.

Wenn die Möglichkeit eine VM Größe sind zwei Begriffe, die die Größen der virtuellen Maschine Größe steuern. Das erste Konzept ist der Bereich in dem VM bereitgestellt wird. Die Liste der VM Größen im Bereich ist unter der Registerkarte Dienste der Webseite Azure-Regionen. Das zweite Konzept ist die physische Hardware derzeit als Host der VM. Physischen Hostserver VMs werden in Clustern des allgemeinen physischen Hardware zusammengefasst. Die Methode ändern virtueller Speicher unterscheidet sich je nach die gewünschte Größe für neue VM derzeit als Host der VM Hardware-Cluster unterstützt.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Sie können auch die [Größe einer VM in der Ressourcen-Manager-Bereitstellungsmodell erstellt](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Fügen Sie Ihr Konto

Konfigurieren Sie Azure PowerShell mit klassischen Azure Ressourcen arbeiten. Schritte Azure PowerShell zum klassischen Ressourcen konfigurieren.

1. Geben Sie auf Aufforderung PowerShell `Add-AzureAccount` und drücken Sie die **EINGABETASTE**. 
2. Geben Sie die e-Mail-Adresse in Ihrem Azure-Abonnement, und klicken Sie auf **Weiter**. 
3. Geben Sie das Kennwort für Ihr Konto ein. 
4. Klicken Sie auf **Anmelden**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Ändern der Größe eines Hardware-Clusters

Größe eine VM Hardware-Cluster hosten die VM für Größe Schritte.

1. Führen Sie den folgenden PowerShell-Befehl Listen Sie die VM Größen in dem Clouddienst enthält die VM Hardware-Cluster.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Führen Sie die folgenden Befehle die VM Größe.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Ändern der Größe auf eine neue Hardware-cluster

VM nicht verfügbar im Cluster Hardware VM Cloud-hosting Größe Größe müssen Service und alle VMs in der Cloud-Dienst neu. Cloud-Dienst befindet sich in einem einzigen Hardware-Cluster VMs in der Cloud-Dienst müssen eine Größe aufweisen, die in einem Cluster Hardware unterstützt wird. Die folgenden Schritte werden eine VM Größe durch Löschen und Neuerstellen des Cloud-Diensts beschrieben.

1. Führen Sie den folgenden PowerShell-Befehl Listen Sie die VM Größen im Bereich. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Notieren Sie alle Konfigurationen für jede VM im Cloud-Dienst enthält die VM geändert werden kann. 
3. Löschen Sie alle VMs in der Cloud-Dienst die Option Datenträger für jeden virtuellen Computer beibehalten.
4. Erstellen der VM die gewünschte VM Größe geändert werden kann.
5. Erstellen Sie alle VMs in der Cloud-Dienst mit einer VM in der Hardware-Cluster jetzt dem Clouddienst verfügbar.

Beispielskript zum Löschen und Neuerstellen eines Cloud-Diensts mit einer neuen VM-Größe finden Sie [hier](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Nächste Schritte

- Die [Größe eine VM in der Ressourcen-Manager-Bereitstellungsmodell erstellt](virtual-machines-windows-resize-vm.md).