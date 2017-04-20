## <a name="what-is-reverse-dns"></a>Reverse-DNS-Neuigkeiten

Konventionelle ermöglichen DNS eine Zuordnung von einem DNS-Namen (z. B. "www.contoso.com") eine IP-Adresse (z.B. 64.4.6.100).  Reverse DNS ermöglicht die Übersetzung einer IP-Adresse (64.4.6.100) auf einen Namen (www.contoso.com).

Reverse-DNS-Einträge werden in einer Vielzahl von Situationen verwendet. Beispielsweise werden reverse-DNS-Einträge verwendet zur Bekämpfung Mails Überprüfen des Absenders einer e-Mail-Nachricht.  Der empfangende Mailserver reverse DNS-Datensatz der IP-Adresse des sendenden Servers abrufen und überprüfen, ob dieser Host berechtigt, aus der ursprünglichen Domäne senden. (Bitte beachten Sie jedoch, dass [Azure Compute-Dienste unterstützen keine e-Mails an externe Domänen senden](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Wie reverse DNS funktioniert

Reverse-DNS-Einträge werden in speziellen DNS-Zonen als "ARPA" Zonen gehostet.  Diese Zonen sind separate DNS-Hierarchie mit normalen Hierarchie Domänen wie "contoso.com".

Der DNS-Datensatz "www.contoso.com" wird z. B. mithilfe von DNS-'A' Datensatz mit dem Namen "Www" in der Zone "contoso.com" implementiert.  Diesen A-Datensatz verweist auf die entsprechende IP-Adresse in diesem Fall 64.4.6.100.  Reverse-Lookup wird separat implementiert mit einem "PTR" Datensatz mit dem Namen '100' im Bereich '6.4.64.in-addr.arpa' (Beachten Sie, dass IP-Adressen in ARPA Zonen storniert werden.)  Diese PTR-Datensatz verweist wurde ordnungsgemäß konfiguriert mit dem Namen "www.contoso.com".

Wenn eine Organisation einen IP-Adressblock zugewiesen ist, erwerben sie auch die entsprechende ARPA Zone verwalten. Azure verwendet IP-Adressblöcke für ARPA-Zonen werden gehostet und von Microsoft. Ihr ISP kann ARPA Zone eine eigene IP-Adressen für Sie hosten oder können Sie hosten ARPA Zone im DNS-Dienst Ihrer Wahl wie Azure DNS.

>[AZURE.NOTE] Forward-DNS Lookups und reverse-DNS-Lookups werden in separaten, parallele DNS-Hierarchien implementiert. Reverse-Lookup für den "www.contoso.com" ist **nicht** in der Zone "contoso.com" gehostet eher es befindet sich in der ARPA-Zone für die entsprechende IP-Adresse blockieren.

Weitere Informationen zu reverse DNS finden Sie unter [Reverse-DNS-Lookup](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure-Unterstützung für reverse DNS

Azure unterstützt zwei separate Szenarien zur reverse DNS:

1. Hosten von ARPA Zone entspricht der IP-Adresse blockieren.
2. Damit Sie reverse DNS-Datensatz für Ihren Azure Dienst zugewiesene IP-Adresse konfigurieren.

Unterstützung der ersten Azure DNS kann ARPA Zonen hosten und verwalten die PTR-Datensätze für jede reverse-DNS-Lookup verwendet werden.  Der Prozess von ARPA-Zone erstellen, die Delegierung einrichten, konfigurieren und PTR-Einträge entspricht der normalen DNS-Zonen.  Unterschiede werden die Delegierung muss über Internetdienstanbieter als der DNS-Registrierung konfiguriert werden und nur der PTR Datensatztyp verwendet werden soll.

Diese Unterstützung ermöglicht Azure Konfigurieren der reverse-Lookup für die IP-Adressen zu Ihrem Dienst zugewiesen.  Reverse-Lookup wird von Azure als PTR-Eintrag in der entsprechenden ARPA-Zone konfiguriert.  Diese ARPA-Zonen für alle IP-Adressbereiche von Azure verwendet werden von Microsoft gehostet. **Der Rest dieses Artikels beschreibt dieses Szenario ausführlich.**
