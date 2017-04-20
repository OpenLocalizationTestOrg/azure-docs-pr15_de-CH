<properties
 pageTitle="HPC Pack Compute Clusterknoten verwalten | Microsoft Azure"
 description="PowerShell-Skript Tools hinzufügen, entfernen, starten und Beenden von HPC Pack Compute Clusterknoten in Azure erfahren"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Verwalten Sie Anzahl und Verfügbarkeit von Compute-Knoten in einem Cluster HPC Pack in Azure

Erstellung ein Clusters HPC Pack in Azure VMs sollten Sie problemlos hinzufügen, entfernen, starten (bereitstellen) bzw. stoppen (Identitätsintegrationsprodukts) eine Anzahl von Compute-Knoten VMs im Cluster. Um diese Aufgaben führen Sie Azure PowerShell-Skripts aus, die auf dem Head-Knoten VM installiert. Diese Skripts können Sie die Anzahl und Verfügbarkeit der Clusterressourcen HPC Pack steuern, Kosten steuern können.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack Cluster in Azure VMs** - mithilfe ein Clusters HPC Pack im klassischen Bereitstellungsmodell mindestens HPC Pack 2012 R2 Update 1. Beispielsweise können Sie die Bereitstellung automatisieren, mit der aktuellen HPC Pack VM Azure und Azure PowerShell-Skript. Informationen und Komponenten finden Sie unter [Erstellen einer HPC-Cluster mit HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Nach der Bereitstellung, suchen Sie den Knoten Verwaltungsskripts % CCP\_HOME % Bin auf dem Head-Knoten. Sie müssen jedes Skripts als Administrator ausführen.

* **Azure veröffentlichen Settings Datei oder Management** - dazu eine der folgenden auf dem Head-Knoten:

    * **Importieren der Azure veröffentlichen Einstellungsdatei**. Führen Sie hierzu die folgenden Azure PowerShell-Cmdlets auf dem Head-Knoten:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Konfigurieren des Zertifikats Azure Management auf dem Head-Knoten**. Haben Sie die CER-Datei im CurrentUser\My-Zertifikatspeicher importieren und führen Sie dann folgende Azure PowerShell-Cmdlet für Ihre Azure-Umgebung (AzureCloud oder AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Hinzufügen von Compute-Knoten VMs

Hinzufügen von Compute-Knoten mit dem Skript **HpcIaaSNode.ps1 hinzufügen** .

### <a name="syntax"></a>Syntax
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parameter

* **Dienstname** - Name der Cloud service, neue Computeknoten VMs hinzugefügt.

* **ImageName** - Azure VM Bildname Azure-Verwaltungsportal oder Azure PowerShell-Cmdlet **Get-AzureVMImage**gewonnen. Das Bild muss Folgendes erfüllen:

    1. Ein Windows-Betriebssystem installiert.

    2. HPC Pack muss in der Compute-Knoten Rolle installiert werden.

    3. Das Bild muss eine private Bild in der Kategorie Benutzer nicht öffentlichen Azure VM-Image.

* **Menge** - Anzahl von Computeknoten VMs hinzugefügt werden.

* **InstanceSize** - Größe von Compute-Knoten VMs.

* **DomainUserName** - Domänenbenutzername, der verwendet wird, um neue virtuelle Computer der Domäne hinzuzufügen.

* **DomainUserPassword** - Kennwort des Domänenbenutzers.

* **NodeNameSeries** (optional) – benennen Muster für den Compute-Knoten. Muss das Format &lt; *Root\_Name*&gt;&lt;*Start\_Nummer*&gt;%. Beispielsweise den Namen MyCN % 10 % bedeutet, dass eine Reihe von Compute-Knoten MyCN11 ab. Wenn nicht angegeben, verwendet das Skript den Benennung Serie im HPC-Cluster konfigurierten Knoten.

### <a name="example"></a>Beispiel

Im folgende Beispiel fügt 20 Größe großer Computeknoten VMs in der Cloud-Dienst *hpcservice1*, basierend auf der VM Image *hpccnimage1*.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Computeknoten VMs entfernen

Entfernen Sie Compute-Knoten mit dem Skript **Entfernen HpcIaaSNode.ps1**

### <a name="syntax"></a>Syntax

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parameter

* **Name** - Namen der Cluster-Knoten entfernt werden. Platzhalter werden unterstützt. Der Name des Parameters ist Name. Sie können nicht die **Namen** und **Knoten** -Parameter angeben.

* **Knoten** - das HpcNode-Objekt für die Knoten entfernt werden, bis das HPC-PowerShell-Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)gewonnen. Der Name des Parameters ist Knoten. Sie können nicht die **Namen** und **Knoten** -Parameter angeben.

* **DeleteVHD** (optional) – auf die zugeordneten Laufwerke für die virtuellen Computer löschen, die entfernt werden.

* **Erzwingen** (optional) – auf HPC-Knoten offline erzwingen, bevor Sie diese entfernen.

* **Bestätigen** (optional) - Aufforderung zur Bestätigung vor der Ausführung.

* **WhatIf** - beschreiben, was passiert, wenn der Befehl ausgeführt, ohne tatsächlich Befehl festlegen.

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird offline Knoten mit Namen *HPCNode-CN -* und die Knoten und die zugeordneten Laufwerke entfernt.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Compute-Knoten VMs zu starten

Starten Sie Compute-Knoten mit dem **Start-HpcIaaSNode.ps1** Skript.

### <a name="syntax"></a>Syntax

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parameter

* **Name** - Namen der Clusterknoten gestartet werden. Platzhalter werden unterstützt. Der Name des Parameters ist Name. Sie können nicht die **Namen** und **Knoten** -Parameter angeben.

* **Knoten**- das HpcNode-Objekt für die Knoten gestartet werden, bis das HPC-PowerShell-Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)gewonnen. Der Name des Parameters ist Knoten. Sie können nicht die **Namen** und **Knoten** -Parameter angeben.

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird Knoten mit Namen *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Compute-Knoten VMs beenden

Beenden Sie Compute-Knoten mit dem **Stop-HpcIaaSNode.ps1** Skript.

### <a name="syntax"></a>Syntax

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parameter


* **Name**- Namen der Clusterknoten beendet werden. Platzhalter werden unterstützt. Der Name des Parameters ist Name. Sie können nicht die **Namen** und **Knoten** -Parameter angeben.

* **Knoten** - das HpcNode-Objekt für die Knoten angehalten werden, bis das HPC-PowerShell-Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)gewonnen. Der Name des Parameters ist Knoten. Sie können nicht die **Namen** und **Knoten** -Parameter angeben.

* **Erzwingen** (optional) – auf offline-HPC Knoten vor dem Beenden erzwingen.

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird offline Knoten mit Namen *HPCNode-CN -* und dann die Knoten.

Stop-HPCIaaSNode.ps1-Namen-HPCNodeCN-*-Kraft

## <a name="next-steps"></a>Nächste Schritte

* Sie können automatisch vergrößert oder verkleinert die Clusterknoten nach aktuellen Arbeitslast von Aufträgen und Aufgaben des Clusters finden Sie unter [automatisch vergrößert und verkleinert die Clusterressourcen HPC Pack in Azure nach Arbeitslast Cluster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
