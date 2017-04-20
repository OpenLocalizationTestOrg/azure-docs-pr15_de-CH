<properties
    pageTitle="Bereitstellen eine Anwendung auf virtuellen Skalierung | Microsoft Azure"
    description="Bereitstellen einer Anwendung auf virtuellen skalieren"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="upgrade-a-virtual-machine-scale-set"></a>Aktualisieren einer virtuellen Skalierung festlegen

Dieser Artikel beschreibt, wie Sie für eine Azure Virtual Machine Maßstab ohne Ausfallzeit OS Update bereitstellen können. In diesem Kontext umfasst ein Betriebssystem-Update Version oder SKU des Betriebssystems ändern oder den URI eines benutzerdefinierten Bildes ändern. Aktualisierung ohne Ausfallzeiten virtuelle Computer ein gleichzeitig oder in Gruppen (z. B. eine Fehlerdomäne gleichzeitig) aktualisieren. Damit kann alle virtuellen Computer, die nicht aktualisiert werden weiterhin ausgeführt.

Um Mehrdeutigkeiten zu vermeiden, wir unterscheiden drei OS Update durchführen möchten:

- Ändern der Version oder SKU des Plattform-Images. Ändern Sie z. B. Ubuntu Version von 14.04.201506100, 14.04.201507060, 14.04.2-LTS oder Ubuntu 15.10/letzte SKU 16.04.0-LTS/latest. Dieses Szenario wird in diesem Artikel behandelt.

- Ändern den URI, der auf eine neue Version eines benutzerdefinierten Bildes zeigt erstellt (**Eigenschaften** > **VirtualMachineProfile** > **StorageProfile** > **OsDisk** > **Bild** > **Uri**). Dieses Szenario wird in diesem Artikel behandelt.

- Das Betriebssystem innerhalb eines virtuellen Computers Patches (Beispiele hierfür sind einen Sicherheitspatch installieren und Ausführen von Windows Update). Dieses Szenario wird unterstützt, aber in diesem Artikel nicht behandelt.

Die ersten beiden Optionen sind unterstützte Vorschriften dieses Artikels. Sie müssen einen neuen Maßstab auszuführenden dritte Option erstellen.

Virtual Machine Maßstab legt, die als Teil eines Clusters [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) bereitgestellt werden hier nicht behandelt.

Grundlegende Reihenfolge zum Ändern des OS Version/SKU des Plattform-Images oder URI ein benutzerdefiniertes Bild sieht wie folgt aus:

1. Modell Satz virtuellen Computer zu erhalten.

2. Version, SKU oder URI-Wert im Modell ändern.

3. Aktualisieren Sie das Modell.

4. Ein *ManualUpgrade* rufen auf den virtuellen Computern in der Skalierungsgruppe. Dieser Schritt ist nur relevant, wenn *UpgradePolicy* in der Skala auf **manuell** festgelegt ist. Wenn auf **Automatic**festgelegt ist, werden alle virtuellen Computer, wodurch Ausfallzeiten aktualisiert.


Diese Hintergrundinformationen Beachten Sie sehen wie die Version der Maßstab in PowerShell und mithilfe der REST-API aktualisiert werden konnte. Beispiele decken bei einem Plattform-Image, aber dieser Artikel enthält ausreichend Informationen zur Anpassung des Prozesses ein benutzerdefiniertes Bild.

## <a name="powershell"></a>PowerShell##

In diesem Beispiel wird einen Maßstab Windows virtuellen Computer auf die neue Version 4.0.20160229 aktualisiert. Nach der Aktualisierung des Modells erfolgt eine einer virtuellen Instanz gleichzeitig.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Wenn Sie den URI für ein benutzerdefiniertes Abbild statt eine Plattform-Image-Version aktualisieren, ersetzen Sie die Zeile "legen Sie die neue Version" Folgendes:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>Die REST-API

Hier werden Python-Beispiele, mit denen die REST-API von Azure OS Version Update wiederherstellen. Beide verwenden einfache [Azurerm](https://pypi.python.org/pypi/azurerm) Bibliothek Azure REST API Wrapperfunktionen GET Set Modell, gefolgt von PUT mit einem aktualisierten Modell. Sie sehen auch auf virtuellen Instanzen zu den virtuellen Computern aktualisieren.

### <a name="vmssupgrade"></a>Vmssupgrade

 [Vmssupgrade](https://github.com/gbowerman/vmsstools) ist eine Python-Skript, mit dem ein einen ausgeführten virtuellen Computer Maßstab Rollout, einer Domäne aktualisieren gleichzeitig.

![Vmssupgrade Skript virtuellen Computer oder eine Domäne aktualisieren](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Dieses Skript können Sie bestimmte virtuelle Computer aktualisieren oder geben Sie eine Domäne aktualisieren. Ändern einer Plattform-Image-Version oder den URI eines benutzerdefinierten Bildes unterstützt.

### <a name="vmsseditor"></a>Vmsseditor

[Vmsseditor](https://github.com/gbowerman/vmssdashboard) ist eine allgemeine virtuellen Maßstab legt, die VM-Status als ein Heatmap zeigt eine Zeile darstellt, in dem eine Domäne aktualisieren. Unter anderem können Sie aktualisieren Sie das Modell für eine Skala mit einer neuen Version, SKU oder benutzerdefiniertes Bild URI und Fehlerdomänen aktualisieren wählen. Wenn Sie dies tun, werden alle virtuellen Computer in dieser Domäne aktualisieren auf das neue Modell aktualisiert. Alternativ können Sie ein paralleles Update basierend auf die Batchgröße Ihrer Wahl tun.  

Der folgende Screenshot zeigt ein Modell einen Maßstab für Ubuntu 14.04-2LTS Version 14.04.201507060. Viele weitere Optionen wurden seit diesem Screenshot Tools hinzugefügt.

![Vmsseditor Modell einen Maßstab für Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Klicken Sie auf **Aktualisieren** und dann **Details abrufen**, virtuelle Computer in UD 0 starten.

![Vmsseditor mit aktualisiert](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)
