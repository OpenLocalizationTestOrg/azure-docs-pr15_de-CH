<properties
   pageTitle="Delegieren der Domäne Azure DNS | Microsoft Azure"
   description="Verstehen Sie, wie Delegierung Domäne ändern und Azure DNS-Namenserver Domänen bereitstellen."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/30/2016"
   ms.author="sewhee"/>


# <a name="delegate-a-domain-to-azure-dns"></a>Delegieren einer Azure DNS-Domäne

Azure DNS ermöglicht Ihnen, eine DNS-Zone hosten und Verwalten von DNS-Datensätze für eine Domäne in Azure. DNS-Abfragen muss für eine Domäne zu Azure DNS die Domäne Azure DNS aus der übergeordneten Domäne delegiert werden. Bedenken Sie Azure DNS kein Domain-Registrar. Dieser Artikel beschreibt die Funktionsweise Domäne delegieren und Domänen Azure DNS zu delegieren.




## <a name="how-dns-delegation-works"></a>Funktionsweise der DNS-Delegierung

### <a name="domains-and-zones"></a>Domänen und Zonen

Domain Name System ist eine Hierarchie von Domänen. Die Hierarchie beginnt mit "Root" Domäne, deren Name lautet**.**.  Darunter sind Domänen der obersten Ebene, z. B. "com", 'net', 'Organisation', 'uk' oder "jp".  Darunter sind Domänen der zweiten Ebene 'org.uk' oder 'co.jp'.  Und so weiter. Domänen in der DNS-Hierarchie mit separaten DNS-Zonen befinden. Diese Zonen sind global verteilt, von DNS-Namenserver weltweit gehostet.

**DNS-zone**

Eine Domäne ist ein eindeutiger Name im Domain Name System, z. B. "contoso.com". Eine DNS-Zone dient die DNS-Datensätze für eine bestimmte Domäne hosten. Beispielsweise enthalten die Domäne "contoso.com" eine Anzahl von DNS-Datensätzen wie "mail.contoso.com" (für einen e-Mail-Server) und "www.contoso.com" (für eine Website).

**Domainregistrierungsstelle**

Eine Registrierungsstelle ist ein Internet-Domänennamen bereitstellen kann. Sie überprüft, ob die Internet-Domäne verwenden möchten steht und kaufen kann. Sobald der Domänenname registriert wurde, werden Sie der rechtmäßige Eigentümer für den Domänennamen. Wenn bereits eine Internetdomäne verwenden Sie aktuelle Domainregistrierungsstelle Azure DNS delegieren.

>[AZURE.NOTE] Weitere Informationen zum Eigentümer eines bestimmten Domänennamens oder Informationen zum Kauf einer Domäne erfahren [Internet domainmanagement in Azure AD](https://msdn.microsoft.com/library/azure/hh969248.aspx).

### <a name="resolution-and-delegation"></a>Auflösung und Delegierung

Es gibt zwei Typen von DNS-Servern:

- Ein _autorisierender_ DNS-Server hostet DNS-Zonen. Fragen DNS-Datensätze in diesen Zonen.
- Ein _rekursive_ DNS-Server ist kein host für DNS-Zonen. Sie beantwortet alle DNS-Abfragen durch Aufrufen von autorisierenden DNS-Server die Daten einholen.

>[AZURE.NOTE] Azure DNS bietet einen autorisierenden DNS-Dienst.  Es bietet keine rekursiven DNS-Dienst.

> Cloud-Dienste und VMs in Azure werden automatisch konfiguriert eine rekursive DNS-Dienste wird separat als Teil der Azure Infrastruktur.  Informationen zum Ändern dieser DNS finden Sie unter [Auflösung in Azure](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

DNS-Clients in PCs und mobilen Geräten rufen normalerweise einen rekursiven DNS-Server DNS-Abfragen ausführen, die Clientanwendungen benötigen.

Empfängt ein rekursive DNS-Server eine Abfrage für einen DNS-Eintrag wie "www.contoso.com" ist zunächst dem Namen Server hostet die Zone für die Domäne "contoso.com". Hierzu es beginnt mit den Stammservern und dort findet die Namenserver "com" Zone hostet. Anschließend fragt es Namenserver der Namenserver als Host der Zone "contoso.com" zu "com".  Schließlich ist diese Namenserver "www.contoso.com" Abfragen.

Der DNS-Name aufgelöst wird aufgerufen. Streng genommen DNS-Auflösung enthält zusätzliche Schritte wie folgenden CNAMEs, jedoch, die nicht wichtig für das Verständnis der Funktionsweise der DNS-Delegierung.

Wie verweist eine übergeordnete Zone' Namenserver für eine untergeordnete Zone auf'? Dies geschieht durch eine besondere Art der DNS-Eintrag ein NS-Eintrag (NS steht für "Namenserver") bezeichnet. Beispielsweise die Stammzone enthält NS-Einträge für "com" und zeigt den Namenserver für die Zone "com". Bereich "com" enthält wiederum NS-Einträge für "contoso.com" Namenserver für die Zone "contoso.com" zeigt. Einrichten von NS-Einträge für eine untergeordnete Zone in einer übergeordneten Zone heißt Domäne delegieren.


![DNS-Namenserver](./media/dns-domain-delegation/image1.png)

Jede Delegation hat eigentlich zwei Kopien der NS-Datensätze. eine in der übergeordneten Zone auf das untergeordnete Element und in der untergeordneten Zone selbst. Die Zone "contoso.com" enthält die NS-Einträge für "contoso.com" (zusätzlich in 'com' NS-Einträge). Diese autorisierende NS-Einträge bezeichnet und sie sitzen an der Spitze der untergeordneten Zone.


## <a name="delegating-a-domain-to-azure-dns"></a>Delegieren einer Azure DNS-Domäne

Nach dem Erstellen der DNS-Zone in Azure DNS müssen Sie NS-Einträge in der übergeordneten Zone zu Azure DNS die autorisierende Quelle für die Auflösung von Namen für die Zone eingerichtet. Domänen von einer Registrierungsstelle erworben bieten die Registrierungsstelle können diese NS-Einträge einrichten.

>[AZURE.NOTE] Sie haben keine Domäne besitzen, um eine DNS-Zone mit diesen Domänennamen in Azure DNS erstellen. Allerdings müssen Sie Besitzer der Domäne die Delegierung Azure DNS mit dem Registrar einrichten.

Beispielsweise die Domäne "contoso.com" kaufen und erstellen eine Zone mit dem Namen "contoso.com" in Azure DNS. Als Besitzer der Domäne bieten Ihrer Registrierungsstelle die Option zum Konfigurieren von Namenserveradressen (d. h. die NS-Einträge) für Ihre Domäne Ihnen. Der Registrator speichert diese NS-Datensätze in der übergeordneten Domäne in diesem Fall ".com". Kunden werden dann an Ihrer Domäne in Azure DNS-Zone weitergeleitet beim Auflösen von DNS-Einträge in "contoso.com".

### <a name="finding-the-name-server-names"></a>Suchen die Server-Namen

Bevor die DNS-Zone Azure DNS zu delegieren, müssen Sie wissen, die Namen der Server für die Zone. Azure DNS weist Namenserver aus einem Pool eine Zone erstellt wird.

Am einfachsten finden die Namenserver der Zone zugewiesen ist der Azure-Portal.  In diesem Beispiel die Zone 'contoso.net' Namens-Server zugewiesen wurde ' ns1-01.azure-Dns.com ", Dns-01.azure-ns2 .net" ns3-01.azure-Dns.org', und ' ns4-01.azure-Dns.info ":

 ![DNS-Namenserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS erstellt automatisch autorisierenden NS-Ressourceneinträge in der Zone mit der zugewiesenen Namenservern.  Servernamen Azure PowerShell oder Azure CLI finden müssen Sie einfach diese Datensätze abzurufen.

Mithilfe von Azure PowerShell können autorisierende NS-Einträge wie folgt abgerufen: Beachten Sie, dass der Name des Eintrags “@” wird verwendet, um Datensätze an der Spitze der Zone.

    PS> $zone = Get-AzureRmDnsZone –Name contoso.net –ResourceGroupName MyResourceGroup
    PS> Get-AzureRmDnsRecordSet –Name “@” –RecordType NS –Zone $zone

    Name              : @
    ZoneName          : contoso.net
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                        ns4-01.azure-dns.info}
    Tags              : {}

Außerdem können die plattformübergreifende Azure CLI zu autorisierenden NS-Datensätze abrufen daher Namenserver der Zone zugewiesen:

    C:\> azure network dns record-set show MyResourceGroup contoso.net @ NS
    info:    Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
    data:    Id                              : /subscriptions/.../resourceGroups/MyResourceGroup/providers/Microsoft.Network/dnszones/contoso.net/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 172800
    data:    NS records
    data:        Name server domain name     : ns1-01.azure-dns.com.
    data:        Name server domain name     : ns2-01.azure-dns.net.
    data:        Name server domain name     : ns3-01.azure-dns.org.
    data:        Name server domain name     : ns4-01.azure-dns.info.
    data:
    info:    network dns record-set show command OK

### <a name="to-set-up-delegation"></a>Delegierung einrichten

Jeder Registrar hat ihre eigenen Verwaltungstools DNS-Namenservereinträge für eine Domäne zu ändern. Die Registrierungsstelle DNS Management Seite bearbeiten Sie NS-Datensätze und Ersetzen Sie die NS-Datensätze mit den erstellten Azure DNS.

Beim Delegieren von Azure DNS Domäne verwenden Sie die Server-Namen von Azure DNS bereitgestellt.  Sie sollten immer alle 4 Servernamen, unabhängig vom Namen der Domäne verwenden.  Domäne Delegierung erfordert keine Servernamen die gleiche Domäne höchster Ebene als der Domäne verwenden.

Verwenden Sie nicht "Verbindungsdatensätze auf Azure DNS Server den Namen IP-Adressen, da diese Adressen in Zukunft ändern können. Delegationen mit Server-Namen in eine eigene Zone "Waschtisch Name Servers" bezeichnet sind derzeit nicht in Azure DNS unterstützt.

### <a name="to-verify-name-resolution-is-working"></a>Überprüfen der Auflösung arbeiten

Nach Fertigstellen, stellen Sie sicher, dass Auflösung arbeitet mit einem Tool wie "Nslookup" Abfrage des SOA-Eintrags für die Zone (die automatisch erstellt wird, wenn die Zone erstellt wird).

Beachten Sie, dass Sie keinen Azure DNS-Namenserver an, da der DNS-Auflösung Standardvorgang Namenserver automatisch finden die Delegierung ordnungsgemäß eingerichtet wurde.

    nslookup –type=SOA contoso.com

    Server: ns1-04.azure-dns.com
    Address: 208.76.47.4

    contoso.com
    primary name server = ns1-04.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)

## <a name="delegating-sub-domains-in-azure-dns"></a>Delegieren von Unterdomänen in Azure DNS

Wenn Sie eine separate untergeordnete Zone einrichten möchten, können Sie eine Teildomäne in Azure DNS delegieren. Angenommen eine separate untergeordnete Zone einrichten möchten, z. B. eingerichtet haben und delegierten "contoso.com" in Azure DNS 'partners.contoso.com'.

Eine untergeordnete Domäne einrichten geht ähnlich wie eine normale Delegation. Der einzige Unterschied ist, dass in Schritt 3 erstellten NS-Einträge in der übergeordneten Zone "contoso.com" Azure DNS, anstatt über eine Registrierungsstelle eingerichtet werden müssen.


1. Erstellen Sie die untergeordnete Zone 'partners.contoso.com' in Azure DNS.
2. Autorisierende NS-Datensätze in der untergeordneten Zone hostet die untergeordnete Zone in Azure DNS-Namenserver zu suchen.
3. Delegieren Sie die untergeordnete Zone NS-Einträge in der übergeordneten Zone auf die untergeordnete Zone konfigurieren.


### <a name="to-delegate-a-sub-domain"></a>Eine untergeordnete Domäne delegieren

Der folgende PowerShell veranschaulicht dies. Dieselben Schritte können Azure-Portal oder über die CLI plattformübergreifende Azure ausgeführt werden.

#### <a name="step-1-create-the-parent-and-child-zones"></a>Schritt 1. Erstellen von übergeordneten und untergeordneten Zonen

Zuerst erstellen Sie die übergeordneten und untergeordneten Zonen. In derselben Ressourcengruppe oder anderen Ressourcengruppen können.

    $parent = New-AzureRmDnsZone -Name contoso.com -ResourceGroupName RG1
    $child = New-AzureRmDnsZone -Name partners.contoso.com -ResourceGroupName RG1

#### <a name="step-2-retrieve-ns-records"></a>Schritt 2. Abrufen NS-Einträge

Danach rufen wir autorisierenden NS-Datensätze aus untergeordneten wie im nächsten Beispiel gezeigt.  Enthält die Name-Server der untergeordneten Zone zugewiesen.

    $child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

#### <a name="step-3-delegate-the-child-zone"></a>Schritt 3. Die untergeordnete Zone delegieren

Erstellen Sie entsprechende NS-Datensatz in der übergeordneten Zone festlegen, um die Delegierung abzuschließen. Beachten Sie, dass Namen Datensatz in der übergeordneten Zone Zonenname Kind in diesem Fall entspricht "Partner".

    $parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
    $parent_ns_recordset.Records = $child_ns_recordset.Records
    Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset

### <a name="to-verify-name-resolution-is-working"></a>Überprüfen der Auflösung arbeiten

Sie können überprüfen, ob alles ordnungsgemäß eingerichtet wird durch Nachschlagen der untergeordneten Zone des SOA-Eintrags.

    nslookup –type=SOA partners.contoso.com

    Server: ns1-08.azure-dns.com
    Address: 208.76.47.8

    partners.contoso.com
        primary name server = ns1-08.azure-dns.com
        responsible mail addr = msnhst.microsoft.com
        serial = 1
        refresh = 900 (15 mins)
        retry = 300 (5 mins)
        expire = 604800 (7 days)
        default TTL = 300 (5 mins)

## <a name="next-steps"></a>Nächste Schritte

[Verwalten von DNS-Zonen](dns-operations-dnszones.md)

[Verwalten von DNS-Datensätzen](dns-operations-recordsets.md)

