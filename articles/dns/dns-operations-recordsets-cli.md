<properties
   pageTitle="DNS-Datensätze und Datensätze mit Azure CLI Azure DNS verwalten | Microsoft Azure"
   description="Verwalten von DNS-Datensätze und Azure DNS Datensätze beim Hosten Ihrer Domäne Azure DNS. Alle CLI-Befehle für Operationen mit Recordsets und Datensätze."
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>Verwalten von DNS-Datensätzen und Datensatz wird mit CLI


> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


Dieser Artikel beschreibt, wie Datensätze und Datensätze für die DNS-Zone mit plattformübergreifenden Azure Befehlszeilenschnittstelle (CLI) verwalten.

Es ist wichtig, den Unterschied zwischen DNS-Datensätze und einzelnen DNS-Einträge. Eine Datensatzgruppe ist eine Auflistung von Datensätzen in einer Zone, die denselben Namen und denselben Typ aufweisen. Weitere Informationen finden Sie unter [Grundlegendes zu Recordsets und Datensätze](dns-getstarted-create-recordset-cli.md).


## <a name="configure-the-cross-platform-azure-cli"></a>Konfigurieren der plattformübergreifenden Azure-CLI

Azure DNS wird ein Azure nur Ressourcen-Manager. Ein Azure Service Management API keinen. Sicherstellen, dass der Azure-CLI Ressourcen-Manager-Modus konfiguriert ist mit der `azure config mode arm` Befehl.

Siehst du **Fehler: "Dns" ist keine Azure Befehl**, es ist wahrscheinlich, weil Sie Azure CLI in Azure Service Verwaltungsmodus, nicht im Ressourcen-Manager-Modus.

## <a name="create-a-new-record-set-and-record"></a>Erstellen Sie einen neuen Datensatz und Datensatz

Erstellen Sie einen neuen Eintrag im Azure-Portal finden Sie unter [Erstellen einer Datensatzgruppe und Datensätze](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Abrufen einer Datensatzgruppe

Rufen Sie eine vorhandene Datensatzgruppe verwenden `azure network dns record-set show`. Geben Sie die Ressourcengruppe Zonenname Rekord relative Namen und Datensatztyp. Verwenden Sie das Beispiel, durch eigene Werte ersetzen.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Liste Datensätze

Liste aller Datensätze in einer DNS-Zone mit den `azure network dns record-set list` Befehl. Sie müssen Namen und Namen angeben.

### <a name="to-list-all-record-sets"></a>Listen Sie alle Datensätze

Dieses Beispiel gibt alle Datensätze unabhängig von Name oder Datensatztyp:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Zu Liste Datensätze eines bestimmten Typs

Dieses Beispiel gibt alle Datensätze, die den angegebenen Datensatztyp (in diesem Fall "A"-Datensätze) entsprechen:

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Hinzufügen eines Datensatzes zu einem Recordset

Hinzufügen von Datensätzen zu Datensätze mithilfe der `azure network dns record-set add-record`Befehl. Parameter zum Hinzufügen von Datensätzen zu einer Datensatzgruppe variieren je nach Art des Datensatzes festgelegt wird. Wenn Sie einen Datensatz vom Typ "A" verwenden, Sie können nur beispielsweise Datensätze mit dem Parameter `-a <IPv4 address>`.

Verwenden Sie eine Datensatzgruppe erstellen, die `azure network dns record-set create`Befehl. Geben Sie die Ressourcengruppe Zonenname Rekord relative Namen Datensatztyp und Zeit Live (TTL). Wenn die `--ttl` Parameter nicht definiert ist, der Standardwert (in Sekunden) 4.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Nach Erstellung die "A" Datensatzgruppe hinzufügen die IPv4-Adresse mithilfe der `azure network dns record-set add-record`Befehl.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


Folgende Beispiele zeigen, wie jeder Datensatztyp einen Datensatz erstellen. Jeder Datensatz enthält einen einzelnen Datensatz.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Aktualisiert einen Datensatz in einem Recordset

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Um eine andere IP-Adresse (1.2.3.4) vorhandenen "A"-Datensatz festlegen ("Www"):

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Um einen vorhandenen Wert aus einem Datensatz entfernen
Mit `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Entfernen eines Datensatzes aus einer Datensatzgruppe

Datensätze können aus einem Datensatz mit entfernt `azure network dns record-set delete-record`. Der Datensatz, der entfernt wird, muss eine genaue Übereinstimmung mit einem vorhandenen Datensatz über alle Parameter.

Entfernen den letzten Datensatz aus einer Datensatzgruppe wird die Datensatzgruppe nicht gelöscht. Weitere Informationen finden Sie im Abschnitt dieses Artikels genannt [Datensatz löschen](#delete).

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>AAAA-Datensatz aus einem Datensatz entfernen

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>Entfernen Sie einen CNAME-Datensatz aus einer Datensatzgruppe

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Entfernen eines MX-Eintrags aus einer Datensatzgruppe

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Einen NS-Datensatz Datensatz entfernen

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Entfernen Sie einen PTR-Datensatz aus einer Datensatzgruppe
In diesem Fall "Meine-Arpa-Zone.com" ARPA Zeitzone darstellt IP-Bereich darstellt.  Jede PTR-Eintrag in dieser Zone festgelegt entspricht einer IP-Adresse innerhalb dieses IP-Bereichs.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>Entfernen eines SRV-Eintrags aus einer Datensatzgruppe

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>TXT-Eintrag aus einem Datensatz entfernen

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Datensatz löschen

Datensätze können gelöscht werden, mit den `Remove-AzureRmDnsRecordSet` Cmdlet. Die SOA können nicht gelöscht werden und NS-Datensatz an der Spitze Zone festgelegt (Name = "@") , wurden automatisch erstellt, wenn die Zone erstellt wurde. Sie werden automatisch gelöscht, wenn die Zone gelöscht.

Im folgenden Beispiel wird die DNS-Zone "contoso.com" "A" Datensatzgruppe "Test a" entfernt:

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

Optional *-Q* -Schalter kann verwendet werden, um Bestätigung gebeten zu unterdrücken.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Übersicht über Azure DNS](dns-overview.md). Informationen zur Automatisierung von DNS finden Sie unter [Erstellen von DNS-Zonen und Datensatz wird mit dem .NET SDK](dns-sdk.md).

Reverse-DNS-Einträge arbeiten, finden Sie unter [Verwalten von reverse DNS-Einträge für die Azure-CLI mit](dns-reverse-dns-record-operations-cli.md).
