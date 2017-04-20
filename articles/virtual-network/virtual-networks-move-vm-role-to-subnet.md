<properties 
   pageTitle="Eine VM oder Rolle Instanz in ein anderes Subnetz verschieben"
   description="Erfahren Sie, wie VMs und Instanzen in ein anderes Subnetz verschieben"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Eine VM oder Rolle Instanz in ein anderes Subnetz verschieben

PowerShell können Sie Ihre virtuellen Computer in einem Subnetz im gleichen virtuellen Netzwerk (VNet) verschieben. Instanzen können die mithilfe bearbeiten, anstatt PowerShell verschoben werden.

>[AZURE.NOTE] Dieser Artikel enthält Informationen, die im Vergleich zu Azure klassische Bereitstellungen nur.

Gründe für das Verschieben von VMs in ein anderes Subnetz Subnetz Migration ist nützlich, wenn ältere Subnetz zu klein ist und nicht durch vorhandene ausgeführten VMs in diesem Subnet erweitert werden. In diesem Fall erstellen Sie einen neuen Subnetz und VMs für das neue Subnetz migrieren und nach Abschluss der Migration können Sie das alte leere Subnetz.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Einen virtueller Computer in ein anderes Subnetz verschieben

Führen Sie zum Verschieben einer VM Set AzureSubnet PowerShell-Cmdlet, das Beispiel als Vorlage verwenden. Im folgenden Beispiel werden wir TestVM aus seinem Subnetz vorhanden mit Subnetz 2 verschoben. Achten Sie darauf, dass im Beispiel entsprechend Ihrer Umgebung bearbeiten. Beachten Sie, dass bei jedem Ausführen von Update-AzureVM-Cmdlet als Teil einer Prozedur VM als Teil des Aktualisierungsvorgangs neu startet.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Wenn Sie interne private Adresse für die VM angegeben, müssen Sie diese Einstellung deaktivieren, bevor Sie ein neues Subnetz VM verschieben können. Verwenden Sie in diesem Fall Folgendes:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Eine Rolleninstanz in ein anderes Subnetz verschieben

Zum Verschieben einer Rolleninstanz bearbeiten Sie mithilfe Datei. Im folgenden Beispiel bewegen wir "Role0" im virtuellen Netzwerk *VNETName* vorhanden Subnetz *Subnetz*2. Da die Instanz der Rolle bereits bereitgestellt wurde, wird nur das Umbenennen Subnetz Subnetz 2 =. Achten Sie darauf, dass im Beispiel entsprechend Ihrer Umgebung bearbeiten.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
