<properties
   pageTitle="ExpressRoute Speicherorte | Microsoft Azure"
   description="Dieser Artikel enthält eine ausführliche Übersicht über Standorte, angeboten und Verbindung zu Azure-Regionen."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute-Partner und peering Speicherorte

Die Tabellen in diesem Artikel enthalten Informationen zum ExpressRoute konnektivitätsanbieter geografischen ExpressRoute Microsoft Cloud-Dienste über ExpressRoute und ExpressRoute-Systemintegratoren (SIs) unterstützt.

## <a name="partners"></a>ExpressRoute-konnektivitätsanbieter

ExpressRoute werden in allen Azure-Regionen und Standorten. Die folgende Karte enthält eine Liste der Azure-Regionen und ExpressRoute-Speicherorte. ExpressRoute Speicherorte verweisen, die dem Microsoft mit mehreren peers.

![Karte][0]

Sie haben Zugriff auf Azure Services in allen Regionen in einem geopolitischen Bereich an mindestens ExpressRoute geopolitischen Bereich verbunden. Die folgende Tabelle enthält eine Zuordnung von Azure ExpressRoute Positionen innerhalb eines Bereichs geopolitischen Regionen.

|**Geopolitische region**|**Azure-Regionen**|**ExpressRoute Positionen**|
|---|---|---|
|**Nordamerika**|USA Osten Westen der USA USA 2 OST, USA, südlichen zentralen USA, North USA, Kanada Central, Kanada Osten|Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal +, Quebec Stadt +, Toronto|
|**Südamerika**|Brasilien Süd|Sao Paulo|
|**Europa**|Nordeuropa, Westeuropa, Großbritannien West, South Großbritannien|Amsterdam, Berlin, London, Newport(Wales) +, Paris|
|**Asien**|Ostasien, Südostasien|Hongkong, Singapur|
|**Japan**|Japan, Japan OST|Osaka, Tokio|
|**Australien**|Australien Südost Australien OST|Melbourne, Sydney|
|**Indien**|Indien West Indien Central Indien Süd|Chennai, Mumbai|



In der folgenden Tabelle Informationen Regionen und geopolitische Grenzen für nationale Wolken.

|**Geopolitische region**|**Azure-Regionen**|**ExpressRoute Positionen**|
|---|---|---|---|
|**US-Regierung cloud**|US Gov Iowa, USA Gov Virginia|Chicago, Dallas, New York, Washington DC|
|**China**|China, China OST|Peking, Shanghai|
|**Deutschland**|Deutschland Central, Deutschland OST|Berlin, Frankfurt|


Konnektivität geopolitischen Regionen wird auf standard ExpressRoute-SKU nicht unterstützt. Sie müssen das ExpressRoute Premium Add-on für globale Konnektivität unterstützen aktivieren. Verbindung zum nationalen Cloud-Umgebung wird nicht unterstützt. Bei Bedarf können Sie mit Ihrem Anbieter Verbindung arbeiten.


## <a name="connectivity-provider-locations"></a>Konnektivität Anbieter Speicherorte

> [AZURE.SELECTOR]
[Speicherorte von Provider](expressroute-locations.md#connectivity-provider-locations)
[Anbieter nach Standort](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Produktion Azure
| **Speicherort**  | **Dienstanbieter** |
|---------------|-----------------------|
| **Amsterdam** | Aryaka Netzwerke, AT & T NetBond, British Telecom, Colt, Equinix, EuNetworks, GÉANT, InterCloud, Internet Solutions - Cloud Verbindung Interxion, Level 3 Communications, Orange, Tata Communications, TeleCity Group, Telenor, Verizon |
| **Atlanta** | Equinix |
| **Chennai** | TATA Communications |
| **Chicago** | AT & T NetBond Comcast Equinix, Level 3 Communications, Zayo Gruppe |
| **Hamburg** | AT & T NetBond, Cologix, Equinix, Level 3 Communications, Megaport |
| **Dublin** | COLT, Telecity Group |
| **Hongkong** | British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW globalen beschränkt, Tata Communications Verizon |
| **London** | AT & T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Einsatz, NTT Communications, Orange, Tata Communications, Telecity Group, Telenor, Verizon, Vodafone |
| **Las Vegas** | Level 3 Communications + Megaport
| **Los Angeles** | CoreSite, Equinix, Megaport, NTT, Zayo Gruppe |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **München** | Equinix, Megaport, Zayo Group |
| **Newport(Wales) +** | Nächste Generation Daten + |
| **Montreal** | Cologix + |
| **Mumbai** | TATA Communications |
| **Osaka** | Equinix Internet Initiative Japan GmbH - IIJ, NTT Communications, Softbank |
| **Paris** | Interxion, Equinix + |
| **Sao Paulo** | Equinix, Telefonica |
| **Seattle** | Equinix, Level 3 Communications, Megaport |
| **Silicon Valley** | Aryaka Netzwerke AT & T NetBond, British Telecom, CenturyLink +, Comcast, Equinix, Level 3 Communications, Orange, Tata Communications, Verizon, Zayo Gruppe |
| **Singapur** | Aryaka Netzwerke AT & T NetBond, British Telecom, Equinix, InterCloud, Megaport, Orange, SingTel, Tata Communications, Verizon |
| **Sydney** | AARNet, AT & T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Corporation, Verizon |
| **Tokio** | Aryaka Netzwerke, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank, Verizon |
| **Hamburg** | Cologix, Equinix, Zayo Group |
| **Washington DC** | Aryaka Netzwerke AT & T NetBond British Telecom Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, Orange, Tata Communications, Verizon, Zayo Gruppe |

 **+**in Kürze steht

### <a name="national-cloud-environments"></a>Nationale Cloud-Umgebung

#### <a name="us-government-cloud"></a>US-Regierung cloud

| **Speicherort**  |**Dienstanbieter** |
|---------------|--------------------|
| **Chicago** | AT & T NetBond, Equinix, Level 3 Communications, Verizon |
| **Hamburg** |  Equinix, Verizon + |
| **München** | Equinix, Level 3 Communications + Verizon |
| **Washington DC** | AT & T NetBond, Equinix, Level 3 Communications, Verizon |

#### <a name="china"></a>China

| **Speicherort**  | **Dienstanbieter** |
|---------------|-----------------------|
| **Peking** | China Telecom |
| **Shanghai** |  China Telecom |
Weitere Informationen finden Sie unter [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Deutschland

| **Speicherort**  | **Dienstanbieter** |
|---------------|-----------------------|
| **Berlin** | COLT + e-Schutz |
| **Frankfurt** | COLT Equinix Interxion |

## <a name="nonpartners"></a>Konnektivität über Dienstanbieter nicht aufgeführt

Wenn Ihr Provider Konnektivität in vorherigen Abschnitten nicht aufgeführt ist, können Sie dennoch eine Verbindung erstellen.

- Überprüfen Sie bei Ihrem konnektivitätsanbieter, um festzustellen, ob sie einen Austausch in der obigen Tabelle verbunden sind. Sie können die folgenden Links, um weitere Informationen über Exchange-Anbieter angebotenen Dienste überprüfen. Ethernet-Austausch sind bereits mehrere konnektivitätsanbieter verbunden.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Haben Sie Ihre Verbindung Netzwerk an peering Auswahl erweitern.
    - Stellen Sie sicher, dass hoher Verfügbarkeit Connectivity-Anbieter die Verbindung erweitert, so dass kein Einzelpunktversagen.
- Bestellen Sie ExpressRoute-Verbindung mit dem Dienstanbieter Konnektivität Verbindung zu Microsoft.
    - Führen Sie Schritte [erstellen eine ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) Verbindung einrichten.

|**Speicherort**|**Exchange**|**Konnektivitätsanbieter**|
|-------------|------------|-------------------------|
| **München** | Equinix | Leuchtturm |
| **Seattle** | Equinix | Alaska Kommunikation |
| **Silicon Valley** | Equinix | XO Communications |
| **Singapur** | Equinix | 1CLOUDSTAR |
| **Washington DC** | Equinix | Leuchtturm |

## <a name="expressroute-system-integrators"></a>Systemintegratoren, die ExpressRoute

Aktivieren der privaten Verbindung an Ihre Bedürfnisse kann schwierig sein, basierend auf Ihrem Netzwerk. Sie können mit Systemintegratoren gemäß der folgenden Tabelle bei Onboarding, ExpressRoute arbeiten.

|**Kontinent**|**Systemintegratoren**|
|-------------|---------------------|
| **Asien** | Avanade Inc., OneAs1a|
| **Europa** | Avanade Inc., Dotnet Solutions|
| **UNS** | Avanade Inc. Equinix Professional Services, Perficient, Projektleiter|

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu ExpressRoute finden Sie im [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md).
- Stellen Sie sicher, dass alle erforderlichen Komponenten vorhanden sind. [ExpressRoute Komponenten](expressroute-prerequisites.md)anzeigen

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Karte"
