<properties
   pageTitle="Importieren und Exportieren eine Domäne Zonendatei Azure DNS mit CLI | Microsoft Azure"
   description="Informationen Sie zum Importieren und Exportieren einer DNS-Zonendatei Azure DNS mithilfe von Azure-CLI"
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

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importieren Sie und exportieren Sie einer DNS-Zone mit der Azure-CLI


Dieser Artikel führt Sie durch das Importieren und Exportieren von DNS-Zonendateien Azure DNS mit der Azure-CLI.

## <a name="introduction-to-dns-zone-migration"></a>Einführung in die DNS-Zonenmigration

Eine DNS-Zone-Datei ist eine Textdatei, die Details jedes Datensatzes (DNS = Domain Name System) in der Zone enthält. Es folgt ein Standardformat für DNS-Datensätze DNS-Systeme übertragen. Mit einer Zonendatei ist eine schnelle, zuverlässige und bequeme Möglichkeit, eine DNS-Zone in oder aus Azure DNS übertragen.

Azure DNS unterstützt den Import und Export von Zonendateien Azure Befehlszeilenschnittstelle (CLI). Azure-CLI ist eine plattformübergreifende Befehlszeilenprogramm zum Verwalten von Azure Services. Es ist für Windows, Mac und Linux-Plattformen von [Azure Downloadseite](https://azure.microsoft.com/downloads/)verfügbar. Plattformübergreifende Unterstützung ist besonders wichtig für das Importieren und Exportieren von Zonendateien, da am häufigsten verwendete Namen Serversoftware zu [binden](https://www.isc.org/downloads/bind/), in der Regel unter Linux ausgeführt wird.

## <a name="obtain-your-existing-dns-zone-file"></a>Die vorhandene DNS-Zone-Datei abrufen

Bevor Sie in Azure DNS eine DNS-Zone-Datei importieren, müssen Sie eine Kopie der Zonendatei. Die Quelle dieser Datei hängt davon ab, die DNS-Zone derzeit gehostet wird.

- Wenn die DNS-Zone von einem Partner (wie Domainregistrierungsstelle, dedizierten DNS Hostinganbieter oder alternative Cloudanbieter) gehostet wird, sollte die DNS-Zonendatei herunterladen stellen.

- Wenn die DNS-Zone auf Windows DNS befindet, ist der Standardordner für die Zonendateien **%systemroot%\system32\dns**. Der vollständige Pfad zu jeder Zonendatei wird auch auf der Registerkarte **Allgemein** die DNS-Verwaltungskonsole.

- Wenn mit BIND DNS-Zone gehostet wird, wird der Speicherort der Zonendatei für jede Zone in BIND Konfiguration Datei **named.conf**angegeben.

**Arbeiten mit Zonendateien GoDaddy**<BR>
Zonendateien GoDaddy heruntergeladen haben etwas Format. Sie müssen dies beheben, bevor Sie diese Zone in Azure DNS importieren. RData jeden DNS-Datensatz DNS-Namen werden als vollqualifizierte Namen angegeben, aber sie haben Beendungssequenz "." Dies bedeutet, dass sie von anderen DNS-Systemen als relative Namen interpretiert werden. Müssen Sie die Zonendatei, die beendet Anhängen bearbeiten "." in ihren Namen, bevor Sie sie in Azure DNS importieren.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Azure DNS eine DNS-Zone-Datei importieren


Importieren einer Zonendatei erstellt eine neue Zone in Azure DNS, falls noch nicht vorhanden ist. Bereich bereits vorhanden, müssen die Datensätze in der Zonendatei mit vorhandenen Datensätze zusammengeführt werden.

### <a name="merge-behavior"></a>Zusammenführen von Verhalten

- Standardmäßig werden vorhandene und neue Datensätze zusammengeführt. Identische Datensätze innerhalb eines zusammengeführten Datensatz werden dedupliziert.

- Durch Angabe der `--force` Option importieren werden vorhandene Datensätze mit neuen Datensatz. Vorhandene Datensätze, die nicht über einen entsprechenden Eintrag in der importierten Zonendatei werden nicht entfernt.

- Wenn Datensätze zusammengeführt werden, wird die Gültigkeitsdauer (TTL) der bereits vorhandenen Datensätze verwendet. Wenn `--force` ist, die Gültigkeitsdauer des neuen Datensatzes wird verwendet.

- Start der Autoritätsursprung (SOA) Parameter (außer `host`) werden immer aus der importierten Zonendatei, ob `--force` wird verwendet. Ebenso wird für Festlegen der Zone Spitze Namenservereintrag TTL immer die importierten Zonendatei entnommen.

- Ein CNAME-Datensatzes ersetzt kein vorhandenes CNAME mit demselben Namen, es sei denn, die `--force` angegeben wurde.

- Bei ein Konflikt zwischen CNAME-Eintrag und einen anderen Datensatz mit demselben Namen unterschiedliche Typen (unabhängig von der vorhandenen oder neuen) wird der vorhandene Datensatz beibehalten. Dies ist unabhängig von der Verwendung des `--force`.

### <a name="additional-information-about-importing"></a>Weitere Informationen zum Importieren von

Die folgenden Hinweise bieten weitere technische Details zur Zone Importiervorgang.

- Die `$TTL` Richtlinie ist optional und wird unterstützt. Wenn keine `$TTL` Richtlinie erhält, ohne eine explizite TTL importierten soll eine Standardgültigkeitsdauer 3600 Sekunden. Wenn zwei Datensätze in den gleichen Datensatz andere TTLs angeben, wird der niedrigere Wert verwendet.

- Die `$ORIGIN` Richtlinie ist optional und wird unterstützt. Wenn keine `$ORIGIN` festgelegt ist, wird der Standardwert ist der Zonenname angegeben in der Befehlszeile (plus die beendet ".").

- Die `$INCLUDE` und `$GENERATE` Richtlinien werden nicht unterstützt.

- Diese Datensatztypen unterstützt: A, AAAA, CNAME, MX, NS, SOA, SRV und TXT.

- SOA-Eintrag wird Azure DNS automatisch beim Erstellen eine Zone. Beim Importieren einer Zonendatei werden alle SOA-Parameter aus der Zone Datei *außer* der `host` Parameter. Dieser Parameter verwendet den Wert von Azure DNS verwendet. Ist dieser Parameter auf dem primären Namenserver bereitgestellten Azure DNS verweisen muss.

- Festlegen der Zone Spitze Namenservereintrag ist auch Azure DNS automatisch beim Erstellen der Zone. Die Gültigkeitsdauer der dieser Datensatz wird importiert. Dieser Datensatz enthält die Server-Namen von Azure DNS bereitgestellt. Die Daten des Datensatzes werden durch die Werte in den importierten Zonendatei nicht überschrieben.

- Bei Public Preview unterstützt Azure DNS nur einzelne Zeichenfolge TXT-Datensätze. Mehrfachzeichenfolge TXT-Datensätze verkettet und auf 255 Zeichen gekürzt.

### <a name="cli-format-and-values"></a>CLI-Format und Werte


Das Format des Befehls Azure-Befehlszeilenschnittstelle importieren eine DNS-Zone ist:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Werte:

- `<resource group>`ist der Name der Ressourcengruppe für die Zone in Azure DNS.
- `<zone name>`ist der Name der Zone.
- `<zone file name>`ist der Pfadname der Zonendatei importiert werden.

Wenn eine Zone mit diesem Namen in der Ressourcengruppe nicht existiert, wird es für Sie erstellt. Die Zone bereits vorhanden, werden importierte Datensätze mit vorhandenen Datensatz zusammengeführt. Um vorhandene Datensätze zu überschreiben, verwenden die `--force` Option.

Das Format einer Zonendatei überprüfen, ohne tatsächlich importieren, die `--parse-only` Option.

### <a name="step-1-import-a-zone-file"></a>Schritt 1. Importieren einer Zonendatei

So importieren Sie eine Zonendatei für die Zone **contoso.com**.

1. Melden Sie sich bei Azure-Abonnement mithilfe der Azure-CLI.

        azure login

2. Wählen Sie das Abonnement der neue DNS-Zone erstellt werden soll.

        azure account set <subscription name>

3. Azure DNS wird ein Azure Ressourcen-Manager nur, damit der Azure-CLI Ressourcenmanager Modus gewechselt werden muss.

        azure config mode arm

4. Bevor Sie Azure DNS-Dienst verwenden, müssen Sie Ihr Abonnement verwenden die Microsoft.Network Ressource registrieren. (Dies ist einmalig für jedes Abonnement.)

        azure provider register Microsoft.Network

5. Wenn Sie eine bereits haben, müssen Sie eine Ressourcengruppe Ressourcen-Manager erstellen.

        azure group create myresourcegroup westeurope

6. Führen Sie den Befehl zum Importieren der Zone **contoso.com** aus der Datei **contoso.com.txt** in eine neue DNS-Zone in Ressource Gruppe **Myresourcegroup** `azure network dns zone import`.<BR>Dieser Befehl lädt die Zonendatei und analysieren. Der Befehl führt eine Reihe von Befehlen auf Azure DNS-Dienst die Zone erstellt und alle des Datensatzes in der Zone. Der Befehl ist auch im Konsolenfenster mit Fehler oder Warnungen Fortschritte. Da Datensätze nacheinander erstellt werden, dauert es einige Minuten eine große Zonendatei importieren.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Schritt 2. Überprüfen Sie die zone

Überprüfen Sie die DNS-Zone nach der Datei zu importieren, können Sie eine der folgenden Methoden verwenden:

- Sie können Datensätze mit folgenden Azure CLI-Befehl auflisten.

        azure network dns record-set list myresourcegroup contoso.com

- Liste der Datensätze mithilfe des PowerShell-Cmdlets `Get-AzureRmDnsRecordSet`.

- Sie können `nslookup` Auflösung für Datensätze überprüfen. Da noch die Zone delegiert ist nicht, müssen Sie die richtige Azure DNS-Namenserver explizit angeben. Im folgenden Beispiel veranschaulicht das Abrufen der Server-Namen, die der Zone zugewiesen. IT wie "Www" Datensatz über Abfragen `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Schritt 3. Aktualisieren der DNS-Delegierung

Nachdem Sie überprüft haben, dass die Zone ordnungsgemäß importiert wurden, müssen Sie die DNS-Delegierung Azure DNS-Namenserver auf aktualisieren. Weitere Informationen finden Sie im Artikel [Aktualisieren der DNS-Delegierung](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Exportieren einer DNS-Zonendatei aus dem Azure DNS

Das Format des Befehls Azure-Befehlszeilenschnittstelle importieren eine DNS-Zone ist:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Werte:

- `<resource group>`ist der Name der Ressourcengruppe für die Zone in Azure DNS.
- `<zone name>`ist der Name der Zone.
- `<zone file name>`ist der Pfadname der Zonendatei exportiert werden.

Als Zone importieren müssen Sie zuerst sich, wählen Sie Ihr Abonnement und Azure CLI Ressourcenmanager Modus konfigurieren.

### <a name="to-export-a-zone-file"></a>So exportieren Sie eine zone


1. Melden Sie sich bei Azure-Abonnement mithilfe der Azure-CLI.

        azure login

2. Wählen Sie das Abonnement der neue DNS-Zone erstellt werden soll.

        azure account set <subscription name>

3. Azure DNS wird ein Azure nur Ressourcen-Manager. Azure-CLI muss in den Ressourcen-Manager-Modus gewechselt werden.

        azure config mode arm

4. Führen Sie zum Exportieren der vorhandenen Azure DNS-Zone **contoso.com** in Ressource Gruppe **Myresourcegroup** in die Datei **contoso.com.txt** (im aktuellen Ordner) `azure network dns zone export`. Dieser Befehl rufen den Dienst Azure DNS-Datensätze in der Zone auflisten und Exportieren der Ergebnisse in einer Zonendatei BIND kompatibel.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

