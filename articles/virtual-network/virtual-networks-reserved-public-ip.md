<properties
   pageTitle="Reservierte IP | Microsoft Azure"
   description="Verstehen Sie und reservierte IP-Adressen verwalten"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="reserved-ip-overview"></a>Reservierte IP-Übersicht
IP-Adressen in Azure in zwei Kategorien unterteilen: dynamische und reserviert. Öffentliche IP-Adressen von Azure verwaltet sind standardmäßig dynamisch. Das bedeutet, die IP-Adresse für einen bestimmten Clouddienst (VIP) oder eine VM-Rolle Instanz oder direkt (ILPIP) können von Zeit zu Zeit ändern, wenn Ressourcen heruntergefahren sind oder freigegeben.

Um zu verhindern, dass IP-Adressen ändern, können Sie eine IP-Adresse reservieren. Reservierte IP-Adressen dienen nur als VIP sicherzustellen, dass die IP-Adresse für den Clouddienst dasselbe auch bei Herunterfahren oder Ressourcen freigegeben. Darüber hinaus können Sie vorhandene dynamische IP als VIP reservierte IP-Adresse konvertieren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie eine statische öffentliche IP-Adresse mit dem [Ressourcen-Manager-Bereitstellungsmodell](virtual-network-ip-addresses-overview-arm.md)reservieren.

Stellen Sie sicher, dass Sie verstehen [IP-Adressen](virtual-network-ip-addresses-overview-classic.md) in Azure.

## <a name="when-do-i-need-a-reserved-ip"></a>Wann wird die reservierte IP-Adresse?
- **Sie möchten sicherstellen, dass die IP-Adresse in Ihrem Abonnement reserviert ist**. Wenn eine IP-Adresse reserviert, die nicht aus Ihrem Abonnement unter allen Umständen freigegeben werden soll, verwenden Sie eine reservierte öffentliche IP-Adresse.  
- **Sie möchten Ihre IP mit dem Cloud-Dienst sogar über Zustand angehalten oder freigegeben (VMs)**. Wenn möchten Sie Ihren Dienst Zugriff auf eine IP-Adresse, die nicht geändert werden, auch wenn VMs in der Cloud-Dienst anhalten oder freigegeben.
- **Sie möchten sicherstellen, dass ausgehender Datenverkehr von Azure eine vorhersagbare IP-Adresse verwendet**. Sie müssen die lokale Firewall nur Datenverkehr von bestimmten IP-Adressen konfiguriert. Durch das Reservieren einer IP, IP-Quelladresse kennen und Ihre Firewallregeln aufgrund einer Änderung der IP-aktualisieren müssen.

## <a name="faq"></a>Häufig gestellte Fragen
1. Kann ich eine reservierte IP-Adresse für alle Azure-Dienste verwenden?  
  - Reservierte IP-Adressen kann nur VMs und VIP offen gelegten Cloud Instanz Rollen verwendet werden.
1. Wie viele reservierte IP-Adressen kann ich haben?  
  - Zu diesem Zeitpunkt alle Azure-Abonnements 20 reservierte IP-Adressen verwenden dürfen. Allerdings können Sie zusätzliche reservierte IP-Adressen anfordern. Seite der [Abonnement und Service](../azure-subscription-service-limits.md) für Weitere Informationen.
1. Gibt es eine Gebühr für die reservierte IP-Adressen?
  - Anzeigen Sie [Reservierte IP-Adresse Preisdetails](http://go.microsoft.com/fwlink/?LinkID=398482) für Preisinformationen
1. Wie wird reservieren eine IP-Adresse?
  - PowerShell oder [Azure Management REST-API](https://msdn.microsoft.com/library/azure/dn722420.aspx) können Sie um eine IP-Adresse in einer bestimmten Region zu reservieren. Dieser reservierte IP-Adresse ist Ihr Abonnement zugeordnet. Sie können keine IP-Adresse mit dem Verwaltungsportal reservieren.
1. Kann ich hiermit mit Affinität basierend VNets?
  - Reservierte IP-Adressen werden nur in regionalen VNets unterstützt. Es wird nicht für VNets unterstützt, Gruppen zugeordnet sind. Weitere Informationen zum Zuordnen einer VNet zu einer Region oder einer Gruppe finden Sie unter [regionalen VNets und Gruppen](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Reservierte VIPs verwalten

Vor der Verwendung reservierte IP-Adressen müssen Sie es zu Ihrem Abonnement hinzufügen. Um eine reservierte IP-Adresse aus dem Pool öffentlicher IP-Adressen in den *USA* Speicherort zu erstellen, führen Sie den folgenden PowerShell-Befehl:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Beachten Sie jedoch, dass Sie angeben können, was IP wird reserviert. Um anzuzeigen, welche IP-Adressen in Ihrem Abonnement reserviert sind, führen Sie folgenden PowerShell-Befehl, und beachten Sie die Werte für *ReservedIPName* und die *Adresse*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Wenn IP reservierte bleibt sie Ihr Abonnement zugeordnet, bis Sie sie löschen. Führen Sie zum Löschen der reservierten IP-Adresse oben folgenden PowerShell-Befehl:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Reservieren Sie die IP-Adresse einem vorhandenen Cloud-Dienst

*ServiceName -* Parameter hinzufügen, um die IP-Adresse einem vorhandenen Cloud-Dienst zu reservieren. Reservieren Sie die IP-Adresse eines Cloud-Diensts *TestService* an der *USA* führen Sie den folgenden PowerShell-Befehl:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Eine reservierte IP-Adresse einen neuen Cloud-Dienst zuordnen
Das folgende Skript erstellt eine neue reservierte IP-Adresse und einen neuen Cloud-Dienst mit dem Namen *TestService*associates.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Beim Erstellen einer reservierten IP-Adresse mit einem Cloud-Dienst müssen Sie weiterhin mithilfe der VM finden *VIP:&lt;Portnummer >* für die eingehende Kommunikation. Reserviert eine IP bedeutet nicht, dass Sie den virtuellen Computer direkt verbinden können. Der reservierten IP-Adresse zugewiesen Cloud-Dienst, der die VM bereitgestellt wurde. VM IP direkt herstellen möchten, müssen Sie auf Instanzebene für eine öffentliche IP-Adresse konfigurieren. Auf Instanzebene öffentliche IP ist eine öffentliche IP-Adresse (eine ILPIP genannt), der direkt an die VM zugewiesen ist. Es kann nicht reserviert werden. Weitere Informationen finden Sie [Auf Instanzebene öffentliche IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>Eine reservierte IP-Adresse von einer laufenden entfernen
Führen Sie zum Entfernen der reservierten IP-Adresse auf den neuen Dienst erstellt im obigen Skript hinzugefügt folgenden PowerShell-Befehl:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Eine reservierte IP-Adresse ist aus einer laufenden Bereitstellung die Reservierung aus Ihrem Abonnement entfernen nicht. Einfach freigegeben IP von einer anderen Ressource in Ihrem Abonnement verwendet werden.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Zuordnen eine reservierte IP-Adresse zu einer laufenden Bereitstellung
Das folgende Skript erstellt einen neuen Clouddienst mit dem Namen *TestService2* mit einen neuen virtuellen Computer mit dem Namen *TestVM2*und der vorhandenen reservierten IP-Adresse *MyReservedIP* zum Cloud-Dienst namens verknüpft.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Zuordnen eine reservierte IP-Adresse an einen Clouddienst mit einer Service-Konfigurationsdatei
Eine reservierte IP-Adresse an einen Clouddienst stehen mit einer Service-Konfigurationsdatei (mithilfe). Unten stehenden Beispiel veranschaulicht, wie einen Cloud-Dienst zu einer reservierten VIP namens *MyReservedIP*konfigurieren:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Nächste Schritte

- Verständnis der Funktionsweise von [IP-Adressen](virtual-network-ip-addresses-overview-classic.md) im klassischen Bereitstellungsmodell.

- Enthält Informationen Sie zu [reservierten privaten IP-Adressen](virtual-networks-reserved-private-ip.md).

- Erfahren Sie mehr über [Adressen Instanz auf öffentliche IP (ILPIP)](virtual-networks-instance-level-public-ip.md).
