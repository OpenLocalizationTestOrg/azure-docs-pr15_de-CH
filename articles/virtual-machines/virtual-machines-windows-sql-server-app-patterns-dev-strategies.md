<properties
    pageTitle="SQL Server-Anwendungsmuster auf VMs | Microsoft Azure"
    description="Dieser Artikel behandelt die Anwendungsmuster für SQL Server auf Azure VMs. Es bietet Lösungsarchitekten und Entwicklern eine Grundlage für gute Architektur und Design."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="luisherring"
    manager="jhubbard"
    editor=""
    tags="azure-service-management,azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="lvargas" />

# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Anwendungsmuster und Entwicklungsstrategien für SQL Server in Azure Virtual Machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="summary"></a>Zusammenfassung:
Ermitteln, welche Anwendungsmuster oder Muster für die SQL Server-basierten Anwendung in Azure Umgebung ist eine wichtige Entscheidung und erfordert Kenntnisse wie SQL Server und jede Infrastrukturkomponente Azure zusammenarbeiten. Mit SQL Server in Azure Infrastruktur-Services können Sie problemlos migrieren, verwalten und überwachen die vorhandenen SQL Server-Applikationen erstellt-unter Windows Server mit virtuellen Computern in Azure.

Das Ziel dieses Artikels ist Lösungsarchitekten und Entwickler bieten eine Grundlage für gute Architektur und Design bei vorhandenen Anwendungsmigration Azure sowie neue Anwendungsentwicklung in Azure.

Für jedes Anwendungsmuster finden Sie einen lokalen Szenario der jeweiligen Cloud-fähigen Lösung und zugehörige technische Hinweise. Dieser Artikel erläutert darüber hinaus Azure-spezifischen Strategien, damit Ihre Anwendung ordnungsgemäß entwerfen können. Durch viele mögliche Anwendungsmuster empfiehlt es sich, Architekten und Entwickler das am besten geeignete Muster für ihre Anwendung und Benutzer wählen.

**Technische Mitarbeiter:** Luis Carlos Vargas Hering Madhan Arumugam Ramakrishnan

**Technische Prüfer:** Corey Sanders Drew McDaniel Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Einführung

Trennung der Komponenten von verschiedenen Anwendungsebenen auf verschiedenen Computern sowie in separate Komponenten können Sie viele Arten von n-Tier-Anwendung entwickeln. Beispielsweise die Clientanwendung platzieren und Geschäftsregeln in einer Maschine, Front-End-Web-Tier- und Datenebenenkomponenten auf einem anderen Computer Zugriff und eine Ebene Back-End-Datenbank auf einem anderen Computer. Diese Art strukturieren kann jede Ebene voneinander zu isolieren. Wenn Sie ändern, woher die Daten stammen, müssen Sie Client oder Web-Anwendung jedoch nur die Tier-Komponenten ändern.

Normale *n-Tier* -Anwendung enthält die Präsentationsebene, die Geschäftsebene und die Datenebene:


| Ebene              | Beschreibung                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Präsentation** | Die *Darstellungsebene* (Webebene, Front-End-Ebene) ist die Ebene, auf der Benutzer mit einer Anwendung interagieren.                                                                      |
| **Unternehmen**     | Das *tier Business* (mittlere Ebene) ist die Ebene, die Präsentation Datenebene, und Datenebene miteinander kommunizieren und die Kernfunktionen des Systems. |
| **Daten**         | Die *Datenebene* ist im Grunde der Server, der die Daten einer Anwendung (z. B. einen Server mit SQL Server) gespeichert.                                                             |

Anwendungsebenen beschreiben logischen Gruppen von Funktionen und Komponenten in einer Anwendung. die Ebenen die physische Verteilung der Funktionen und Komponenten auf separaten physischen Servern, Computern, Netzwerken oder Remotestandorten beschreiben. Ebenen einer Anwendung können auf demselben physischen Computer (der gleichen Ebene) befinden oder über separate Computer (n-Tier) verteilt werden können und die Komponenten in jeder Ebene mit Komponenten in anderen Schichten über klar definierte Schnittstellen kommunizieren. Sie können der Begriff Ebene auf physischen Verteilungsmuster zweistufigen dreistufige und mehrstufige vorstellen. Ein **Muster 2-Tier-Anwendung** enthält zwei Anwendungsebenen: Anwendungsserver und Datenbankserver. Direkte Kommunikation geschieht zwischen dem Anwendungsserver und dem Datenbankserver. Der Application Server enthält Web- und Unternehmensebene Komponenten. Im **3-Tier-Anwendungsmuster**sind drei Anwendungskategorien: Webserver, Anwendungsserver, enthält die Geschäftslogikebene und Business-Tier-Komponenten und der Datenbankserver. Die Kommunikation zwischen dem Webserver und dem Datenbankserver geschieht über Application Server. Informationen zur Anwendungsschichten und Ebenen finden Sie unter [Microsoft Application Architecture Guide](https://msdn.microsoft.com/library/ff650706.aspx).

Sie vor dem Lesen dieses Artikels benötigen Sie Kenntnisse über die grundlegenden Konzepte von SQL Server und Azure. Informationen finden Sie in der [Onlinedokumentation zu SQL Server](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) und [Azure.com](https://azure.microsoft.com/).

Dieser Artikel beschreibt mehrere Anwendungsmuster, die für die einfache Anwendung der komplexen Anwendung sein können. Bevor die Details jedes Muster, empfehlen wir, dass Sie sich mit den verfügbaren Daten Storage Services in Azure [Azure Storage](../storage/storage-introduction.md)und [Azure SQL-Datenbank](../sql-database/sql-database-technical-overview.md)mit [SQL Server ein Azure Virtual Machine](virtual-machines-windows-sql-server-iaas-overview.md)sollte. Zu den besten Designentscheidungen für die Anwendung, wann welche Storage-Service klar verstehen.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Wählen Sie SQL Server in Azure VM, wenn:

- Sie benötigen auf SQL Server und Windows-Steuerelement. Dies könnte beispielsweise SQL Server Version, spezielle Hotfixes, Leistungskonfiguration usw..

- Kompatibilität mit lokalen SQL Server benötigen und vorhandene Anwendung in Azure als verschieben-ist.

- Die Funktionen der Azure-Umgebung nutzen möchten, aber Azure SQL-Datenbank unterstützt nicht alle Features, die die Anwendung erfordert. Dazu zählen die folgenden Bereiche:

    - **Größe**: Zeitpunkt dieser Artikel wurde aktualisiert SQL Datenbank Datenbank bis zu 1 TB Daten unterstützt. Wenn Ihre Anwendung mehr als 1 TB Daten erfordert und benutzerdefinierte Sharding-Lösungen möchten, empfiehlt es sich, dass SQL Server eine Azure Virtual Machine verwenden. Aktuelle Informationen finden Sie unter [Skalierung, Azure SQL-Datenbanken](https://msdn.microsoft.com/library/azure/dn495641.aspx) und [Azure SQL Datenbank Dienstebenen und Leistung](../sql-database/sql-database-service-tiers.md).
    - **HIPAA-Compliance**: Gesundheitswesen Kunden und unabhängigen Softwareanbietern (ISVs) können [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) [Azure SQL-Datenbank](../sql-database/sql-database-technical-overview.md) , da SQL Server eine Azure Virtual Machine von HIPAA Business zuordnen Vereinbarung (BAA) behandelt. Weitere Informationen über die Kompatibilität [Microsoft Azure Trust Center: Compliance](https://azure.microsoft.com/support/trust-center/compliance/).
    - **Instanz - Funktionen**: zurzeit SQL Datenbank keine Funktionen unterstützt, die außerhalb der Datenbank (z. B. verknüpfte Server Agent-Aufträge, FileStream Service Broker usw..). Weitere Informationen finden Sie unter [Azure SQL-Datenbank-Richtlinien und Einschränkungen](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>Ebene 1 (einfach): virtuelle Maschine

In diesem Anwendungsmuster stellen Sie die SQL Server-Anwendung und Datenbank einer eigenständigen virtuellen Maschine in Azure. Derselbe virtuelle Computer enthält die Client-Web-Anwendung, Geschäftskomponenten Datenzugriffsschicht und Datenbankserver. Präsentations-, Geschäfts- und Datenzugriffscode logisch getrennt, aber auf einem einzelnen Server Computer physisch befinden. Die meisten Kunden mit dieser Anwendung starten und dann sie indem Sie weitere Webrollen oder virtuelle Computer auf ihr System.

Dieses Anwendungsmuster ist hilfreich, wenn

- Durchführen eine einfache Migration Azure-Plattform zu beurteilen, ob die Plattform Anforderungen der Anwendung beantwortet werden soll.

- Alle Anwendungsebenen im gleichen virtuellen Computer im gleichen Azure Rechenzentrum die Latenz zwischen Ebenen gehostet werden sollen.

- Möchten schnell Entwicklung bereitstellen und Testen der Umgebung für kurze Zeit.

- Belastungstests für unterschiedlichem Arbeitslast ausführen möchten aber gleichzeitig nicht möchten und viele physische Computer jederzeit verwalten.

Die folgende Abbildung veranschaulicht einen einfachen lokalen Szenario und wie seine aktiviert Cloudlösung in einer virtuellen Maschine in Azure bereitstellen können.

![1-Tier-Anwendungsmuster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Bereitstellen der Geschäftsschicht (Komponenten Zugriff auf Daten und Geschäftslogik) auf derselben physischen Ebene als Darstellungsschicht kann Application Performance maximieren, wenn Sie eine separate Ebene Skalierbarkeits- oder Sicherheit Gründen verwenden müssen.

Da dies ein sehr häufiges Muster zu sinnvoll, den folgenden Artikel zur Migration zum Verschieben von Daten in der SQL Server-VM: [Migrieren einer Datenbank in SQL Server auf eine Azure-VM](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3-Tier (einfach): mehrere virtuelle Maschinen

In diesem Anwendungsmuster bereitgestellt eine 3-Tier-Anwendung in Azure mit jeder Anwendungsebene auf einem anderen virtuellen Computer. Dies bietet eine flexible Umgebung für eine einfach skalieren und Dezentrales Skalieren. Bei einem virtuellen Computer die Client-Web-Anwendung enthält, die die Geschäftskomponenten hostet und anderen Datenbankserver hostet.

Dieses Anwendungsmuster ist hilfreich, wenn

- Migration von komplexen Anwendung auf Azure Virtual Machines durchführen möchten.

- Andere Anwendungsebenen in unterschiedlichen Regionen gehostet werden soll. Beispielsweise können Sie Datenbanken freigegeben haben, die mehrere Bereiche für Berichtszwecke bereitgestellt werden.

- Anwendung von lokalen virtualisierten Plattformen zu Azure virtuelle Computer verschieben möchten. Eine ausführliche Erläuterung zum Anwendung finden Sie unter [Was ist eine Enterprise-Anwendung](https://msdn.microsoft.com/library/aa267045.aspx).

- Möchten schnell Entwicklung bereitstellen und Testen der Umgebung für kurze Zeit.

- Belastungstests für unterschiedlichem Arbeitslast ausführen möchten aber gleichzeitig nicht möchten und viele physische Computer jederzeit verwalten.

Das folgende Diagramm veranschaulicht, wie eine einfache 3-Tier-Anwendung in Azure platzieren, platzieren Sie jede Anwendungsebene auf einem anderen virtuellen Computer.

![3-Tier-Anwendungsmuster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

In diesem Anwendungsmuster gibt es nur eine Virtual Machine (VM) in einer Ebene. Haben Sie mehrere VMs in Azure, empfiehlt sich das Einrichten eines virtuellen Netzwerks. [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) erstellt eine vertrauenswürdige Sicherheitsgrenze und VMs Kommunikation untereinander über die private IP-Adresse ermöglicht. Darüber hinaus stellen Sie sicher, dass alle Internet-Verbindungen nur auf die Präsentationsebene. Bei diesem Anwendungsmuster verwalten Sie die Gruppe Netzwerksicherheitsregeln Zugriffskontrolle. Weitere Informationen finden Sie in [externen Zugriff für die VM mit Azure-Portal](virtual-machines-windows-nsg-quickstart-portal.md).

Im Diagramm können Internetprotokolle TCP, UDP, HTTP oder HTTPS sein.

>[AZURE.NOTE] Einrichten eines virtuellen Netzwerks in Azure ist kostenlos. Sie sind jedoch für das VPN-Gateway berechnet, die lokale Verbindung. Diese Gebühr basiert auf die Zeitdauer, die Verbindung bereitgestellt und verfügbar ist.

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>tier 2-Ebenen- und 3-Tier mit Skalierung

In diesem Anwendungsmuster bereitgestellt Tier 2 oder 3-Tier-datenbankanwendung auf Azure Virtual Machines mit jeder Anwendungsebene auf einem anderen virtuellen Computer. Darüber hinaus skalieren Sie die Präsentationsebene aufgrund erhöhten eingehende Clientanforderungen.

Dieses Anwendungsmuster ist hilfreich, wenn

- Anwendung von lokalen virtualisierten Plattformen zu Azure virtuelle Computer verschieben möchten.

- Die Präsentationsebene aufgrund erhöhten eingehende Clientanforderungen skalieren möchten.

- Möchten schnell Entwicklung bereitstellen und Testen der Umgebung für kurze Zeit.

- Belastungstests für unterschiedlichem Arbeitslast ausführen möchten aber gleichzeitig nicht möchten und viele physische Computer jederzeit verwalten.

- Möchten Sie eine infrastrukturumgebung besitzen, die oben und unten bei Bedarf skalieren können.

Die folgende Abbildung veranschaulicht, wie Sie den Anwendungsebenen in mehrere virtuelle Computer in Azure durch dezentrales Skalieren der Präsentationsebene aufgrund erhöhten eingehende Clientanforderungen platzieren. Wie das Diagramm dient Azure Lastenausgleich Verteilen des Datenverkehrs über mehrere virtuelle Maschinen und auch der Webserver herstellen. Mehrere Instanzen der Webserver hinter einem Lastenausgleich gewährleistet hohe Verfügbarkeit der Präsentationsebene.

![Anwendungsmuster - Präsentation Ebene skalieren](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Best Practices für die 2-Tier-3-Tier oder n-Tier-Muster, die mehrere VMs in einer Ebene

Es wird empfohlen, platzieren den virtuellen Computern, die auf der gleichen Ebene im selben Cloud-Dienst und in der gleichen die Verfügbarkeit gehören festgelegt. Beispielsweise legen Sie einen Webserver in **CloudService1** und **AvailabilitySet1** sowie einen Datenbankserver in **CloudService2** und **AvailabilitySet2**. Verfügbarkeit in Azure festlegen können Sie hohe Verfügbarkeit Knoten in separaten Fehlerdomänen und Domänen zu aktualisieren.

Um mehrere VM-Instanzen von einer Ebene zu nutzen, müssen Sie Azure Lastenausgleich zwischen Anwendungsebenen konfigurieren. Zum Lastenausgleich auf jeder Ebene konfigurieren, erstellen Sie ein Lastenausgleich jedes Tier VMs getrennt. Für eine bestimmte Ebene zuerst VMs in derselben Cloud-Dienst erstellen. Dadurch haben sie die öffentliche virtuelle IP-Adresse. Erstellen Sie anschließend einen Endpunkt auf einem virtuellen Computer auf dieser Ebene. Danach weisen Sie dieselbe den anderen virtuellen Computern auf dieser Ebene für den Lastenausgleich. Erstellen Sie eine Gruppe mit Lastenausgleich, Datenverkehr auf mehrere virtuelle Computer verteilen und gleichzeitig ermöglichen Lastenausgleich Knoten Verbindung beim Ausfall eines Knotens Backend-VM bestimmen. Mehrere Instanzen der Webserver hinter einem Lastenausgleich wird die hohe Verfügbarkeit der Präsentationsebene beispielsweise sichergestellt.

Empfohlen stellen Sie sicher, dass alle Internet-Verbindungen zuerst auf die Präsentationsebene. Die Präsentationsebene greift auf Geschäftsebene und die Geschäftsebene greift dann auf Datenebene. Weitere Informationen zum Zugriff auf die Präsentationsebene anzeigen Sie [externen Zugriff für die VM mit Azure-portal](virtual-machines-windows-nsg-quickstart-portal.md)

Beachten Sie, dass der Lastenausgleich in Azure ähnlich Balancers in einer lokalen Umgebung geladen. Weitere Informationen finden Sie in der [Lastenausgleich für Azure Infrastrukturdienste](virtual-machines-windows-load-balance.md).

Darüber hinaus empfehlen wir, dass Sie ein privates Netzwerk für die virtuellen Computer Einrichten von Azure virtuelles Netzwerk. Dies ermöglicht ihnen die Kommunikation untereinander über private IP-Adresse. Weitere Informationen finden Sie in [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>tier 2-Ebenen- und 3-Tier mit Skalierung

In diesem Anwendungsmuster bereitgestellt Tier 2 oder 3-Tier-datenbankanwendung auf Azure Virtual Machines mit jeder Anwendungsebene auf einem anderen virtuellen Computer. Sie möchten außerdem Anwendungsserverkomponenten mehreren virtuellen Maschinen aufgrund der Komplexität der Anwendung verteilen.

Dieses Anwendungsmuster ist hilfreich, wenn

- Anwendung von lokalen virtualisierten Plattformen zu Azure virtuelle Computer verschieben möchten.

- Möchten Sie die Anwendungsserverkomponenten mehreren virtuellen Maschinen aufgrund der Komplexität der Anwendung verteilen.

- Business Logic hohe LOB (Line of Business) Applications, Azure Virtual Machines lokalen verschieben möchten. LOB sind eine Reihe von wichtigen computeranwendung, die für ein Unternehmen wie Buchhaltung, Human Resources (HR), Lohn, Supply Chain Management und ressourcenplanung Anwendung ausgeführt.

- Möchten schnell Entwicklung bereitstellen und Testen der Umgebung für kurze Zeit.

- Belastungstests für unterschiedlichem Arbeitslast ausführen möchten aber gleichzeitig nicht möchten und viele physische Computer jederzeit verwalten.

- Möchten Sie eine infrastrukturumgebung besitzen, die oben und unten bei Bedarf skalieren können.

Die folgende Abbildung veranschaulicht einen lokalen Szenario und deren Cloudlösung aktiviert. In diesem Szenario wird die Anwendungsebenen in mehrere virtuelle Computer in Azure durch dezentrales Skalieren der Geschäftsebene Geschäftslogikebene und Datenzugriffskomponenten platzieren. Wie das Diagramm dient Azure Lastenausgleich Verteilen des Datenverkehrs über mehrere virtuelle Maschinen und auch der Webserver herstellen. Mehrere Instanzen der Anwendungsserver hinter einem Lastenausgleich gewährleistet hohe Verfügbarkeit der Geschäftsebene. Weitere Informationen finden Sie unter [Best Practices für 2, 3-Tier oder n-Tier-Anwendungsmuster, die mehrere virtuelle Computer in einer Ebene aufweisen](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Anwendungsmuster mit Business Ebene skalieren](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>2-Ebenen- und 3-Tier Präsentation Business Ebenen skalieren und HADR

In diesem Muster Application bereitgestellt Tier 2 oder 3-Tier-datenbankanwendung auf Azure Virtual Machines verteilt die Präsentationsebene (Webserver) und Business-Tier (Anwendungsserver) Komponenten mehreren virtuellen Maschinen. Darüber hinaus implementieren Sie hohe Verfügbarkeit und Disaster Recovery-Lösung für Datenbanken in Azure virtuellen Maschinen.

Dieses Anwendungsmuster ist hilfreich, wenn

- Gewünschte Anwendung lokal virtualisierten Plattformen in Azure SQL Server hohe Verfügbarkeit und Disaster Recovery-Funktionen.

- Die Präsentationsebene, die Geschäftsebene zunehmende Zahl von Clientanfragen und der Komplexität Ihrer Anwendung skaliert werden soll.

- Möchten schnell Entwicklung bereitstellen und Testen der Umgebung für kurze Zeit.

- Belastungstests für unterschiedlichem Arbeitslast ausführen möchten aber gleichzeitig nicht möchten und viele physische Computer jederzeit verwalten.

- Möchten Sie eine infrastrukturumgebung besitzen, die oben und unten bei Bedarf skalieren können.

Die folgende Abbildung veranschaulicht einen lokalen Szenario und deren Cloudlösung aktiviert. In diesem Szenario skalieren Sie die Präsentationsebene und Tier Geschäftskomponenten in mehrere virtuelle Computer in Azure. Darüber hinaus implementieren Sie hohe Verfügbarkeit und Disaster Recovery (HADR) Techniken für SQL Server-Datenbanken in Azure.

Ausführen mehrerer Kopien einer Anwendung in verschiedenen unbedingt VMs Ausgleich der Anforderungslast sind. Wenn Sie mehrere virtuelle Computer haben, müssen Sie sicherstellen, dass alle virtuellen Computer zugreifen und an einer Stelle ausgeführt sind. Konfigurieren des Lastenausgleichs Azure Lastenausgleich überwacht den Zustand der VMs und leitet eingehende Anrufe an die fehlerfreien funktionierenden VM Knoten ordnungsgemäß. Informationen zum Einrichten des Lastenausgleichs von virtuellen Computern finden Sie unter [Lastenausgleich für Azure Infrastrukturdienste](virtual-machines-windows-load-balance.md). Mehrere Instanzen der Web- und Server hinter einem Lastenausgleich gewährleistet die hohe Verfügbarkeit der Präsentations- und leisten.

![Dezentrales Skalieren und hohe Verfügbarkeit](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Best Practices für die Anwendungsmuster erfordert SQL HADR

Beim Einrichten von SQL Server hohe Verfügbarkeit und Disaster Recovery-Lösung in Azure Virtual Machines ist ein virtuelles Netzwerk für die virtuellen Computer [Azure virtuelles](../virtual-network/virtual-networks-overview.md) Netzwerk erforderlich.  Virtuelle Computer in einem virtuellen Netzwerk wird eine stabile private IP-Adresse auch nach einem Ausfallzeiten, umgehen Sie somit die Aktualisierungszeit für DNS-Auflösung erforderlich. Darüber hinaus das virtuelle Netzwerk können Sie den lokalen Netzwerk in Azure erweitern und vertrauenswürdigen Sicherheitsgrenze erstellt. Beispielsweise verfügt die Anwendung Unternehmensdomäne beschränkt (wie Windows-Authentifizierung, Active Directory), muss [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) einrichten.

Die meisten Kunden Produktionscode in Azure ausführen, halten primäre und sekundäre Replikate in Azure.

Umfassende Informationen und Lernprogramme auf hohe Verfügbarkeit und Disaster Recovery-Verfahren finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>2-Ebenen- und 3-Tier-Azure VMs mit Cloud-Diensten

In diesem Anwendungsmuster bereitstellen Tier 2 oder 3-Tier-Anwendung in Azure mit beiden [Azure Cloud Services](../cloud-services/cloud-services-choose-me.md#tellmecs) (Web- und Workerrollen Rollen - Plattform als Service (PaaS)) und [Azure Virtual Machines](virtual-machines-windows-about.md) (Infrastructure as a Service (IaaS)). Mithilfe von [Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) für die Präsentationsebene Tier-Unternehmen und SQL Server in [Azure Virtual Machines](virtual-machines-windows-about.md) für die Datenebene ist für die meisten auf Windows Azure ausgeführte. Deshalb müssen eine Computinginstanz Cloud Services eine einfache Verwaltung, Bereitstellung, Überwachung und Dezentrales Skalieren bietet.

Mit Cloud-Diensten Azure verwaltet die Infrastruktur zur Routinewartung führt, patches Betriebssysteme und Service und Hardwareausfälle Wiederherstellung versucht. Wenn Ihre Anwendung skalieren, werden automatische und manuelle Skalierung Optionen für Cloud-Dienstprojekt erhöhen oder verringern die Anzahl der Instanzen oder virtuelle Computer, die von der Anwendung verwendet werden. Lokalen Visual Studio können Sie darüber hinaus Ihre Anwendung ein Cloud-Dienstprojekt in Azure bereitstellen.

Zusammenfassend möchten Sie umfassende eigene Verwaltungsaufgaben für die Präsentation Unternehmen tier und Ihre Anwendung nicht erfordern keine komplexe Konfiguration der Software oder des Betriebssystems, Azure Cloud Services verwenden. Wenn Azure SQL-Datenbank nicht alle Funktionen, die Sie suchen unterstützt, verwenden Sie SQL Server ein Azure Virtual Machine für die Datenebene. Ausführen einer Anwendung in Azure Cloud Services und Speichern von Daten in Azure Virtual Machines kombiniert die Vorteile beider Dienste. Einen ausführlichen Vergleich finden Sie im Abschnitt in diesem Thema auf den [Entwicklungsstrategien in Azure vergleichen](#comparing-web-development-strategies-in-azure).

In diesem Anwendungsmuster umfasst die Präsentationsebene Webrolle, läuft eine Cloud-Services-Komponente in der Azure Umgebung angepasst für Webprogrammierung von IIS und ASP.NET unterstützt. Geschäfts- oder Back-End-Ebene umfasst eine Worker-Rolle, läuft eine Cloud-Services-Komponente in der Azure Umgebung eignet sich für allgemeine Entwicklung und Hintergrund für eine Webrolle ausführen kann. Die Datenbankebene befindet sich auf einem virtuellen SQL Server-Computer in Azure. Die Kommunikation zwischen der Präsentations- und der Datenbankebene erfolgt direkt oder über die Geschäftsebene – Worker-Rolle Komponenten.

Dieses Anwendungsmuster ist hilfreich, wenn

- Gewünschte Anwendung lokal virtualisierten Plattformen in Azure SQL Server hohe Verfügbarkeit und Disaster Recovery-Funktionen.

- Möchten Sie eine infrastrukturumgebung besitzen, die oben und unten bei Bedarf skalieren können.

- Azure SQL-Datenbank unterstützt nicht alle Features, die die Anwendung oder Datenbank.

- Belastungstests für unterschiedlichem Arbeitslast ausführen möchten aber gleichzeitig nicht möchten und viele physische Computer jederzeit verwalten.

Die folgende Abbildung veranschaulicht einen lokalen Szenario und deren Cloudlösung aktiviert. In diesem Fall setzen Sie die Präsentationsebene Webrollen Geschäftsebene Workerthreads aber der Datenebene in virtuellen Maschinen in Azure. Mehrere Kopien der Präsentationsebene Webs Rollen ausführen gewährleistet Saldo fordert Sie laden. Kombinieren von Azure Cloud Services mit Azure Virtual Machines empfiehlt Microsoft [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) sowie einrichten. [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)haben Sie stabile und permanente private IP-Adressen im selben Clouddienst in der Cloud. Definieren ein virtuelles Netzwerk für virtuelle Computer und cloud-Services können sie kommunizieren untereinander über die private IP-Adresse. Darüber hinaus mit virtuellen Maschinen Azure Web-Worker-Rollen im gleichen [Virtuellen Netzwerk Azure](../virtual-network/virtual-networks-overview.md) niedrige Latenz und sichere Verbindung. Weitere Informationen finden Sie unter [Was ist ein Clouddienst](../cloud-services/cloud-services-choose-me.md).

Wie das Diagramm Azure Lastenausgleich Datenverkehr über mehrere virtuelle Maschinen hinweg verteilt und bestimmt die Webserver oder Anwendungsserver herstellen. Mehrere Instanzen der Web- und Server hinter einem Lastenausgleich gewährleistet hohe Verfügbarkeit der Präsentationsebene und die Geschäftsebene. Weitere Informationen finden Sie unter [Best Practices für die Anwendungsmuster SQL HADR erfordern](#best-practices-for-application-patterns-requiring-sql-hadr).

![Anwendungsmuster mit Cloud-Diensten](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Ein anderer Ansatz zum Implementieren dieses Anwendungsmusters ist eine konsolidierte Webrolle mit Präsentationsebene und Geschäftskomponenten Tier, wie im folgenden Diagramm dargestellt. Diese Anwendungsmuster ist nützlich für Applikationen, die statusbehaftete Design erfordern. Da Azure statusfreie Compute-Knoten auf Web-und Workerrollen bietet, empfehlen wir implementieren eine Logik zum Speichern des Sitzungszustands mithilfe einer der folgenden Technologien: [Azure Caching](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure-Tabellenspeicher](../storage/storage-dotnet-how-to-use-tables.md) oder [Azure SQL-Datenbank](../sql-database/sql-database-technical-overview.md).

![Anwendungsmuster mit Cloud-Diensten](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Mit VMs Azure, SQL Azure-Datenbank und Azure App Service (webapps)

Das Hauptziel dieses Musters Anwendung ist kombinieren Azure-Infrastruktur als Service (IaaS) Komponenten mit Azure Platform-as-a-Service (PaaS) in der Projektmappe anzeigen. Dieses Muster konzentriert sich auf Azure SQL-Datenbank für relationale Datenspeicher. Es enthält keine SQL Server unter einer Azure Virtual Machine gehört der Azure-Infrastruktur als ein Service-Angebot.

In diesem Anwendungsmuster stellen Sie eine datenbankanwendung in Azure Präsentations- und Ebenen auf demselben virtuellen Computer und den Zugriff auf eine Datenbank in Azure SQL Datenbank SQL Server. Sie können die Präsentationsebene mit herkömmlichen IIS-basierten Web Solutions implementieren. Oder einen kombinierten Präsentation und Geschäftsebene mithilfe von [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)implementieren.

Dieses Anwendungsmuster ist hilfreich, wenn

- Schon einen vorhandenen SQL-Datenbankserver in Azure konfiguriert und die Anwendung schnell testen möchten.

- Die Funktionen der Azure-Umgebung testen möchten.

- Möchten schnell Entwicklung bereitstellen und Testen der Umgebung für kurze Zeit.

- Die Business Logic und Datenzugriffskomponenten können eigenständig in eine Anwendung sein.

Die folgende Abbildung veranschaulicht einen lokalen Szenario und deren Cloudlösung aktiviert. In diesem Fall setzen Sie die Ebenen der Anwendung in einer virtuellen Maschine in Azure und Daten in Azure SQL-Datenbank.

![Gemischte Anwendungsmuster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Azure Web Apps mit einer kombinierten Web- und Anwendungsebene implementieren möchten, sollten Sie Dynamic Link Libraries (DLLs) im Rahmen einer Web-Anwendung die mittleren Ebene oder Anwendung Ebene beibehalten.

Außerdem überprüfen Sie die Empfehlungen [Vergleich Web Entwicklungsstrategien in Azure](#comparing-web-development-strategies-in-azure) Abschnitt am Ende dieses Artikels Programmiertechniken erfahren.

## <a name="n-tier-hybrid-application-pattern"></a>Mehrstufige hybride Anwendungsmuster

Mehrstufige hybride Anwendung Muster implementieren Sie die Anwendung in mehreren Stufen zwischen lokalen und Azure. Daher erstellen Sie eine flexible und wieder verwendbare Hybrid-System ändern oder eine bestimmte Ebene ohne die anderen Ebenen hinzufügen. Erweitern Sie Ihr Unternehmensnetzwerk zur Cloud verwenden Sie [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) Service.

Diese hybride Anwendungsmuster ist nützlich, wenn

- Anwendung erstellen, die teilweise in der Cloud ausgeführt und teilweise betrieben werden soll.

- Einige oder alle Elemente einer vorhandenen lokalen Anwendung in die Cloud migrieren möchten.

- Anwendung von lokalen virtualisierten Plattformen in Azure verschieben möchten.

- Möchten Sie eine infrastrukturumgebung besitzen, die oben und unten bei Bedarf skalieren können.

- Möchten schnell Entwicklung bereitstellen und Testen der Umgebung für kurze Zeit.

- Kostengünstige zu Backups für Unternehmen ergeben sollen.

Die folgende Abbildung veranschaulicht eine mehrstufige hybride Anwendungsmuster, die lokalen und Azure umfasst. Wie in der Abbildung gezeigt, enthält lokale Infrastruktur Domänencontroller zur Authentifizierung und Autorisierung unterstützt [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) . Beachten Sie, dass das Szenario Teile der Datenebene in einem lokalen Rechenzentrum Wohnort zeigt während Teile der Datenebene in Azure Leben. Je nach Anwendung können Sie mehrere Hybrid-Szenarios implementieren. Sie können z. B. die Präsentationsebene und Geschäftsebene in einer lokalen Umgebung jedoch der Datenebene in Azure halten.

![N-Tier-Anwendungsmuster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

In Azure können Active Directory als eigenständige cloudverzeichnis für Ihre Organisation, oder Sie können auch vorhandene lokale Active Directory in [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)integrieren. Wie das Diagramm können Tier Geschäftskomponenten an mehrere Datenquellen wie [SQL Server in Azure](virtual-machines-windows-sql-server-iaas-overview.md) über einen privaten internen IP-Adresse, lokalen SQL Server über [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)oder [SQL-Datenbank](../sql-database/sql-database-technical-overview.md) mit.NET Framework Data Provider Technologies Zugriff. In diesem Diagramm ist Azure SQL-Datenbank einen optionalen Speicher Datendienst.

Mehrstufige hybride Anwendung Muster können Sie den folgenden Arbeitsablauf in der angegebenen Reihenfolge durchführen:

1. Identifizieren Sie Unternehmen ergeben, die cloud mit [Microsoft Assessment and Planning (MAP) Toolkit](http://microsoft.com/map)verschoben werden müssen. Das MAP Toolkit und Performance-Daten von Computern für die Virtualisierung erwägen sammelt und Leitfaden auf Kapazität und Bewertung planen.

1. Planen Sie die Ressourcen und die Konfiguration der Azure-Plattform Speicherkonten und virtuellen Maschinen.

1. Richten Sie die Netzwerkkonnektivität zwischen dem lokalen Netzwerk und [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Um die Verbindung zwischen Unternehmensnetzwerk lokalen und eines virtuellen Computers in Azure festzulegen, verwenden Sie eine der beiden folgenden Methoden:

    1. Verbindung zwischen lokalen und Azure über öffentliche Endpunkte auf einem virtuellen Computer in Azure. Diese Methode stellt eine einfache Installation und können Sie den virtuellen Computer mit SQL Server-Authentifizierung. Außerdem richten Sie Ihre Gruppe Netzwerksicherheitsregeln öffentlichen Datenverkehr der VM Weitere Informationen finden Sie in [externen Zugriff für die VM mit Azure-Portal](virtual-machines-windows-nsg-quickstart-portal.md).

    1. Herstellen einer Verbindungs zwischen lokalen und Azure Azure Virtual Private Network (VPN) Tunnel. Diese Methode ermöglicht es, Domänenrichtlinien einer virtuellen Maschine in Azure erweitern. Darüber hinaus können Sie Firewallregeln einrichten und Verwenden von Windows-Authentifizierung in Ihrem System. Derzeit unterstützt Azure sicheren VPN-Standort-zu-Standort- und Punkt-zu-Standort-VPN-Verbindungen:

        - Mit sicheren Standort-zu-Standort-Verbindung können Sie in Azure Netzwerkkonnektivität zwischen dem lokalen Netzwerk und virtuellen Netzwerk herstellen. Es wird empfohlen, für die Verbindung der lokalen Rechenzentrum für Azure.

        - Sichere Punkt-zu-Standort-Verbindung können Sie die Netzwerkkonnektivität zwischen dem virtuellen Netzwerk in Azure und einzelne Computer überall einrichten. Es wird hauptsächlich für Entwicklungs- und Testzwecke empfohlen.

    Informationen zum Herstellen einer Verbindung zu SQL Server in Azure anzeigen Sie [Connect auf eine SQL Server-VM auf Azure](virtual-machines-windows-classic-sql-connect.md)

1. Geplante Aufträge und lokalen in einen virtuellen Datenträger in Azure Daten Alarme einrichten. Weitere Informationen finden Sie unter [SQL Server sichern und Wiederherstellen mit Azure BLOB-Speicher](https://msdn.microsoft.com/library/jj919148.aspx) und [Backup und Wiederherstellung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

1. Je nach Anwendung können Sie eine der folgenden drei Szenarien implementieren:

    1. Sie hält Ihre Webserver, Anwendungsserver und ohne Kleinschreibung Daten in einem Datenbankserver in Azure die Daten lokal halten.

    1. Hält Ihr Webserver und Anwendungsserver lokalen während der Datenbankserver auf einem virtuellen Computer in Azure.

    1. Sie können Ihre Datenbankserver, Webserver und Anwendungsserver lokalen Datenbank-Replikaten in virtuellen Maschinen in Azure halten. Diese Einstellung ermöglicht dem lokalen Webserver oder reporting Anwendungszugriffs auf Datenbankreplikate in Azure. Daher erreichen Sie die Arbeitslast in einer lokalen Datenbank gesenkt. Wir empfehlen Implementierung dieses Szenarios für hohe Arbeitslasten und Entwicklung zu lesen. Informationen zum Erstellen von Datenbankreplikaten in Azure finden Sie AlwaysOn Availability Gruppen auf [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Vergleichen von Web Entwicklungsstrategien in Azure

Zum Implementieren und Bereitstellen einer mehrstufigen SQL Server-basierte Anwendung in Azure, können Sie eine der folgenden zwei Programmiermethoden verwenden:

- Einrichten einer herkömmlichen Webserver (IIS – Internetinformationsdienste) in Azure und Access-Datenbanken in SQL Server in Azure Virtual Machines.

- Implementierung und Bereitstellung von Cloud-Dienst in Azure. Stellen Sie sicher, diese Clouddienst Datenbanken in SQL Server in Azure virtuellen Maschinen zugreifen kann. Cloud-Dienst kann mehrere Web- und Workerrollen Rollen enthalten.

Die folgende Tabelle enthält einen Vergleich der traditionellen Webentwicklung mit Azure Cloud Services und Azure Web Apps in Bezug auf SQL Server in Azure Virtual Machines. Die Tabelle enthält Azure Web Apps, wie SQL Server in Azure VM als Datenquelle für Azure Web Apps über ihre öffentliche virtuelle IP-Adresse oder DNS-Namen verwendet.

||Herkömmliche Webentwicklung in Azure Virtual Machines|Cloud-Dienste in Azure|Web-Hosting mit Azure webapps|
|---|---|---|---|
|**Anwendungsmigration lokal**|Vorhandenen Applikationen als-ist.|Programme benötigen Web-und Workerrollen.|Vorhandenen Applikationen als-ist aber für eigenständige ASP.NET-Webanwendungen und Webdienste, die schnelle Skalierung erfordern.|
|**Entwicklung und Bereitstellung**|Visual Studio WebMatrix Visual Web Developer, WebDeploy, FTP, TFS, PowerShell IIS-Manager.|Visual Studio TFS-Azure SDK PowerShell. Cloud-Dienst hat zwei Unternehmen, können Sie Ihrem Servicepaket und Konfiguration bereitstellen: Staging und Produktion. Bereitstellung einen Cloud-Dienst in die Stagingumgebung Test vor Produktion fördern.|Visual Studio WebMatrix Visual Web Developer, FTP, GIT, BitBucket, CodePlex, Ablage, GitHub, Mercurial, TFS, Web bereitstellen, PowerShell.|
|**Verwaltung und Einrichtung**|Sie sind verantwortlich für administrative Aufgaben der Anwendung, Daten, Firewallregeln, virtuelles Netzwerk und Betriebssystem.|Sie sind verantwortlich für administrative Aufgaben der Anwendung, Daten, Firewallregeln und virtuelles Netzwerk.|Sie sind verantwortlich für administrative Aufgaben der Anwendung und Daten.|
|**Hohe Verfügbarkeit und Disaster Recovery (HADR)**|Wir empfehlen Sie virtuelle Computer in der gleichen Verfügbarkeit und im selben Cloud-Dienst. Halten Ihre virtuellen Computer in derselben ermöglicht Verfügbarkeit Azure Hochverfügbarkeits-Knoten in separaten Fehlerdomänen und Domänen zu aktualisieren. Ebenso halten Ihre virtuellen Computer im selben Cloud-Dienst kann Lastenausgleich und VMs können untereinander direkt über das lokale Netzwerk in Azure-Rechenzentrum.<br/><br/>Sie sind verantwortlich für eine hohe Verfügbarkeit und Disaster Recovery-Lösung für SQL Server in Azure Virtual Machines Ausfallzeiten zu implementieren. Unterstützte HADR Technologies finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Sie sind verantwortlich für das Sichern Ihrer eigenen Daten und Anwendung.<br/><br/>Azure kann die virtuellen Computer verschieben, schlägt der Hostcomputer im Rechenzentrum durch Hardwareprobleme. Darüber hinaus möglicherweise geplanter Ausfallzeiten Ihrer VM Hostcomputers für Sicherheit oder Software aktualisiert wird Updates. Daher wird empfohlen, mindestens zwei VMs in jede Anwendungsebene um die kontinuierliche Verfügbarkeit verwalten. Azure bietet keine SLA für eine einzelne virtuelle Maschine. Weitere Informationen finden Sie unter [Azure Resiliency technische Anleitung](../resiliency/resiliency-technical-guidance.md).|Azure verwaltet die Fehler aus der zugrunde liegenden Hardware- oder Software. Wir empfehlen, mehrere Instanzen einer Rolle Web oder Arbeitnehmer, die hohe Verfügbarkeit der Anwendung implementieren. Informationen finden Sie in der [Cloud-Dienste, virtuelle Computer Virtual Network Service Level Agreement](http://www.microsoft.com/download/details.aspx?id=38427) und [Disaster Recovery und hohe Verfügbarkeit für Azure applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Sie sind verantwortlich für das Sichern Ihrer eigenen Daten und Anwendung.<br/><br/>Datenbanken in einer SQL Server-Datenbank in eine Azure-VM sind Sie verantwortlich für die Implementierung einer hohen Verfügbarkeit und Disaster Recovery-Lösung, um Ausfallzeiten zu vermeiden. Unterstützte HDAR Technologies finden Sie unter hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines.<br/><br/>**SQL Server-Database Mirroring**: Azure Cloud-Dienste (Web-Worker-Rollen). SQL Server VMs und ein Cloud-Dienstprojekt werden im gleichen virtuellen Azure-Netzwerk. SQL Server VM nicht im gleichen virtuellen Netzwerk befindet, müssen Sie einen SQL Server-Alias um Kommunikation mit der Instanz von SQL Server weiterleiten erstellen. Der Aliasname muss außerdem den SQL Server-Namen übereinstimmen.|Hoher Verfügbarkeit ist von Azure Worker-Rollen und Azure BLOB-Speicher Azure SQL-Datenbank geerbt. Azure-Speicher verwaltet z. B. drei Replikate Blob, Tabelle und Warteschlangendaten. Azure SQL-Datenbank hält jeweils drei Kopien der Daten – ein primäres Replikat und zwei sekundäre Replikate. Weitere Informationen finden Sie unter [Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/) und [Azure SQL-Datenbank](../sql-database/sql-database-technical-overview.md).<br/><br/>Wenn Sie SQL Server für Azure Web Apps in Azure VM als Datenquelle verwenden, denken Sie daran, dass Azure Web Apps Azure Virtual Network nicht unterstützt. In anderen Worten müssen alle Verbindungen von Azure Web Apps mit SQL Server VMs in Azure öffentliche Endpunkte von virtuellen Maschinen durchlaufen. Dadurch könnten einige Nachteile für hohe Verfügbarkeit und Disaster Recovery-Szenarien. Beispielsweise wäre die Client-Anwendung in Azure Web Apps mit SQL Server VM-Database mirroring nicht auf dem neuen Primärserver verbinden Bedarf später Azure Virtual Network zwischen SQL Server Host VMs in Azure einrichten. Daher ist Azure Web Apps mit **SQL Server Database Mirroring** derzeit nicht unterstützt.<br/><br/>**SQL Server AlwaysOn Availability Gruppen**: Sie SQL Server VMs in Azure Azure Web Apps mit AlwaysOn Availability Gruppen einrichten können. Aber Sie AlwaysOn Availability Gruppe Listener, um die Kommunikation über öffentliche Lastenausgleich Endpunkte primäres Replikat weiterleiten müssen.|
|**Cross-lokale Konnektivität**|Azure Virtual Network können lokale Verbindung.|Azure Virtual Network können lokale Verbindung.|Azure Virtual Network wird unterstützt. Weitere Informationen finden Sie unter [Web Apps virtuellen Netzwerkintegration](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/).|
|**Skalierung**|Skalierung ist erhöhen die Größe der virtuellen Computer oder das Hinzufügen weiterer Datenträger verfügbar. Weitere Informationen zu virtuellen Größen finden Sie [Größen für Azure Virtual Machine](virtual-machines-windows-sizes.md).<br/><br/>**Für Datenbankserver**: Scale-Out-Datenbank partitionieren Techniken und SQL Server AlwaysOn Availability Gruppen steht.<br/><br/>Verwenden Sie für hohe Arbeitslasten lesen [AlwaysOn Availability Gruppen](https://msdn.microsoft.com/library/hh510230.aspx) mehrere sekundäre Knoten als auch SQL Server-Replikation.<br/><br/>Für hohe Schreib-Arbeitslasten können Sie horizontale Partitionierung Daten auf mehreren physischen Servern Scale-Out-Anwendung zu implementieren.<br/><br/>Darüber hinaus können Sie ein dezentrales Skalieren mit [SQL Server mit Data Dependent Routing](https://technet.microsoft.com/library/cc966448.aspx)implementieren. Mit Daten von Routing (DDR), müssen Sie zum Implementieren der Partitionierungsmethode in der Clientanwendung in der Regel in der Geschäftsschicht Tier Datenbankanfragen an mehrere SQL Server-Knoten weitergeleitet. Die Geschäftsebene enthält Mappings wie die Daten partitioniert und Knoten Daten enthält.<br/><br/>Sie können Clientanwendungen skalieren, die virtuellen Maschinen ausgeführt werden. Weitere Informationen finden Sie unter [Skalieren eine Anwendung](../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Hinweis**: die **Automatische Skalierung** in Azure Funktion automatisch vergrößern oder verkleinern die virtuellen Computer, die von der Anwendung verwendet werden. Dieses Feature wird sichergestellt, dass durch der Endbenutzer negativ Spitzenzeiten wird und VMs heruntergefahren werden, wenn der Bedarf gering ist. Es wird empfohlen, die Option skalieren nicht enthält SQL Server VMs für den Clouddienst festlegen. Der Grund ist, dass das skalieren können Azure auf einem virtuellen Computer aktivieren, wird die CPU-Nutzung auf diesem virtuellen Computer über einige Schwellenwert und einen virtuellen Computer ausschalten, geht die CPU-Auslastung niedriger als. Skalieren-Funktion ist nützlich für statusfreie Anwendung z. B. Webserver, wobei jede VM Arbeitslast ohne Verweise auf vorherigen Zustand verwalten können. Skalierungsgröße Feature ist jedoch nicht für statusbehaftete Anwendung wie SQL Server, nur einmal in die Datenbank schreiben kann.|Skalierung ist in mehrere Web- und Workerrollen Rollen verfügbar. Weitere Informationen zu virtuellen Größen für Webrollen und Worker-Rollen finden Sie unter [Größen für Cloud-Dienste konfigurieren](../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Bei Verwendung von **Cloud-Diensten**können Sie mehrere Rollen Verarbeitung verteilen und auch flexible Skalierung Ihrer Anwendung definieren. Cloud-Dienst enthält Webrollen oder Worker-Rollen mit jeweils eigenen Anwendungsdateien und Konfiguration. Sie skalieren einen Cloud-Dienst durch Erhöhen der Anzahl der Rolleninstanzen (virtuelle Maschinen) für eine Rolle bereitgestellt und Herunterskalieren Cloud-Dienst durch Verringern der Anzahl der Instanzen. Ausführliche Informationen finden Sie unter [Azure Ausführungsmodelle](../cloud-services/cloud-services-choose-me.md).<br/><br/>Dezentrales Skalieren wird über Azure Hochverfügbarkeitsfunktionen Unterstützung durch [Cloud-Dienste, virtuelle Computer Virtual Network Service Level Agreement](http://www.microsoft.com/download/details.aspx?id=38427) und Lastenausgleich.<br/><br/>Eine Anwendung mit mehreren Ebenen wird empfohlen, Web-Worker Rollen Anwendung mit VMs über Azure Virtual Network Server-Datenbank verbinden. Darüber hinaus Azure Lastenausgleich für VMs im gleichen Clouddienst Benutzeranfragen auf diese verteilt. Virtuelle Computer verbunden, auf diese Weise können direkt über das lokale Netzwerk in Azure-Rechenzentrum miteinander.<br/><br/>**Skalieren** einrichten auf klassischen Azure-Portal als auch der Zeitplan. Weitere Informationen finden Sie unter [Skalieren eine Anwendung](../cloud-services/cloud-services-how-to-scale.md).|**Oben und unten skalieren**: Sie können erhöhen/verringern die Größe der Instanz (VM) reserviert für Ihre Website.<br/><br/>Skalieren: reservierte Instanzen (VMs) können für Ihre Website hinzufügen.<br/><br/>**Skalieren** einrichten auf das Portal als auch der Zeitplan. Weitere Informationen finden Sie unter [Skalierung Web Apps](../app-service-web/web-sites-scale.md).|

Weitere Informationen über das Auswählen zwischen diesen Programmiermethoden finden Sie [Azure Web Apps, Cloud-Dienste und VMs: bei](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ausführen von SQL Server in Azure virtuellen Maschinen finden Sie unter [SQL Server auf Azure virtuelle Computer](virtual-machines-windows-sql-server-iaas-overview.md).
