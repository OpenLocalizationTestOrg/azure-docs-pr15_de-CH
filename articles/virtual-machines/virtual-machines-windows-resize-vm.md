<properties
    pageTitle="Fenstergröße VM | Microsoft Azure"
    description="Die Größe einer virtuellen Windows-Maschine im Ressourcen-Manager-Bereitstellungsmodell mit Azure Powershell erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>Fenstergröße VM

Dieser Artikel beschreibt, wie ein VM Windows Azure Powershell mit Ressourcen-Manager-Bereitstellungsmodell erstellt Größe.

Nach der Erstellung einer virtuellen Maschine (VM) können Sie die VM oben oder unten skalieren, durch VM ändern. In einigen Fällen müssen Sie zunächst die VM freigeben. Dies ist die neue Größe nicht auf der Hardware-Cluster, die derzeit die VM befindet.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Ändern der Größe einer Windows-VM nicht in einem Verfügbarkeit

1. Liste der VM-Größen, die auf Hardware-Clusters, der virtuellen Computer gehostet wird. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Wenn die gewünschte Größe aufgeführt ist, führen Sie die folgenden Befehle die VM Größe. Wenn die gewünschte Größe nicht aufgeführt ist, fahren Sie mit Schritt 3 fort.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Wenn die gewünschte Größe nicht aufgeführt ist führen Sie folgende Befehle auf den virtuellen Computer freigeben, Größe und die VM starten.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] Aufheben der VM frei der VM zugewiesenen dynamischen IP-Adressen. Betriebssystem und Daten-Festplatten sind nicht betroffen. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Ändern der Größe einer Windows-VM in einem Verfügbarkeit

Wenn die neue Größe für einen virtuellen Computer in einem Verfügbarkeit nicht auf derzeit als Host der VM Hardware-Cluster verfügbar ist, müssen alle VMs in den Verfügbarkeit Größe die VM freigegeben werden. Sie müssen auch um die Größe der anderen VMs Verfügbarkeit festgelegt, nachdem eine VM geändert wurde. Gehen Sie zum Ändern der Größe einer VM in einem Verfügbarkeit.

1. Liste der VM-Größen, die auf Hardware-Clusters, der virtuellen Computer gehostet wird.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Wenn die gewünschte Größe aufgeführt ist, führen Sie die folgenden Befehle die VM Größe. Wenn es nicht aufgeführt ist, gehen Sie zu Schritt 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Wenn die gewünschte Größe nicht aufgeführt wird, weiterhin mit den folgenden Schritten freigeben VMs in den Verfügbarkeit und Größe VMs neu starten.

4.  Beenden Sie alle virtuellen Computer in den Verfügbarkeit.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Größe und starten Sie die VMs in den Verfügbarkeit.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

- Zusätzliche Skalierbarkeit mehrere VM-Instanzen ausführen und skalieren. Weitere Informationen finden Sie in der [Windows-Computer in eine Virtual Machine Skalierung automatisch zu skalieren](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).



