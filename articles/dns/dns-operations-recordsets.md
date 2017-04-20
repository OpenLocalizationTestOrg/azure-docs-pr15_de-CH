<properties
   pageTitle="DNS-Datensätze und Datensätze mithilfe von Azure-Portal verwalten | Microsoft Azure"
   description="Verwalten von DNS-Datensätze und Azure DNS Datensätze beim Hosten Ihrer Domäne Azure DNS. Alle PowerShell Befehle für Operationen mit Recordsets und Datensätze."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>Verwalten von DNS-Einträgen und Datensätze mithilfe von PowerShell



> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)



Dieser Artikel beschreibt, wie Datensätze und Datensätze für die DNS-Zone mit Windows PowerShell verwalten.

Es ist wichtig, den Unterschied zwischen DNS-Datensätze und einzelnen DNS-Einträge. Eine Datensatzgruppe ist die Auflistung der Einträge in einer Zone, die denselben Namen und denselben Typ aufweisen. Weitere Informationen finden Sie unter [Erstellen von DNS-Datensätzen und Datensätzen mithilfe des Azure-Portals](dns-getstarted-create-recordset-portal.md).

Zum Verwalten von Datensätzen und Datensätze benötigen Sie die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Weitere Informationen zum Arbeiten mit PowerShell finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Erstellen Sie eine neue Datensatzgruppe und einen Datensatz

Erstellen Sie einen Datensatz mithilfe von PowerShell finden Sie unter [Erstellen von DNS-Datensätzen und Datensätze mithilfe von PowerShell](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Abrufen einer Datensatzgruppe

Rufen Sie eine vorhandene Datensatzgruppe verwenden `Get-AzureRmDnsRecordSet`. Geben Sie relative Namen Datensatztyp und Zone Rekord an.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Mit `New-AzureRmDnsRecordSet`, dessen Namen muss ein relativer Name, d. h. es muss den Zonennamen ausschließen.

Sie können die Zone mit den Namen und den Namen oder mit einem Zonenobjekt:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Gibt ein lokales Objekt, das die Datensatzgruppe darstellt, die in Azure DNS erstellt wurde.

## <a name="list-record-sets"></a>Liste Datensätze

Können`Get-AzureRmDnsRecordSet` , Liste Datensätze Wenn Sie auslassen der *-Name* bzw. *RecordType-* Parameter.

### <a name="to-list-all-record-sets"></a>Listen Sie alle Datensätze

Dieses Beispiel gibt alle Datensätze unabhängig von Name oder Datensatztyp:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Zu Liste Datensätze für einen bestimmten Datensatztyp

Dieses Beispiel gibt alle Datensätze, die bestimmten Datensatz übereinstimmen. In diesem Fall ist die Datensatzgruppe zurückgegeben "A" Datensätze:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Die Zone kann entweder *– Zonenname* und *ResourceGroupName-* Parameter (wie gezeigt) oder eine Zone Objekt angegeben werden:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Hinzufügen eines Datensatzes zu einem Recordset

Hinzufügen von Datensätzen zu Datensätze mithilfe der `Add-AzureRmDnsRecordConfig` Cmdlet. Dies ist ein Offlinevorgang. Nur das lokale Objekt, das Datensatzgruppe darstellt, wird geändert.

Die Parameter zum Hinzufügen von Datensätzen zu einer Datensatzgruppe variieren je nach der Datensatzgruppe. Bei Verwendung eine Datensatzgruppe vom Typ "A" Sie können nur beispielsweise Datensätze mit dem Parameter *-IPv4Address*.

Jeder Eintrag durch zusätzliche Aufrufe können zusätzliche Datensätze hinzugefügt werden `Add-AzureRmDnsRecordConfig`. Sie können bis zu 20 Datensätze auf jede Datensatzgruppe hinzufügen. Jedoch Datensätze vom Typ "CNAME" darf höchstens ein Datensatz und eine Datensatzgruppe kann nicht zwei identische Datensätze enthalten. Leere Datensätze (mit null) erstellt werden, aber Azure DNS-Namenserver nicht angezeigt.

Nach die Datensatzgruppe die gewünschte Auflistung der Datensätze enthält, müssen Sie mit commit die `Set-AzureRmDnsRecordSet` Cmdlet. Nach eine Datensatzgruppe, ersetzt den vorhandenen Eintrag in Azure DNS.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Erstellen ein A-Datensatzes mit einem einzelnen Datensatz festlegen

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Die Reihenfolge der Vorgänge zum Erstellen eines Datensatzes kann auch *geleitet*, was bedeutet, dass Sie das Recordset-Objekt übergeben, indem die Pipe als als Parameter übergeben. Zum Beispiel:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Beispiele für zusätzliche Datensatztyp

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Vorhandene Datensätze ändern

Die Schritte zum Ändern einer vorhandenen Datensatzgruppe ähneln die Schritte beim Erstellen von Datensätzen. Die Reihenfolge der Arbeitsgänge lautet wie folgt:

1.  Abrufen den vorhandenen Datensatz mithilfe `Get-AzureRmDnsRecordSet`.

2.  Ändern Sie den Eintrag Datensätze hinzufügen, Datensätze entfernen, um das Ändern der Datensatzparameter oder Ändern der Gültigkeitsdauer (TTL) einstellen. Dies ist ein Offlinevorgang. Nur das lokale Objekt, das Datensatzgruppe darstellt, wird geändert.

3.  Änderungen mithilfe der `Set-AzureRmDnsRecordSet` Cmdlet. Dies ersetzt den vorhandenen Eintrag in Azure DNS.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Einen Datensatz in einem vorhandenen Datensatz aktualisieren

In diesem Beispiel ändern wir die IP-Adresse eines vorhandenen Eintrag "A":

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Die `Set-AzureRmDnsRecordSet` -Cmdlet verwendet Etag überprüft um sicherzustellen, dass gleichzeitige Änderung nicht überschrieben werden. Verwenden der *-Überschreiben* Flag zu Kontrollen. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Ein SOA-Eintrag ändern

Sie können keine Datensätze hinzufügen oder Entfernen von automatisch erstellten SOA-Eintrag der Zone Spitze festlegen (Name = "@"). Jedoch können die Parameter im SOA-Eintrag (außer "Host") und des Datensatzes TTL.

Das folgende Beispiel zeigt die *Email-* Eigenschaft des SOA-Eintrags ändern:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>NS-Ressourceneinträge in der Zone Spitze ändern

Sie können nicht hinzufügen, entfernen oder ändern Datensätze eigen-automatisch NS der Zone Spitze (Name = "@"). Die einzige Änderung zulässig ist ist die Datensatzgruppe TTL ändern.

Das folgende Beispiel zeigt die TTL-Eigenschaft des Recordsets NS ändern:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Datensätze einer vorhandenen Datensatz hinzufügen

In diesem Beispiel werden zwei zusätzliche MX-Einträge an die vorhandenen Datensatz hinzugefügt:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Entfernen eines Datensatzes aus einem vorhandenen Datensatz

Datensätze können aus einem Datensatz mit entfernt `Remove-AzureRmDnsRecordConfig`. Der Datensatz, der entfernt wird, muss eine genaue Übereinstimmung mit einem vorhandenen Datensatz über alle Parameter. Änderung müssen mit Commit `Set-AzureRmDnsRecordSet`.

Entfernen den letzten Datensatz aus einer Datensatzgruppe wird die Datensatzgruppe nicht gelöscht. Weitere Informationen finden Sie unter folgenden [Datensatz löschen](#delete-a-record-set) .


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Die Reihenfolge der Arbeitsgänge zum Entfernen eines Datensatzes aus einer Datensatzgruppe kann auch geleitet werden bedeutet, dass Sie das Recordset-Objekt übergeben, indem die Pipe als als Parameter übergeben. Zum Beispiel:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>AAAA-Datensatz aus einem Datensatz entfernen

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>Entfernen Sie einen CNAME-Datensatz aus einer Datensatzgruppe

Da eine CNAME-Datensatzgruppe höchstens einen Eintrag enthalten kann, lässt diesen Datensatz entfernen eine leere Datensatzgruppe.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Entfernen eines MX-Eintrags aus einer Datensatzgruppe

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Einen NS-Datensatz Datensatz entfernen

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>Entfernen eines SRV-Eintrags aus einer Datensatzgruppe

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>TXT-Eintrag aus einem Datensatz entfernen

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Datensatz löschen

Datensätze können gelöscht werden, mit den `Remove-AzureRmDnsRecordSet` Cmdlet. Die SOA können nicht gelöscht werden und NS-Datensatz an der Spitze Zone festgelegt (Name = "@") , wurden automatisch erstellt, wenn die Zone erstellt wurde. Sie werden automatisch gelöscht, wenn die Zone gelöscht.

Verwenden Sie eine der folgenden drei Methoden eine Datensatzgruppe zu entfernen:

### <a name="specify-all-the-parameters-by-name"></a>Geben Sie die Parameter nach Namen

Das optionale *-Force* Schalter kann verwendet werden, um Bestätigung gebeten zu unterdrücken.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Geben Sie der Datensatzgruppe nach Name und Typ und Bereich von Objekt

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Geben Sie den Eintrag von Objekt

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Wenn Sie den Eintrag mit einem Objekt angeben, können Etag prüft, dass gleichzeitige Änderung nicht gelöscht werden. Das optionale *-Überschreiben* Flag unterdrückt diese Kontrollen. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag) .

Das Recordset-Objekt kann auch geleitet werden, anstatt als Parameter übergeben wird:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Übersicht über Azure DNS](dns-overview.md). Informationen zur Automatisierung von DNS finden Sie unter [Erstellen von DNS-Zonen und Datensatz wird mit dem .NET SDK](dns-sdk.md).

Weitere Informationen über reverse-DNS-Einträge finden Sie unter [Verwalten von reverse DNS-Datensätze für Ihre Dienste mithilfe von PowerShell](dns-reverse-dns-record-operations-ps.md).
