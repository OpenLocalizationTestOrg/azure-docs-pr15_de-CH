<properties
    pageTitle="Erstellen einer VM Verfügbarkeit | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer Verfügbarkeit für Ihre virtuellen Computer mithilfe von Azure-Portal oder mit dem Ressourcen-Manager-Bereitstellungsmodell PowerShell festgelegt."
    keywords="Festlegen der Verfügbarkeit"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


# <a name="create-an-availability-set"></a>Erstellen einer Verfügbarkeit 

Bei Verwendung des Portals soll die VM zu einer müssen Sie die Verfügbarkeit zuerst erstellen.

Weitere Informationen zum Erstellen und verwenden Verfügbarkeit finden Sie unter [Verwalten der Verfügbarkeit virtueller Maschinen](virtual-machines-windows-manage-availability.md).


## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Verwenden Sie das Portal Verfügbarkeit festlegen, bevor Sie Ihre virtuellen Computer erstellen

1. Klicken Sie im Hub auf **Durchsuchen** , und wählen Sie **Verfügbarkeit festgelegt**.

2. **Verfügbarkeit wird Blade**klicken Sie auf **Hinzufügen**.

    ![Screenshot, der zeigt die hinzufügen-Schaltfläche zum Erstellen einer neuen Verfügbarkeit festgelegt.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. Geben Sie die Informationen für Ihre-Blade **Erstellen Verfügbarkeit** .

    ![Screenshot mit den Informationen angezeigt, müssen Sie die EINGABETASTE, um die Verfügbarkeit zu erstellen.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

    - **Name** - der Name sollte 1 bis 80 Zeichen bestehend aus Ziffern, Buchstaben, Punkte, Unterstriche und Bindestriche enthalten. Das erste Zeichen muss ein Buchstabe oder eine Zahl sein. Das letzte Zeichen muss ein Buchstabe, Zahl oder Unterstrich.
    - **Fehlerdomänen** - Fehlerdomänen definieren die Gruppe von virtuellen Computern, die einen gemeinsamen Quelle und Netzwerk Netzschalter freigeben. Standardmäßig die VMs auf bis zu drei Fehlerdomänen getrennt und können zwischen 1 und 3 geändert werden.
    - **Aktualisieren von Domänen** - fünf Domänen zugewiesen sind, und das Update zwischen 1 und 20 einstellbar. Aktualisierungsdomänen geben Gruppen von virtuellen Maschinen und zugrunde liegende physische Hardware gleichzeitig neu gestartet werden kann. Beispielsweise wenn wir fünf Domänen aktualisieren, bei mehr als fünf virtuelle Computer in einer einzigen verfügbaren festlegen angeben, dem sechsten virtuellen Computer in der Domäne aktualisieren den ersten virtuellen Computer der siebte in derselben UD als zweiten virtuellen Computer platziert usw. Die Reihenfolge der Neustarts möglicherweise nicht sequenziell, aber nur ein aktualisieren gleichzeitig gestartet.
    - **Abonnement** - Abonnement, wenn Sie mehrere haben.
    - **Ressourcengruppe** – wählen Sie eine vorhandene Ressourcengruppe auf den Pfeil und wählen eine Ressourcengruppe in der Dropdown unten. Eine neue Ressourcengruppe können durch einen Namen eingeben. Der Name darf keines der folgenden Zeichen enthalten: Buchstaben, Zahlen, Punkte, Bindestriche, Unterstriche und öffnende oder schließende Klammer. Der Name darf nicht mit einem Punkt enden. Alle VMs in der Availability Group in derselben Ressourcengruppe wie die Verfügbarkeit erstellt werden müssen.
    - **Ort** : Wählen Sie einen Speicherort aus der Dropdown-Liste.

4. Wenn die Informationen einzugeben, klicken Sie auf **Erstellen**. Erstellte verfügbarkeitsgruppe finden Sie sie in der Liste nach der Aktualisierung des Portals.

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Verwenden Sie das Portal zum Erstellen einer virtuellen Maschine und Verfügbarkeit gleichzeitig festlegen

Beim Erstellen einer neuen VM über das Portal können Sie auch eine neue Verfügbarkeit festgelegt für die VM erstellt den ersten virtuellen Computer in der Gruppe erstellen.

![Screenshot, der zeigt den Prozess zum Erstellen einer neuen Verfügbarkeit während den virtuellen Computer erstellen.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## <a name="add-a-new-vm-to-an-existing-availability-set"></a>Hinzufügen einer neuen VM eine vorhandene Verfügbarkeit

Sicherstellen Sie für jeden zusätzlichen VM, die Sie erstellen, die in der Gruppe gehören sollen, dass in derselben **Ressourcengruppe** erstellen und Sie dann die Verfügbarkeit in Schritt 3 festgelegt wählen. 

![Screenshot zeigt eine Verfügbarkeit richten für die VM auswählen.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## <a name="use-powershell-to-create-an-availability-set"></a>Mit PowerShell einen Verfügbarkeit erstellen

Dieses Beispiel erstellt eine Verfügbarkeit an **Westen der USA** in der Ressourcengruppe **RMResGroup** festgelegt. Dies muss erfolgen, bevor Sie den ersten virtuellen Computer erstellen, die in den.

    New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
    
Weitere Informationen finden Sie unter [New-AzureRmAvailabilitySet](https://msdn.microsoft.com/library/mt619453.aspx).


## <a name="troubleshooting"></a>Problembehandlung

- Beim Erstellen einer VM nicht gewünschte verfügbarkeitsgruppe in der Dropdown-Liste im Portal kann in einer anderen Ressourcengruppe erstellen. Wenn Sie nicht wissen, Ressourcengruppe für Ihre Verfügbarkeit, Hub Menü und auf Durchsuchen > Verfügbarkeit wird eine Liste Ihrer Verfügbarkeit legt und die Ressourcengruppen an.


## <a name="next-steps"></a>Nächste Schritte

Fügen Sie zusätzlichen Speicher Ihrer VM ein weiterer [Datenträger](virtual-machines-windows-attach-disk-portal.md)hinzufügen hinzu.
