<properties
   pageTitle="Zugriffssteuerungslisten (ACLs) für Endpunkte verwalten, mithilfe von PowerShell"
   description="Verwalten Sie ACLs mit PowerShell"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Zugriffssteuerungslisten (ACLs) für Endpunkte verwalten, mithilfe von PowerShell

Sie können erstellen und verwalten Network Access Control Lists (ACLs) für Endpunkte Azure PowerShell oder im Verwaltungsportal. In diesem Thema finden Sie Verfahren für die ACL-Aufgaben, die mithilfe von PowerShell abgeschlossen werden kann. Die Liste der Azure-PowerShell-Cmdlets finden Sie unter [Azure Management Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721). Weitere Informationen zu ACLs finden Sie unter [Was ist ein Netzwerk Zugriffssteuerungsliste (ACL)?](virtual-networks-acl.md). Sollten Ihre ACLs mit dem Verwaltungsportal verwalten finden Sie unter [Festlegen von Endpunkten einer virtuellen Maschine](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Netzwerk-Zugriffssteuerungslisten mithilfe von Azure PowerShell verwalten

Können Azure PowerShell-Cmdlets erstellen, entfernen und konfigurieren () Network Access Control Lists (ACLs). Wir haben einige Beispiele für die Arten aufgenommen, eine ACL mit PowerShell konfigurieren können.

Rufen Sie eine vollständige Liste der ACL-PowerShell-Cmdlets können Sie Folgendes verwenden:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Erstellen Sie eine Netzwerk-ACL mit Regeln, die von einem remote-Subnetz zugänglich

Das folgende Beispiel veranschaulicht eine Möglichkeit, eine neue ACL erstellen, die Regeln enthält. Diese ACL wird zu einem virtuellen Endpunkt angewendet. ACL-Regeln im folgenden Beispiel werden einem Remotesubnetz zugreifen. Öffnen Sie zum Erstellen einer neuen Netzwerk-ACL mit einem Remotesubnetz zulassen einer Azure PowerShell ISE. Kopieren Sie und fügen Sie das folgende Skript mit Ihren eigenen Werten konfigurieren Skript und führen Sie das Skript.

1. Erstellen Sie das neue Netzwerk ACL-Objekt.

        $acl1 = New-AzureAclConfig

1. Legen Sie eine Regel, die von einem remote-Subnetz Zugriff erlaubt. Im folgenden Beispiel legen Sie die remote-Subnetz *10.0.0.0/8* Zugriff auf den virtuellen Endpunkt Regel *100* (hat Priorität über Regel 200). Ersetzen Sie die Werte mit Konfiguration Bedarf. Der Name "SharePoint ACL-Konfiguration" sollte mit dem Anzeigenamen ersetzt, die diese Regel aufrufen.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Zusätzliche Regeln wiederholen Sie das Cmdlet Konfiguration Bedarf Werte ersetzen. Die Regel ändern Zahl Reihenfolge entsprechend die Reihenfolge, in der die Regeln angewendet werden sollen. Die niedrigere Nummer Vorrang vor höheren Nummer.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Als Nächstes erstellen Sie eine neue (hinzufügen) oder stellen Sie die ACL für einen vorhandenen Endpunkt (festlegen). In diesem Beispiel werden wir einen neuen virtuellen Endpunkt bezeichnet "Web" hinzufügen und aktualisieren den virtuellen Endpunkt mit ACL.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Dann kombinieren Sie die Cmdlets und führen Sie das Skript aus. Die kombinierten Cmdlets würde beispielsweise wie folgt aussehen:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Entfernen einer Netzwerk ACL-Regel, die von einem remote-Subnetz Zugriff erlaubt

Das folgende Beispiel veranschaulicht eine Möglichkeit, eine ACL Netzwerkregel entfernen.  Um ein Netzwerk-ACL-Regel mit einem Remotesubnetz zulassen zu entfernen, öffnen Sie eine Azure PowerShell ISE. Kopieren Sie und fügen Sie das folgende Skript mit Ihren eigenen Werten konfigurieren Skript und führen Sie das Skript.

1. Zunächst ist die Netzwerk-ACL für den virtuellen Endpunkt abzurufen. Sie werden die ACL-Regel entfernen. In diesem Fall werden wir sie Regel-ID entfernen Dies wird nur die Regel-ID 0 aus der Zugriffssteuerungsliste entfernt. Es entfernt nicht das ACL-Objekt vom virtuellen Endpunkt.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Anschließend müssen Sie den virtuellen Endpunkt Netzwerk ACL-Objekt zuweisen und aktualisieren den virtuellen Computer.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Entfernen einer ACL Netzwerk von einem virtuellen Endpunkt

In bestimmten Szenarien möchten Sie einen Endpunkt virtuellen Netzwerk ACL-Objekt entfernen. Öffnen Sie hierzu ein Azure PowerShell ISE. Kopieren Sie und fügen Sie das folgende Skript mit Ihren eigenen Werten konfigurieren Skript und führen Sie das Skript.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte

[Was ist ein Netzwerk Zugriffssteuerungsliste (ACL)?](virtual-networks-acl.md)
