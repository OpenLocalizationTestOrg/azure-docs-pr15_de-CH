<properties 
   pageTitle="DNS-Zonen und Einträge | Microsoft Azure" 
   description="Übersicht über Unterstützung für DNS-Zonen und Einträge in Microsoft Azure DNS." 
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
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>DNS-Zonen und Einträge

Diese Seite erklärt die Schlüsselkonzepte von Domänen, DNS-Zonen und DNS-Datensätze Datensätze und wie sie in Azure DNS unterstützt werden.

## <a name="domain-names"></a>Domänennamen
 
Domain Name System ist eine Hierarchie von Domänen. Die Hierarchie beginnt mit "Root" Domäne, deren Name lautet**.**.  Darunter sind Domänen der obersten Ebene, z. B. "com", 'net', 'Organisation', 'uk' oder "jp".  Darunter sind Domänen der zweiten Ebene 'org.uk' oder 'co.jp'. Domänen in der DNS-Hierarchie sind global verteilt, von DNS-Namenserver weltweit gehostet.

Eine Domänennamen-Registrierungsstelle ist eine Organisation, die einen Domänennamen, z. B. "contoso.com" erwerben kann.  Kauf eines Domänennamens berechtigt Sie zur Steuerung der DNS-Hierarchie unter diesem Namen ermöglicht z. B. den Namen "www.contoso.com" Website Ihres Unternehmens leiten. Registrar kann die Domäne in eigenen Namenserver in Ihrem Auftrag hosten oder Alternativ können alternative Namenserver.

Azure DNS bietet eine Name Global verteilten, hochverfügbare Serverinfrastruktur mit Ihrer Domäne hosten. Domänen in Azure DNS hosten, können Sie Ihre DNS-Datensätze mit denselben Anmeldeinformationen, APIs, Tools, Abrechnung und Unterstützung als Ihre Azure Services verwalten.

Azure DNS unterstützt derzeit nicht Domainnamen erwerben. Wenn Sie einen Domainnamen erwerben möchten, müssen Sie eine Drittanbieter-Domänennamen-Registrierungsstelle verwenden. Der Registrator berechnen normalerweise eine jährliche Gebühr. Domänen können dann für die Verwaltung von DNS-Einträgen in Azure DNS gehostet werden. Einzelheiten finden Sie unter [Delegaten Azure DNS Domäne](dns-domain-delegation.md) .

## <a name="dns-zones"></a>DNS-Zonen 

Eine DNS-Zone dient die DNS-Datensätze für eine bestimmte Domäne hosten. Zum Hosten Ihrer Domäne in Azure DNS zu starten, müssen Sie eine DNS-Zone für diesen Domänennamen. Jede DNS-Eintrag für Ihre Domäne wird in diese DNS-Zone erstellt. 

Beispielsweise enthalten die Domäne "contoso.com" DNS-Einträge "www.contoso.com" (für eine Website) wie "mail.contoso.com" (für einen e-Mail-Server).

Beim Erstellen einer DNS-Zone in Azure DNS muss der Namen der Zone in der Ressourcengruppe eindeutig sein. Gleichnamige Zone kann in einer anderen Ressourcengruppe oder ein anderes Azure Abonnement wiederverwendet werden. Wenn mehrere Zonen denselben Namen haben, wird jede Instanz verschiedene Namensserveradressen zugewiesen. Nur einen Satz von Adressen kann mit der Domänennamen-Registrierungsstelle konfiguriert.

>[AZURE.NOTE] Sie müssen keinen Domainnamen eine DNS-Zone mit diesen Domänennamen in Azure DNS erstellen. Allerdings müssen Sie eigene Domäne Azure DNS-Namenserver als richtige Namenserver für Domänenname mit dem Domänennamen-Registrierungsstelle konfiguriert.

Weitere Informationen finden Sie unter [Delegaten Azure DNS Domäne](dns-domain-delegation.md).

## <a name="dns-records"></a>DNS-Datensätze

### <a name="record-types"></a>Datensatztypen

Jede DNS-Eintrag hat einen Namen und einen Typ. Datensätze sind in verschiedene Daten organisiert, die sie enthalten. Zumeist wird ein "A" Datensatz eine IPv4-Adresse einen Namen zuordnet. Ein anderes ist eine 'MX' Datensatz ein Mailserver zugeordnet ist.

Azure DNS unterstützt allgemeine DNS-Eintragstypen: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV und TXT.

### <a name="record-names"></a>Record-Namen

In Azure DNS werden Datensätze mit relativen Namen angegeben. Ein *vollqualifizierter* Domänenname (FQDN) enthält den Zonennamen Gegensatz zu *relativen* Namen. Beispielsweise kann relative Eintragsnamen "Www" im Bereich "contoso.com" den vollqualifizierten Namen "www.contoso.com".

Ein Datensatz *Apex* ist ein DNS-Eintrag auf der ( *Spitze*) einer DNS-Zone. In der DNS-Zone "contoso.com" ist ein Datensatz Apex z. B. auch den voll gekennzeichneten Namen "contoso.com" (Dies ist eine *naked* Domäne bezeichnet).  Gemäß der Konvention, den relativen Namen '@' zum Apex Datensätze erstellen.

### <a name="record-sets"></a>Datensätze
Manchmal müssen Sie mehrere DNS-Datensätze mit einem angegebenen Namen und Typ zu erstellen. Angenommen Sie, zwei verschiedene IP-Adressen den "www.contoso.com" Website gehostet wird. Die Website erfordert zwei Datensätze für jede IP-Adresse. Dies ist ein Beispiel für eine Datensatzgruppe:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS verwaltet DNS-Datensätze mit *Recordsets*. Datensatzgruppe *(auch bekannt als einen Ressourceneintragssatz)* ist die Auflistung der DNS-Einträge in einer Zone, die den gleichen Namen haben und vom gleichen Typ. Die meisten Datensätze enthalten einen einzelnen Datensatz, aber wie diesem, in dem ein Datensatz mehrere Datensätze enthält Beispiele sind nicht ungewöhnlich.

Beispielsweise nehmen Sie einen A-Eintrag "Www" bereits in der Zone "contoso.com" erstellt haben, zeigt die IP-Adresse '134.170.185.46' (der erste Datensatz oben).  Den zweiten Datensatz erstellen würden Sie diesen Datensatz an die vorhandenen Datensatz hinzufügen, anstatt einen neuen Datensatz erstellen.

Die SOA- und CNAME Datensatztypen sind Ausnahmen. Die DNS-Standards nicht mehrere Datensätze mit demselben Namen für diese Typen ermöglichen, so können diese Datensätze nur einen Datensatz enthalten.

### <a name="time-to-live"></a>Time-to-live

Gültigkeitsdauer oder TTL gibt an, wie lange jeder Datensatz von Clients zwischengespeichert werden, bevor erneut abgefragt wird. Im obigen Beispiel ist die Gültigkeitsdauer 3600 Sekunden oder 1 Stunde.

In Azure DNS ist die Gültigkeitsdauer angegeben für den Datensatz ein, nicht für jeden Datensatz der gleiche Wert für alle Datensätze in diesem Datensatz verwendet wird.  Sie können einen TTL-Wert zwischen 1 und 2.147.483.647 Sekunden angeben.

### <a name="wildcard-records"></a>Platzhalter-Datensätze

Azure DNS unterstützt [Platzhalter Datensätze](https://en.wikipedia.org/wiki/Wildcard_DNS_record). (Es sei denn, eher entspricht aus einer Datensatzgruppe nicht Platzhalter) werden als Antwort auf eine Abfrage mit einem übereinstimmenden Namen Platzhalter Datensätze zurückgegeben. Platzhalter-Datensätze sind für alle Datensatztypen mit Ausnahme von NS- und SOA unterstützt.  

Verwenden Sie zum Erstellen einer Datensatzgruppe Platzhalter Datensatz Namen '\*'. Sie können auch alternativ mit "\*"als linke Bezeichnung, z. B."\*.foo".

### <a name="cname-records"></a>CNAME-Einträge

CNAME-Datensätze können nicht mit anderen Datensatz mit demselben Namen verwendet werden. Beispielsweise kann kein CNAME-Eintrag mit dem relativen Namen "Www" festlegen und einen relativen Namen "Www" gleichzeitig erstellen.

Da die Zone Apex (Name = ‘@’) enthält immer NS- und SOA Rekord, die beim Erstellen der Zone erstellt wurden, Festlegen der Zone Spitze CNAME-Eintrag kann nicht erstellt werden.

Diese Einschränkungen entstehen durch den DNS-Standards und nicht Azure DNS-Einschränkungen.

### <a name="ns-records"></a>NS-Datensätze

Ein NS-Datensatzgruppe wird an der Spitze jeder Zone automatisch erstellt (Name = ‘@’), und automatisch gelöscht, wenn die Zone gelöscht (es kann nicht gelöscht werden getrennt).  Die Gültigkeitsdauer dieser Datensatz ändern, aber vorkonfigurierte auf den Azure DNS-Namenserver der Zone zugewiesen sind Datensätze kann nicht geändert werden.

Erstellen und andere NS-Einträge in der Zone nicht an der Spitze Zone löschen.  Dadurch können Sie untergeordnete Zonen konfigurieren (siehe [Delegieren von Unterdomänen in Azure DNS](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns).)

### <a name="soa-records"></a>SOA-Einträgen

Eine SOA-Datensatzgruppe wird an der Spitze jeder Zone automatisch erstellt (Name = ‘@’), und wird automatisch gelöscht, wenn die Zone gelöscht wird.  SOA-Einträgen nicht erstellt oder separat gelöscht werden. 

Sie können alle Eigenschaften des SOA-Eintrags außer "Host"-Eigenschaft vorkonfiguriert auf den primären Namen Servername von Azure DNS ändern.

### <a name="spf-records"></a>SPF-Datensätze

Sender Policy Framework (SPF) Datensätze dienen zur Angabe der e-Mail-Server e-Mail für einen bestimmten Domänennamen dürfen.  SPF-Datensätze Konfiguration unbedingt Empfänger Ihre e-Mail-Adresse als "Junk" markiert.

Einen neuen Datensatztyp "SPF" um dieses Szenario zu unterstützen, den DNS-RFCs ursprünglich eingeführt. Zur Unterstützung älterer Namenserver dürfen sie auch die Verwendung des Datensatztyps TXT SPF-Datensätze an.  Diese Mehrdeutigkeit führte zu Verwechslungen durch [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1)behoben wurde.  Dies SPF-Datensätze sollte nur mit den TXT-Eintragstyp erstellt werden und veraltete SPF-Datensatztyp. 

**SPF-Datensätze von Azure DNS unterstützt und mit dem TXT-Datensatztyp erstellt werden soll.** Veraltete SPF Datensatztyp wird nicht unterstützt. Beim [Importieren eines DNS zone Datei](dns-import-export.md)alle SPF-Datensätze mit dem SPF-Datensatztyp sind TXT Datensatztyp konvertiert. 

### <a name="srv-records"></a>SRV-Einträge

[SRV-Einträge](https://en.wikipedia.org/wiki/SRV_record) werden von verschiedenen Diensten verwendet, Serverstandorte angeben. Wenn einen SRV-Eintrag in DNS Azure angeben:

- Der *Service* und das *Protokoll* als Teil der Datensatz ein Unterstrich vorangestellt anzugeben.  Beispielsweise "\_sip. \_tcp.name ".  Für einen Datensatz der Zone Spitze besteht keine Notwendigkeit an '@' in dessen Namen einfach mit Service und Protokoll, z. B. "\_sip. \_Tcp ".
- *Priorität*, *Gewicht*, *Port*und *Ziel* werden als Parameter für jeden Datensatz in der Datensatzgruppe angegeben. 

## <a name="tags-and-metadata"></a>Tags und Metadaten

### <a name="tags"></a>Tags
Tags sind eine Liste von Name-Wert-Paaren und werden zur Bezeichnung Ressourcen von Azure-Ressourcen-Manager.  Azure Ressourcen-Manager verwendet Tags, um gefilterte Ansichten Ihrer Azure Rechnung aktivieren und ermöglicht außerdem eine Richtlinie festlegen, die welche Tags erforderlich sind. Weitere Informationen zu Tags finden Sie unter [Tags Azure Ressourcen zu verwenden](../resource-group-using-tags.md).

Azure DNS unterstützt Azure Ressourcenmanager Tags auf DNS-Zonenressourcen.  Es unterstützt keine Tags auf DNS-Datensätze.

### <a name="metadata"></a>Metadaten

Als Alternative zum Datensatz Tags unterstützt Azure DNS Datensätze "Metadaten" hinzufügen.  Wie Tags kann Metadaten Name-Wert-Paare mit Datensatz verknüpfen.  Dies ist hilfreich, beispielsweise um den Zweck der einzelnen Datensatz Datensatz.  Im Gegensatz zu Tags Metadaten kann nicht zu einer gefilterten Ansicht Ihrer Azure Rechnung verwendet werden und nicht in eine Azure-Ressourcen-Manager-Richtlinie angegeben werden.

## <a name="limits"></a>Grenzen

Die folgenden vorgegebenen Grenzwerte gelten Azure DNS verwenden:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Nächste Schritte

- Um zu Azure DNS verwenden, erfahren Sie, wie Sie [eine DNS-Zone erstellen](dns-getstarted-create-dnszone-portal.md) und [DNS-Datensätze](dns-getstarted-create-recordset-portal.md).
- Informationen zum Migrieren einer bestehenden DNS-Zone [DNS-Zonendatei](dns-import-export.md)zu importieren.
