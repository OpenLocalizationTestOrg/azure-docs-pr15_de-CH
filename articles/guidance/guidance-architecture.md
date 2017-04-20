
<properties
   pageTitle="Azure Hinweise | Patterns & Practices | Microsoft Azure"
   description="Azure Referenzarchitekturen"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Azure Referenzarchitekturen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_Dieser Inhalt ist in der Entwicklung. Es empfiehlt, damit wir sie für eine Vorschau verfügbar machen. Wir schätzen Ihr Feedback._

Unsere Referenzarchitekturen werden nach Szenario mit mehreren verwandten gruppiert angeordnet.
Jede einzelne Architektur bietet empfohlenen Vorgehensweisen und Schritte sowie eine ausführbare Komponente, die die Empfehlung verkörpert.
Viele der Architekturen sind progressive Aufbauend auf vorhergehenden Architekturen, die weniger Anforderungen.

## <a name="designing-your-infrastructure-for-resiliency"></a>Entwerfen der Infrastruktur für Stabilität

Diese beginnt mit empfohlenen Vorgehensweisen für optimale VM-Konfiguration und endet mit einer Bereitstellung mit mehreren mit Failover.

- [Windows VM ausgeführte auf Windows Azure](guidance-compute-single-vm.md)
- [Linux VM ausgeführte auf Windows Azure](guidance-compute-single-vm-linux.md)
- [Mehrere VMs für Skalierbarkeits- und ausführen](guidance-compute-multi-vm.md)
- [Ausführen von Windows-VMs für N-Tier-Architektur](guidance-compute-n-tier-vm.md)
- [Laufende Linux VMs für N-Tier-Architektur](guidance-compute-n-tier-vm-linux.md)
- [Windows-VMs in mehrere Bereiche für hohe Verfügbarkeit ausgeführt](guidance-compute-multiple-datacenters.md)
- [Ausführen von Linux VMs in mehreren Regionen für hohe Verfügbarkeit](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Azure herstellen lokalen Netzwerk

Diese Serie beginnt mit Methoden Azure das vorhandene Netzwerk verbinden. Klicken Sie dann erweitert auf an Verfügbarkeit und Sicherheit.

- [Implementieren einer Hybrid-Netzwerkarchitektur mit Azure und lokalen VPN](guidance-hybrid-network-vpn.md)
- [Implementieren einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
- [Implementierung einer hochverfügbaren Hybrid-Netzwerkarchitektur](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Sichern des Netzwerks hybrid

Dieser Serie behandelt bewährte Methoden zum Erstellen von DMZ in Azure sichere Verbindungen aus Ihrem lokalen Rechenzentrum und im Internet.

- [Implementierung einer zwischen Azure und lokalen Datencenter](guidance-iaas-ra-secure-vnet-hybrid.md)
- [Implementierung einer zwischen Azure und dem Internet](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Identität Dienstleistungen

Diese Reihe startet veranschaulichen von Azure Active Directory Benutzerauthentifizierung in Azure. Anschließend wird die Größe des komplexen Szenarien ADDS-Infrastruktur in Azure erweitern und ADFS für Delegierung verwenden.

- [Implementieren von Azure Active Directory](./guidance-identity-aad.md)
- [Erweitern Sie Active Directory-Verzeichnisdienst (ADDS) in Azure](./guidance-identity-adds-extend-domain.md)
- [Erstellen einer Ressourcengesamtstruktur Active Directory Directory Services (ADDS) in Azure](./guidance-identity-adds-resource-forest.md)
- [Implementieren von Active Directory Federation Services (ADFS) in Azure](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Skalierbare Webanwendung Azure PaaS Architektur

Diese Reihe umfasst Vorschläge zum Erstellen von skalierbaren und hochgradig verfügbaren webapps. 

- [Einfache Anwendung](guidance-web-apps-basic.md)
- [Verbesserung der Skalierbarkeit einer Web-Anwendung](guidance-web-apps-scalability.md)
- [Anwendung mit hoher Verfügbarkeit](guidance-web-apps-multi-region.md)
