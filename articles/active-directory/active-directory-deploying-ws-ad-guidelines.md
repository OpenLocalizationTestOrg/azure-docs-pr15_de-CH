<properties
   pageTitle="Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure Virtual Machines | Microsoft Azure"
   description="Wenn Sie wissen wie Active Directory-Domänendienste und AD-Verbunddienste lokal, so funktionieren auf Azure virtuellen Computern."
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure virtuelle Computer

Dieser Artikel beschreibt die wichtigsten Unterschiede zwischen dem Bereitstellen von Windows Server Active Directory-Domänendienste (AD DS) und Active Directory-Verbunddienste (AD FS) auf lokalen und auf Microsoft Azure virtuellen Computern bereitstellen.

## <a name="scope-and-audience"></a>Umfang und Zielgruppe

Der Artikel soll für die bereits mit der Bereitstellung von Active Directory lokal. Er behandelt die Unterschiede zwischen Active Directory Microsoft Azure VMs-Azure virtuelle Netzwerke und herkömmlichen lokalen Active Directory-Infrastrukturen bereitstellen. Azure virtuelle Computer und Azure virtuelle Netzwerke gehören ein Infrastruktur-als-a-Service (IaaS) für Unternehmen in der Cloud computing-Ressourcen nutzen.

Die mit AD Bereitstellung nicht vertraut sind, finden Sie unter [AD DS Deployment Guide](https://technet.microsoft.com/library/cc753963) oder [Planen der Bereitstellung von AD FS](https://technet.microsoft.com/library/dn151324.aspx) entsprechend.

Dieser Artikel setzt voraus, dass der Leser mit den folgenden Konzepten:

- Windows Server AD DS-Bereitstellung und Verwaltung
- Bereitstellung und Konfiguration von DNS zur Unterstützung von Windows Server AD DS-Infrastruktur
- Windows Server AD FS-Bereitstellung und Verwaltung
- Bereitstellen, konfigurieren und Verwalten der relying Party Applications (Websites und Webdienste), die Windows Server AD FS-Tokens von Belegen
- Allgemeine virtuellen Konzepte wie ein virtueller Computer, virtuelle Laufwerke und virtuelle Netzwerke konfigurieren

Dieser Artikel erläutert die Vorschriften für ein Hybrid-Bereitstellungsszenario Windows Server AD DS oder AD FS werden teilweise bereitgestellten lokalen und teilweise auf Azure virtuelle Computer bereitgestellt. Das Dokument deckt zunächst wichtige Unterschiede zwischen Windows Server AD DS und AD FS auf Azure virtuelle Computer lokal und wichtige Entscheidungen Entwurf und Bereitstellung. Der Rest des Papiers erläutert Richtlinien für jeden Entscheidungspunkte ausführlicher und Richtlinien zu verschiedenen Bereitstellungsszenarien.

Dieser Artikel behandelt nicht die Konfiguration von [Azure Active Directory](http://azure.microsoft.com/services/active-directory/)ist ein REST-basierter Dienst Identitätsmanagement und Kontrollfunktionen Zugriff für Cloudanwendungen. Azure Active Directory (Azure AD) und Windows Server AD DS, jedoch sollen gemeinsam bieten eine Identitäts- und Management-Lösung für heutige hybride IT-Umgebung und moderne Anwendung. Zum Verständnis der Unterschiede und Vertrauensstellungen zwischen Windows Server AD DS und Azure AD, Folgendes ein:

1. Möglicherweise ausführen Windows Server AD DS in der Cloud auf Azure virtuellen Maschinen bei Azure Verwendung der lokalen Datencenter in die Cloud erweitern.
2. Azure AD können zu Ihrem Benutzer SSO-Software-as-a-Service (SaaS) Anwendung. Microsoft Office 365 verwendet diese Technologie beispielsweise und Programme auf Azure oder andere Cloudplattformen können es auch verwenden.
3. Azure AD (der Access Control Service) können Benutzer anmelden mit Identitäten von Facebook, Google, Microsoft und anderen Applikationen in der Cloud oder lokal gehostet werden.

Weitere Informationen zu diesen Unterschieden finden Sie unter [Azure Identität](fundamentals-identity.md).

## <a name="related-resources"></a>Verwandte Ressourcen

Sie downloaden und [Azure Virtual Machine Readiness Assessment](https://www.microsoft.com/download/details.aspx?id=40898)ausführen. Die Bewertung Überprüfen Ihrer lokalen Umgebung automatisch und benutzerdefinierten Bericht anhand der Anleitung in diesem Thema die Umgebung in Azure Migration gefunden.

Es wird empfohlen, auch Lernprogramme, Handbücher und Videos, die die folgenden Themen lesen:

- [Konfigurieren eines virtuellen Netzwerks Cloud nur in Azure-Portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [Konfigurieren einer Standort-zu-Standort-VPN in Azure-Portal](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [Installieren einer neuen Active Directory-Gesamtstruktur in Azure virtual network](active-directory-new-forest-virtual-machine.md)
- [Installieren von Active Directory Active Directory Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure IT Pro IaaS: (01) Virtual Machine-Grundlagen](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure IT Pro IaaS: (05) erstellen virtuelle Netzwerke und standortübergreifende Konnektivität](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Einführung

Grundvoraussetzungen für die Bereitstellung von Windows Server Active Directory in Azure virtuelle Computer unterscheiden sich geringfügig von lokalen virtuellen Maschinen (und teilweise physischen Computern) bereitstellen. Beispielsweise sind die Domänencontroller (DCs) auf Azure virtuellen Computern bereitstellen Replikate in einer vorhandenen lokalen bei Windows Server AD DS corporate Domäne/Gesamtstruktur und Azure-Bereitstellung weitgehend auf die gleiche Weise behandelt werden kann wie jede andere zusätzliche Windows Server Active Directory-Website kann. Also müssen in Windows Server AD DS, eine Website, die Subnetze mit dieser Site verknüpft und an anderen Standorten entsprechende Websitehyperlinks Subnetze definiert werden. Es gibt jedoch einige Unterschiede, die für alle Azure-Bereitstellung und einige bestimmte Bereitstellungsszenario variieren. Zwei grundlegende Unterschiede werden unten beschrieben:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azure virtuelle Computer möglicherweise die Verbindung mit dem lokalen Unternehmensnetzwerk.

Azure virtuelle Computer auf einem lokalen Firmennetzwerk verbinden erfordert Azure virtuelles Netzwerk, Standort, Standort oder Standort-zu-Punkt-virtual private Network (VPN) Komponente Azure VMs problemlos herstellen und lokalen Computern. VPN-Komponente könnte auch lokale Domänenmitgliedscomputer auf Windows Server Active Directory-Domäne, deren Domänencontroller ausschließlich Azure virtuelle Computer gehostet werden. Es ist wichtig zu beachten, schlägt das VPN-Authentifizierung und andere Vorgänge, die auf Windows Server Active Directory abhängig sind auch fehl. Während Benutzer möglicherweise Anmelden mit vorhandenen zwischengespeicherten Anmeldeinformationen für alle Peer-to-Peer- oder Client-Server-Authentifizierung versucht die Tickets bisher ausgegeben oder veraltet sind fehl.

Anzeigen Sie [Virtuelles Netzwerk](http://azure.microsoft.com/documentation/services/virtual-network/) für Demo-video und Lernprogramme, einschließlich [Konfigurieren einer Standort-zu-Standort VPN in Azure-portal](../vpn-gateway/vpn-gateway-site-to-site-create.md)

> [AZURE.NOTE] Sie können auch Windows Server Active Directory in Azure virtual Network bereitstellen, die keine Verbindung mit einem lokalen Netzwerk. Die Richtlinien in diesem Thema angenommen, Azure virtual Network verwendet wird, da IP-Adressen sind für Windows Server bietet.

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statische IP-Adressen müssen mit Azure PowerShell konfiguriert werden.

Dynamische Adressen standardmäßig zugewiesen, jedoch Set-AzureStaticVNetIP-Cmdlet verwenden, um eine statische IP-Adresse zuordnen. Eine statische IP-Adresse, die durch Service Heilung und VM Herunterfahren/Neustarten erhalten bleiben festgelegt. Weitere Informationen finden Sie unter [statische interne IP-Adresse für virtuelle Computer](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Begriffe und Definitionen

Folgendes ist eine nicht erschöpfende Liste der Begriffe für verschiedene Azure Technologien, die in diesem Artikel verwiesen werden.

- **Azure virtuelle Computer**: die IaaS-Angebot in Azure, die Kunden mit nahezu jedem traditionell VMs ermöglicht lokalen Server-Arbeitslast.

- **Azure virtual Network**: Netzwerkdienst in Azure, mit denen Kunden erstellen und Verwalten virtueller Netzwerke in Azure und sicher miteinander zu ihrer lokalen Netzwerkinfrastruktur mit einem virtuellen privaten Netzwerk (VPN).

- **Virtuelle IP-Adresse**: ein Internetzugriff IP-Adresse, die nicht an bestimmte Computer oder Netzwerk-Schnittstellenkarte gebunden ist. Cloud-Dienste werden eine virtuelle IP-Adresse für den Empfang von Netzwerkverkehr der Azure-VM umgeleitet wird. Eine virtuelle IP-Adresse ist eine Eigenschaft von einem Clouddienst mindestens Azure virtuelle Computer enthalten. Beachten Sie, dass ein Azure virtuelles Netzwerk eine oder mehrere Clouddienste enthalten kann. Virtuelle IP-Adressen bieten native Lastenausgleich.

- **Dynamische IP-Adresse**: die IP-Adresse, die ausschließlich intern verwendet wird. Es sollte als eine statische IP-Adresse konfiguriert werden (mithilfe des Set-AzureStaticVNetIP-Cmdlets) für virtuelle Computer, die Domänencontroller und DNS-Serverrollen.

- **Reparatur Service**: der Prozess in der Azure automatisch wieder ein Dienst in einen Ausführungszustand nachdem festgestellt wurde, dass der Dienst ausgefallen ist. Reparatur Service ist einer der Aspekte von Azure, die Verfügbarkeit und Stabilität unterstützt. Unwahrscheinlich, nach Dienst Heilung Vorfall für einen Domänencontroller auf einer VM Ergebnis ähnelt einem nicht geplanten Neustart jedoch hat einige Nebenwirkungen:

 - Virtuelle Netzwerkadapter auf dem virtuellen Computer ändern
 - Die MAC-Adresse des virtuellen Netzwerkadapter ändern
 - Die Prozessor-CPU-ID der VM ändern
 - IP-Konfiguration des virtuellen Netzwerkadapter wird nicht geändert, solange die VM ein virtuelles Netzwerk angeschlossen ist und die VM-IP-Adresse statisch.

 Keines dieser Verhaltensweisen betreffen Windows Server Active Directory hat keine Abhängigkeit von der MAC-Adresse oder die Prozessor-CPU ID und alle Windows Server Active Directory-Bereitstellung in Azure sollten in Azure virtual Network ausgeführt werden, wie oben beschrieben.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Ist es sicher, Windows Server Active Directory-Domänencontroller virtualisieren?

Bereitstellen von Windows Server Active Directory-Domänencontroller auf Azure virtuellen Computern unterliegt den Richtlinien DCs lokal auf einem virtuellen Computer ausgeführt. Virtualisierte Domänencontroller ausgeführt wird als sicher angesehen als Richtlinien für das Sichern und Wiederherstellen von DCs eingehalten werden. Weitere Informationen zu Einschränkungen und Richtlinien für virtualisierte Domänencontroller ausgeführt finden Sie unter [Running Domain Controllers in Hyper-V](https://technet.microsoft.com/library/dd363553).

Hypervisoren bereitstellen oder verharmlosen Technologie, die für die vielen verteilten Systemen, einschließlich Windows Server Active Directory auftreten können. Beispielsweise auf einem physischen Server Klonen ein Datenträgers oder nicht unterstützte Methoden Rollback den Status eines Servers einschließlich SANs usw., jedoch auf einem physischen Server ist schwieriger als die Wiederherstellung einer Momentaufnahme eines virtuellen Computers in einem Hypervisor. Azure bietet Funktionen, die in den unerwünschten Zustand führen können. Beispielsweise sollten Sie VHD-Dateien der Domänencontroller nicht kopieren, anstatt regelmäßige Backups, da eine ähnliche Situation mit Snapshot Wiederherstellungsoptionen Wiederherstellung führen kann.

Solche Rollbacks einführen USN Bläschen, die dauerhaft unterschiedliche Zustände zwischen Domänencontrollern zu. Das kann Probleme wie:

- Veraltete Objekte
- Inkonsistente Kennwörter
- Inkonsistente Werte
- Rollback der Schemamaster Schemakonflikten

Finden Sie weitere Informationen wie DCs beeinträchtigt [USN und USN-Rollback](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Beginnend mit Windows Server 2012, [zusätzliche Sicherheitsmaßnahmen sind in AD DS integriert](https://technet.microsoft.com/library/hh831734.aspx). Diese Sicherheitsmaßnahmen schützen virtualisierte Domänencontroller gegen die genannten Probleme als die zugrunde liegenden Plattform Hypervisor VM-GenerationID unterstützt. Azure unterstützt VM-GenerationID, was bedeutet, dass Domänencontroller, die Windows Server 2012 oder später Azure virtuelle Computer zusätzlichen Sicherheitsmaßnahmen.

> [AZURE.NOTE] Sie beenden und starten einen virtuellen Computer, die Domänencontroller im Gastbetriebssystem anstatt die Option **Herunterfahren** im klassischen Azure-Portal in Azure ausgeführt wird. Heute führt mit dem Verwaltungsportal einen virtuellen Computer herunterfahren VM freigegeben werden. Eine VM freigegeben hat den Vorteil nicht kündigen, aber auch setzt die VM-GenerationID ist ein Domänencontroller nicht erwünscht. Beim Zurücksetzen der VM-GenerationID Aufrufkennung der AD DS-Datenbank auch Zurücksetzen der RID-Pool verworfen und SYSVOL als nicht autorisierend gekennzeichnet. Weitere Informationen finden Sie unter [Einführung in Active Directory-Domänendienste (AD DS) Virtualisierung](https://technet.microsoft.com/library/hh831734.aspx) und [Sicher Virtualisierung DFSR](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Gründe für die Bereitstellung von Windows Server AD DS auf Azure Virtual Machines

Viele Windows Server AD DS-Bereitstellungsszenarien eignen sich für die Bereitstellung als VMs auf Azure. Beispielsweise haben Sie ein Unternehmen in Europa, die zur Authentifizierung von Benutzern an einem Remotestandort in Asien. Das Unternehmen wurde nicht zuvor Windows Server Active Directory DCs in Asien aufgrund bereitgestellt werden und beschränkt Fachwissen zum Verwalten der Server nach der Bereitstellung bereitgestellt. Daher sind Authentifizierungsanfragen von Asien von DCs in Europa suboptimale Ergebnisse gewartet. In diesem Fall kann einen Domänencontroller auf einem virtuellen Computer bereitstellen, das Sie in Azure-Rechenzentrum in Asien ausgeführt werden muss. Ein Azure virtuelles Netzwerk, das direkt an den remote-Standort verbunden ist, DC zuordnen wird authentifizierungsleistung verbessern.

Azure eignet sich auch als Ersatz für andernfalls kostspielige Disaster Recovery (DR)-Standorten. Die relativ Kostengünstige Hosting wenigen Domänencontrollern und ein einzelnes virtuelles Netzwerk in Azure stellt eine attraktive Alternative.

Sie möchten schließlich eine netzwerkanwendung in Azure wie SharePoint bereitstellen, die Windows Server Active Directory erfordert jedoch keine Abhängigkeit im lokalen Netzwerk oder Unternehmen Windows Server Active Directory. In diesem Fall Bereitstellen einer isolierten Gesamtstruktur auf Azure zu SharePoint Server Anforderungen ist optimal. Erneut wird die deploying Network Applications, die Verbindung mit dem lokalen Netzwerk und Active Directory des Unternehmens erfordern ebenfalls unterstützt.

> [AZURE.NOTE] Da es eine Schicht-3-Verbindung bereitstellt, können VPN-Komponente, die Konnektivität zwischen Azure virtuelles Netzwerk und einem lokalen Netzwerk auch Mitgliedsserver, die Ausführen von lokalen Domänencontrollern nutzen, die in Azure virtual Network Azure virtuelle Computer ausgeführt. Wenn jedoch das VPN nicht verfügbar ist, Kommunikation zwischen lokalen Computern Azure-basierten Domänencontroller funktioniert nicht, was Authentifizierung und verschiedene andere Fehler.  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Bereitstellen von Windows Server Active Directory-Domänencontroller auf Azure Virtual Machines im Vergleich zu lokalen Kontraste

- Für alle Bereitstellungsszenario Windows Server Active Directory als einer einzigen VM mehr muss Azure virtuelles Netzwerk für IP-Adresse Konsistenz verwenden. Beachten Sie, dass dieses Handbuch setzt voraus, dass Domänencontroller in Azure virtual Network ausgeführt werden.

- Wie bei lokalen Domänencontrollern werden statische IP-Adressen empfohlen. Eine statische IP-Adresse kann nur mithilfe von Azure PowerShell konfiguriert werden. Weitere Informationen finden Sie unter [statische interne IP-Adresse für virtuelle Computer](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) . Haben Sie monitoring-Systeme oder andere Lösungen, die für statische IP-Adresskonfiguration innerhalb des Gast-Betriebssystems prüfen können Sie den Netzwerkadapter-Eigenschaften des virtuellen Computers die statische IP-Adresse zuweisen. Aber wissen, dass der Netzwerkadapter verworfen wird, wenn die VM wird Service Heilung oder im klassischen Portal heruntergefahren und hat die Adresse freigegeben. In diesem Fall müssen statische IP-Adresse innerhalb der Gast zurückgesetzt werden.

- Bereitstellen virtueller Computer in einem virtuellen Netzwerk nicht implizieren (Verbindung mit einem lokalen Netzwerk oder erforderlich); das virtuelle Netzwerk kann nur diese Möglichkeit. Erstellen Sie ein virtuelles Netzwerk für die private Kommunikation zwischen Azure und Ihrem lokalen Netzwerk. Sie müssen einen VPN-Endpunkt im lokalen Netzwerk bereitstellen. Das VPN wird von Azure mit dem lokalen Netzwerk geöffnet. Weitere Informationen finden Sie unter [Übersicht über Virtual Network](../virtual-network/virtual-networks-overview.md) und [Konfigurieren eines Standort-zu-Standort-VPN in Azure-Portal](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] Eine Option zum [Erstellen eines Punkt-zu-Standort-VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) steht einzelne Windows-basierten Computern direkt Azure virtuelles Netzwerk herstellen.

- Unabhängig davon, ob Sie eine virtuelle erstellen Netzwerk oder Azure Zuschläge für ausgehenden Datenverkehr jedoch nicht eindringen. Wie viel ausgehenden Datenverkehr von einer Bereitstellung generiert beeinflussen Windows Server Active Directory Designoptionen. Beispielsweise begrenzt Bereitstellung eines schreibgeschützten Domänencontrollers (RODC) ausgehenden Datenverkehr da es ausgehende nicht repliziert. Aber die Entscheidung zum Bereitstellen eines RODC muss die Notwendigkeit Schreibvorgänge für den Domänencontroller und die [Kompatibilität](https://technet.microsoft.com/library/cc755190) , die Programme und Dienste auf der Website mit RODCs abgewogen werden. Weitere Informationen zum Datenverkehr Gebühren finden Sie unter [Azure Preise auf einen Blick](http://azure.microsoft.com/pricing/).

- Abgeschlossen haben steuern Sie über welchen Server Ressourcen für VMs lokal, wie Arbeitsspeicher, Festplattengröße und usw. auf Azure muss zuerst eine Liste der vorkonfigurierten Servergrößen. Für einen Domänencontroller ist ein Datenträger neben Betriebssystem-Datenträger erforderlich Windows Server Active Directory-Datenbank gespeichert.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Kann Windows Server AD FS auf Azure virtuellen Computern bereitstellen?

Ja, Windows Server AD FS auf Azure virtuelle Computer bereitstellen und [best Practices für ADFS Bereitstellung](https://technet.microsoft.com/library/dn151324.aspx) lokalen AD FS-Bereitstellung in Azure gelten. Aber einige der best Practices wie Lastenausgleich und hohe Verfügbarkeit benötigen Technologies über AD FS selbst erwartet. Sie müssen von der zugrunde liegenden Infrastruktur bereitgestellt. Lassen Sie uns einige dieser optimalen Methoden und sehen, wie sie mithilfe von Azure VMs und Azure virtuelles Netzwerk erreicht werden können.

1. **Setzen Sie Security token Service (STS) Server direkt mit dem Internet.**

    Dies ist wichtig, da der STS stellt Sicherheitstokens. Daher sollte das gleiche Schutzniveau als Domänencontroller STS-Server wie AD FS-Server behandelt werden. Wenn ein STS gefährdet, können böswillige Benutzer Zugriffstoken mit potenziell Ansprüche ihrer Wahl relying Party Applications und andere STS-Server vertrauen Unternehmen ausgeben.

2. **Bereitstellen Sie Active Directory-Domänencontroller für alle Benutzerdomänen im gleichen Netzwerk wie die AD FS-Server.**

    AD FS-Server verwenden Active Directory-Domänendienste zur Authentifizierung von Benutzern. Es wird empfohlen, Domänencontroller im gleichen Netzwerk wie die AD FS-Server bereitstellen. Dies bietet Business Continuity, die Verknüpfung zwischen der Azure und Ihre lokalen Netzwerk unterbrochen, und ermöglicht die geringere Latenz und mehr Performance für Benutzernamen.

3. **Bereitstellen Sie mehrerer AD FS-Knoten für hohe Verfügbarkeit und die Last.**

    In den meisten Fällen ist der Ausfall einer Anwendung AD FS kann unzulässige Anträge, die erfordern Sicherheitstokens sind häufig wichtige mission. Dies hat zur Folge, und da AD FS jetzt kritisch auf Umwandeln befindet, der AD FS-Dienst muss hochverfügbare über mehrere AD FS Proxys und AD FS-Server. Um Anfragen zu erreichen, sind Lastenausgleich normalerweise vor der AD FS-Proxys und den AD FS-Servern bereitgestellt.

4. **Bereitstellen Sie ein oder mehrere Web Application Proxy-Knoten für den Internetzugriff.**

    Wenn Benutzer Programme geschützt durch AD FS-Dienst zugreifen, muss der AD FS-Dienst über das Internet verfügbar sein. Dies ist den Webproxydienst Anwendung bereitstellen. Es wird dringend empfohlen, mehrere Knoten zu hohe Verfügbarkeit bereitzustellen und Lastenausgleich.

5. **Zugriff auf interne Netzwerkressourcen vom Web Application Proxy Knoten einschränken.**

    Um externe Benutzer AD FS über das Internet zugreifen können, müssen Sie Anwendung Webproxy Knoten (oder AD FS-Proxy in früheren Versionen von Windows Server) bereitstellen. Web Application Proxy Knoten sind direkt mit dem Internet verbunden. Sind keine Domäne sein und nur benötigen Zugriff auf die AD FS-Server über TCP-Port 443 und 80. Kommunikation mit anderen Computern (insbesondere Domänencontroller) blockiert wird dringend empfohlen.

    Dies ist in der Regel erreicht lokal mit einer DMZ. Firewalls verwenden einen weißen Betriebsmodus beschränkt Datenverkehr von der DMZ zum lokalen Netzwerk (nur von der angegebenen IP-Adressen und bestimmte Ports zugelassen und alle anderen Datenverkehr blockiert).

Das folgende Diagramm zeigt ein herkömmlichen lokalen AD FS-Bereitstellung.

![Diagramm einer herkömmlichen lokalen Active Directory Federation Services-Bereitstellung](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Da Azure nicht systemeigenen bereitstellen, jedoch mit vollem Funktionsumfang Firewallfunktion andere Optionen verwendet werden, um Datenverkehr einschränken. Die folgende Tabelle zeigt jede Option und vor- und Nachteile.

| Option | Vorteile | Nachteil |
| ------ | --------- | ------------ |
| [Azure-Netzwerk-ACLs](virtual-networks-acl.md) | Günstiger und einfacher Erstkonfiguration | Weitere ACL-Konfiguration erforderlich, die Bereitstellung neuer VMs hinzugefügt |
| [Barracuda NG firewall](https://www.barracuda.com/products/ngfirewall) | Whitelist-Modus, und es muss keine ACL-Konfiguration | Zunehmende Kosten und Komplexität für Erstinstallation |

Die allgemeinen Schritte zum Bereitstellen von AD FS werden in diesem Fall wie folgt:

1. Erstellen Sie ein virtuelles Netzwerk mit standortübergreifende Verbindung über ein VPN oder [ExpressRoute](http://azure.microsoft.com/services/expressroute/).

2. Bereitstellen Sie Domänencontroller im virtuellen Netzwerk. Dieser Schritt ist optional, aber empfohlen.

3. Domäne AD FS-Server auf dem virtuellen Netzwerk bereitstellen.

4. Erstellen einer [internen Lastenausgleich festlegen](http://azure.microsoft.com/blog/internal-load-balancing/) , die die AD FS-Servern enthält und eine neue private IP-Adresse innerhalb des virtuellen Netzwerks (dynamische IP-Adresse).

  1. Aktualisieren der DNS FQDN auf private (dynamischen) IP-Adresse des internen Lastenausgleich ein erstellen.

5. Erstellen Sie Cloud-Dienst (oder ein separates virtuelles Netzwerk) für die Anwendung Webproxy-Knoten.

6. Bereitstellen Sie Web Application Proxy Knoten im Cloud-Dienst oder virtuelles Netzwerk

  1. Erstellen einer externen Lastenausgleich, die die Anwendung Webproxy-Knoten enthält.

  2. Aktualisieren des externen DNS-Namens (FQDN) auf Cloud-Dienst öffentliche IP-Adresse (die virtuelle IP-Adresse).

  3. Konfigurieren Sie AD FS-Proxys um den vollqualifizierten Domänennamen verwenden, die auf die internen Lastenausgleich für den AD FS-Servern entspricht.

  4. Anspruchsbasierte Websites den externen FQDN für ihre Anspruchsanbieter zu aktualisieren.

7. Beschränken des Zugriffs zwischen Web-Anwendungsproxy auf jedem Computer in AD FS virtuelles Netzwerk.

Um Datenverkehr einschränken festgelegten Lastenausgleich für Azure internen Lastenausgleich nur Verkehr für TCP-Ports 80 und 443 konfiguriert werden muss und andere Datenverkehr an interne dynamische IP-Adresse der Gruppe Lastenausgleich gelöscht.

![Diagramm der ADFS-Netzwerk-ACLs TCP 443 + 80 zulässig.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Auf die AD FS-Server würde nur von folgenden Quellen zulässig:

- Azure internen Lastenausgleich.
- Die IP-Adresse des Administrators im lokalen Netzwerk.

> [AZURE.WARNING] Der Entwurf muss Web Application Proxy Knoten hindern andere VMs in Azure virtual Network oder Orte im lokalen Netzwerk. Kann dies durch Konfigurieren von Firewall-Regeln in der lokalen Einheit für Express Route Verbindungen oder VPN-Gerät für Standort-zu-Standort-VPN-Verbindungen.

Ein Nachteil dieser Option muss das Konfigurieren des Netzwerks ACLs für mehrere Geräte einschließlich interner Lastenausgleich ADFS-Server und andere Server, die an das virtuelle Netzwerk hinzugefügt werden. Wenn ein Gerät ohne Netzwerk-ACLs, um Datenverkehr zu beschränken der Bereitstellung hinzugefügt wird, kann die gesamte Bereitstellung gefährdet. Ändert die IP-Adressen der Knoten Web Application Proxy immer, muss das Netzwerk ACLs zurückgesetzt werden (d. h. des Proxys konfiguriert werden sollten [Statische dynamische IP-Adressen](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![ADFS Azure mit ACLS.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Eine weitere Option ist mit [Barracuda NG Firewall](https://www.barracuda.com/products/ngfirewall) -Appliance zwischen AD FS-Proxyserver und den AD FS-Servern. Diese Option entspricht der best Practices für die Sicherheit und Verfügbarkeit und weniger Verwaltungsaufwand nach der Erstinstallation Barracuda NG Firewall-Appliance bietet einen weißen Modus Firewall-Verwaltung und direkt auf ein Azure virtuelles Netzwerk installiert werden. Das überflüssig Netzwerk jederzeit konfiguriert werden müssen, die die Bereitstellung ein neues Servers hinzugefügt wird. Aber diese Option Ausbringung Komplexität und Kosten.

In diesem Fall werden statt einer zwei virtuelle Netzwerke bereitgestellt. Wir nennen sie VNet1 und VNet2. VNet1 enthält die Proxys und VNet2 der Tata_signal und das Netzwerk mit dem Firmennetzwerk verbunden. VNet1 ist (nahezu) auch physisch aus VNet2 und wiederum vom Firmennetzwerk isoliert. VNet1 ist VNet2 verbunden mit einer speziellen Tunneling-Technologie als Transport Independent Network Architecture (TINA) bezeichnet. TINA Tunnel gehört jeder virtuelle Netzwerke eine Barracuda NG Firewall – ein Barracuda auf den einzelnen virtuellen Netzwerken.  Hohe Verfügbarkeit wird empfohlen, zwei Barrakudas auf jedes virtuelle Netzwerk bereitzustellen. eine aktive anderen Passiv. Sie bieten sehr umfangreiche Firewall die Bedienung einer herkömmlichen lokalen DMZ in Azure imitieren ermöglichen.

![ADFS Azure-Firewall.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Weitere Informationen finden Sie unter [AD FS: Erweitern eine lokalen Ansprüche Frontend-Anwendung mit dem Internet](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Eine Alternative zur AD FS-Bereitstellung ist das Ziel nur Office 365 SSO

Es ist eine Alternative zur Bereitstellung von AD FS insgesamt ist das Ziel nur für Office 365 anmelden können. In diesem Fall können Sie einfach DirSync mit Kennwort Sync lokal bereitstellen und erreichen das gleiche Endergebnis mit minimaler Bereitstellungsprozess, weil dadurch keine AD FS oder Azure erforderlich ist.

Die folgende Tabelle vergleicht die Funktionsweise der Vorgänge mit und ohne AD FS.

| Office 365 single Sign-On AD FS mit DirSync | Office anmelden 365 identisch mit DirSync + Kennwort synchronisieren |
| ------------- | ------------- |
| 1. der Benutzer mit einem Unternehmensnetzwerk anmelden und auf Windows Server Active Directory authentifiziert. | 1. der Benutzer mit einem Unternehmensnetzwerk anmelden und auf Windows Server Active Directory authentifiziert. |
| 2. der Benutzer versucht, auf Office 365 (ich @contoso.com). | 2. der Benutzer versucht, auf Office 365 (ich @contoso.com). |
| 3. Office 365 leitet den Benutzer zu Azure AD. | 3. Office 365 leitet den Benutzer zu Azure AD. |
| 4 da Azure AD nicht den Benutzer authentifiziert kann und eine Vertrauensstellung mit AD FS lokal versteht Benutzerinformationen er die auf AD FS. | 4. Azure AD nicht Kerberos-Tickets direkt akzeptieren und keine Vertrauensstellung existiert angefordert wird, dass der Benutzer Anmeldeinformationen eingeben. |
| 5. der Benutzer sendet ein Kerberos-Ticket AD FS STS. | 5. der Benutzer des gleiche Kennworts für lokale und Azure AD mit Benutzernamen und Kennwort durch DirSync synchronisiert wurde überprüft. |
| 6. AD FS transformiert das Kerberos-Ticket der erforderliche token Format/Ansprüche und leitet den Benutzer zu Azure AD. | 6. Azure AD leitet den Benutzer zu Office 365. |
| 7. authentifizierende Azure AD (einer anderen Transformation wird). |  7. Benutzer kann Office 365 und Azure AD-Token mit OWA anmelden. |
| 8. Azure AD leitet den Benutzer zu Office 365. |  |
| 9. der Benutzer ist Office 365 automatisch angemeldet. |  |

In Office 365 mit Kennwort Sync Szenario (keine AD FS) DirSync-auf Fassung "gleiche anmelden", "identisch" lediglich bedeutet, dass Benutzer die gleichen lokalen Anmeldeinformationen erneut beim Zugriff auf Office 365 eingeben müssen. Beachten Sie, daß diese Daten vom Browser des Benutzers an nachfolgende Anweisungen reduzieren können.

### <a name="additional-food-for-thought"></a>Zusätzliche Denkanstöße

- Wenn Sie einen AD FS-Proxy auf Azure virtuellen Computer bereitstellen, ist Konnektivität zu AD FS-Servern erforderlich. Werden lokale empfohlen Nutzung von Standort zu Standort VPN-Konnektivität virtuellen Netzwerk Web Application Proxy Knoten mit den AD FS-Servern kommunizieren können.

- Wenn AD FS-Server auf einem virtuellen Computer Azure bereitstellen, Verbindung mit Windows Server Active Directory-Domänencontroller Attribut speichert und Konfigurationsdatenbanken ist erforderlich und verlangen eine ExpressRoute oder eine Standort-zu-Standort-VPN-Verbindung zwischen Azure virtual Network und dem lokalen Netzwerk.

- Kosten gelten für den gesamten Datenverkehr von Azure virtuellen Maschinen (ausgehenden Datenverkehr). Wenn Kosten die treibende Kraft, empfiehlt Azure verlassen AD FS-Servern auf-Knoten Webproxy-Anwendung bereitstellen. Wenn AD FS-Servern auf Azure virtuelle Maschinen eingesetzt werden, zusätzliche Kosten entstehen, lokale Benutzer authentifizieren. Ausgehenden Datenverkehr verursacht Kosten unabhängig davon, ob die ExpressRoute oder VPN-Standort-zu-Standort-Verbindung durchlaufen wird.

- Beachten Sie Azure systemeigene Lastenausgleich der Server-Funktionen für hohe Verfügbarkeit der AD FS-Server verwenden möchten, dass Lastenausgleich Prüfpunkte, die verwendet werden stellt, um den Zustand der virtuellen Computer in die Cloud-Dienst zu bestimmen. Bei Azure virtuellen Maschinen (im Gegensatz zu Web oder Worker-Rollen) muss eine benutzerdefinierte Überprüfung verwendet werden, da der Standard-Prüfpunkte auf Agent nicht auf Azure virtuelle Computer vorhanden ist. Der Einfachheit halber können Sie einen benutzerdefinierten TCP-Prüfpunkt – dazu muss nur eine TCP-Verbindung (TCP SYN-Segment gesendet und ein TCP-SYN-ACK-Segment geantwortet) virtuellen Gesundheit bestimmen erfolgreich eingerichtet werden. Sie können benutzerdefinierte Probe um einen TCP-Port verwenden, den die virtuellen Computer aktiv überwacht werden.

> [AZURE.NOTE] Computer, die dieselben Ports direkt mit dem Internet (z. B. Port 80 und 443) verfügbar machen können nicht denselben Clouddienst verwenden. Wird daher empfohlen, eine dedizierte Cloud erstellen Service für Windows Server AD FS-Server zur Vermeidung von potenziellen zwischen Anschluss für eine Anwendung und Windows Server AD FS überlappt.

## <a name="deployment-scenarios"></a>Bereitstellungsszenarien

Im folgenden Abschnitt werden erfolgreiche Bereitstellungsszenarios um die Aufmerksamkeit auf wichtige Punkte, die ergriffen werden müssen berücksichtigt. Jedes Szenario enthält Links auf Weitere Informationen über die Beschlüsse und Faktoren zu berücksichtigen.

1. [AD DS: Bereitstellen einer AD DS-aware-Anwendung erfordert kein Netzwerk-Konnektivität](#BKMK_CloudOnly)

    Beispielsweise wird ein Internetzugriff SharePoint Services auf einem virtuellen Computer Azure bereitgestellt. Die Anwendung hat keine Abhängigkeit im Unternehmens-Netzwerk. Die Anwendung erfordert Windows Server AD DS jedoch Unternehmen Windows Server AD DS nicht erforderlich.

2. [AD FS: Erweitern einer lokalen Ansprüche Frontend-Anwendung mit dem Internet](#BKMK_CloudOnlyFed)

    Beispielsweise muss eine Ansprüche unterstützende Anwendung, die von Benutzer in Firmen verwendet und wurde erfolgreich bereitgestellten lokalen über das Internet. Die Anwendung muss sowohl von Geschäftspartnern über eigene corporate Identity bestehende Geschäftskunden direkt über das Internet zugegriffen werden.

3. [AD DS: Bereitstellen einer Windows Server AD DS-aware-Anwendung, die Verbindung zum Firmennetzwerk benötigt](#BKMK_HybridExt)

    Beispielsweise wird eine LDAP-fähige Anwendung unterstützt die integrierte Windows-Authentifizierung verwendet Windows Server AD DS als Repository für Konfigurations-und Benutzerprofil auf einem virtuellen Computer Azure bereitgestellt. Es empfiehlt sich für die Anwendung zu nutzen vorhandene Unternehmen Windows Server AD DS-auf. Die Anwendung ist nicht Ansprüche.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Bereitstellen einer AD DS-aware-Anwendung erfordert kein Netzwerk-Konnektivität

![AD DS-Bereitstellung Cloud nur](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**Abbildung 1**

#### <a name="description"></a>Beschreibung

SharePoint ist auf Azure VM und die Anwendung ist nicht von corporate Netzwerkressourcen. Die Anwendung erfordert Windows Server AD DS jedoch *nicht* benötigen Unternehmen Windows Server AD DS. Sind keine Kerberos oder verbundenen Vertrauensstellungen erforderlich, da Benutzer selbst bereitgestellte Anwendung in Windows Server AD DS-Domäne sind, die auch in der Cloud in Azure virtuelle Computer gehostet wird.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Szenario-Aspekte und wie Technologiebereichen für das Szenario

- [Netzwerktopologie](#BKMK_NetworkTopology): erstellen ein Azure virtuelles Netzwerk ohne standortübergreifende Konnektivität (auch Standort-zu-Standort-Verbindung).

- [DC-Bereitstellungskonfiguration](#BKMK_DeploymentConfig): einen neuen Domänencontroller in einer neuen Windows Server Active Directory-Gesamtstruktur mit Einzeldomänen-bereitstellen. Dies sollte zusammen mit dem Windows-DNS-Server bereitgestellt werden.

- [Windows Server Active Directory-Standorttopologie](#BKMK_ADSiteTopology): Standard Windows Server Active Directory-Standort (alle Computer werden in Standardname-des-ersten-Standorts) verwenden.

- [IP-Adressen und DNS](#BKMK_IPAddressDNS):

 - Festlegen einer statischen IP-Adresse für den Domänencontroller festlegen AzureStaticVNetIP Azure PowerShell-Cmdlet.
 - Installieren und Konfigurieren von Windows-DNS-Server auf den Domänencontrollern in Azure.
 - Konfigurieren Sie die Eigenschaften virtuelles Netzwerk mit Namen und IP-Adresse der VM, der Domänencontroller und DNS-Serverrollen hostet.

- [Globaler Katalog](#BKMK_GC): der erste Domänencontroller in der Gesamtstruktur muss ein globaler Katalogserver sein. Zusätzliche Domänencontroller sollten auch als globale Kataloge konfiguriert werden, da in einer Gesamtstruktur mit einer Domäne der globale Katalog keine zusätzliche Aufgaben des Domänencontrollers erforderlich ist.

- [Platzierung von Windows Server AD DS-Datenbank und SYSVOL](#BKMK_PlaceDB): DCs Azure VMs um Windows Server Active Directory-Datenbank, Protokollen und SYSVOL speichern unter einen Datenträger hinzufügen.

- [Sicherung und Wiederherstellung](#BKMK_BUR): bestimmen, Systemstatus-Sicherungskopien gespeichert werden soll. Fügen Sie ggf. einen anderen Datenträger DC VM, Backups zu speichern.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: eine lokale Ansprüche Front-End-Anwendung mit dem Internet erweitern

![Föderation mit standortübergreifenden](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**Abbildung 2**

#### <a name="description"></a>Beschreibung

Eine Ansprüche unterstützende Anwendung, die von Benutzer in Firmen verwendet und wurde erfolgreich bereitgestellten lokalen muss direkt über das Internet. Die Anwendung fungiert als ein Web-Frontend zu einer SQL-Datenbank, in der Daten gespeichert. Von der Anwendung verwendeten SQL-Server befinden sich im Unternehmensnetzwerk. Zwei Windows Server AD FS Tata_signal und ein Lastenausgleich wurden bereitgestellten lokalen den Zugriff auf die Benutzer im Unternehmen. Die Anwendung jetzt außerdem direkt über das Internet nach Geschäftspartnern über eigene corporate Identity und bestehende Geschäftskunden zugegriffen werden muss.

Im Bemühen um Vereinfachung und die Bereitstellung und Konfiguration dieser Anforderung erfüllen wird entschieden, dass zwei zusätzliche-Frontends und zwei Web Windows Server AD FS-Proxyserver auf Azure virtuellen Computern installiert werden. Alle vier virtuelle Computer direkt mit dem Internet ausgesetzt und Verbindung mit dem lokalen Netzwerk Azure Virtual Network Standort-zu-Standort-VPN-Funktionalität bereitgestellt werden.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Szenario-Aspekte und wie Technologiebereichen für das Szenario

- [Netzwerktopologie](#BKMK_NetworkTopology): Erstellen einer Azure virtual Network und [standortübergreifende Verbindung konfigurieren](../vpn-gateway/vpn-gateway-site-to-site-create.md).

 > [AZURE.NOTE] Jeder Windows Server AD FS-Zertifikate sicher, dass die URL definiert die Vorlage und die resultierende Zertifikate Windows AD FS Serverinstanzen auf Windows Azure ausgeführte erreichbar. Dies erfordert möglicherweise die standortübergreifende Verbindung Teilen Ihrer PKI-Infrastruktur. Für Beispiel die CRL Endpunkt LDAP-basierte und ausschließlich lokal gehosteten dann standortübergreifende Konnektivität erforderlich werden. Wenn dies nicht erwünscht ist, ist es möglicherweise erforderlich, verwenden Sie Zertifikate von einer Zertifizierungsstelle, deren CRL über das Internet zugänglich ist.

- [Cloud-Services-Konfiguration](#BKMK_CloudSvcConfig): sicherzustellen, dass zwei Cloud-Dienste nacheinander zwei Lastenausgleich virtuellen IP-Adressen. Virtuelle IP-Adresse der ersten Cloud-Dienst werden zwei Windows Server AD FS-Proxy VMs Ports 80 und 443 weitergeleitet. Der Windows AD FS-Proxy VMs wird die IP-Adresse des lokalen Lastenausgleich, Fronten Windows Server AD FS Tata_signal darauf konfiguriert werden. Die zweite Clouddienst virtuelle IP-Adresse gelangen zu zwei VMs Web Front-End-Ports 80 und 443 erneut ausführen. Konfigurieren Sie eine benutzerdefinierte Überprüfung um sicherzustellen, dass der Lastenausgleich nur Datenverkehr funktioniert Windows Server AD FS-Proxy und Web Frontend VMs leitet.

- [Konfiguration Verbundservers](#BKMK_FedSrvConfig): Konfigurieren Sie Windows Server AD FS als Verbundserver (STS) Sicherheitstoken in der Cloud erstellt Windows Server Active Directory-Gesamtstruktur generiert. Föderation Ansprüche Provider Vertrauensstellungen mit anderen Partnern Identitäten akzeptieren möchten festgelegt konfigurieren relying Party Vertrauensstellungen mit anderen Applikationen Token generiert werden soll

    In den meisten Fällen sind Windows Server AD FS-Proxyserver Internetzugriff Funktion aus Sicherheitsgründen bereitgestellt, während ihre Windows Server AD FS Föderation Gegenstücke direkte Internet-Verbindung getrennt. Unabhängig von Ihrem Bereitstellungsszenario müssen Sie den Cloud-Dienst eine virtuelle IP-Adresse konfigurieren, das eine öffentliche IP-Adresse und Port Lastenausgleich über zwei Windows Server AD FS STS-Instanzen oder Proxyinstanzen kann.

- [Konfiguration mit hoher Verfügbarkeit Windows Server AD FS](#BKMK_ADFSHighAvail): Es ist ratsam, Bereitstellen einer Windows Server AD FS-Farm mit mindestens zwei Server für Failover und Lastausgleich. Sie können verwenden Windows interne Datenbank (AID) für Windows Server AD FS-Konfigurationsdaten und die internen Lastenausgleich Belastbarkeit Azure verwenden, um eingehende Anfragen auf Servern in der Farm zu verteilen.

Weitere Informationen finden Sie unter [AD DS Deployment Guide](https://technet.microsoft.com/library/cc753963).


### <a name="BKMK_HybridExt"></a>3. AD DS: Bereitstellen einer Windows Server AD DS-aware-Anwendung, die Verbindung zum Firmennetzwerk benötigt

![Standortübergreifende AD DS-Bereitstellung](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**Abbildung 3**

#### <a name="description"></a>Beschreibung

Eine LDAP-Anwendung wird auf einem virtuellen Computer Azure bereitgestellt. Es unterstützt die integrierte Windows-Authentifizierung und verwendet Windows Server AD DS als Repository für Konfigurations- und Profildaten. Das Ziel ist für die Anwendung zu nutzen das vorhandene Unternehmen Windows Server AD DS-auf. Die Anwendung ist nicht Ansprüche. Benutzer müssen auch direkt aus dem Internet auf die Anwendung zugreifen. Um Leistung und optimieren beschlossen zwei zusätzliche Domänencontroller der Domäne des Unternehmens gehören neben der Anwendung in Azure bereitgestellt werden.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Szenario-Aspekte und wie Technologiebereichen für das Szenario

- [Netzwerktopologie](#BKMK_NetworkTopology): Azure virtuelles Netzwerk mit [standortübergreifenden Verbindung](../vpn-gateway/vpn-gateway-site-to-site-create.md)erstellen.

- [Methode](#BKMK_InstallMethod): Replikat Domänencontroller der Domäne des Unternehmens Windows Server Active Directory bereitstellen. Für ein Replikat DC können Windows Server AD DS auf dem virtuellen Computer installieren und optional Funktion (Installieren von Media, IFM) die Datenmenge reduzieren, die während der Installation der neuen Domänencontroller repliziert werden. Ein Lernprogramm finden Sie unter [Active Directory Active Directory Azure installieren](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Auch wenn Medium verwenden, kann es effizienter sein, virtuellen DC lokalen und die gesamte virtuelle Festplatte (VHD) zur Cloud statt Windows Server AD DS während der Installation repliziert. Aus Sicherheitsgründen empfohlen die VHD-Datei vom lokalen Netzwerk löschen, wenn in Azure kopiert wurde.

- [Windows Server Active Directory-Standorttopologie](#BKMK_ADSiteTopology): eine neue Azure-Website in Active Directory-Standorte und-Dienste erstellen. Erstellen Sie ein Windows Server Active Directory-Subnetzobjekt Azure virtuelle Netzwerk und Subnetz hinzufügen. Erstellen Sie ein neues Standortverknüpfungsobjekt, die neue Azure-Website und der Website in der Azure virtuelle Netzwerk VPN-Endpunkt um steuern und Optimieren von Windows Server Active Directory-Verkehr zu und von Azure.

- [IP-Adressen und DNS](#BKMK_IPAddressDNS):

 - Festlegen einer statischen IP-Adresse für den Domänencontroller festlegen AzureStaticVNetIP Azure PowerShell-Cmdlet.
 - Installieren und Konfigurieren von Windows-DNS-Server auf den Domänencontrollern in Azure.
 - Konfigurieren Sie die Eigenschaften virtuelles Netzwerk mit Namen und IP-Adresse der VM, der Domänencontroller und DNS-Serverrollen hostet.

- [Geografisch verteilte DCs](#BKMK_DistributedDCs): Weitere virtuelle Netzwerke konfigurieren. Wenn die Active Directory-Standorttopologie DCs in Regionen erfordert, die verschiedenen Azure-Regionen entsprechen, als Active Directory-Standorte entsprechend erstellen möchten.

- [Schreibgeschützte Domänencontroller](#BKMK_RODC): Sie möglicherweise einen RODC in der Azure-Website bereitstellen, je nach Ihren Anforderungen für die Durchführung schriftlich gegen die Domänencontroller und die Kompatibilität der Programme und Dienste mit RODCs. Weitere Informationen zur Anwendungskompatibilität finden Sie in der [Read-Only Domain Controller zur Anwendungskompatibilität](https://technet.microsoft.com/library/cc755190).

- [Globaler Katalog](#BKMK_GC): GCs sind Serviceanfragen zur Anmeldung mit mehreren Gesamtstrukturen erforderlich. Wenn Sie einen globalen Katalog in der Azure-Website nicht bereitstellen, entstehen Ihnen Kosten Ausgang Authentifizierung Abfragen globalen Kataloge in anderen Sites führen. Um Datenverkehr zu minimieren, können Sie das Zwischenspeichern der universellen Gruppenmitgliedschaft für Azure Site in Active Directory-Standorte und-Dienste aktivieren.

    Bereitstellen eine GC konfigurieren Sie Websitehyperlinks und Site Links Kosten die GC in Azure Site nicht als Quelle DC durch andere GCs bevorzugt, die dieselben teilweisen Partitionen replizieren.

- [Platzierung von Windows Server AD DS-Datenbank und SYSVOL](#BKMK_PlaceDB): DCs Azure VMs um Windows Server Active Directory-Datenbank, Protokollen und SYSVOL speichern unter einen Datenträger hinzufügen.

- [Sicherung und Wiederherstellung](#BKMK_BUR): bestimmen, Systemstatus-Sicherungskopien gespeichert werden soll. Fügen Sie ggf. einen anderen Datenträger DC VM, Backups zu speichern.

## <a name="deployment-decisions-and-factors"></a>Bereitstellung Beschlüsse und Faktoren

Diese Tabelle fasst Windows Server Active Directory Technologiebereichen, die in den oben beschriebenen Szenarien und Beschlüssen, mit Links auf folgenden genauer betrachten betroffen sind. Einige Technologiebereiche möglicherweise nicht für jedes Bereitstellungsszenario und einige Technologiebereiche möglicherweise um ein Bereitstellungsszenario wichtiger als andere Technologiebereichen.

Beispielsweise wenn ein Replikat DC in einem virtuellen Netzwerk bereitstellen und Gesamtstruktur nur eine Domäne, werden wählen einen globalen Katalogserver bereitstellen in diesem Fall nicht für das Bereitstellungsszenario da zusätzliche Replikation Vorschriften nicht erstellt werden. Andererseits, wenn die Gesamtstruktur mehrere Domänen besitzt, kann dann die Entscheidung, einen globalen Katalog in einem virtuellen Netzwerk bereitzustellen Bandbreite Leistung, Authentifizierung, Verzeichnissuche, und Einfluss auf.

| Windows Server Active Directory Technologiebereich | Beschlüsse | Faktoren |
| ---- | ---- | ---- |
| [Netzwerktopologie](#BKMK_NetworkTopology) | Erstellen Sie ein virtuelles Netzwerk? | <li>Vorschriften Corp zugreifen</li> <li>Authentifizierung</li> <li>Account-management</li> |
| [DC-Bereitstellungskonfiguration](#BKMK_DeploymentConfig) | <li>Bereitstellen eine separate Gesamtstruktur ohne Vertrauensstellungen?</li> <li>Bereitstellen einer neuen Gesamtstruktur mit?</li> <li>Bereitstellen einer neuen Gesamtstruktur mit Windows Server Active Directory-Gesamtstruktur oder Kerberos?</li> <li>Erweitern Sie Corp Gesamtstruktur durch Bereitstellung eines Replikats DC?</li> <li>Erweitern Sie Corp Gesamtstruktur durch Bereitstellung einer neuen untergeordneten Domäne oder Domänenstruktur?</li> | <li>Sicherheit</li> <li>Compliance</li> <li>Kosten</li> <li>Stabilität und Fehlertoleranz</li> <li>Anwendungskompatibilität</li> |
| [Windows Server Active Directory-Standorttopologie](#BKMK_ADSiteTopology) | Wie konfigurieren Sie Subnetze, Websites und Links zu Websites mit Azure Virtual Network Traffic optimieren und Kosten? | <li>Subnetz- und Standortobjekte Definitionen</li> <li>Site-Link-Eigenschaften und Benachrichtigung ändern</li> <li>Replikation Komprimierung</li> |
| [IP-Adressen und DNS](#BKMK_IPAddressDNS) | So konfigurieren Sie IP-Adressen und Auflösung? | <li>Verwenden Sie das Cmdlet mit Set-AzureStaticVNetIP eine statische IP-Adresse zuweisen</li> <li>Windows Server-DNS-Server installieren und Konfigurieren von Eigenschaften der virtuellen Namen und IP-Adresse der VM, der Domänencontroller und DNS-Serverrollen hostet</li> |
| [Geografisch verteilte DCs](#BKMK_DistributedDCs) | Wie in separaten virtuellen Netzwerken auf Domänencontrollern repliziert? | Wenn die Active Directory-Standorttopologie DCs in Ländern entspricht anderen Azure-Regionen erfordert, als Active Directory-Standorte entsprechend erstellen möchten. [Virtuelles Netzwerk konfigurieren, virtual Network Connectivity](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) Replikation zwischen Domänencontrollern in separaten virtuellen Netzwerken. |
| [Schreibgeschützte Domänencontroller](#BKMK_RODC) | Verwenden Sie schreibgeschützt oder beschreibbare Domänencontroller? | <li>HBI-PII Attribute filtern</li> <li>Filter-Schlüssel</li> <li>Limit ausgehenden Datenverkehr</li> |
| [Globaler Katalog](#BKMK_GC) | Installieren globalen Katalog? | <li>Einzeldomänen-Gesamtstruktur stellen alle DCs GCs</li> <li>Gesamtstruktur mit mehreren Domänen sind globale Kataloge für die Authentifizierung erforderlich</li> |
| [Installationsmethode](#BKMK_InstallMethod) | Wie DC in Azure installieren? | Entweder: <li>Mit Windows PowerShell oder Dcpromo AD DS installieren</li> <li>Verschieben Sie virtuelle Festplatte einer virtuellen lokalen DC</li> |
| [Platzierung von Windows Server AD DS-Datenbank und SYSVOL](#BKMK_PlaceDB) | Wo Windows Server AD DS-Datenbank, Protokollen und SYSVOL gespeichert? | Dcpromo.exe Standardwerte zu ändern. Diese wichtigen Active Directory-Dateien *müssen* werden Azure Datenträger auf platziert statt Betriebssystem, die Schreibcache implementieren. |
| [Sicherung und Wiederherstellung](#BKMK_BUR) | Wie Sichern und Wiederherstellen von Daten? | System State Backups erstellen |
| [Verbund-Serverkonfiguration](#BKMK_FedSrvConfig) | <li>Bereitstellen einer neuen Gesamtstruktur mit in die Cloud?</li> <li>Bereitstellen AD FS lokal und setzen einen Proxy in die Cloud?</li> | <li>Sicherheit</li> <li>Compliance</li> <li>Kosten</li> <li>Zugriff auf Programme Geschäftspartnern</li> |
| [Cloud-Services-Konfiguration](#BKMK_CloudSvcConfig) | Cloud-Dienst wird implizit erstmals bereitgestellt, einen virtuellen Computer erstellen. Müssen Sie zusätzliche Cloud-Dienste bereitstellen? | <li>Erfordert eine VM oder VMs direkten Kontakt zum Internet?</li> <li> Ist der Dienst den Lastenausgleich erforderlich?</li> |
| [Föderation an öffentliche und private IP-Adressierung (dynamische IP und virtuelle IP-Adresse)](#BKMK_FedReqVIPDIP) | <li>Muss die Windows AD FS Serverinstanz direkt aus dem Internet erreichbar?</li> <li>Ist in der Cloud bereitgestellte Anwendung eigene Internet verwendeten IP-Adresse und Anschlussnummer erforderlich?</li> | Erstellen Sie eine Cloud-Services für jede virtuelle IP-Adresse, die von der Bereitstellung erforderlich |
| [Windows Server AD FS-Konfiguration mit hoher Verfügbarkeit](#BKMK_ADFSHighAvail) | <li>Wie viele Knoten in meinem Windows Server AD FS-Serverfarm?</li> <li>Wie viele Knoten in meinem Windows Server AD FS-Proxy Farm bereitstellen?</li> | Stabilität und Fehlertoleranz |

### <a name="BKMK_NetworkTopology"></a>Netzwerktopologie

Um die Konsistenz der IP-Adresse und DNS von Windows Server AD DS erfüllen ist es erforderlich, zunächst eine [Azure virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) erstellen und die virtuellen Computer zuordnen. Bei der Erstellung müssen Sie entscheiden, ob optional Verbindung mit dem lokalen Unternehmensnetzwerk erweitern die Azure virtuellen Computern auf dem lokalen Computer verbunden – dies geschieht mithilfe traditioneller VPN-Technologie und erfordert, dass VPN-Endpunkt am Rand des Unternehmensnetzwerks verfügbar gemacht werden. Also wird VPN mit dem Unternehmensnetzwerk nicht umgekehrt von Azure initiiert.

Beachten Sie, dass beim Erweitern eines virtuellen Netzwerks mit dem lokalen Netzwerk über die standardmäßigen Gebühren, die für jede VM gelten zusätzliche Gebühren anfallen. Insbesondere fallen für CPU-Zeit des Azure Virtual Network-Gateways und ausgehenden Datenverkehr von jeder VM mit lokalen Computer über das VPN kommuniziert. Weitere Informationen zu Netzwerk-Datenverkehr Gebühren finden Sie unter [Azure Preise auf einen Blick](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>DC-Bereitstellungskonfiguration

Die Konfiguration des Domänencontrollers hängt die Vorschriften für den Dienst in Azure ausgeführt werden soll. Beispielsweise können Sie eine neue Gesamtstruktur isoliert eigene Unternehmensgesamtstruktur zu Testzwecken ein Proof of Concept, eine neue Anwendung oder ein Projekt für kurzfristig, die Directory Services nicht Zugriff auf interne Ressourcen jedoch bereitstellen.

Ein Vorteil lokalen eine isolierte Gesamtstruktur-DC Replikation nicht mit DCs, wodurch weniger ausgehenden Netzwerkverkehr erzeugt durch das System direkt gesenkt. Weitere Informationen zu Netzwerk-Datenverkehr Gebühren finden Sie unter [Azure Preise auf einen Blick](http://azure.microsoft.com/pricing/).

Nehmen Sie beispielsweise an Datenschutz benötigt für einen Dienst, aber der Dienst Zugriff auf interne Windows Server Active Directory abhängig. Wenn für den Dienst in der Cloud Hostdaten zugelassen sind, können Sie eine neue untergeordnete Domäne für Ihre interne Gesamtstruktur auf Azure bereitstellen. In diesem Fall können Sie einen DC für die neue untergeordnete Domäne (ohne globalen Katalog um Hilfe will) bereitstellen. Dieses Szenario mit einem Replikat DC-Bereitstellung erfordert ein virtuelles Netzwerk für die Verbindung mit Ihrem lokalen Domänencontroller.

Wenn Sie eine neue Gesamtstruktur erstellen, wählen Sie [Active Directory-Vertrauensstellungen](https://technet.microsoft.com/library/cc771397) oder [Föderation vertraut](https://technet.microsoft.com/library/dd807036). Saldo der Anforderungen von Kompatibilität, Sicherheit, Compliance, Kosten und Flexibilität. Beispielsweise zur Nutzung der [ausgewählten Authentifizierung](https://technet.microsoft.com/library/cc755844) können Sie eine Windows Server Active Directory-Vertrauensstellung zwischen der lokalen und der Cloud-Gesamtstruktur erstellt und eine neue Gesamtstruktur auf Azure bereitgestellt. Wenn die Anwendung Ansprüche, jedoch können Sie Verbundvertrauensstellungen anstelle von Active Directory-Gesamtstruktur-Vertrauensstellungen bereitstellen. Ein weiterer Faktor werden die Kosten für weitere Datenreplikation von lokalen Windows Server Active Directory in die Cloud erweitern oder mehrere ausgehende Verkehr durch Authentifizierung und Abfrage laden.

An Verfügbarkeit und Fehlertoleranz betreffen auch Ihrer Wahl. Beispielsweise wird die Verknüpfung unterbrochen, vertrauen Applications nutzt eine Kerberos-Vertrauensstellung oder einem Verband alle wahrscheinlich vollständig gegliedert ist, sofern Sie ausreichende Infrastruktur auf Azure bereitgestellt haben. Alternative Bereitstellungskonfigurationen wie Replikat DCs (beschreibbare oder RODCs) erhöhen die Wahrscheinlichkeit Link Ausfällen Ertragen.

### <a name="BKMK_ADSiteTopology"></a>Windows Server Active Directory-Standorttopologie

Sie müssen ordnungsgemäß definiert Sites und Site-Links, um Datenverkehr zu optimieren und Kosten. Websites, Website-Links und Subnetze beeinflussen die Replikationstopologie zwischen DCs und den Fluss des Datenverkehrs bei der Benutzerauthentifizierung. Berücksichtigen Sie die folgenden Datenverkehr Zuschläge bereitstellen und DCs gemäß Ihrem Bereitstellungsszenario konfigurieren:

- Es gibt eine geringe Gebühr pro Stunde für den Gateway:

 - Gestartet kann und beendet nach Bedarf

 - Wenn beendet, sind Azure VMs vom Firmennetzwerk isoliert

- Eingehender Datenverkehr ist

- Ausgehender Datenverkehr wird nach [Azure auf einen Preis](http://azure.microsoft.com/pricing/)berechnet. Sie können Standortverknüpfungseigenschaften zwischen lokalen Websites und Cloud-Websites wie folgt optimieren:

 - Wenn mehrere virtuelle Netzwerke verwenden, konfigurieren Sie Hyperlinks der Site und deren Kosten entsprechend um zu verhindern, dass Windows Server AD DS Priorisieren der Azure-Website über die denselben kostenlos bereitstellen kann. Sie sollten die Bridge alle Verknüpfungsoption (BASL) Website (die standardmäßig aktiviert ist) deaktivieren. Dies gewährleistet, dass nur direkt verbundene Standorte miteinander replizieren. DCs transitiv verbundene Standorte sind nicht mehr direkt miteinander replizieren, jedoch müssen über mindestens einen gemeinsamen Standort repliziert. Wenn die dazwischen liegende Sites aus irgendeinem Grund Replikation zwischen Domänencontrollern in transitiv verbundenen Standorten erfolgt nicht verfügbar, wenn die Verbindung zwischen den Standorten verfügbar ist. Wo Abschnitte transitive Replikation Verhalten erwünscht, erstellen Sie Website Standortverknüpfungsbrücken, die entsprechende Site-Links und wie lokal Unternehmensnetzwerk Websites enthalten.

 - [Konfigurieren von Standortverknüpfungskosten](https://technet.microsoft.com/library/cc794882) entsprechend zu unerwünschten Datenverkehr. Beispielsweise wenn **Versuchen nächstliegenden Standort** aktiviert unbedingt virtuelle Netzwerke am nächsten nicht durch Erhöhung der Kosten des Site-Link-Objekts, das die Azure-Website mit dem Unternehmensnetzwerk verbunden sind.

 - Konfigurieren Sie Site Link [Intervalle](https://technet.microsoft.com/library/cc794878) und [Zeitpläne](https://technet.microsoft.com/library/cc816906) konsistenzanforderungen und Änderungsrate-Objekt. Latenz Toleranz Replikationszeitplan ausgerichtet. Domänencontroller replizieren nur den letzten Zustand einen Wert verringert das Replikationsintervall Kosten speichern kann, ist es ausreichend Objekt Änderungsrate.

- Minimierung der Priorität sicher Replikation geplant und Benachrichtigung ist nicht aktiviert. Dies ist die Standardkonfiguration bei der Replikation zwischen Standorten. Dies ist nicht wichtig, bei der Bereitstellung eines RODC in einem virtuellen Netzwerk da RODC keine ausgehende Änderungen replizieren. Aber wenn Sie einen schreibbaren Domänencontroller bereitstellen, Sie sollten sicherstellen, dass Site-Link nicht Updates mit unnötige Replikation konfiguriert ist. Wenn Sie einen globalen Katalogserver (GC) bereitstellen, stellen Sie sicher, dass jeden anderen Standort mit einem globalen Katalog Domänenpartitionen aus einer Quelle Domänencontroller an einem Standort repliziert mit Website-Link oder Site-Links mit geringeren Kosten als GC Azure Standort verbunden ist.


- Es ist möglich, weiter durch die Replikation zwischen Standorten durch Ändern des Komprimierungsalgorithmus Replikation erzeugten Netzwerkverkehr reduzieren. REG_DWORD Registrierung Eintrag HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator Komprimierungsalgorithmus der Komprimierungsalgorithmus gesteuert. Der Standardwert ist 3 entspricht dem Algorithmus Xpress komprimieren. Sie können ändern Sie den Wert 2, wodurch den Algorithmus zu MSZip. In den meisten Fällen dadurch die Komprimierung jedoch auf Kosten der CPU-Auslastung wird. Weitere Informationen finden Sie unter [wie Active Directory-Replikationstopologie](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP-Adressen und DNS

Azure virtuelle Computer sind "DHCP geleasten Adressen" standardmäßig zugewiesen. Da dynamische Adressen Azure virtuelles Netzwerk für die Lebensdauer des virtuellen Computers mit einem virtuellen Computer beibehalten werden, sind Windows Server AD DS erfüllt.

Daher bei der Verwendung einer dynamischen Adresse in Azure sind Sie wirksam mit einer statischen IP-Adresse ist umleitbar die der Lease und die Lease ist die Lebensdauer der Cloud-Dienst entspricht.

Jedoch wird dynamische Adresse freigegeben, wenn die VM heruntergefahren wird. Um zu verhindern, dass die IP-Adresse freigegeben wird, können Sie [Set-AzureStaticVNetIP zum Zuweisen einer statischen IP-Adresse verwenden](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Für die Auflösung von Namen Bereitstellen eigener (oder nutzen Sie Ihr vorhandenes) DNS-Server-Infrastruktur; Azure bereitgestellte DNS erfüllt nicht die erweiterten Namen Auflösung benötigt Windows Server AD DS. Angenommen, es nicht dynamischen SRV-Einträge unterstützen usw.. Auflösung ist eine wichtige Konfigurationselement für Domänencontroller und Domäne. Domänencontroller müssen Ressourceneinträge registrieren und Auflösen von anderen DC-Ressourceneinträge können.
Für Fehler Toleranz und Leistungsgründen ist zum Installieren des Windows Server-DNS-Dienstes auf den Domänencontrollern auf Windows Azure ausgeführte. Konfigurieren Sie die Eigenschaften Azure virtuelles Netzwerk mit Namen und IP-Adresse des DNS-Servers. Beim Starten von anderen virtuellen Computern auf dem virtuellen Netzwerk werden ihre DNS-Resolver-Clienteinstellungen mit DNS-Server als Teil der dynamischen IP-Adresszuweisung konfiguriert.

> [AZURE.NOTE] Sie können nicht auf lokalen Computern Windows Server AD DS, Active Directory-Domäne beitreten, die direkt über das Internet auf Azure gehostet wird. Erforderlichen Port für Active Directory und die Domäne Verknüpfungsoperation es unpraktisch, direkt rendern setzen die notwendigen Ports und wirksam, eine gesamte DC mit dem Internet.

VMs registrieren ihren DNS-Namen automatisch beim Start oder ein.

Weitere Informationen zu diesem Beispiel und ein weiteres Beispiel, das zeigt, wie die erste VM bereitstellen und Installieren von AD DS finden Sie unter [neue Active Directory-Gesamtstruktur für Microsoft Azure installieren](active-directory-new-forest-virtual-machine.md). Weitere Informationen zur Verwendung von Windows PowerShell finden Sie unter [Azure PowerShell installieren](../powershell-install-configure.md) und [Azure Management Cmdlets](https://msdn.microsoft.com/library/azure/jj152841).

### <a name="BKMK_DistributedDCs"></a>Geografisch verteilte DCs

Azure bietet Vorteile, wenn mehrere Domänencontroller in verschiedenen virtuellen Netzwerken:

- Multi-Site-Fehlertoleranz

- Physischen Nähe Zweigstellen (geringere Latenz)

Informationen zum Konfigurieren von direkte Kommunikation zwischen virtuellen Netzwerken finden Sie unter [virtuelle Netzwerkkonnektivität virtuelles Netzwerk konfigurieren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Schreibgeschützte Domänencontroller

Sie müssen wählen, ob Sie schreibgeschützt oder schreibbare Domänencontroller bereitstellen. Sie können geneigt RODCs bereitstellen, da Sie keinen physischen Kontrolle RODCs ihre physische Sicherheit gefährdet wie Zweigstellen ist an Standorten bereitgestellt werden sollen.

Azure nicht das physische Sicherheitsrisiko ein, sondern RODCs könnte noch kostengünstiger, da die Features bieten sie allerdings sehr unterschiedlichen Gründen dieser Umgebungen geeignet sind. Z. B. RODCs keine ausgehende Replikation und selektiv Schlüsseln (Kennwörtern) aufgefüllt. Der Nachteil mangelnde geheimen Schlüssel möglicherweise bei Bedarf ausgehenden Datenverkehr überprüfen als Benutzer oder Computer authentifiziert. Aber Geheimnisse selektiv aufgefüllt und zwischengespeichert.

RODCs bieten einen Vorteil in und um HBI und PII betrifft, da hinzufügen können, die Daten auf den RODC enthalten Attributsatz (FAS) gefiltert. FAS ist eine anpassbare Satz von Attributen, die nicht auf RODCs repliziert werden. Das FAS können vorsichtshalber nicht zulässig oder nicht personenbezogene Informationen oder HBI Azure speichern möchten. Weitere Informationen finden Sie unter [RODC gefilterten Attributgruppe [(https://technet.microsoft.com/library/cc753459)].

Stellen Sie sicher, dass Applikationen mit RODCs kompatibel ist, den Sie verwenden möchten. Viele Windows Server Active Directory-fähigen Programme arbeiten mit RODCs, aber einige Programme fehlschlagen, wenn sie nicht an einen beschreibbaren DC zugreifen oder ineffizient durchführen können. Weitere Informationen finden Sie unter [schreibgeschützten Domänencontroller zur Anwendungskompatibilität](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Globaler Katalog

Sie müssen wählen, ob Sie einen globalen Katalog (GC) installieren. In einer Gesamtstruktur mit einer Domäne sollten Sie alle Domänencontroller als globale Katalogserver konfigurieren. Keine erhöht Kosten werden keine zusätzlichen Replikationsverkehr, da.

In einer Gesamtstruktur mit mehreren Domänen sind GCs universelle Gruppenmitgliedschaften während des Authentifizierungsvorgangs erweitert. Wenn Sie einen globalen Katalog nicht bereitstellen, generiert Arbeitslasten auf das virtuelle Netzwerk einem DC in Azure authentifizieren indirekt ausgehende Authentifizierungsdatenverkehr GCs lokal bei jedem Authentifizierungsversuch abgefragt.

Kosten im Zusammenhang mit globalen Kataloge sind kleiner vorhersehbar, da sie jede Domäne (teilweise) hosten. Wenn die Arbeitslast einen Internetzugriff Dienst hostet und Benutzer mit Windows Server AD DS authentifiziert, konnte die Kosten vollständig unberechenbar. GC Abfragen außerhalb der Cloud-Website während der Authentifizierung reduzieren können Sie das [Zwischenspeichern der universellen Gruppenmitgliedschaft aktivieren](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Installationsmethode

Sie müssen auswählen, wie die DCs im virtuellen Netzwerk installieren:

- Neue Domänencontroller heraufstufen. Weitere Informationen finden Sie in der [Installation einer neuen Active Directory-Gesamtstruktur in Azure virtual Network](active-directory-new-forest-virtual-machine.md).

- Verschieben Sie die virtuelle Festplatte einer virtuellen lokalen DC in die Cloud. In diesem Fall müssen Sie der virtuellen lokalen DC ", nicht"kopiert"oder"geklonte"verschoben"

Verwenden Sie nur Azure VMs DCS (im Gegensatz zu Azure "Web" oder "Arbeitskraft" Rolle VMs). Sie sind langlebig und Haltbarkeit des Status für einen Domänencontroller erforderlich. Azure virtuellen Maschinen dienen zur Arbeitslasten wie DCs.

Verwenden Sie SYSPREP nicht bereitgestellt oder Klonen DCs. Die Fähigkeit, DCs Klonen beginnt nur verfügbar in Windows Server 2012. Cloning-Funktion erfordert Unterstützung für VMGenerationID im zugrunde liegenden Hypervisor. Hyper-V in Windows Server 2012 und Azure virtuelle Netzwerke unterstützen VMGenerationID, wie Anbietern von Drittanbietern Virtualisierungssoftware.

### <a name="BKMK_PlaceDB"></a>Platzierung von Windows Server AD DS-Datenbank und SYSVOL

Wählen Sie Windows Server AD DS-Datenbank, Protokollen und SYSVOL finden. Sie müssen auf Azure Datenträger bereitgestellt werden.

> [AZURE.NOTE] Azure Datenträger sind auf 1 TB beschränkt.

Festplattenlaufwerke Daten werden standardmäßig nicht Cache schreibt. Daten von Festplatten, die eine VM verwenden Durchschreibcache zugeordnet sind. Durchschreibcache sorgt dafür engagiert schreiben dauerhaften Azure-Speicher vor die Buchung aus der Perspektive des Betriebssystems des virtuellen Computers abgeschlossen ist. Es bietet Haltbarkeit Kosten etwas langsamer schreibt.

Dies ist wichtig für Windows Server AD DS, da Schreibvorgänge Datenträgerspeicher Annahmen der DC erklärt. Windows Server AD DS versucht, Schreib-Cache deaktivieren, jedoch ist der Datenträger-e/a-System es berücksichtigt. Fehler beim Zwischenspeichern deaktivieren, unter Umständen ein USN-Rollback was veraltete Objekte und andere Probleme entstehen.

Empfohlen für virtuelle Domänencontroller führen Sie folgende Schritte aus:

- Host-Cache-Einstellung auf den Azure keine fest Dadurch werden Probleme mit Schreibcache für AD DS-Operationen.

- Speichern von Datenbank, Protokollen und SYSVOL auf den beiden Datenträger oder separate Datenträger. Normalerweise ist dies ein separater Datenträger von der Festplatte für das Betriebssystem selbst verwendet. Der Punkt ist, dass Windows Server AD DS-Datenbank und SYSVOL nicht auf einen Datenträger Azure Betriebssystemtyp gespeichert werden müssen. Standardmäßig installiert die AD DS-Installation dieser Komponenten in %SystemRoot% für Azure nicht empfohlen.

### <a name="BKMK_BUR"></a>Sicherung und Wiederherstellung

Achten Sie Was ist wird nicht für Backup und Wiederherstellung eines Domänencontrollers im Allgemeinen und insbesondere in einer VM ausgeführt. [Sicherung und Wiederherstellung für virtualisierte Domänencontroller](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers)anzeigen

Erstellen Sie System State Backups mithilfe nur backup-Software, die speziell bewusst backup für Windows Server AD DS wie Windows Server-Sicherung.

Kopieren Sie oder Klonen Sie VHD-Dateien der Domänencontroller anstatt regelmäßige Backups. Eine Wiederherstellung sollte immer erforderlich, damit virtuelle Festplatten geklonte oder kopierte Windows Server 2012 und unterstützten Hypervisor USN Bläschen führen wird

### <a name="BKMK_FedSrvConfig"></a>Verbund-Serverkonfiguration

Die Konfiguration des Windows Server AD FS-Verbundserver (Tata_signal) hängt teilweise Anträge auf Azure bereitgestellt werden soll, ob Zugriff auf Ressourcen im lokalen Netzwerk benötigen.

Wenn die Anwendung die folgenden Kriterien erfüllen, können Sie die Anwendung isoliert aus dem lokalen Netzwerk bereitstellen.

- Sie akzeptieren SAML-Sicherheitstoken

- Sie sind mit dem Internet exponierbarer

- Sie greifen nicht auf lokale Ressourcen

In diesem Fall Windows Server AD FS Tata_signal wie folgt konfigurieren:

1. Konfigurieren einer isolierten Einzeldomänen-Gesamtstruktur auf Azure.

2. Zugriff verbundenen der Gesamtstruktur Windows Server AD FS-Verbundserverfarm konfigurieren.

3. Konfigurieren Sie Windows Server AD FS (Verbundserverfarm und Verbundserverfarm Proxy) in der lokalen Gesamtstruktur.

4. Richten Sie eine Vertrauensstellung Föderation zwischen lokalen und Azure Instanzen von Windows Server AD FS.

Andererseits, wenn Anträge auf lokale Ressourcen zugreifen müssen, können Sie Windows Server AD FS mit der Anwendung in Azure wie folgt konfigurieren:

1. Konfigurieren der Konnektivität zwischen lokalen Netzwerken und Azure.

2. Konfigurieren Sie einen Windows Server AD FS Verbundserverfarm in der lokalen Gesamtstruktur.

3. Konfigurieren Sie einen Windows Server AD FS Verbundserverfarm Proxy auf Azure.

Diese Konfiguration hat den Vorteil der Reduzierung der lokalen Ressourcen, wie Windows Server AD FS mit in einem Perimeternetzwerk konfigurieren.

Beachten Sie, dass in beiden Fällen Sie Vertrauensstellungen mit mehr Identitätsprovider herstellen können ggf. geschäftliche Zusammenarbeit.

### <a name="BKMK_CloudSvcConfig"></a>Cloud-Services-Konfiguration

Cloud-Dienste sind erforderlich, wenn Sie einen virtuellen Computer direkt mit dem Internet verfügbar machen oder eine Anwendung Internetzugriff mit Lastenausgleich. Dies ist möglich, da jeder Cloud-Dienst eine einzige konfigurierbare virtuelle IP-Adresse bietet.

### <a name="BKMK_FedReqVIPDIP"></a>Föderation an öffentliche und private IP-Adressierung (dynamische IP und virtuelle IP-Adresse)

Jede Azure virtuelle Maschine erhält eine dynamische IP-Adresse. Eine dynamische IP-Adresse ist eine private Adresse in Azure. In den meisten Fällen jedoch werden eine virtuelle IP-Adresse für die Windows Server AD FS-Bereitstellung konfiguriert. Virtuelle IP-Adresse muss Windows Server AD FS-Endpunkte zum Internet und Föderationspartner und Clients für Authentifizierung und laufende Verwaltung verwendet. Eine virtuelle IP-Adresse ist eine Eigenschaft eines Cloud-Diensts enthält ein oder mehrere virtuelle Azure-Computer. Sind die Ansprüche auf Azure und Windows Server AD FS bereitgestellte Anwendung Internetzugriff und Freigabe verwendeten Ports, jede erfordert eine virtuelle IP-Adresse selbst, und daher muss ein Cloud-Dienst für die Anwendung und ein zweites für Windows Server AD FS.

Definitionen der Begriffe virtuelle IP-Adresse und dynamische IP-Adresse finden Sie [Begriffe und Definitionen](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS-Konfiguration mit hoher Verfügbarkeit

Beim eigenständigen Windows Server AD FS-Verbunddienste bereitstellen kann, wird empfohlen, eine Serverfarm mit mindestens zwei Knoten für AD FS STS und Proxys für die Produktion bereitstellen.

Finden Sie unter [AD FS 2.0-Topologie Bereitstellungsaspekte](https://technet.microsoft.com/library/gg982489) in die [AD FS 2.0 Design Guide](https://technet.microsoft.com/library/dd807036) entscheiden die optimale Bereitstellung Konfigurationsoptionen Ihren Bedürfnissen anpassen.

> [AZURE.NOTE] Um Lastenausgleich für Windows Server AD FS-Endpunkte auf Azure zu erhalten alle Mitglieder der Windows Server AD FS-Farm im selben Cloud-Dienst konfigurieren und Load balancing Funktion Azure für HTTP (Standard: 80) und HTTPS-Ports (Standard: 443) verwenden. Weitere Informationen finden Sie unter [Azure Lastenausgleich Prüfpunkt](https://msdn.microsoft.com/library/azure/jj151530).
Windows Server Network Load Balancing (NLB) wird auf Azure nicht unterstützt.
