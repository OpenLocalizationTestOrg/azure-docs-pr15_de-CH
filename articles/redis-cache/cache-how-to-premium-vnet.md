<properties 
    pageTitle="Virtuelle Netzwerk-Support für Premium Azure Redis Cache konfigurieren | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Verwalten von virtuellen Netzwerk-Support für Premium-Tier Azure Redis Cache Instanzen" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Virtuelle Netzwerk-Support für Premium Azure Redis Cache konfigurieren
Azure Redis Cache hat verschiedene Cache die Flexibilität bei der Wahl der Cachegröße und -Features, einschließlich der neuen Premium-Ebene.

Die Azure Redis Cache Premium Ebene gehören clustering, Dauerhaftigkeit und virtual Network (VNet)-Unterstützung. Ein VNet ist ein privates Netzwerk in der Cloud. Wenn eine Cacheinstanz Azure Redis ein vnet konfiguriert ist, ist nicht öffentlich angesprochen und kann nur von virtuellen Computern und Komponenten von der VNet zugegriffen werden. Dieser Artikel beschreibt die Unterstützung für eine Prämie Azure Redis Cache-Instanz virtuelles Netzwerk konfigurieren.

>[AZURE.NOTE] Azure Redis Cache unterstützt sowohl klassische und ARM VNets.

Informationen über andere Cache Zusatzfunktionen finden Sie unter [Einführung in Azure Redis Cache Premium-Ebene](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Warum VNet?
Bereitstellung von [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/) bietet höhere Sicherheit und Isolation für Ihren Azure Redis Cache sowie Subnetze Zugriffsrichtlinien und andere Features Zugriff auf Azure Redis Cache weiter einschränken.

## <a name="virtual-network-support"></a>Virtuelles Netzwerk-support
Unterstützung für Virtual Network (VNet) ist während der Erstellung des Caches auf den **Neuen Redis Cache** konfiguriert. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Nach Auswahl eine Prämie Tarif können Sie Azure Redis Cache VNet-Integration durch Auswählen einer VNet in demselben Abonnement und Speicherort der Cache konfigurieren. Um eine neue VNet verwenden, zuerst nach der Anleitung in [ein virtuelles Netzwerk mit der Azure-Portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder [erstellen ein virtuelles Netzwerks (klassisch) mithilfe des Azure-Portals](../virtual-network/virtual-networks-create-vnet-classic-portal.md) erstellen Sie und wieder **Neue Redis Cache** Blade erstellen und Konfigurieren des Premium-Caches.

Konfigurieren Sie das VNet für den neuen Cache **Neue Redis Cache** -Blade **Virtuelles Netzwerk** auf und wählen Sie das gewünschte VNet aus der Dropdown-Liste.

![Virtuelles Netzwerk][redis-cache-vnet]

Wählen Sie das gewünschte Subnet aus Liste **Subnetz** , und geben Sie die gewünschte **statische IP-Adresse**. Verwenden Sie eine klassische VNet Feld **statische IP-Adresse** ist optional und wenn keines angegeben ist, wird eine aus ausgewählten Subnet ausgewählt.

>[AZURE.IMPORTANT] Beim Bereitstellen einer Azure Redis Cache auf einem ARM-VNet muss der Cache in einem dedizierten Subnetz sein, keine anderen Ressourcen außer Azure Redis Cacheinstanzen enthält. Bei einem Azure Redis Cache ein ARM-VNet in einem Subnet bereitstellen, andere Ressourcen enthält, schlägt die Bereitstellung fehl.

![Virtuelles Netzwerk][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] Die ersten vier Adressen in einem Subnetz sind reserviert und können nicht verwendet werden. Weitere Informationen finden Sie unter [gibt es Einschränkungen auf IP-Adressen in diesen Subnetzen?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Nach dem Erstellen des Caches können Sie die Konfiguration für die VNet von **Virtual Network** **Settings** Blatt auf anzeigen.

![Virtuelles Netzwerk][redis-cache-vnet-info]


Geben Sie zum Herstellen der Cacheinstanz Azure Redis bei einem VNet mit den Hostnamen des Cache in der Verbindungszeichenfolge, wie im folgenden Beispiel gezeigt.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis Cache VNet FAQ

Die folgende Liste enthält Antworten auf häufig gestellte Fragen zur Azure Redis Cache Skalierung.

-   [Was sind einige der häufigsten Probleme Fehlkonfiguration Azure Redis Cache und VNets?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Kann ich VNets mit Standard- oder grundlegende Cache verwenden?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Warum Erstellen eines Redis Caches einige Subnetze aber nicht erkennt?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Funktionieren alle Cache-Funktionen, wenn einen Cache in einem VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Was sind einige der häufigsten Probleme Fehlkonfiguration Azure Redis Cache und VNets?

Azure Redis Cache in einem VNet befindet, werden die Ports in der folgenden Tabelle verwendet. Wenn diese Ports blockiert sind, kann der Cache nicht ordnungsgemäß. Eine oder mehrere dieser Ports blockiert ist das häufigste Problem Fehlkonfiguration bei Azure Redis Cache in einem VNet verwenden.

| Anschlüsse     | Richtung        | Übertragungsprotokoll | Zweck                                                                           | Remote-IP-                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443     | Ausgehende         | TCP                | Redis Abhängigkeiten auf Azure Storage/PKI (Internet)                                | *                                   |
| 53          | Ausgehende         | TCP/UDP            | Redis Abhängigkeit DNS (Internet/VNet)                                         | *                                   |
| 6379, 6380  | Eingehende          | TCP                | Clientkommunikation mit Redis Azure Lastenausgleich                               | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 8443        | Eingehend/ausgehend | TCP                | Implementierungsdetails für Redis                                                   | VIRTUAL_NETWORK                     |
| 8500        | Eingehende          | TCP/UDP            | Azure Lastenausgleich                                                              | AZURE_LOADBALANCER                  |
| 10221 10231 | Eingehend/ausgehend | TCP                | Implementierungsdetails für Redis (Remoteendpunkt zum VIRTUAL_NETWORK können) | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 13000 13999 | Eingehende          | TCP                | Clientkommunikation Redis Cluster Azure Lastenausgleich                      | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 15000 15999 | Eingehende          | TCP                | Clientkommunikation Redis Cluster Azure Lastenausgleich                      | VIRTUAL_NETWORK AZURE_LOADBALANCER |
| 16001       | Eingehende          | TCP/UDP            | Azure Lastenausgleich                                                              | AZURE_LOADBALANCER                  |
| 20226       | Eingehende + ausgehende | TCP                | Implementierungsdetails für Redis-Cluster                                          | VIRTUAL_NETWORK                     |


Gibt es Konnektivitätsanforderungen für Azure Redis Cache, die ursprünglich nicht in einem virtuellen Netzwerk erfüllt werden kann. Azure Redis Cache erforderlich aller folgenden Elemente in einem virtuellen Netzwerk ordnungsgemäß.

-  Ausgehende Netzwerkkonnektivität zum Azure-Speicher Endpunkte weltweit. Dies schließt Endpunkte befindet sich im Bereich Azure Redis Cacheinstanz sowie Speicher Endpunkte in **anderen** Azure-Regionen befindet. Unter den folgenden DNS-Domänen Azure Storage Endpunkte zu beheben: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*und *file.core.windows.net*. 
-  Ausgehende Netzwerkkonnektivität, *ocsp.msocsp.com*, *mscrl.microsoft.com* und *crl.microsoft.com*. Diese Verbindung wird benötigt SSL-Funktionen.
-  Die DNS-Konfiguration für das virtuelle Netzwerk muss alle Endpunkte und die früheren genannten Domänen auflösen. Diese DNS-Anforderung erreicht dadurch eine gültige DNS-Infrastruktur konfiguriert und für das virtuelle Netzwerk verwaltet.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Kann ich VNets mit Standard- oder grundlegende Cache verwenden?

VNets kann nur mit Premium-Caches verwendet werden.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Warum Erstellen eines Redis Caches einige Subnetze aber nicht erkennt?

Wenn Sie einen ARM VNet eine Azure Redis Cache bereitstellen, muss der Cache in einem dedizierten Subnetz sein, keine anderen Ressourcentyp enthält. Bei einer Azure Redis Cache für ein Subnetz ARM VNet bereitstellen, andere Ressourcen enthält, schlägt die Bereitstellung fehl. Sie müssen vorhandenen Ressourcen innerhalb des Subnetzes vor der Erstellung eines neuen Redis Caches löschen.

Sie können eine klassische VNet verschiedener Ressourcen bereitstellen, solange Sie genügend IP-Adressen.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Funktionieren alle Cache-Funktionen, wenn einen Cache in einem VNET?

Wenn Ihr Cache Teil einer VNET ist, können nur Clients in der VNET Cache zugreifen. Daher funktionieren nicht Cache Management Features zu diesem Zeitpunkt.

-   Konsole redis - Redis-Konsole verwendet gehosteten virtuellen Computer, die nicht Teil der VNET Redis cli.exe-Client keine Verbindung zu Ihren Cache.


## <a name="use-expressroute-with-azure-redis-cache"></a>Verwenden Sie ExpressRoute mit Azure Redis Cache

Kunden können eine [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) -Verbindung ihre virtuelle Infrastruktur, so erweitern ihre lokalen Netzwerk in Azure. 

Neu erstellte ExpressRoute-Verbindung gibt standardmäßig eine Standardroute, die ausgehende Internet-Konnektivität ermöglicht. Clientanwendungen können mit dieser Konfiguration mit anderen Azure einschließlich Azure Redis Cache Endpunkten verbunden.

Jedoch ist eine gemeinsame Kundenkonfiguration, erzwingt ausgehenden Internetdatenverkehr stattdessen lokale, eigene Standardroute (0.0.0.0/0) definiert. Dieser Datenverkehr wird immer Verbindung mit Azure Redis Cache da ausgehende Datenverkehr entweder blockierte lokal oder NAT auf Adressen, die nicht mehr mit verschiedenen Azure Endpunkte nicht erkannt.

Die Lösung ist eine (oder mehrere) benutzerdefinierte Routen (Decision) im Subnetz definieren, Azure Redis Cache enthält. Ein UDR definiert Subnet-spezifische Routen, die statt der Standard-Route berücksichtigt werden.

Gegebenenfalls empfiehlt es sich mit der folgenden Konfiguration:

- ExpressRoute-Konfiguration kündigt 0.0.0.0/0 und Kraft Tunnel standardmäßig alle ausgehenden Datenverkehr lokal.
- Das Subnetz mit Azure Redis Cache zugewiesen UDR definiert 0.0.0.0/0 mit nächsten Hop Internet (Beispiel weiter unten in diesem Artikel).

Kombinierte bewirkt Schritte, dass die Ebene UDR ExpressRoute erzwungene Tunneln, Vorrang damit aus dem Azure Redis Cache ausgehenden Internetzugriff.

Obwohl eine Azure Redis Cache-Instanz herstellen von einer lokalen Anwendung ExpressRoute wird nicht normalerweise aus Leistungsgründen (für optimale Leistung Azure Redis Cache Kunden im Bereich Azure Redis Cache sein soll) dabei, interne corporate Proxys ausgehende Netzwerkpfad passieren kann nicht, und können Kraft lokal getunnelt werden. Dabei ändert die effektive NAT-Adresse ausgehenden Netzwerkverkehr von Azure Redis Cache. Ändern der Adresse NAT ein Azure Redis Cache verursacht Instanz ausgehenden Netzwerkverkehr Verbindungsfehler auf viele der oben aufgeführten Endpunkte. Dies führt dazu, dass Versuche Azure Redis Cache erstellen.

**Wichtig:**  In einer UDR **muss** definierten Routen sein von ExpressRoute Konfiguration angekündigte Routen Vorrang spezifisch genug. Im folgende Beispiel verwendet Bereich Breite 0.0.0.0/0 und so möglicherweise versehentlich durch übergangen Routenbekanntgaben Weitere Adressbereiche verwenden.

**Sehr wichtig:**  Azure Redis Cache ist nicht mit ExpressRoute Konfigurationen unterstützt, **falsch Cross - Routen aus dem öffentlichen peering an den privaten Pfad peering ankündigen**. ExpressRoute Konfigurationen öffentliche peering konfiguriert, erhalten Routenbekanntgaben von Microsoft für eine Reihe von Microsoft Azure IP-Adressbereiche. Diese Adressbereiche fälschlicherweise als Kreuz auf private peering Pfad angekündigt sind, ist das Ergebnis alle ausgehende Netzwerkpakete von Azure Redis Cache Instanz Subnetz falsch als Kraft des Kunden vor Ort Infrastruktur getunnelt werden. Dieser Datenfluss wird Azure Redis Cache. Die Lösung für dieses Problem ist zu Cross-Werbung Routen von öffentlichen peeringpfad private peering Pfad.

Hintergrundinformationen zu benutzerdefinierten Routen steht in dieser [Übersicht](../virtual-network/virtual-networks-udr-overview.md). 

Weitere Informationen zu ExpressRoute finden Sie unter [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md)

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr Cache-Premiumfunktionen verwenden.

-   [Einführung in Azure Redis Cache Premium-Ebene](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

