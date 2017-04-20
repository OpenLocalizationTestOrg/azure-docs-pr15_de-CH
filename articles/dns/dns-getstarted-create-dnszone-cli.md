<properties
   pageTitle="Erstellen eine DNS-Zone mit CLI | Microsoft Azure"
   description="Erstellen Sie DNS-Zonen für Azure DNS schrittweise zu hosten mit CLI DNS-Domäne"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Erstellen einer Azure DNS-Zone mit CLI


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


Dieser Artikel führt Sie durch die Schritte zum Erstellen einer DNS-Zone mit CLI. Sie können auch eine DNS-Zone mit PowerShell oder Azure-Portal erstellen.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Bevor Sie beginnen

Diese Anleitung verwenden Microsoft Azure CLI. Die neuesten Azure CLI aktualisieren (0.9.8 oder höher) der Azure DNS-Befehle. Typ `azure -v` überprüft die Azure-CLI-Version derzeit auf Ihrem Computer installiert ist.

## <a name="step-1---set-up-azure-cli"></a>Schritt 1: Einrichten von Azure-CLI

### <a name="1-install-azure-cli"></a>1. installieren Sie 1. Azure CLI

Sie können CLI für Windows Azure, Linux oder Mac. Die folgenden Schritte müssen abgeschlossen sein, bevor Sie Azure DNS mit Azure CLI verwalten können. Weitere Informationen finden in [der Azure-CLI installieren](../xplat-cli-install.md). DNS-Befehle erfordern Azure CLI Version 0.9.8 oder höher.

Der Netzwerk-Providerbefehle in CLI kann mit dem folgenden Befehl gefunden:

    azure network

### <a name="2-switch-cli-mode"></a>2. schalten den CLI-Modus

Azure DNS verwendet Azure-Ressourcen-Manager. Sicherstellen Sie, dass Sie CLI um ARM Befehle wechseln.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. Melden Sie sich bei Ihrem Azure-Konto

Sie werden aufgefordert, Ihre Anmeldeinformationen authentifizieren. Denken Sie daran, dass Sie nur ORGID Konten.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Wählen Sie das Abonnement

Auswählen von Azure Abonnements verwenden.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. erstellen Sie 5. eine Ressourcengruppe

Azure Ressourcen-Manager muss alle Ressourcengruppen einen Speicherort angeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global, nicht sind, wirkt sich die Wahl der Gruppe Ort nicht Azure DNS.

Wenn Sie eine vorhandene Ressourcengruppe verwenden, können Sie diesen Schritt überspringen.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. registrieren

Der Ressourcenanbieter Microsoft.Network Azure DNS-Dienst verwaltet. Azure-Abonnement muss registriert sein, um diese Ressourcenanbieter verwenden, bevor Sie Azure DNS verwenden können. Dies wird einmalig für jedes Abonnement.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Schritt 2 - Erstellen einer DNS-zone

Erstellt eine DNS-Zone mit den `azure network dns zone create` Befehl. Sie können optional eine DNS-Zone mit Tags erstellen. Tags sind eine Liste von Name-Wert-Paaren und werden von Azure Ressourcenmanager Bezeichnung Ressourcen für Rechnungsadresse oder gruppieren. Weitere Informationen zu Tags finden Sie unter [Tags Azure Ressourcen zu verwenden](../resource-group-using-tags.md).

In Azure DNS Zonennamen anzugeben ohne die Beendungssequenz **'.'**. Z. B. "**contoso.com**" statt "**" contoso.com ".**".


### <a name="to-create-a-dns-zone"></a>Erstellen eine DNS-zone

Das folgende Beispiel erstellt eine DNS-Zone *contoso.com* in der Ressourcengruppe namens *MyResourceGroup*aufgerufen.

Anhand des Beispiels der DNS-Zone, die Werte für Ihre eigenen erstellen.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>Zum Erstellen einer DNS-Zone und Tags.

Azure DNS CLI unterstützt Tags von DNS-Zonen mit optionalen angegeben *-Tag* Parameter. Das folgende Beispiel veranschaulicht eine DNS-Zone mit zwei Tags erstellen, Projekt = Demo und Env = Test.

Verwenden Sie im folgenden Beispiel zum Erstellen einer DNS-Zone und ersetzen die Werte für Ihre eigenen Tags.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Datensätze anzeigen

Erstellen einer DNS-Zone erstellt außerdem die folgenden DNS-Einträge:

- Der Datensatz "Start of Authority" (SOA). Dies ist am Stamm jeder DNS-Zone vorhanden.

- Autorisierenden Datensätze der Namensserver (NS). Diese zeigen die Namenserver die Zone hosten. Azure DNS verwendet einen Pool Namens-Server und Zonen in Azure DNS so verschiedene Namenserver zugeordnet werden. Weitere Informationen finden Sie unter [Delegaten Azure DNS Domäne](dns-domain-delegation.md) .

Verwenden Sie zum Anzeigen dieser Datensätze `azure network dns-record-set show`.<BR>
*Syntax: show Netzwerk Dns Datensatzgruppe < Ressourcengruppe >< Dns-Zone-Name > <name><type>*


Im folgenden Beispiel mit Ressource Gruppe *Myresourcegroup*Befehl Datensatz setzen Namen *"@"* (für eine Stammdatensatz) und *SOA*geben, ergibt die folgende Ausgabe:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Verwenden Sie den folgenden Befehl zum Anzeigen der Zone erstellt NS-Einträge:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Datensatz wird auf der ( *Spitze*) eine DNS-Zone mit **@** der Datensatz namens festgelegt.

## <a name="test"></a>Test

Testen die DNS-Zone mit DNS-Tools wie Nslookup, DIG, oder `Resolve-DnsName` PowerShell-Cmdlet.

Wenn Ihre Domäne für die neue Zone in Azure DNS noch delegiert noch nicht, müssen Sie direkt die DNS-Abfrage direkt auf einem Namenserver für die Zone. Der Namenserver für die Zone erhalten in NS-Einträge "Azure Netzwerk DNS-Datensatz-Satz anzeigen" aufgeführt. Achten Sie ersetzen die richtigen Werte für die Zone im folgenden Befehl.

Das folgende Beispiel verwendet DIG Abfragen der Domäne contoso.com mit der Namenserver für die DNS-Zone zugewiesen. Die Abfrage wurde auf einem Server für die verwendet * @ * und mit DIG Zonennamen.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie nach dem Erstellen einer DNS-Zone, [Datensätze und Datensätze](dns-getstarted-create-recordset-cli.md) zum Auflösen von Namen für Ihre Internetdomäne starten.
