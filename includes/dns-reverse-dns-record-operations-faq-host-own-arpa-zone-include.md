<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>FAQ - Hosting der ARPA Zone in Azure DNS

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Kann ich für mein Internetdienstanbieter zugewiesene IP-Blöcke Azure DNS ARPA Zonen hosten?
Ja. Mit ARPA Zonen für eigene IP-Adressbereiche in Azure DNS unterstützt.

Einfach [in Azure DNS Zone erstellen](dns-getstarted-create-dnszone.md)und Arbeiten mit Ihrem Internetdienstanbieter [die Zone](dns-domain-delegation.md)delegieren.  Sie können PTR-Einträge für jede reverse-Lookup auf gleiche Weise wie andere Datensatztypen.

Sie können auch [eine vorhandenen reverse-Lookupzone mithilfe der Azure-Befehlszeilenschnittstelle importieren](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Wieviel meine Kosten ARPA Zone hosten?
ARPA-Zone für Ihr Internetdienstanbieter zugewiesene IP-Hosting wird Block in Azure DNS [Standardsätze Azure DNS](https://azure.microsoft.com/pricing/details/dns/)berechnet.

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kann ARPA IPv4- und IPv6-Adressen in Azure DNS-Zonen hosten?
Ja.