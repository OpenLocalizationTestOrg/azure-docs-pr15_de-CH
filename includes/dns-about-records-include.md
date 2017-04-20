## <a name="about-records"></a>Datensätze

Jede DNS-Eintrag hat einen Namen und einen Typ. Datensätze sind in verschiedene Daten organisiert, die sie enthalten. Zumeist wird ein "A" Datensatz eine IPv4-Adresse einen Namen zuordnet. Ein weiterer ist ein 'MX' Datensatz ein Mailserver zugeordnet ist.

Azure DNS unterstützt alle gemeinsame DNS-Eintragstypen, einschließlich A, AAAA, CNAME MX, NS, PTR, SOA, SRV und TXT. Beachten Sie Folgendes:
- SOA-Datensätze werden automatisch mit den einzelnen Zonen erstellt, sie können nicht separat erstellt werden.
- SPF-Datensätze sollte mit dem TXT Datensatz erstellt werden. Weitere Informationen finden Sie auf [dieser Seite](http://tools.ietf.org/html/rfc7208#section-3.1).

In Azure DNS werden Datensätze mit relativen Namen angegeben. "Qualified" Domain Name (FQDN) enthält den Zonennamen Gegensatz "relativ" ein. Relative Eintragsnamen "Www" im Bereich "contoso.com" gibt z. B. den voll gekennzeichneten Eintragsnamen www.contoso.com.

## <a name="about-record-sets"></a>Über Datensätze

Manchmal müssen Sie mehrere DNS-Datensätze mit einem angegebenen Namen und Typ zu erstellen. Angenommen Sie, zwei verschiedene IP-Adressen den "www.contoso.com" Website gehostet wird. Die Website erfordert zwei Datensätze für jede IP-Adresse. Dies ist ein Beispiel für eine Datensatzgruppe:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS verwaltet DNS-Einträge mit Datensätzen. Eine Datensatzgruppe ist die Auflistung der DNS-Einträge in einer Zone, die denselben Namen und denselben Typ aufweisen. Die meisten Datensätze enthalten einen einzelnen Datensatz, aber wie diesem, in dem ein Datensatz mehrere Datensätze enthält Beispiele sind nicht ungewöhnlich.

SOA und CNAME-Datensätze sind Ausnahmen. Die DNS-Standards ermöglichen nicht mehrere Datensätze mit demselben Namen für diese Typen.

Gültigkeitsdauer oder TTL gibt an, wie lange jeder Datensatz von Clients zwischengespeichert werden, bevor erneut abgefragt wird. In diesem Beispiel ist die Gültigkeitsdauer 3600 Sekunden oder 1 Stunde. Die Gültigkeitsdauer ist angegeben für den Datensatz ein, nicht für jeden Datensatz der gleiche Wert für alle Datensätze in diesem Datensatz verwendet wird.

#### <a name="wildcard-record-sets"></a>Platzhalter-Datensätze

Azure DNS unterstützt [Platzhalter Datensätze](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Diese werden für eine Abfrage mit einem übereinstimmenden Namen zurückgegeben (es sei denn, eher entspricht aus einer Datensatzgruppe nicht Platzhalter). Platzhalter-Datensätze sind für alle Datensatztypen mit Ausnahme von NS- und SOA unterstützt.  

Verwenden Sie zum Erstellen einer Datensatzgruppe Platzhalter Name des Datensatzes "\*". Oder verwenden Sie einen Namen mit der Bezeichnung "\*", z. B."\*.foo".

#### <a name="cname-record-sets"></a>CNAME-Datensätze

CNAME-Datensätze können nicht mit anderen Datensatz mit demselben Namen verwendet werden. Beispielsweise kann kein CNAME-Eintrag mit dem relativen Namen "Www" festlegen und einen relativen Namen "Www" gleichzeitig erstellen. Da die Zone Apex (Name = ‘@’) enthält immer NS- und SOA Rekord, die beim Erstellen der Zone erstellt wurden, Festlegen der Zone Spitze CNAME-Eintrag kann nicht erstellt werden. Diese ergeben sich aus den DNS-Standards und Azure DNS gibt.
