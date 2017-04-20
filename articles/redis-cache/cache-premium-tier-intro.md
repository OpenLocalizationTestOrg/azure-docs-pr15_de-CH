<properties 
    pageTitle="Einführung in Azure Redis Cache Premium Tier | Microsoft Azure" 
    description="Enthält Informationen zum Erstellen und Verwalten von Redis für Redis-clustering und Unterstützung für die Premium-Tier Azure Redis Cacheinstanzen VNET" 
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

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Einführung in Azure Redis Cache Premium-Ebene
Azure Redis Cache ist ein verteiltes, verwaltete Cache, der hochgradig skalierbaren und reaktionsfähige Anwendung extrem schnellen Zugriff auf Ihre Daten erstellen kann. 

Neue Premium-Ebene ist eine Enterprise bereit Ebene umfasst alle Standard-Tier-Funktionen und mehr wie eine bessere Leistung, größere Arbeitslasten, Disaster Recovery, importieren/exportieren und erweiterte Sicherheit. Lesen Sie weitere Informationen zu den zusätzlichen Features der Cacheschicht Premium.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Leistungssteigerung im Vergleich zu Standard oder Basic Ebene
**Bessere Performance Standard oder grundlegende Ebene fest.** Caches in der Premium werden auf Hardware verfügt über schnellere Prozessoren und mehr Leistung als Basic oder Standard-Ebene bereitgestellt. Premium-Tier-Caches haben höhere Durchsatzraten und niedriger Latenz. 

**Durchsatz für dieselbe Größe Cache ist in Premium im Vergleich zu Standard-Stufe.** Z. B. der Durchsatz 53 GB P4 (Premium) Cache ist 250K Anfragen pro Sekunde im Vergleich zu 150 K C6 (Standard).

Weitere Informationen zur Größe, Durchsatz und Bandbreite mit Premium-Caches finden Sie unter [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Dauerhaftigkeit von Daten redis
Premium-Ebene können Sie die Cachedaten in Azure Storage-Konto beibehalten. In einem Cache Basic-Standard werden die Daten nur im Arbeitsspeicher gespeichert. Bei der zugrunde liegenden Infrastruktur kann Probleme Datenverlust. Wir empfehlen die Funktion Redis Persistenz in der Premium Resiliency Datenverlust zu. Azure Redis Cache bietet RDB und AOF (demnächst verfügbar) [Redis Dauerhaftigkeit](http://redis.io/topics/persistence). 

Zur Konfigurierung von Dauerhaftigkeit finden Sie unter [Persistenz Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Redis cluster
Ggf. mehr als 53 GB Cache erstellen oder Splitter Daten über mehrere Redis Knoten können Sie clustering der Premium-Ebene wird Redis. Jeder Knoten besteht aus einem Primary-Replikat Cache Paar für hohe Verfügbarkeit von Azure verwaltet. 

**Redis-clustering bietet maximale Skalierung und Durchsatz.** Durchsatz nimmt linear erhöht die Anzahl der Splitter (Knoten) im Cluster. . Erstellen Sie einen Cluster P4 10 Splitter, wenn 250K verfügbar ist * 10 = 2,5 Millionen Anfragen pro Sekunde. [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) für Weitere Informationen zur Größe, Durchsatz und Bandbreite mit Premium-Caches anzeigen

Zunächst mit clustering finden Sie unter [Cluster Premium Azure Redis Cache konfigurieren](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Verbesserte Sicherheit und isolation

In der Basic oder Standard erstellte Cache werden im öffentlichen Internet zugegriffen. Auf Cache beschränkt basierend auf der Zugriffstaste. Mit der Premium können Sie weitere sicherstellen, dass nur Clients in einem bestimmten Netzwerk Cache zugreifen können. Sie können Redis Cache in einem [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/)bereitstellen. Können alle Features wie Subnetze Zugriffsrichtlinien, VNet und anderen Funktionen weiterhin Zugriff auf Redis.

Weitere Informationen finden Sie unter [Unterstützung für einen Premium Azure Redis virtuelles Netzwerk konfigurieren](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Import/Export

Import/Export wird Azure Redis Cache Data Management ermöglicht Azure Redis Cache Daten importieren oder Exportieren von Daten aus dem Azure Redis Cache durch Importieren und Exportieren von Schnappschüssen Redis-Cache-Datenbank (RDB) aus einem Premium-Cache auf einer Seitenblob in einem Azure Storage-Konto. Dadurch können Sie zwischen verschiedenen Azure Redis Cacheinstanzen migrieren oder Auffüllen mit Daten verwenden.

Import kann verwendet werden, von einem Redis-Server in jeder Umgebung unter Linux, Windows oder Cloud-Anbieter wie Amazon Web Services und andere Redis oder mit Redis kompatibel RDB-Dateien zu. Importieren von Daten ist eine einfache Möglichkeit einen Cache mit vorab aufgefüllten Daten erstellt. Während des Importvorgangs Azure Redis Cache RDB-Dateien von Azure-Speicher in den Arbeitsspeicher geladen und die Schlüssel in den Cache eingefügt.

Können Sie Daten in Azure Redis Cache Redis kompatibel RDB-Dateien exportieren. Diese Funktion können Sie Daten von einer Azure Redis Cache-Instanz in eine andere oder einen anderen Redis-Server verschieben. Während des Exportvorgangs auf dem virtuellen Computer, auf dem die Serverinstanz Azure Redis Cache gehostet wird eine temporäre Datei erstellt und die Datei wird dem designierten Speicher-Konto hochgeladen. Nach Abschluss des Exportvorgangs mit dem Status Erfolg, wird die temporäre Datei gelöscht.

Weitere Informationen finden Sie unter [Daten importieren und Exportieren von Daten aus dem Azure Redis Cache](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Neustart

Die Premium-Ebene können Sie einen oder mehrere Knoten der Cache bei Bedarf neu starten. Dadurch können Sie der Anwendung Robustheit im Fehlerfall. Sie können die folgenden Knoten neu starten.

-   Master-Knoten des cache
-   Untergeordnete Knoten des cache
-   Master- und untergeordnete Knoten des cache
-   Bei einem Premium-Cache mit clustering können Sie Master, Slave oder beide Knoten für einzelne Splitter im Cache neu starten

Weitere Informationen finden Sie unter [Starten](cache-administration.md#reboot) und [Häufig gestellte Fragen zu starten](cache-administration.md#reboot-faq).

## <a name="schedule-updates"></a>Planen von updates

Geplante Updates-Feature können Sie ein Wartungsfenster für den Cache festlegen. Im Wartungsfenster angegeben, werden alle Redis Server in diesem aktualisiert. Starten ein Wartungsfenster festzulegen, wählen Sie die gewünschten Tage und geben die Wartung Fenster Stunde für jeden Tag. Beachten Sie, dass die Dauer der Wartung in UTC. 

Weitere Informationen finden Sie unter [Zeitplan](cache-administration.md#schedule-updates) und [Zeitplan Updates häufig gestellte Fragen](cache-administration.md#schedule-updates-faq).

>[AZURE.NOTE] Nur Redis Server im geplanten Wartungsfenster aktualisiert. Das Wartungsfenster gilt nicht Azure Updates oder Updates für die VM-Betriebssystem.

## <a name="to-scale-to-the-premium-tier"></a>Der Premium-Ebene skalieren

Auf die Premium-Stufe wählen Sie einfach die Premium-Ebenen Blatt **Tarif ändern** . Sie können auch Ihren Cache PowerShell mit CLI Premium-Ebene skalieren. Eine schrittweise Anleitung finden Sie unter [Skalierung Azure Redis Cache](cache-how-to-scale.md) und [eine Skalierung automatisieren](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie einen Cache und probieren Sie die neue Premium-Tier.

-   [Dauerhaftigkeit Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md)
-   [Virtuelle Netzwerk-Support für Premium Azure Redis Cache konfigurieren](cache-how-to-premium-vnet.md)
-   [Clustering für Premium Azure Redis Cache konfigurieren](cache-how-to-premium-clustering.md)
-   [Wie Sie Daten importieren und Exportieren von Azure Redis Cache](cache-how-to-import-export-data.md)
-   [Azure Redis Cache verwalten](cache-administration.md)
  

