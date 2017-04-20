<properties
   pageTitle="Azure-Architektur Referenz - IaaS: Implementierung einer zwischen Azure und Internet | Microsoft Azure"
   description="Zum Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Internetzugriff in Azure."
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

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>Implementierung einer zwischen Azure und dem Internet

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt die optimale Methoden zum Implementieren eines sicheren Hybrid, das lokale Netzwerk erweitert, die Datenverkehr aus dem Internet Netzwerk in Azure akzeptiert. Diese Referenzarchitektur erweitert die Architektur gemäß Artikel [Implementierung einer zwischen Azure und Rechenzentrum lokalen][implementing-a-secure-hybrid-network-architecture]. Es wird empfohlen, dieses Dokument lesen und verstehen dieser Verweis Architektur vor dem Lesen dieses Dokuments.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. Diese Referenzarchitektur verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt. 

Normale Anwendungsfälle für diese Architektur gehören:

- Hybrid Applications, Arbeitslasten Ausführen teilweise lokalen, und teilweise in Azure.

- Azure-Infrastruktur, der eingehende Datenverkehr vom lokalen und dem Internet weiterleitet.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm zeigt wichtigen Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm wird auf der Seite "DMZ – Public".

[! [0]][0]

- **Öffentliche IP-Adresse (PIP).** Dies ist die IP-Adresse des öffentlichen Endpunkts. Externe Benutzer, die mit dem Internet verbunden können das System über diese Adresse zugreifen.

- **Virtuelles Netzwerkgerät (NVA).**  NVA ist ein allgemeiner Begriff, der eine VM Aufgaben wie zulassen oder Verweigern des Zugriffs als eine Firewall, Optimierung WAN-Operationen (einschließlich Netzwerk), benutzerdefinierte routing oder andere Netzwerkfunktionen beschreibt.

- **Azure Lastenausgleich.** Alle eingehende Anfragen aus dem Internet durch dieses System zum Lastenausgleich geleitet und an NVAs in der öffentlichen DMZ eingehende Subnetz.

- **Öffentliche DMZ eingehende Subnetz.** Diesem Subnetz akzeptiert Anfragen von Azure Lastenausgleich. Anfragen werden an einen NVAs in der DMZ übergeben.

- **Öffentliche DMZ ausgehende Subnetz.** Anfragen, die von der NVA genehmigt durchlaufen dieses Subnetz internen Lastenausgleich für die Webebene.

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure-Ressourcen-Manager um die Referenzarchitektur installieren, die diese Empfehlung folgt bereitgestellt. Wenn Sie eigene Referenzarchitektur erstellen diese Empfehlung Sie befolgen nur eine bestimmte Anforderung, die Empfehlung nicht passen.

### <a name="nva-recommendations"></a>Empfohlene NVA

Implementieren Sie einen Satz von NVAs für Datenverkehr im Internet und weitere Datenverkehr vor Ort. Es ist ein Sicherheitsrisiko nur einen Satz von NVAs für beide verwenden, da dadurch keine Sicherheitszaun des Netzwerkverkehrs zwischen den beiden bietet. Es ist ein Vorteil dieses Design zu verwenden, da die Komplexität verringert der Sicherheitsregeln überprüfen und verdeutlicht die Regeln für jede eingehende netzwerkanforderung entsprechen. Eine Gruppe von NVAs implementiert beispielsweise Regeln für Datenverkehr im Internet und einen anderen Satz von NVAs implementieren Regeln für vor-Ort-Datenverkehr.

Einschließen eine Ebene 7 NVA zu beenden anwendungsverbindungen Ebene NVA mit den Back-End-Ebenen. Dies garantiert symmetrische Konnektivität, Antwortverkehr von Back-End-Tiers durch NVA zurückgegeben werden.  

### <a name="public-load-balancer-recommendations"></a>Öffentliche Load Balancer recommendations ###

Skalierbar und Verfügbarkeit bereitstellen die öffentliche DMZ eine [Verfügbarkeit] eingehende NVAs[ availability-set] und einem [Internet facing Lastenausgleich] [ load-balancer] auf NVAs in den Verfügbarkeit Internetanfragen verteilt.  

Konfigurieren Sie Lastenausgleich um Anfragen nur an den Ports für Internet-Datenverkehr. Z. B. beschränken Sie auf Port 80 eingehenden HTTP-Anfragen und eingehende HTTPS-Anfragen an Port 443.

## <a name="scalability-considerations"></a>Aspekte der Skalierung

Entwerfen der Infrastruktur mit einer Internetschnittstelle Lastenausgleich vor eingehenden öffentlichen DMZ-Subnetz an. Selbst wenn Ihre Architektur anfangs ein NVA erfordert, werden leichter auf mehrere NVAs in Zukunft skalieren, wenn der Lastenausgleich mit Internetzugriff bereits bereitgestellt ist.

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Der Lastenausgleich mit Internetzugriff benötigt jedes NVA in der öffentlichen DMZ eingehende Subnetz [Integritätstest]implementieren[lb-probe]. Ein Health-Prüfpunkt, der nicht reagiert auf diesen Endpunkt als verfügbar und Lastenausgleich leitet Anfragen an andere NVAs in der gleichen Verfügbarkeit. Beachten Sie, dass alle NVAs nicht reagieren, die Anwendung nicht funktioniert, daher es wichtig ist, zu überwachen, wenn die Anzahl der fehlerfreien NVA Instanzen Schwellenwert alert DevOps konfiguriert.

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

Einschränken der Überwachung und Management-Funktionen für den eingehenden öffentlichen DMZ NVA des aus Sprung im Subnetz Management nur auf Anfragen reagieren. Wie bei der [Implementierung einer zwischen Azure und Rechenzentrum lokalen] [ implementing-a-secure-hybrid-network-architecture] dokumentieren, definieren Sie eine Netzwerk-Route vom lokalen Netzwerk über das Gateway auf das Springen in Management Subnetz Zugriff einschränken.

Wenn Gateway-Konnektivität Ihres lokalen Netzwerks in Azure ausfällt, können Sie weiterhin im Feld Gehe zu erreichen, durch Bereitstellung eines PIPS zu dem Sprung und Remoting in aus dem Internet hinzufügen.

## <a name="security-considerations"></a>Sicherheitsaspekte

Diese Referenzarchitektur implementiert mehrere Sicherheitsstufen:

- Der Lastenausgleich mit Internetzugriff leitet Anfragen an die NVAs im eingehenden öffentlichen DMZ Subnetz nur und nur an den Ports für die Anwendung erforderlich.

- NSG Regeln für das ein- und ausgehende öffentliche DMZ-Subnetz verhindern, dass die NVAs der Regeln NSG außerhalb Anfragen blockieren beeinträchtigt.

- Die NAT-routing-Konfiguration für die NVAs leitet eingehende Anfragen auf Port 80 und Port 443 Web-Tier-Lastenausgleich, aber auf andere Ports ignoriert.

Beachten Sie, dass alle eingehende Anfragen an allen Anschlüssen protokollieren soll. Überwachen Sie regelmäßig Protokolle auf Anforderungen erwarteten Parameter wie diese Einbruchsversuche hindeuten.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Mithilfe von NSGs an Block/Datenverkehr zwischen Anwendungsebenen

Alle Stufen Web, Geschäfts- und Einschränken des Datenverkehrs zwischen diesen NSGs verwenden. Also Geschäftsebene mithilfe einer NSG sämtlichen Verkehr blockieren, die von der Webebene stammen nicht und Datenebene verwendet eine NSG um gesamten Verkehr zu blockieren, die nicht auf der Geschäftsebene stammen. Haben Sie bezüglich NSG Regeln umfassenderen Zugriff auf diesen Ebenen erweitern, wiegen Sie diese Vorschriften gegen Sicherheitsrisiken. Jedes neue eingehende Weg Chance eine versehentlich oder absichtlich Daten Anwendung oder absichtlich herbeigeführten Schäden.

## <a name="solution-deployment"></a>Bereitstellung der Lösung

Bereitstellung für eine Referenzarchitektur, die diese Empfehlung implementiert ist auf Github verfügbar. Diese Referenzarchitektur enthält ein virtuelles Netzwerk (VNet), Netzwerk-Sicherheitsgruppe (NSG), Lastenausgleich und zwei virtuellen Maschinen (VMs).

Die Referenzarchitektur kann mit Windows oder Linux VMs anhand der Anweisungen bereitgestellt werden: 

1. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Sobald die Verknüpfung im Azure-Portal geöffnet hat, geben Sie Werte für einige der Einstellungen: 
    - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-public-dmz-network-rg` im Textfeld.
    - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
    - Wählen Sie aus dem Dropdown-Feld, **Windows** oder **Linux** **Betriebssystem** .
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie, bis die Bereitstellung abgeschlossen.

4. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Sobald die Verknüpfung im Azure-Portal geöffnet hat, geben Sie Werte für einige der Einstellungen: 
    - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-public-dmz-wl-rg` im Textfeld.
    - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
    - Klicken Sie auf die Schaltfläche **Einkauf** .

6. Warten Sie, bis die Bereitstellung abgeschlossen.

7. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Sobald die Verknüpfung im Azure-Portal geöffnet hat, geben Sie Werte für einige der Einstellungen: 
    - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Vorhandene verwenden** und geben `ra-public-dmz-network-rg` im Textfeld.
    - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
    - Klicken Sie auf die Schaltfläche **Einkauf** .

9. Warten Sie, bis die Bereitstellung abgeschlossen.

10. Parameterdateien enthalten hartcodierte Administrator-Benutzernamen und das Kennwort für alle virtuellen Computer, und es wird dringend empfohlen, dass Sie beide sofort ändern. Für jeden virtuellen Computer in der Bereitstellung im Azure-Portal wählen Sie aus und dann auf **Kennwort zurücksetzen** **Support + Problembehandlung** Blatt. Wählen Sie **Kennwort zurücksetzen** im Feld Dropdown- **Modus** und anschließend einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um beizubehalten.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Sichere Hybrid-Netzwerkarchitektur"
