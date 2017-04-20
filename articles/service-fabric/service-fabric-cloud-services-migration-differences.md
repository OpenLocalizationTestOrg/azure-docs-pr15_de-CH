<properties
   pageTitle="Unterschiede zwischen Cloud-Services und Service | Microsoft Azure"
   description="Eine grundlegende Übersicht über Migration Anwendung von Clouddiensten Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Erfahren Sie mehr über die Unterschiede zwischen Cloud-Services und Service vor der Migration Applications.
Microsoft Azure Service Fabric ist der nächsten Generation Cloud Anwendungsplattform für hochgradig skalierbare und zuverlässige verteilte Anwendung. Viele neue Features zur Verpackung bereitstellen, aktualisieren und Verwalten von verteilten Cloudanwendungen eingeführt. 

Dies ist ein Leitfaden zum Migrieren von Clientanwendungen Clouddienste Service Fabric. Es konzentriert sich hauptsächlich auf Architektur und design Unterschiede zwischen Cloud-Services und Service.
 
## <a name="applications-and-infrastructure"></a>Applikationen und Infrastruktur

Grundlegende Unterschiede zwischen Cloud-Services und Service ist die Beziehung zwischen VMs Arbeitslasten und Applikationen. Eine Arbeitslast wird als Code schreiben, um eine bestimmte Aufgabe ausführen oder eine definiert.
 
 - **Cloud-Dienste geht wie VMs bereitstellen.** Der geschriebene Code ist eng eine VM-Instanz oder ein Web Worker-Rolle. Zum Bereitstellen einer Arbeitslast Cloud-Dienste ist eine oder mehrere VM-Instanzen, die Arbeitslast ausgeführt. Es gibt keine Trennung und VMs und so gibt es keine formale Definition einer Anwendung. Eine Anwendung kann als Web oder Worker-Rolle Instanzen innerhalb einer Bereitstellung Cloud-Services oder einer ganzen Bereitstellung Cloud-Dienste vorstellen. In diesem Beispiel wird eine Anwendung als mehrere Instanzen angezeigt.
 
![Cloud Services Applications und Topologie][1]

 - **Service ist, ist zum Bereitstellen von Clientanwendungen vorhandenen VMs oder Service Fabric-Computern unter Windows oder Linux.** Die Dienste, die Sie schreiben sind vollständig getrennt von der zugrunde liegenden Infrastruktur entfernt von Service Fabric-Anwendungsplattform abstrahiert wird, damit eine Anwendung in mehreren Umgebungen bereitgestellt werden kann. Eine Arbeitslast in Service Fabric heißt "Service" und einen oder mehrere Dienste in einer formal definiert Anwendung, die auf Service Fabric-Anwendungsplattform gruppiert. Multifunktionsgeräte können einen Service Fabric-Cluster bereitgestellt werden.
 
![Service Fabric Applikationen und Topologie][2]
 
Fabric Service selbst ist ein Plattform-Anwendungsebene, die auf Windows oder Linux Clouddienste ein System für die Bereitstellung von Azure verwalteten VMs mit Arbeitslasten zugeordnet.
Das Anwendungsmodell Service Fabric ist eine Reihe von Vorteilen:

 - Schnelle Bereitstellungszeiten. VM-Instanzen erstellen kann zeitaufwändig sein. Service Fabric werden VMs nur einmal zu einem Cluster, die Service Fabric-Anwendungsplattform hostet bereitgestellt. Von diesem Zeitpunkt an können Pakete sehr schnell zum Cluster bereitgestellt werden.
 - HD-hosting. Cloud-Services hostet eine Worker-Rolle VM eine Arbeitslast. In Service Fabric sind getrennt von VMs, die ausgeführt werden, was bedeutet, dass eine große Anzahl von Anträgen zu wenige VMs bereitstellen können die Gesamtkosten für größere Installationen senken können.
 - Plattform überall ausführen kann Service Stoff hat Windows Server oder Linux-Computer Azure oder lokal. Die Plattform bietet eine Abstraktionsschicht über die zugrunde liegende Infrastruktur Ihrer Anwendung kann auf unterschiedlichen Umgebungen ausführen. 
 - Verteilte Anwendung. Service Fabric ist eine Plattform, jedoch nicht nur Hosts distributed Applications hilft auch verwalten, unabhängig von der Hosting-VM-Lebenszyklus oder Lebenszyklus Computer.

## <a name="application-architecture"></a>Anwendungsarchitektur

Die Architektur der Cloud Services-Anwendung enthält normalerweise zahlreiche externe dienstabhängig wie Service Bus, Azure Tabelle BLOB-Speicher, SQL, Redis und anderen Zustand und Daten von einer Anwendung und Kommunikation zwischen Web- und Workerrollen in einer Bereitstellung Cloud-Dienste verwalten. Ein Beispiel für eine vollständige Clouddienste Anwendung könnte aussehen:  

![Cloud-Architektur][9]

Service Fabric Applikationen können auch dieselbe externe Dienste in einer vollständigen Anwendung verwenden. In diesem Beispiel Clouddienste Architektur ist einfachste Migrationspfad von Cloud-Diensten mit Service Fabric ersetzt nur die Bereitstellung Cloud-Diensten mit einer Fabric Service halten der Gesamtarchitektur identisch. Web- und Workerrollen können Service Fabric zustandsloser Dienste mit minimalem Code portiert werden.

![Service Fabric-Architektur nach einfachen migration][10]

In dieser Phase sollte das System weiterhin unverändert. Nutzen Sie Service Fabric statusbehaftete Features, externe Speicher kann wie Stateful gegebenenfalls verinnerlicht werden. Dies ist stärker als eine einfache Migration von Web- und Workerrollen statusfreie Service Fabric-Services Bedarf benutzerdefinierte Dienste, die entsprechende Funktionalität Ihrer Anwendung bereitzustellen, wie externe Dienste schreiben. Dies bietet die folgenden Vorteile: 

 - Entfernen externe Abhängigkeiten 
 - Vereinheitlichung von Bereitstellung, Management und Upgrade-Modelle. 
 
Diese Dienste verinnerlichen resultierende Architektur Beispiel könnte wie folgt aussehen:

![Nach der vollständigen Migration Service Fabric-Architektur][11]

## <a name="communication-and-workflow"></a>Kommunikation und workflow

Meisten Cloud-Dienst bestehen aus mehreren Ebenen. Ebenso besteht eine Service Fabric-Anwendung mehrere Dienste (normalerweise viele Dienste). Zwei häufige Kommunikation sind direkte Kommunikation über ein externes dauerhaften Speicher.

### <a name="direct-communication"></a>Direkte Kommunikation

Direkte Kommunikation können direkt über Endpunkt jeder Schicht ausgesetzt Ebenen kommunizieren. Statusfreie Umgebung oder Cloud-Dienste, diese Mittel Auswählen einer Instanz einer VM-Rolle nach dem Zufallsprinzip Roundrobin Saldo geladen und direkt mit dem Endpunkt.

![Cloud-Dienste direkte Kommunikation][5]

 Direkte Kommunikation ist eine allgemeine Kommunikationsmodell Service Fabric. Der Hauptunterschied zwischen Service und Cloud-Dienste ist, in einer VM Verbindung Clouddiensten während Fabric Service Dienst herstellen. Dies ist eine wichtige Unterscheidung für einige Gründe:

 - Dienste in Service Fabric werden nicht auf die VMs gebunden, die sie hosten. Dienste im Cluster bewegen kann und tatsächlich sollen aus verschiedenen Gründen verschieben: Ressource Netzwerklastenausgleich, Failover, Anwendung und Infrastruktur-Upgrades und Platzierung oder Last Einschränkungen. Dies bedeutet, dass eine Dienstinstanz Adresse jederzeit ändern kann. 
 - VM in Service Fabric kann mehrere Dienste mit eindeutigen Endpunkten hosten.

Fabric Service bietet einen Service Discovery-Mechanismus aufgerufen Namensdienst, die aufzulösende Endpunktadressen Dienste verwendet werden kann. 

![Direkte Service Fabric][6]

### <a name="queues"></a>Warteschlangen

Allgemeine Kommunikationsmechanismus zwischen den Ebenen statusfreie Umgebung wie Cloud-Services ist eine Warteschlange externen Speicher verwenden, um Arbeitsaufgaben aus einer Ebene in eine andere dauerhaft speichern. Ein häufiges Szenario ist eine Webebene, die Aufträge an eine Azure-Warteschlange oder Service Bus, Arbeitskraft Instanzen entfernen und die Aufträge verarbeiten können.

![Cloud-Services Warteschlange Kommunikation][7]

Gleiche Kommunikationsmodell kann Fabric Service verwendet werden. Dies kann hilfreich sein bei der Migration einer vorhandenen Anwendung Cloud-Dienste mit Service Fabric. 

![Direkte Service Fabric][8]
 
## <a name="next-steps"></a>Nächste Schritte

Der einfachste Migrationspfad von Clouddiensten Service Fabric ist nur die Bereitstellung Cloud-Dienste mit einer Fabric Service halten der Gesamtarchitektur der Anwendung ungefähr gleich ersetzen. Folgende Artikel enthält eine Anleitung zum Konvertieren eines Web oder Worker-Rolle eine statusfreie Service Fabric-Service Hilfe.

 - [Einfache Migration: Konvertieren einer Web oder Worker-Rolle in eine statusfreie Service Fabric Service](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
