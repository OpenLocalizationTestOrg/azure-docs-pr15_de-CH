<properties
   pageTitle="Erste Schritte mit Azure DNS | Microsoft Azure"
   description="Informationen Sie zum Erstellen von DNS-Zonen für Azure DNS. Dies ist Schritt für Schritt zu Ihrem ersten DNS-Zone zum Hosten der DNS-Domäne mithilfe von PowerShell starten erstellt haben."
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

# <a name="create-a-dns-zone-using-powershell"></a>Erstellen einer DNS-Zone mit Powershell

> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)

Dieser Artikel führt Sie durch die Schritte zum Erstellen einer DNS-Zone mit PowerShell. Sie können auch eine DNS-Zone mit CLI oder Azure-Portal erstellen.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Etags und tags

### <a name="etags"></a>Etags

Angenommen, zwei Personen oder zwei Prozesse versuchen, einen DNS-Datensatz gleichzeitig ändern. Welche wins? Und der Gewinner weiß, dass sie Änderungen von anderen Benutzern erstellt nur überschrieben haben?

Azure DNS verwendet Etags gleichzeitige Änderung derselben Ressource sicher behandelt. Jede DNS-Ressourceneinträge (Zone oder Datensatz) hat ein Etag zugeordnet. Wenn eine Ressource abgerufen wird, wird auch der Etag abgerufen. Beim Aktualisieren einer Ressource können wieder dem Etag übergeben Azure DNS überprüfen kann, ob das Etag auf dem Server entspricht. Da das Etag regeneriert wird jedes Update einer Ressource führt, zeigt ein Etag-Konflikt eine gleichzeitige Änderung aufgetreten ist. Etags sind auch beim Erstellen einer neuen Ressource verwendet, um sicherzustellen, dass die Ressource nicht bereits vorhanden ist.

Azure DNS-PowerShell verwendet Etags zu blockieren gleichzeitige Änderung Zonen legt. Das optionale *-Überschreiben* Switch Etag Schecks zu verwendet werden können, wobei gleichzeitigen Änderungen, die aufgetreten sind überschrieben werden.

Ebene der Azure DNS-REST-API werden Etags mit HTTP-Header angegeben.  Ihr Verhalten ist in der folgenden Tabelle angegeben:

|Kopfzeile|Verhalten|
|------|--------|
|Keine|PUT ist immer erfolgreich (keine Etag überprüft)|
|If-match<etag>|PUT ist nur erfolgreich, wenn Ressource existiert und Etag entspricht|
|If-Match *     | PUT ist nur erfolgreich, wenn Ressource vorhanden ist|
|If-none-Match * |  PUT ist nur erfolgreich, wenn die Ressource nicht vorhanden ist|

### <a name="tags"></a>Tags

Tags sind Etags unterschiedlich. Tags sind eine Liste von Name-Wert-Paaren und werden von Azure Ressourcenmanager Bezeichnung Ressourcen für Rechnungsadresse oder gruppieren. Weitere Informationen zu Tags finden Sie unter [Tags Azure Ressourcen zu verwenden](../resource-group-using-tags.md).

Azure DNS-PowerShell unterstützt Tags zu Zonen und Datensätze mit den Optionen angegeben `-Tag` Parameter.


## <a name="before-you-begin"></a>Bevor Sie beginnen

Überprüfen Sie Folgendes vor der Konfiguration.

- Ein Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.

- Sie müssen die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets (Version 1.0 oder höher) installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.

## <a name="step-1---sign-in"></a>Schritt 1: anmelden

Öffnen Sie die PowerShell-Konsole und Ihr Konto verbinden. Weitere Informationen finden Sie unter [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

Verwenden Sie Folgendes Beispiel zu verbinden:

    Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto.

    Get-AzureRmSubscription

Geben Sie das Abonnement, das Sie verwenden möchten.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Schritt 2 - Erstellen einer Ressourcengruppe

Azure Ressourcen-Manager muss alle Ressourcengruppen einen Speicherort angeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global, nicht sind, wirkt sich die Wahl der Gruppe Ort nicht Azure DNS.

Wenn Sie eine vorhandene Ressourcengruppe verwenden, können Sie diesen Schritt überspringen.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Schritt 3: Registrieren

Der Ressourcenanbieter Microsoft.Network Azure DNS-Dienst verwaltet. Azure-Abonnement muss registriert sein, um diese Ressourcenanbieter verwenden, bevor Sie Azure DNS verwenden können. Dies wird einmalig für jedes Abonnement.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Schritt 4 - Erstellen einer DNS-zone

Erstellt eine DNS-Zone mit den `New-AzureRmDnsZone` Cmdlet. Es gibt Beispiele unten für eine DNS-Zone mit oder ohne Tags erstellen. Weitere Informationen zu Tags finden Sie im Abschnitt [Tags](#tags) in diesem Artikel.

>[AZURE.NOTE] In Azure DNS Zonennamen anzugeben ohne die Beendungssequenz **'.'**. Z. B. "**contoso.com**" statt "**" contoso.com ".**".

### <a name="to-create-a-dns-zone"></a>Erstellen eine DNS-zone

Das folgende Beispiel erstellt eine DNS-Zone *contoso.com* in der Ressourcengruppe namens *MyResourceGroup*aufgerufen. Anhand des Beispiels Erstellen einer DNS-Zone, die eigene Werte ersetzen.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>Erstellen eine DNS-Zone mit tags

Im folgenden Beispiel wird veranschaulicht, wie eine DNS-Zone mit zwei Tags erstellen *Projekt = Demo* und *Env = Test*. Anhand des Beispiels Erstellen einer DNS-Zone, die eigene Werte ersetzen.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Datensätze anzeigen

Erstellen einer DNS-Zone erstellt außerdem die folgenden DNS-Einträge:

- Den Eintrag *Autoritätsursprung* (SOA). Dies ist am Stamm jeder DNS-Zone vorhanden.

- Autorisierenden Datensätze der Namensserver (NS). Diese zeigen die Namenserver die Zone hosten. Azure DNS verwendet einen Pool Namens-Server und Zonen in Azure DNS können also andere Namenserver zugewiesen werden. Finden Sie weitere Informationen [Azure DNS Domäne delegieren](dns-domain-delegation.md) .

Verwenden Sie zum Anzeigen dieser Datensätze `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Datensatz wird auf der ( *Spitze*) eine DNS-Zone mit **@** der Datensatz namens festgelegt.


## <a name="test"></a>Test

Sie können Ihrer DNS-Zone mit DNS-Tools wie Nslookup oder Dig [Auflösen DnsName PowerShell-Cmdlet](https://technet.microsoft.com/library/jj590781.aspx)testen.

Wenn Ihre Domäne für die neue Zone in Azure DNS noch delegiert noch nicht, müssen Sie die DNS-Abfrage direkt auf einem Namenserver für die Zone direkt. Der Namenserver für die Zone erhalten in den NS-Einträge durch aufgeführt `Get-AzureRmDnsRecordSet` oben. Achten Sie ersetzen die Werte für Ihre Zone in folgenden Befehl.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie nach dem Erstellen einer DNS-Zone, [Datensätze und Datensätze](dns-getstarted-create-recordset.md) zum Auflösen von Namen für Ihre Internetdomäne starten.

