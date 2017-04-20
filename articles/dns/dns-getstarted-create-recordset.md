<properties
   pageTitle="Erstellen Sie Datensätze für eine DNS-Zone mit PowerShell und eine Datensatzgruppe | Microsoft Azure"
   description="Azure DNS-Einträge erstellen Datensatz festgelegt und Datensätze mit PowerShell"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Erstellen Sie DNS-Datensätze und Datensätze mithilfe von PowerShell


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)

Dieser Artikel führt Sie durch den Prozess der Erstellung von Datensätzen und Recordsets mithilfe von Windows PowerShell. Nach dem Erstellen der DNS-Zone, fügen Sie die DNS-Datensätze für Ihre Domäne. Dazu müssen Sie DNS-Einträge und Datensätze.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Überprüfen Sie, ob Sie die neueste Version von PowerShell

Stellen Sie sicher, dass Sie die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets installiert haben. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.

## <a name="create-a-record-set-and-record"></a>Erstellen einer Datensatzgruppe und Datensatz

Erläutert, wie ein Datensatz und Datensatz erstellt.


### <a name="1-connect-to-your-subscription"></a>1. Verbinden Sie 1. Ihr Abonnement

Öffnen Sie die PowerShell-Konsole und Ihr Konto verbinden. Verwenden Sie Folgendes Beispiel zu verbinden:

    Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto.

    Get-AzureRmSubscription

Geben Sie das Abonnement, das Sie verwenden möchten.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Weitere Informationen zum Arbeiten mit PowerShell finden Sie unter [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2. Erstellen einer Datensatzgruppe

Erstellen Sie Datensätze mithilfe der `New-AzureRmDnsRecordSet` Cmdlet. Beim Erstellen einer Datensatzgruppe müssen Sie der Datensatz Name, Zone, Time to live (TTL) und den Datensatztyp festlegen.

Zum Erstellen eines Datensatzes in der Spitze der Zone festgelegt (in diesem Fall "contoso.com"), verwenden Sie dessen Namen "@", einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Konvention.

Im folgende Beispiel erstellt einen Eintrag in der DNS-Zone "contoso.com" durch den relativen Namen "Www" festgelegt. Der voll gekennzeichnete Name der Datensatzgruppe ist "www.contoso.com". Der Datensatztyp ist "A" und die Gültigkeitsdauer beträgt 60 Sekunden. Nachdem dieser Schritt beendet haben Sie eine Datensatzgruppe leer "Www", die der Variablen *$rs*zugewiesen.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Wenn ein Datensatz bereits vorhanden

Wenn bereits ein Eintrag vorhanden ist, schlägt der Befehl fehl, wenn die *-Überschreiben* Switch verwendet. Die *-Überschreiben* Option löst eine Sicherheitsabfrage mit unterdrückt die *-Force* wechseln.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


In diesem Beispiel wird die Zone mithilfe der Zonenname und Namen angeben. Oder Sie können ein Zonenobjekt als durch `Get-AzureRmDnsZone` oder `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Gibt ein lokales Objekt, das die Datensatzgruppe darstellt, die in Azure DNS erstellt wurde.

### <a name="3-add-a-record"></a>3. Hinzufügen eines Datensatzes

Um die neu erstellte "Www" Datensatz verwenden, müssen Sie Datensätze hinzufügen. Sie können IPv4 *ein* Datensatz "Www" Rekorde anhand des folgenden Beispiels. Dieses Beispiel verwendet die Variable *$rs* , die Sie im vorherigen Schritt festgelegt.

Hinzufügen von Datensätzen zu einem Datensatz festlegen `Add-AzureRmDnsRecordConfig` ist ein offline-Vorgang. Nur die lokale Variable *$rs* wird aktualisiert.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. Änderungen

Änderungen Sie, die Datensatzgruppe. Mit `Set-AzureRmDnsRecordSet` Änderungen an den Eintrag Azure DNS hochladen.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. die Datensatzgruppe abrufen

Festlegen von Azure DNS Datensatz abrufen `Get-AzureRmDnsRecordSet` wie im folgenden Beispiel gezeigt.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Nslookup-Tool oder andere DNS-Tools können auch die neue Datensatzgruppe abzufragen.

Wenn Sie noch nicht die Domäne Azure DNS-Namenserver delegiert haben, müssen Sie explizit angeben, Name, Server und Adresse für die Zone.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Erstellen einer Datensatzgruppe jedes mit einem einzelnen Datensatz


Folgende Beispiele zeigen, wie jeder Datensatztyp einen Datensatz erstellen. Jeder Datensatz enthält einen einzelnen Datensatz.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Nächste Schritte

[Verwalten von DNS-Zonen mit PowerShell](dns-operations-dnszones.md)

[Verwalten von DNS-Einträgen und Datensätze mithilfe von PowerShell](dns-operations-recordsets.md)

[Automatisieren von Azure Operationen mit .NET SDK](dns-sdk.md)
