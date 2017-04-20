<properties
   pageTitle="Azure Service Fabric-Cluster unter Windows Server und Linux erstellen | Microsoft Azure"
   description="Service Fabric-Cluster unter Windows Server und Linux, d. h. Sie bereitstellen können und Host Service Fabric Applikationen überall ausführen, können Sie Windows Server oder Linux ausführen."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Erstellen Sie Service Fabric-Cluster unter Windows Server oder Linux

Azure Service Fabric ermöglicht die Erstellung von auf virtuellen Computern oder Computern mit Windows Server oder Linux Cluster Service Fabric. Sind Sie bereitstellen und Ausführen Service Fabric-Applikationen in jeder Umgebung mehrere Windows Server oder Linux-Computern, die miteinander verbunden sind, werden lokal Microsoft Azure oder jede Cloud-Anbieter.

##<a name="create-service-fabric-clusters-on-azure"></a>Azure Service Fabric-Cluster erstellen

Erstellen eines Clusters auf Azure Ressourcenmodell Vorlage oder Azure-Portal erfolgt. Weitere Informationen finden Sie in [Service Fabric-Cluster mithilfe einer Ressourcen-Manager-Vorlage erstellen](service-fabric-cluster-creation-via-arm.md) oder [einen Cluster Service Fabric von Azure-Portal erstellen](service-fabric-cluster-creation-via-portal.md) .

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Unterstützte Betriebssysteme für Cluster in Azure

Sie können Cluster virtuellen Computer unter diesen Betriebssystemen erstellen:

* Windows Server 2012 R2
* Windows Server 2016 (nachdem es allgemein verfügbaren bekannt)
* Linux Ubuntu 16.04 (in public Preview) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Erstellen Sie lokale oder alle Cloudanbieter Service Fabric eigenständige Cluster

Fabric Service bietet ein Installationspaket eigenständige Service Fabric-Cluster lokal erstellen oder ein Cloudanbieter

Weitere Informationen zum Einrichten von eigenständigen Service Fabric-Cluster unter Windows Server finden Sie unter [Service Fabric Clustererstellung für Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Cloud-Bereitstellungen im Vergleich zu lokalen Bereitstellung
Prozess zum Erstellen eines Clusters Fabric Service vor Ort ähnelt dem Erstellen eines Clusters auf jeder Ihrer Wahl mit VMs. Die ersten Schritte zur Bereitstellung von VMs unterliegen Cloud-Anbieter oder lokalen Umgebung, die Sie verwenden. Haben Sie eine Reihe von VMs mit Netzwerkkonnektivität zwischen ihnen aktiviert, die Schritte zum Einrichten des Pakets Service Fabric Cluster bearbeiten und Ausführen der Clustererstellung und Verwaltungsskripts identisch sind. Dies gewährleistet, dass Ihr Wissen und Ihre Erfahrung und Service Fabric Clusterverwaltung übertragbar Wenn neue Hostingumgebungen Ziel auswählen.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Vorteile eines eigenständigen Service Fabric-Cluster
* Wählen Sie alle Cloudanbieter Cluster gehostet werden.
* Service Fabric Applikationen einmal geschrieben können in mehrere Hostingumgebungen mit minimaler keine Änderungen ausgeführt werden.
* Kenntnisse über das Erstellen von Service Fabric führt über eine Hosting-Umgebung in eine andere.
* Erfahrung und Verwalten von Service Fabric Cluster führt über von einer Umgebung in eine andere.
* Breite erreichen ist mit Einschränkungen ungebunden.
* Zusätzliche Zuverlässigkeit und Schutz gegen Ausfälle weit verbreitet ist, da Dienste in eine andere bereitstellungsumgebung verschieben können über Data Center oder Cloud Provider hat einen Stromausfall.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Unterstützte Betriebssysteme für eigenständige Cluster
Sie können Cluster virtuellen Computern oder Computern mit diesen Betriebssystemen zu erstellen:

* Windows Server 2012 R2
* Windows Server 2016 (nachdem es allgemein verfügbaren bekannt)
* Linux (in Kürze verfügbar)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Vorteile des Service Fabric-Cluster in Azure über eigenständige Service Fabric-Cluster erstellt lokale

Service Fabric-Clustern auf Windows Azure ausgeführte bietet gegenüber der lokalen option Wenn Sie spezifischen Bedarf, wo der Cluster ausgeführt haben, dann sollten Sie in Azure ausführen. Azure bieten wir Integration mit anderen Azure-Funktionen und Services und Management des Clusters einfacher und zuverlässiger macht.

* **Azure-Portal:** Azure-Portal erleichtert das Erstellen und Verwalten von Clustern.

* **Azure Resource Manager:** Verwenden der Azure-Ressourcen-Manager einfache Verwaltung aller Ressourcen, die vom Cluster als Ganzes verwendet und vereinfacht Kosten nachverfolgen und Abrechnung.
* **Service Fabric-Cluster als Azure Ressource** Service Fabric-Cluster ist eine Ressource ARM, damit Sie es wie andere ARM Ressourcen in Azure modellieren können.
* **Integration in Azure-Infrastruktur** Service Fabric-Koordinaten mit der zugrunde liegenden Azure Infrastruktur für Betriebssystem, Netzwerk und andere Upgrades, Verfügbarkeit und Zuverlässigkeit der Anwendung.  
* **Diagnose:** Azure bieten wir Integration Azure Diagnostics und Protokollanalyse.
* **Automatische Skalierung:** Für Cluster in Azure Funktionalität wir integrierte automatische Skalierung durch Virtual Machine Scale-Sets. In lokalen und andere Cloud-Umgebung müssen Sie eigene automatische Skalierung Feature oder Skalierung manuell über die Fabric Service stellt APIs Skalierung Cluster erstellen.

## <a name="next-steps"></a>Nächste Schritte
Erstellen Sie einen Cluster virtuellen Computern oder Computern mit Windows Server: [Service Fabric Clustererstellung für Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Erstellen eines Clusters auf VMs oder Linux-Computern: [Service Fabric unter Linux](service-fabric-linux-overview.md)
