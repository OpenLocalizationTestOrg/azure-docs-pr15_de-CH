<properties
   pageTitle="Verwalten von DNS-Zonen mit PowerShell | Microsoft Azure"
   description="Sie können Zonen mit Azure Powershell verwalten. Wie aktualisieren, löschen und Erstellen von DNS-Zonen Azure DNS"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="how-to-manage-dns-zones-using-powershell"></a>Verwalten von DNS-Zonen mit PowerShell

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [PowerShell](dns-operations-dnszones.md)



In diesem Artikel wird die DNS-Zone mit PowerShell verwalten angezeigt. Um die folgenden Schritte müssen Sie die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets (Version 1.0 oder höher) installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.


## <a name="create-a-new-dns-zone"></a>Erstellen Sie eine neue DNS-zone

Erstellen eine DNS-Zone finden Sie unter [Erstellen einer DNS-Zone mit PowerShell](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Abrufen einer DNS-zone

Eine DNS-Zone abzurufen, verwenden Sie die `Get-AzureRmDnsZone` Cmdlet. Dieser Vorgang zurück eine DNS-Zone für eine vorhandene Zone in Azure DNS. Das Objekt enthält enthält Daten über die Zone (wie die Anzahl der Datensätze), jedoch keine Datensätze selbst.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Liste DNS-Zonen

Durch Auslassen der Zonenname von `Get-AzureRmDnsZone`, alle Zonen in einer Ressourcengruppe auflisten. Diese Operation gibt ein Array der Zonenobjekte.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Aktualisieren einer DNS-zone

Eine DNS-Zonenressourceneinträge können Änderungen mit `Set-AzureRmDnsZone`. Dies aktualisiert keine DNS-Datensätze in der Zone (siehe [Verwalten von DNS-Datensätze](dns-operations-recordsets.md)). Es wird nur verwendet, um Eigenschaften der Zone selbst aktualisieren. Dies ist begrenzt auf Azure Ressourcen-Manager "Tags" für die zonenressource. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#Etags-and-tags) .

Verwenden Sie eine der folgenden kann auf zwei Arten aktualisieren DNS-Zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Gibt die Zone für die Zone und den Ressourcenschlüssel verwenden

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Geben Sie die Zone mit einem $zone-Objekt

Geben Sie mithilfe eines $zone-Objekts aus Zone `Get-AzureRmDnsZone`. Bei Verwendung `Set-AzureRmDnsZone` mit einem $zone-Objekt Etag überprüft werden zur gewährleisten gleichzeitig geändert werden nicht überschrieben. Sie können die optionale *-Überschreiben* Switch Kontrollen zu. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#Etags-and-tags) .


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>Löschen einer DNS-Zone

DNS-Zonen können mithilfe des Cmdlets entfernen AzureRmDnsZone gelöscht werden.

Vor dem Löschen einer DNS-Zone in Azure DNS, müssen Sie alle Recordsets außer NS- und SOA Datensätze im Stamm der Zone zu löschen, die beim Erstellen die Zone automatisch erstellt wurden.

Gehen Sie folgt eine DNS-Zone zu entfernen:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Geben Sie die Zone mit dem Namen und den Namen an

Dieser Vorgang ist eine optionale *-Force* Switch unterdrückt gefragt werden die DNS-Zone entfernen möchten.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Geben Sie die Zone mit einem $zone-Objekt

Geben Sie mithilfe eines $zone-Objekts aus Zone `Get-AzureRmDnsZone`. Dieser Vorgang ist eine optionale *-Force* Switch unterdrückt gefragt werden die DNS-Zone entfernen möchten. Mit `Set-AzureRmDnsZone`, Angabe der Zone mit einem $zone-Objekt ermöglicht Etag prüft gleichzeitig geändert werden nicht gelöscht. <BR>
Das optionale *-Überschreiben* Flag unterdrückt diese Kontrollen. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#Etags-and-tags) .

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



Zonenobjekt kann auch geleitet werden, anstatt als Parameter übergeben wird:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie nach dem Erstellen einer DNS-Zone, [Datensätze und Datensätze](dns-getstarted-create-recordset.md) zum Auflösen von Namen für Ihre Internetdomäne starten.