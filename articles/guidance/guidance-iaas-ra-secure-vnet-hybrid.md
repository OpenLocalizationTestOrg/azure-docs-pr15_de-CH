<properties
   pageTitle="Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure | Microsoft Azure"
   description="Zum Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>Implementierung einer zwischen Azure und lokalen Datencenter

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt die optimale Methoden zum Implementieren eines sicheren Hybrid, die einem lokalen Netzwerk in Azure erweitert. Diese Referenzarchitektur implementiert eine DMZ zwischen dem lokalen Netzwerk und ein Azure virtuelles Netzwerk mit benutzerdefinierten Routen (Decision). Die DMZ umfasst hoch verfügbare virtuelle Netzwerkgeräte (NVAs), die Sicherheitsfunktionen wie Firewalls und Paketinspektion implementieren. Alle ausgehender Datenverkehr von der VNet ist Kraft über das lokale Netzwerk mit dem Internet getunnelt überwacht werden können. 

Diese Architektur erfordert eine Verbindung zum lokalen Datencenter implementiert ein [VPN-Gateway]mit[ra-vpn], oder eine [ExpressRoute] [ ra-expressroute] Verbindung.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. Diese Referenzarchitektur verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

Normale Anwendungsfälle für diese Architektur gehören:

- Hybrid Applications, Arbeitslasten Ausführen teilweise lokalen, und teilweise in Azure.

- Azure-Infrastruktur, der eingehende Datenverkehr vom lokalen und dem Internet weiterleitet.

- Clientanwendungen müssen ausgehenden Datenverkehr überwachen. Dies ist häufig vorgeschrieben viele kommerzielle Systeme und Veröffentlichung privater Informationen zu helfen.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm zeigt wichtigen Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm wird auf der Seite "DMZ – privat".

[! [0]][0]

- **Lokalen Netzwerk.** Netzwerk Computer und Geräte über einen privaten lokalen Netzwerk in einer Organisation implementiert.

- **Azure virtual Network (VNet).** Das VNet hostet die Anwendung und anderen Ressourcen in der Cloud.

- **Gateway.** Das Gateway bietet Konnektivität zwischen Router im lokalen Netzwerk und dem VNet.

- **Virtuelles Netzwerkgerät (NVA).** NVA ist ein allgemeiner Begriff, der eine VM Aufgaben wie zulassen oder Verweigern des Zugriffs als eine Firewall, Optimierung WAN-Operationen (einschließlich Netzwerk), benutzerdefinierte routing oder andere Netzwerkfunktionen beschreibt. 

- **Webebene Geschäftsebene und Data Tier Subnetze.** Sind Subnetze Hosten virtueller Computer und Dienste, die eine Beispiel 3-Tier-Anwendung in der Cloud zu implementieren. Siehe [Windows-VMs für N-Tier-Architektur in Azure ausgeführt] [ ra-n-tier] Weitere Informationen.

- **Benutzerdefinierte Routen (UDR).** [Benutzerdefinierte Routen] [ udr-overview] definieren Sie den Datenfluss von IP-Datenverkehr in Azure VNets. 

> [AZURE.NOTE] Je nach der VPN-Verbindung konfigurieren Sie die Border Gateway Protocol (BGP) Routen als Alternative mit Decision die Weiterleitungsregeln implementieren, die Datenverkehr direkt über das lokale Netzwerk sichern.

- **Management-Subnetz.** Diesem Subnetz enthält VMs, die Management- und Überwachungsfunktionen für die Komponenten in den VNet implementieren. 

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure-Ressourcen-Manager um die Referenzarchitektur installieren, die diese Empfehlung folgt bereitgestellt. Wenn Sie eigene Referenzarchitektur erstellen diese Empfehlung Sie befolgen nur eine bestimmte Anforderung, die Empfehlung nicht passen.

### <a name="rbac-recommendations"></a>RBAC-Empfehlung

Um Ressourcen in Ihrer Anwendung mehrere RBAC-Rollen zu erstellen. Erstellen einer DevOps [für benutzerdefinierte Rollen] [ rbac-custom-roles] mit Berechtigungen zum Verwalten der Infrastruktur für die Anwendung. Erstellen einer zentralen IT-Administrator [benutzerdefinierte Rolle] [ rbac-custom-roles] zum Verwalten von Netzwerkressourcen und eine separate Sicherheit IT-Administrator [benutzerdefinierte Rolle] [ rbac-custom-roles] sichere Netzwerkressourcen wie die NVAs verwalten. 

Die Rolle DevOps gehören Berechtigungen zum Bereitstellen von Anwendungskomponenten sowie überwachen und VMs neu starten. Die zentrale IT-Administratorrolle gehören Berechtigungen für Netzwerkressourcen zu überwachen. Keine dieser Rollen müssen Zugang zu NVA Ressourcen, wie dies auf die Sicherheit IT-Administratorrolle beschränkt werden soll.

### <a name="resource-group-recommendations"></a>Ressource Gruppe recommendations

Azure Ressourcen wie VMs, VNets und Lastenausgleich können problemlos in Ressourcengruppen gruppieren verwaltet werden. Jede Ressourcengruppe den Zugriff einschränken können dann über Rollen zuweisen.

Wir empfehlen das Erstellen der folgenden:

- Eine Ressourcengruppe mit Subnetzen (ausgenommen die VMs), NSGs und Gatewayressourcen für die Verbindung mit dem lokalen Netzwerk. Die zentrale IT-Administratorrolle dieser Ressourcengruppe zuweisen.

- Eine Ressourcengruppe, die VMs für NVAs (einschließlich Lastenausgleich), im Feld Gehe zu anderen Management VMs und UDR für das gatewaysubnetz,, das gesamte Datenverkehr über den NVAs zwingt. Diese Ressourcengruppe Sicherheit IT-Administratorrolle zuweisen.

- Getrennte Ressourcengruppen für jede Anwendungsebene, die Lastenausgleich und VMs enthalten. Beachten Sie, dass diese Ressourcengruppe Subnetze für jede Ebene sollte nicht. DevOps-Rolle dieser Ressourcengruppe zuweisen.

### <a name="virtual-network-gateway-recommendations"></a>Virtuelles Netzwerk-Gateways Vorschläge

Lokalen Datenverkehr übergibt das VNet über ein virtuelles Netzwerk-Gateway. Wir empfehlen eine [Azure VPN-Gateway] [ guidance-vpn-gateway] oder ein [Gateway Azure ExpressRoute][guidance-expressroute]. 

### <a name="nva-recommendations"></a>Empfohlene NVA

NVAs bieten verschiedene Services für das Verwalten und Überwachen des Netzwerkverkehrs. Azure Marketplace bietet verschiedene Drittanbieter-NVAs, einschließlich:

- [Barracuda Web Application Firewall] [ barracuda-waf] und [Barracuda NextGen Firewall][barracuda-nf]

- [Geschlossene Netzwerke VNS3 Firewall/Router/VPN][vns3]

- [Fortinet FortiGate-VM][fortinet]

- [SecureSphere Web Application Firewall][securesphere]

- [DenyAll Web Application Firewall][denyall]

- [Check Point vSEC][checkpoint]

Wenn keines dieser Drittanbieter-NVAs Ihren Anforderungen können Sie eine benutzerdefinierte NVA mit VMs erstellen. Ein Beispiel eines benutzerdefinierten NVAs finden Sie in der DMZ in diese Referenzarchitektur, die folgende Funktionen implementiert:

- Datenverkehr über [IP-Weiterleitung] weitergeleitet[ ip-forwarding] auf NVA-Netzwerkkarten.

- Datenverkehr ist zulässig, NVA passieren, nur, wenn es angemessen ist. Jede VM NVA in der Reference Architecture ist ein einfach Linux Router mit eingehenden Datenverkehr auf Netzwerk-Schnittstelle *eth0*und ausgehenden Datenverkehr übereinstimmenden Regeln Skripte über Netzwerk-Schnittstelle *eth1*verteilt. 

- Datenverkehr an das Management Subnetz weitergeleitet verläuft nicht durch die NVAs und die NVAs können nur aus dem Management Subnetz konfiguriert werden. Ggf. Datenverkehr an das Subnetz Management wird durch die NVAs geleitet ist keine Route zum Subnetz Management der NVAs beheben, wenn sie ausfällt.  

- Die VMs für NVA gehören eine Verfügbarkeit hinter einem Lastenausgleich. UDR im gatewaysubnetz leitet Anfragen zum Lastenausgleich NVA.

Eine weitere Empfehlung berücksichtigen verbindet mehrere NVAs nacheinander mit jeder Aufgabe eine spezialisierte NVA. Dadurch wird jede Sicherheitsfunktion jeweils pro NVA verwaltet werden. Beispielsweise kann eine Firewall implementieren NVA Serie mit einer NVA Identitätsdienste ausgeführt befinden. Der Nachteil zur Vereinfachung des Managements ist das Hinzufügen zusätzlicher Netzwerk-Hops, die Latenz, also sicherstellen, dass dies die Leistung der Anwendung nicht.

### <a name="nsg-recommendations"></a>NSG recommendations

VPN-Gateway stellt eine öffentliche IP-Adresse für die Verbindung mit dem lokalen Netzwerk. Empfohlen Erstellen einer Netzwerk-Sicherheitsgruppe (NSG) für eingehenden NVA Subnetz Durchführungsbestimmungen alle Datenverkehr nicht aus dem lokalen Netzwerk blockieren.

Es wird empfohlen, NSGs für jedes Subnetz eine zweite Ebene Schutz vor eingehenden Datenverkehr umgeht eine falsch konfigurierte oder deaktiviert NVA zu implementieren. Web-Tier-Subnetz in die Referenzarchitektur implementiert beispielsweise eine NSG mit einer Regel ignorieren alle Anfragen als die im lokalen Netzwerk (192.168.0.0/16), die VNet oder eine andere Regel, die alle Anträge nicht auf Port 80 ignoriert. 

### <a name="internet-access-recommendations"></a>Internet Access recommendations

[Force-Tunnel] [ azure-forced-tunneling] alle ausgehenden Internetdatenverkehr über das lokale Netzwerk mit dem Standort-zu-Standort-VPN-Tunnel und Route mit Netzwerk-Adresse Umsetzung (NAT). Dies wird sowohl verhindert unbeabsichtigte austreten von vertraulichen Informationen in die Datenebene gespeichert und auch Inspektion und Überwachung sämtlicher ausgehender Datenverkehr.

> [AZURE.NOTE] Blockieren Sie Internet-Datenverkehr aus dem Web, Unternehmen und Anwendungsebenen nicht vollständig. Wenn diese Ebenen auf öffentliche IP-Adressen für VM Diagnoseprotokoll Azure PaaS-Dienste verwenden, Herunterladen von VM-Erweiterungen und andere Funktionen. Azure-Diagnose muss Komponenten lesen und Schreiben in ein Internet-abhängige Azure Storage-Konto.

Weiter wird empfohlen, überprüfen Sie, ob ausgehende Internetverkehr Kraft getunnelt werden. Wenn Sie eine VPN-Verbindung mit dem [RAS-Dienst] verwenden[ routing-and-remote-access-service] auf einem lokalen Server verwenden Sie ein Tool wie [WireShark] [ wireshark] oder [Microsoft Message Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Management Subnetz recommendations

Management-Subnetz enthält das springen Management und Überwachungsfunktionen ausgeführt wird. Implementieren Sie Folgendes im Feld Gehe zu:
- Erstellen Sie eine öffentliche IP-Adresse im Feld Gehe zu. 
- Erstellen Sie eine Route zum Sprung das eingehende Gateway und implementieren eine NSG Management Subnetz aus zugelassene Route nur auf Anfragen reagieren.
- Alle sichere Management-Aufgaben auf im Feld Gehe zu beschränken.

### <a name="nva-recommendations"></a>Empfohlene NVA

Einschließen eine Ebene 7 NVA zu beenden anwendungsverbindungen Ebene NVA mit den Back-End-Ebenen. Dies garantiert symmetrische Konnektivität, Antwortverkehr von Back-End-Tiers durch NVA zurückgegeben werden.

## <a name="scalability-considerations"></a>Aspekte der Skalierung

Die Referenzarchitektur implementiert ein Lastenausgleich lokalen Netzwerkverkehr auf NVA Geräte weiterleiten. Wie bereits erwähnt, die NVA VMs Netzwerk Datenverkehr Routingregeln ausführen und eine [Verfügbarkeit]bereitgestellt[availability-set]. Dadurch können Sie den Durchsatz der NVAs mit der Zeit zu überwachen und NVA Geräte in Antwort zu laden.

Standard-SKU VPN-Gateway unterstützt konstanter Datendurchsatz von bis zu 100 Mbit/s. Hohe Leistung SKU bietet bis zu 200 MB. Für höhere Bandbreiten in Erwägung ziehen, ein ExpressRoute-Gateway. ExpressRoute bietet bis zu 10 Gbit/s Bandbreite niedriger Latenz als eine VPN-Verbindung.

> [AZURE.NOTE] [Implementieren einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-] Artikeln[ guidance-vpn-gateway] und [Implementieren einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute] [ guidance-expressroute] Probleme Erweiterbarkeit Azure Gateways beschrieben.

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Die Referenzarchitektur implementiert ein Lastenausgleich verteilen Anfragen von lokalen Pool NVA Geräte in Azure. Die NVA VMs Netzwerk Datenverkehr Routingregeln ausführen und eine [Verfügbarkeit]bereitgestellt[availability-set]. Lastenausgleich regelmäßig fragt einen Integritätstest für jede NVA implementiert und nicht reagiert NVAs aus dem Pool entfernt. 

Verwenden Sie Azure ExpressRoute Verbindung zwischen VNet und lokalen Netzwerk [Konfigurieren Sie eine VPN-Gateway zu Failover] zu[ guidance-vpn-failover] wird die ExpressRoute-Verbindung nicht verfügbar.

Bestimmte Informationen auf Verfügbarkeit für VPN und ExpressRoute finden Sie unter [Implementieren einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-] Artikeln[ guidance-vpn-gateway] und [Implementieren einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute][guidance-expressroute].

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

Alle Anwendung und ressourcenüberwachung von Management-Subnetz im Feld Gehe zu durchzuführen. Je nach Bedarf Anwendung möglicherweise zusätzliche Überwachung Ressourcen im Subnetz Management hinzuzufügen, aber wieder alle diese zusätzlichen Ressourcen sollten zugänglich im Feld Gehe zu.

Wenn Gateway-Konnektivität Ihres lokalen Netzwerks in Azure ausfällt, können Sie weiterhin im Feld Gehe zu erreichen, durch Bereitstellung eines PIPS zu dem Sprung und Remoting in aus dem Internet hinzufügen.

Jede Ebene Subnetz Referenzarchitektur NSG Regeln geschützt und ggf. zum Erstellen einer Regel um Port 3389 für RDP-Zugriff auf Windows-VMs oder Port 22 für SSH-Zugriff auf Linux VMs zu öffnen. Andere Management und Überwachungstools erfordern Regeln zusätzliche Ports geöffnet.

Wenn Sie ExpressRoute verwenden, um die Konnektivität zwischen lokalen Datencenter und Azure bereitzustellen, verwenden Sie [Azure Konnektivität Toolkit (AzureCT)] [ azurect] überwachen und Verbindungsprobleme.

> [AZURE.NOTE] Weitere Informationen speziell zur Überwachung und Verwaltung VPN- und ExpressRoute in den Artikeln [Implementieren einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-] [ guidance-vpn-gateway] und [Implementieren einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute][guidance-expressroute].

## <a name="security-considerations"></a>Sicherheitsaspekte

Diese Referenzarchitektur implementiert mehrere Sicherheitsstufen: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Alle lokalen Benutzeranfragen durch NVA Routing

UDR im gatewaysubnetz blockiert alle Benutzeranfragen als die lokal. UDR übergibt Anfragen an die NVAs im privaten DMZ-Subnetz zulässig und werden diese Anfragen auf an die Anwendung übergeben, wenn sie durch die NVA Regeln zulässig sind. Andere Routen können der UDR hinzugefügt werden jedoch sicherstellen, dass sie nicht versehentlich NVAs oder administrativen Datenverkehr blockieren zur Verwaltung Subnetz umgehen.

Lastenausgleich vor dem NVAs fungiert auch als ein auf Ports, die nicht im Lastenausgleich Regeln ignorieren. Der Lastenausgleich Referenzarchitektur überwachen nur für HTTP-Anfragen auf Port 80 und Port 443 HTTPS-Anfragen. Zusätzlichen Regeln hinzugefügt, die Lastenausgleich müssen dokumentiert und sollte der Datenverkehr überwacht, um sicherzustellen, dass keine Sicherheitsprobleme.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Mithilfe von NSGs an Block/Datenverkehr zwischen Anwendungsebenen

Alle Stufen Web, Geschäfts- und Einschränken des Datenverkehrs zwischen diesen NSGs verwenden. Also Geschäftsebene mithilfe einer NSG sämtlichen Verkehr blockieren, die von der Webebene stammen nicht und Datenebene verwendet eine NSG um gesamten Verkehr zu blockieren, die nicht auf der Geschäftsebene stammen. Haben Sie bezüglich NSG Regeln umfassenderen Zugriff auf diesen Ebenen erweitern, wiegen Sie diese Vorschriften gegen Sicherheitsrisiken. Jedes neue eingehende Weg Chance eine versehentlich oder absichtlich Daten Anwendung oder absichtlich herbeigeführten Schäden.

### <a name="devops-access"></a>DevOps Zugriff

DevOps für jede Ebene mit [RBAC] ausführen können Vorgänge beschränken[ rbac] Berechtigungen verwalten. Beim Erteilen von Berechtigungen verwenden das [Prinzip der geringsten Berechtigung][security-principle-of-least-privilege]. Melden Sie alle Verwaltungsvorgänge und führen Sie regelmäßige Überwachung durch, um sicherzustellen, dass die geplanten konfigurationsänderungen.

> [AZURE.NOTE] Ausführliche Informationen, Beispiele und Szenarien zum Verwalten von Netzwerkdiensten mit Azure [Microsoft Cloud-Dienste und Sicherheit]finden Sie unter[cloud-services-network-security]. Ausführliche Informationen zum Schützen von Ressourcen in der Cloud finden Sie unter [Erste Schritte mit Microsoft Azure Security][getting-started-with-azure-security]. Weitere Informationen über eine gatewayverbindung Azure Sicherheitsbedenken finden Sie unter [Implementieren einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-] [ guidance-vpn-gateway] und [Implementieren einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute][guidance-expressroute].

## <a name="solution-deployment"></a>Bereitstellung der Lösung

Bereitstellung für eine Referenzarchitektur, die diese Empfehlung implementiert ist auf Github verfügbar. Diese Referenzarchitektur enthält ein virtuelles Netzwerk (VNet), Netzwerk-Sicherheitsgruppe (NSG), Lastenausgleich und zwei virtuellen Maschinen (VMs).

Die Referenzarchitektur kann mit Windows oder Linux VMs anhand der Anweisungen bereitgestellt werden: 

1. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Sobald die Verknüpfung im Azure-Portal geöffnet hat, geben Sie Werte für einige der Einstellungen: 
    - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-private-dmz-rg` im Textfeld.
    - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie, bis die Bereitstellung abgeschlossen.

4. Parameterdateien enthalten hartcodierte Administrator-Benutzernamen und das Kennwort für alle virtuellen Computer, und es wird dringend empfohlen, dass Sie beide sofort ändern. Für jeden virtuellen Computer in der Bereitstellung im Azure-Portal wählen Sie aus und dann auf **Kennwort zurücksetzen** **Support + Problembehandlung** Blatt. Wählen Sie **Kennwort zurücksetzen** im Feld Dropdown- **Modus** und anschließend einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um beizubehalten.

## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zum Implementieren einer [DMZ zwischen Azure und dem Internet](./guidance-iaas-ra-secure-vnet-dmz.md).
- Informationen Sie zum Implementieren einer [hochverfügbaren Hybrid-Netzwerkarchitektur](./guidance-hybrid-network-expressroute-vpn-failover.md).

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Sichere Hybrid-Netzwerkarchitektur"
