<properties
    pageTitle="Trennen Sie einen Datenträger aus einem Windows-VM | Microsoft Azure"
    description="Informationen Sie zum Datenträger von einem virtuellen Computer in Azure mit dem Ressourcen-Manager-Bereitstellungsmodell trennen."
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



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Wie Sie einen Datenträger aus einem Windows-Computer trennen


Wenn Sie einen Datenträger, der zu einer virtuellen Maschine zugeordnet ist nicht mehr benötigen, können Sie problemlos trennen. Entfernt den Datenträger aus dem virtuellen Computer dies nicht aus dem Speicher entfernen. 

> [AZURE.WARNING] Wenn Sie ein Laufwerk, nicht automatisch gelöscht trennen. Wenn Sie Premium-Speicher abonniert haben, weiterhin Speicher für den Datenträger anfallen werden. Weitere Informationen finden Sie in [Preise und Abrechnung bei Premium Speicherung](../storage/storage-premium-storage.md#pricing-and-billing). 

Wenn Sie die vorhandenen Daten auf dem Datenträger erneut verwenden möchten, können Sie es auf dem gleichen virtuellen Computer oder eine anfügen.  


## <a name="detach-a-data-disk-using-the-portal"></a>Trennen Sie einen Datenträger mit dem portal

1. Wählen Sie im Portal Hub **virtuellen Maschinen**.

2. Wählen Sie den virtuellen Computer mit dem Datenträger zu trennen und dann auf **Alle**.

3. Wählen Sie im Blatt **Einstellungen** **Festplatten**.

4. Wählen Sie Blatt **Festplatten** Datenträger, den Sie trennen möchten.

5. Blatt für den Datenträger klicken Sie auf **Trennen**.


    ![Screenshot zeigt die Schaltfläche trennen.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

Der Datenträger im Speicher verbleibt jedoch nicht zu einer virtuellen Maschine zugeordnet.


## <a name="detach-a-data-disk-using-powershell"></a>Trennen Sie einen Datenträger mit PowerShell

In diesem Beispiel wird der erste Befehl den virtuellen Computer mit dem Namen **MyVM07** in der **RG11** -Ressourcengruppe mit dem Cmdlet Get-AzureRmVM. Der Befehl den virtuellen Computer in der Variablen **$VirtualMachine** gespeichert. 

Der zweite Befehl entfernt den Datenträger mit der Bezeichnung DataDisk3 auf dem virtuellen Computer. 

Der letzte Befehl aktualisiert den Zustand des virtuellen Computers bis zum Abschluss den Datenträger entfernen.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Weitere Informationen finden Sie unter [Entfernen AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie den Datenträger erneut verwenden möchten, können Sie nur [dann einen anderen VM](virtual-machines-windows-attach-disk-portal.md)
