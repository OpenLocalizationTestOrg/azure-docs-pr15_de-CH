<properties
   pageTitle="Routing an ExpressRoute | Microsoft Azure"
   description="Diese Seite enthält detaillierte Vorschriften für konfigurieren und Verwalten von routing für ExpressRoute-Schaltkreise."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>ExpressRoute Routinganforderungen  

Verbindung mit Microsoft-Cloud-Diensten mit ExpressRoute müssen Sie einrichten und Verwalten von routing. Einige Konnektivität Anbieter einrichten und Verwalten von routing als verwalteter Dienst. Überprüfen Sie bei Ihrem konnektivitätsanbieter zu dieser Dienstleistung. Wenn nicht, müssen Sie Folgendes beachten. 

Finden Sie im Artikel [Schaltkreise und routing-Domänen](expressroute-circuit-peerings.md) eine routing-Sessions, die in eingerichtet werden, um Konnektivität zu erleichtern.

>[AZURE.NOTE] Microsoft unterstützt keine Routerprotokolle Redundanz (HSRP, VRRP) für Konfigurationen für hohe Verfügbarkeit. Wir setzen auf redundante BGP-Sessions pro peering für hohe Verfügbarkeit.

## <a name="ip-addresses-used-for-peerings"></a>IP-Adressen für peerings

Sie müssen einige Blöcke von IP-Adressen zwischen Ihrem Netzwerk und Microsoft Enterprise (MSEEs) Edge-Router konfigurieren reservieren. Dieser Abschnitt enthält eine Liste und beschreibt die Regeln, wie diese IP-Adressen abgerufen und verwendet werden müssen.

### <a name="ip-addresses-used-for-azure-private-peering"></a>IP-Adressen für Azure private peering

Private IP-Adressen oder öffentliche IP-Adressen können Sie die Peerings konfigurieren. Bereich zum Konfigurieren von Routen muss über virtuelle Netzwerke in Azure erstellt Adressbereiche nicht überlappen. 

 - Sie müssen eine /29 reservieren Subnetz oder zwei /30 Subnets routing-Schnittstellen.
 - Die Subnetze für das routing kann private IP-Adressen oder IP-Adressbereich.
 - Die Subnetze müssen vom Kunden für die Verwendung in der Cloud von Microsoft reservierten Bereich kein Konflikt.
 - Wenn ein /29 Subnetz verwendet wird, werden in zwei /30 Subnetze aufgeteilt werden. 
     - Die erste /30 für die primäre Verbindung und das zweite Subnetz verwendet werden/30 Subnetz für sekundäre Verbindung verwendet werden.
     - Für jede der /30 Subnetze, verwenden Sie die erste IP-Adresse der /30 Subnetz auf dem Router. Microsoft verwendet die zweite IP-Adresse der /30 Subnetz BGP-Sitzung eingerichtet.
     - Sie müssen beide Sitzungen BGP für [Verfügbarkeits-SLA](https://azure.microsoft.com/support/legal/sla/) gültig einrichten.  

#### <a name="example-for-private-peering"></a>Beispiel für private peering

Wenn Sie a.b.c.d/29 verwenden die peering einrichten, werden in zwei /30 Subnetze aufgeteilt werden. Im folgenden Beispiel betrachten wir wie das Subnetz a.b.c.d/29 verwendet wird. 

a.b.c.d/29 a.b.c.d/30 und a.b.c.d+4/30 aufgeteilt und über die Bereitstellung APIs an Microsoft weitergegeben. Verwenden Sie a.b.c.d+1 als IP VRF für primäre PE und Microsoft a.b.c.d+2 VRF IP für primäre MSEE verbrauchen. Verwenden Sie a.b.c.d+5 als IP VRF für sekundäre PE und Microsoft a.b.c.d+6 als IP VRF für sekundäre MSEE.

Nehmen wir einmal in dem 192.168.100.128/29 private peering einrichten auswählen. 192.168.100.128/29 enthält Adressen von 192.168.100.128 bis 192.168.100.135, darunter:

- 192.168.100.128/30 werden link1, 192.168.100.129 mit Microsoft 192.168.100.130 über Anbieter zugewiesen.
- 192.168.100.132/30 werden link2, mit 192.168.100.133 und 192.168.100.134 mit Microsoft-Anbieter zugewiesen.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>IP-Adressen für Azure Public und Microsoft peering

Öffentliche IP-Adressen, die Sie besitzen verwenden BGP-Sitzung einrichten. Microsoft muss an IP-Adressen über Internet Register Routing und Internet Routing Register überprüfen können. 

- Verwenden Sie ein eindeutiges/29 Subnetz oder zwei /30 Subnetze einrichten für jedes peering pro ExpressRoute peering BGP Schaltkreis (Wenn Sie mehrere haben). 
- Wenn ein /29 Subnetz verwendet wird, werden in zwei /30 Subnetze aufgeteilt werden. 
    - Die erste /30 für die primäre Verbindung und das zweite Subnetz verwendet werden/30 Subnetz für sekundäre Verbindung verwendet werden.
    - Für jede der /30 Subnetze, verwenden Sie die erste IP-Adresse der /30 Subnetz auf dem Router. Microsoft verwendet die zweite IP-Adresse der /30 Subnetz BGP-Sitzung eingerichtet.
    - Sie müssen beide Sitzungen BGP für [Verfügbarkeits-SLA](https://azure.microsoft.com/support/legal/sla/) gültig einrichten.

## <a name="public-ip-address-requirement"></a>Öffentliche IP-Adresse erforderlich 

### <a name="private-peering"></a>Private Peering 

Sie können öffentliche oder private IPv4-Adressen für private peering verwenden. Wir bieten End-to-End-Isolierung des gesamten Netzwerkverkehrs überlappende Adressen mit anderen Kunden nicht bei privaten peering möglich ist. Diese Adressen werden nicht Internet angekündigt. 

### <a name="public-peering"></a>Öffentliche Peering

Azure öffentliche peering Pfad können Sie alle über die öffentlichen IP-Adressen in Azure gehostete Dienste herstellen. Dazu gehören [ExpessRoute häufig gestellte Fragen zu](expressroute-faqs.md) Dienstleistungen und von ISVs auf Microsoft Azure gehosteten Dienste. Verbindung mit Microsoft Azure Services für Öffentliche peering wird immer in das Microsoft-Netzwerk aus Ihrem Netzwerk initiiert. Verwenden Sie öffentliche IP-Adressen für Datenverkehr an Microsoft Network.

### <a name="microsoft-peering"></a>Microsoft Peering

Microsoft peering Pfad können Sie die Microsoft-Cloud-Diensten herstellen, die nicht über den Azure öffentliche peering Pfad unterstützt werden. Die Liste der Services umfasst Office 365-Diensten wie Exchange Online, SharePoint Online, Skype für Unternehmen und CRM Online. Microsoft unterstützt bidirektionale Konnektivität auf Microsoft peering. Datenverkehr an Microsoft Cloud-Dienste muss gültige öffentliche IPv4-Adressen verwenden, bevor sie das Microsoft-Netzwerk eingeben.

Stellen Sie sicher, dass Ihre IP-Adresse wie und Sie in einem der unten aufgeführten Register registriert sind.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] Öffentliche IP-Adressen in Microsoft angekündigt, über ExpressRoute müssen mit dem Internet nicht bekannt gegeben. Dadurch kann Konnektivität mit anderen Microsoft-Diensten unterbrochen. Allerdings können öffentliche IP-Adressen von Servern in Ihrem Netzwerk, die mit Microsoft Office 365-Endpunkten kommunizieren über ExpressRoute angekündigt. 

## <a name="dynamic-route-exchange"></a>Dynamische Route exchange

Routing Exchange werden eBGP Protokoll. EBGP Sessions sind zwischen den MSEEs und den Routern eingerichtet. Authentifizierung von BGP-Sitzung ist nicht erforderlich. Falls erforderlich, kann ein MD5-Hash konfiguriert werden. Informationen zum Konfigurieren von BGP-Sitzung finden Sie unter [routing konfigurieren](expressroute-howto-routing-classic.md) und [Circuit provisioning, Workflows und Zustände Circuit](expressroute-workflows.md) .

## <a name="autonomous-system-numbers"></a>Autonome System Zahlen

Microsoft wird als 12076 für Azure public, private Azure und Microsoft peering verwenden. Wir haben ASNs aus 65515, 65520 für die interne Verwendung reserviert. 16 und 32-bit als Zahlen werden unterstützt.

Es gibt keine Anforderungen Data Transfer Symmetrie. Vor- und Rücklauf Pfade können anderen Router-Paare durchlaufen. Identische Routen müssen auf beiden Seiten über mehrere Circuit-Paare gehören Sie angekündigt. Routemetrik müssen nicht identisch sein.

## <a name="route-aggregation-and-prefix-limits"></a>Route-Aggregation und Präfix Grenzen

Wir unterstützen bis zu 4.000 Präfixe uns über die Azure private peering angekündigt. Dies kann bis zu 10.000 Präfixe erhöht werden, wenn ExpressRoute Premium Add-on aktiviert ist. Wir akzeptieren bis 200 Präfixe pro Sitzung für Öffentliche Azure BGP Microsoft peering. 

Überschreitet die Anzahl der Präfixe der werden die BGP-Sitzung gelöscht. Wir akzeptieren Standardrouten auf die private Peeringverbindung. Anbieter muss Standardroute und private IP-Adressen (RFC 1918) von öffentlichen Azure Microsoft peering Pfade filtern. 

## <a name="transit-routing-and-cross-region-routing"></a>Übertragung-routing und routing Cross-region

ExpressRoute kann nicht als Übertragung Router konfiguriert. Sie müssen auf Dienstanbieter Konnektivität für die Übertragung Routingdienste.

## <a name="advertising-default-routes"></a>Standardarbeitspläne Werbung

Standardarbeitspläne dürfen nur auf Azure private peering Sessions. In diesem Fall werden wir alle von den zugeordneten virtuellen Netzwerken zu Ihrem Netzwerk Datenverkehr. Standard-Routen in private peering Werbung führt internetpfad von Azure blockiert. Sie müssen Ihr Unternehmen Rand Datenverkehr vom und zum Internet in Azure gehostete Dienste verwenden. 

 Um eine Verbindung zu anderen Azure Services und Infrastruktur-Services zu aktivieren, müssen Sie sicherstellen, dass eines der folgenden Elemente vorhanden ist:

 - Azure öffentliche peering ist Datenverkehr zu öffentlichen Endpunkten aktiviert
 - Mithilfe von benutzerdefinierten routing Internetkonnektivität für jedes Subnetz erfordert Internet-Verbindung ermöglichen.
 
>[AZURE.NOTE] Ankündigung des Standardrouten unterbricht Windows und andere VM lizenzaktiviert. Anleitung [hier](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) Abhilfe.

## <a name="support-for-bgp-communities-preview"></a>Unterstützung für Communities BGP (Vorschau)


In diesem Abschnitt Überblick wie BGP Communities mit ExpressRoute verwendet werden. Microsoft kündigt Routen öffentlicher und Microsoft peering Pfade mit Routen mit entsprechenden Werten gekennzeichnet. Die Gründe dafür und die Details zu der Community beschrieben werden. Microsoft, berücksichtigt jedoch nicht markierten Routen angekündigt Microsoft Community Werte.

Wenn Sie mit Microsoft über ExpressRoute eine peering überall in einem geopolitischen Bereich verbinden, müssen Sie Zugriff auf alle Microsoft Cloud-Dienste in allen Regionen innerhalb der geopolitischen Grenzen. 

Z. B. Verbindung Microsoft in Amsterdam über ExpressRoute haben Sie Zugriff auf alle Microsoft-Clouddienste im Norden und Westen Europa gehostet. 

Siehe Seite [ExpressRoute Partner und peering](expressroute-locations.md) eine ausführliche Liste der geopolitischen Regionen zugeordneten Azure-Regionen und zugehörige ExpressRoute Standorte peering.

Sie können mehrere ExpressRoute-Verbindung geopolitischen Regionen erwerben. Mehrere Verbindungen bietet erhebliche Vorteile für hohe Verfügbarkeit durch Redundanz Geo. In Fällen, in denen mehrere ExpressRoute-Schaltkreise haben, erhalten Sie dieselben Präfixe von Microsoft auf Öffentliche peering und Microsoft peering Pfade angekündigt. Dies bedeutet, dass Sie mehrere Pfade aus dem Netzwerk in Microsoft. Dies kann optimale Routingentscheidungen innerhalb des Netzwerks zu führen. Daher kann eine optimale Konnektivität Erfahrungen verschiedene Dienste auftreten. 

Microsoft tag Präfixe über öffentliche peering angekündigt und Microsoft peering mit entsprechenden BGP gemeinschaftlichen Werte der Region Präfixe werden. Sie können auf der gemeinschaftlichen Werte entsprechende Routingentscheidungen bieten [optimale routing Kunden](expressroute-optimize-routing.md)verlassen.

| **Geopolitische Region** | **Microsoft Azure-region** | **BGP Gemeinschaft Wert** |
|---|---|---|
| **Nordamerika** |    |  |
|    | Osten der USA | 12076:51004 |
|    | USA 2 OST | 12076:51005 |
|    | Westen der USA | 12076:51006 |
|    | Westen der USA 2 | 12076:51026 |
|    | Westen der USA – zentral | 12076:51027 |
|    | Norden der USA – zentral | 12076:51007 |
|    | Südlichen zentralen USA | 12076:51008 |
|    | USA | 12076:51009 |
|    | Kanada zentral | 12076:51020 |
|    | Kanada Osten | 12076:51021 |
| **Südamerika** |  |  |
|    | Brasilien Süd | 12076:51014 |
| **Europa** |    |  |
|    | Nordeuropa | 12076:51003 |
|    | Westeuropa | 12076:51002 |
| **Asien/Pazifik** |    |   |
|    | Ostasien | 12076:51010 |
|    | Südostasien | 12076:51011 |
| **Japan** |     |   |
|    | Japan OST | 12076:51012 |
|    | Japan West | 12076:51013 |
| **Australien** |    |   | 
|    | Australien OST | 12076:51015 |
|    | Australien Südost | 12076:51016 |
| **Indien** |    |   |
|    | Indien Süd | 12076:51019 |
|    | Indien West | 12076:51018 |
|    | Indien Central | 12076:51017 |

Alle Routen von Microsoft angekündigt werden mit dem entsprechenden Wert gekennzeichnet. 

>[AZURE.IMPORTANT] Globale Präfixe mit einem entsprechenden Wert markiert und werden nur bei aktiviertem ExpressRoute Premium Add-on angekündigt.


Darüber hinaus Microsoft wird auch tag Präfixe basierend auf den Dienst an. Dies gilt nur für Microsoft peering. Die Tabelle enthält eine Zuordnung zu BGP Gemeinschaft Wert.

| **Dienst** | **BGP Gemeinschaft Wert** |
|---|---|
| **Exchange** | 12076:5010 |
| **SharePoint** | 12076:5020 |
| **Skype für Unternehmen** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Andere Office 365-Dienste** | 12076:5100 |

>[AZURE.NOTE] Microsoft berücksichtigt, die Sie auf Routen angekündigt Microsoft BGP Gemeinschaftswerte nicht.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie die ExpressRoute-Verbindung.

    - [Erstellen einer ExpressRoute-Verbindung für das klassische Bereitstellungsmodell](expressroute-howto-circuit-classic.md) oder [Erstellen und Ändern einer ExpressRoute-Verbindung mit Azure Ressource-Manager](expressroute-howto-circuit-arm.md)
    - [Konfigurieren für das klassische Bereitstellungsmodell](expressroute-howto-routing-classic.md) oder [für Ressourcen-Manager-Bereitstellungsmodell konfigurieren](expressroute-howto-routing-arm.md)
    - [Klassische VNet auf ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md) oder [Link ein Ressourcen-Manager-VNet auf ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)


