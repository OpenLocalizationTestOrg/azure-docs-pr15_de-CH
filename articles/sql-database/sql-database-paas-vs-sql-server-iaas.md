<properties
    pageTitle="SQL (PaaS) Datenbank im Vergleich zu SQL Server in der Cloud auf VMs (IaaS) | Microsoft Azure"
    description="Erfahren Sie, welche Cloud SQL Server Option die Anwendung passt: Azure SQL (PaaS) Datenbank oder SQL Server in der Cloud auf Azure Virtual Machines."
    services="sql-database, virtual-machines"
    keywords="SQL Server in der Cloud PaaS-Datenbank SQL Server Cloud cloud SQL Server DBaaS"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Wählen Sie eine Wolke SQL Server Option: Azure SQL (PaaS) Datenbank oder SQL Server auf Azure VMs (IaaS)

Azure hat zwei Optionen zum Hosten von Arbeitslasten in Microsoft Azure SQL Server:

* [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/): eine SQL-Datenbank in Cloud auch eine Plattform als ein Service (PaaS) oder Datenbank als Dienst (DBaaS), die für die Anwendungsentwicklung von Software als Service (SaaS) optimiert ist. Es bietet Kompatibilität mit den meisten SQL Server-Funktionen. Weitere PaaS sehen Sie [PaaS](https://azure.microsoft.com/overview/what-is-paas/).
* [SQL Server auf Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server installiert und in der Cloud auf Windows Server virtuellen Computern (VMs) Azure auch eine Infrastruktur als Service (IaaS).
SQL Server auf Azure virtuellen Computern ist optimiert für Migrieren von vorhandenen SQL Server-Anwendung. Alle Versionen und Editionen von SQL Server zur Verfügung stehen. Es bietet 100 % Kompatibilität mit SQL Server können Sie so viele Datenbanken erforderlich und ausgeführten datenbankübergreifende Transaktionen hosten. Es bietet vollständige Kontrolle für SQL Server und Windows.

Erfahren Sie, wie jede Option in der Microsoft-Datenplattform passt und die richtige Option auf Ihr Unternehmen mit Hilfe. Ob Kosten oder minimale Verwaltung vor allem priorisieren können dieser Artikel entscheiden, welcher Ansatz bietet Business-Anforderungen, die Ihnen am wichtigsten sind.


## <a name="microsofts-data-platform"></a>Microsoft-Datenplattform

Zunächst verstehen Diskussion von Azure im Vergleich zu lokalen SQL Server-Datenbanken gehört, dass Sie es verwenden können. Microsoft Plattform nutzt SQL Server-Technologie und lokalen physischen Computer, private Cloud-Umgebung, von Drittanbietern gehostete private Cloud-Umgebung und öffentliche Cloud verfügbar gemacht. SQL Server auf Azure virtual Mchines können aus lokalen und Cloud-gehosteten Bereitstellung dieselben Serverprodukte, Entwicklungstools und Fachwissen über diese Umgebung mit einzigartigen und vielfältigen geschäftlichen Bedürfnissen.

   ![Cloud-Optionen für SQL Server: SQL Server IaaS oder SaaS SQL-Datenbank in der Cloud.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Wie in der Abbildung kann jedes Angebot der Verwaltungsebene über Infrastruktur (x-Achse) und der Grad der Kosteneffizienz erreicht datenbankkonsolidierung und Automatisierung (y-Achse) gekennzeichnet.

Beim Entwerfen einer Anwendung stehen vier grundlegende Optionen für SQL Server Teil der Anwendung hosten:

- SQL Server auf physischen Computern nicht virtualisierten
- SQL Server in lokalen virtuellen Maschinen (private Cloud)
- SQL Server in Azure Virtual Machine (öffentliche Cloud von Microsoft)
- Azure SQL-Datenbank (öffentliche Cloud von Microsoft)

In den folgenden Abschnitten erfahren Sie in der öffentlichen Cloud von Microsoft SQL Server: Azure SQL-Datenbank und SQL Server auf Azure VMs. Darüber hinaus untersuchen Sie common Business Motivation zum bestimmen, welche Option am besten für Ihre Anwendung.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Genauere Betrachtung Azure SQL-Datenbank und SQL Server auf Azure-VMs

**Azure SQL-Datenbank** ist eine relationale Datenbank-als-a-Service (DBaaS) in den Kategorien von *Software-as-a-Service (SaaS)* und *Platform-as-a-Service (PaaS)*fällt Azure-Cloud gehostet. [SQL-Datenbank](sql-database-technical-overview.md) basiert auf standardisierte Hardware und Software, die im Besitz, gehostet und von Microsoft verwaltet wird. SQL-Datenbank können Sie direkt auf den Dienst mit integrierten Funktionen entwickeln. Wenn Sie Optionen, für mehr Leistung ohne Unterbrechung skaliert Bedarfsbasis SQL-Datenbank verwenden.

**SQL Server auf Azure virtuelle Maschinen (VMs)** fällt in die Kategorie Industrie *Infrastructure-as-a-Service (IaaS)* und ermöglicht SQL Server innerhalb eines virtuellen Computers in der Cloud. Ähnlich wie SQL-Datenbank sie standardisierte Hardware basiert auf, die Besitzer, gehostet und von Microsoft verwaltet. Wenn SQL Server auf einem virtuellen Computer verwenden, können Sie entweder-wie Sie-auf Start für eine SQL Server-Lizenz bereits in einer SQL Server-Abbild enthalten oder einfach eine vorhandene Lizenz verwenden. Sie können auch Skalieren-ab und die VM Bedarf anhalten/fortsetzen.

Im Allgemeinen werden diese beiden SQL-Optionen für unterschiedliche Zwecke optimiert:

- **SQL Datenbank** optimiert das Minimum für die Bereitstellung und Verwaltung von vielen Datenbanken insgesamt senken. Reduzierung der laufenden Verwaltungskosten da nicht virtuelle Computer, Betriebssystem und Datenbank-Software verwalten. Sie müssen keinen Upgrades, Verfügbarkeit oder [Backups](sql-database-automated-backups.md)zu verwalten. Im Allgemeinen Azure SQL-Datenbank kann erheblich mehr Datenbanken von einem einzelnen IT oder Entwicklungsressource.
- **SQL Server unter Azure VMs ausgeführt** ist für vorhandene Anwendung in Azure migrieren oder erweitern vorhandene lokale Applikationen in die Cloud hybridbereitstellungen optimiert. Darüber hinaus können Sie SQL Server auf einem virtuellen Computer zum Entwickeln und Testen von herkömmlichen SQL Server-Applikationen. Mit SQL Server auf Azure VMs haben Sie volle Administratorrechte über einen dedizierten SQL Server-Instanz und eine cloudbasierte VM. Wird perfekt ein Unternehmen bereits IT-Ressourcen zu virtuellen Computer verfügt. Diese Funktionen können Sie ein maßgeschneidertes System der Anwendung Leistung und Verfügbarkeit.

In der folgenden Tabelle sind die wesentlichen Merkmale der SQL-Datenbank und SQL Server auf Azure VMs zusammengefasst:

|       | SQL-Datenbank | SQL Server in Azure Virtual Machine|
| -------------- | ------------ | ---------------------- |
| **Geeignet für:** | Neue Cloud entwickelt Programme, die Zeitdruck in Entwicklung und Marketing. |Vorhandene Anwendung, die schnelle Migration zur Cloud mit minimaler erfordern. Schnelle Entwicklung und Testing Szenarien nicht lokale nicht produktiven SQL Server Hardware kaufen möchten. |
|| Teams, die integrierte hohe Verfügbarkeit, Wiederherstellung und Aktualisierung der Datenbank. |Teams für die konfigurieren und Verwalten von hoher Verfügbarkeit, Disaster Recovery und Patches für SQL Server. Einige bereitgestellt, automatisierte Funktionen dadurch erheblich vereinfacht. |
||Teams, die nicht das zugrunde liegende Betriebssystem und Konfiguration verwalten möchten.| Wenn Sie eine benutzerdefinierte Umgebung mit vollen Administratorrechten benötigen.|
||Bis zu 1 TB bzw. größere Datenbanken, die mit einem Muster skalieren [horizontal oder vertikal partitioniert](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) werden können.|SQL Server-Instanzen mit bis zu 64 TB Speicher. Die Instanz kann beliebig viele Datenbanken unterstützen. |
||[Clientanwendungen erstellen Software-als-a-Service (SaaS)](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Migration und Enterprise und Hybrid erstellen.|
|||||
|**Ressourcen:**|Sie IT-Ressourcen für die Konfiguration und Verwaltung der zugrunde liegenden Infrastruktur einsetzen möchten, aber auf der Anwendungsschicht konzentrieren möchten.|Sie haben einige IT-Ressourcen für Konfiguration und Management. Einige bereitgestellt, automatisierte Funktionen dadurch erheblich vereinfacht.|
|**Gesamtkosten:**|Hardware Kosten und Verwaltungskosten verringert.|Hardwarekosten entfällt.|
|**Business Continuity:**|Neben integrierte Infrastruktur Fehlertoleranzfunktionen bietet Azure SQL-Datenbank, wie [automatisierte Backups](sql-database-automated-backups.md), [Point-In-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore) [Geo-Wiederherstellung](sql-database-recovery-using-backups.md#geo-restore)und [Aktive Geo-Replikation](sql-database-geo-replication-overview.md) zu Business Continuity. Weitere Informationen finden Sie unter [Übersicht über SQL Datenbank Business Continuity](sql-database-business-continuity.md).|SQL Server auf Azure VMs können Sie eine hohe Verfügbarkeit und Disaster Recovery-Lösung, für die Bedürfnisse Ihrer Datenbank einrichten. Daher haben Sie eine System ist optimiert für die Anwendung. Sie können testen und selbst bei Bedarf Failover ausgeführt. Weitere Informationen finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server auf Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Hybridcloud:**|Die lokale Anwendung kann Daten in Azure SQL-Datenbank zugreifen.|Mit SQL Server auf Azure VMs haben Sie Applikationen, die teilweise in der Cloud ausgeführt und teilweise lokal. Beispielsweise können Sie Ihr lokales Netzwerk und Active Directory-Domäne in die Cloud über [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)erweitern. Darüber hinaus können Sie lokale Dateien in Azure-Speicher mit [SQL Server-Datendateien in Azure](http://msdn.microsoft.com/library/dn385720.aspx)speichern. Weitere Informationen finden Sie unter [Einführung in SQL Server 2014 Hybridcloud](http://msdn.microsoft.com/library/dn606154.aspx).|
||Unterstützt die [Transaktionsreplikation SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) als Abonnent Daten replizieren.|[Transaktionsreplikation SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) [AlwaysOn Availability Gruppen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md), Integration Services und Protokollversand zum Replizieren von Daten unterstützt. Zudem werden herkömmliche Backups von SQL Server unterstützt|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Geschäftliche Beweggründe Azure SQL-Datenbank oder SQL Server auf Azure-VMs

### <a name="cost"></a>Kosten

Ob Sie einen Start sind für Bargeld oder ein Team in einem etablierten Unternehmen Budgetrestriktionen befestigt ist begrenzten Mittel häufig hauptsächlich bei Datenbanken hosten. In diesem Abschnitt lernen Sie die Abrechnung und Lizenzierung Grundlagen in Azure in Bezug auf diese beiden Optionen relationale Datenbank: SQL-Datenbank und SQL Server auf Azure VMs. Darüber hinaus lernen die gesamten Kosten berechnen.

#### <a name="billing-and-licensing-basics"></a>Gebühren und Lizenzierung Grundlagen

**SQL-Datenbank** wird als Dienst mit einer Lizenz nicht an Kunden verkauft.  [SQL Server auf Azure VMs](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) wird enthalten Lizenz, die Sie Zahlen pro Minute. Wenn Sie eine vorhandene Lizenz haben, können Sie auch verwenden.  

**SQL-Datenbank** ist derzeit Dienstebenen, die stündlich Festpreis basierend auf Dienstebene berechnet und Leistung auf gewählte verfügbar. Darüber hinaus werden ausgehende Internetverkehr regulären [Datenübertragungsraten](https://azure.microsoft.com/pricing/details/data-transfers/)berechnet. Dienstebenen Basic, Standard und Premium sollen vorhersagbare Leistung mehrere Leistung der Anwendung Peak entsprechen. Sie können zwischen Dienstebenen und Leistung der Anwendung unterschiedliche Durchsatz Bedürfnisse ändern. Hat Ihre Datenbank hohen Transaktionsvolumen und zu viele gleichzeitige Benutzer unterstützen, empfehlen wir die Premium-Service-Tier. Aktuelle Informationen zu den aktuellen unterstützten Dienstebenen finden Sie unter [Service-Tiers hinweg Azure SQL-Datenbank](sql-database-service-tiers.md). Sie können auch [elastische datenbankpools](sql-database-elastic-pool.md) gemeinsam Leistung Ressourcen Datenbankinstanzen erstellen.

**SQL-Datenbank**ist die Datenbanksoftware automatisch konfiguriert gepatcht und Upgrade von Microsoft, die Ihre Verwaltungskosten verringert. Darüber hinaus unterstützen die [integrierten](sql-database-automated-backups.md) Funktionen deutlich zu, vor allem, wenn Sie eine große Anzahl von Datenbanken.

Mit **SQL Server auf Azure VMs**können beliebige Plattform bereitgestellten SQL Server Bilder (beinhaltet eine Lizenz) oder bringen Ihre SQL Server-Lizenz. Unterstützten SQL Server-Versionen (2008 R2, 2012, 2014, 2016) und Ausgaben (Developer Express Web, Standard, Enterprise). Darüber hinaus stehen in der-eigenen-Lizenz-Versionen (BYOL) der Bilder. Wenn Bilder mit dem Azure bereitgestellt werden, hängt von die Betriebskosten VM-Größe und der Edition von SQL Server, die Sie auswählen. Unabhängig von VM-Größe oder SQL Server-Edition bezahlen Sie pro Minute Lizenzierung von SQL Server und Windows Server zusammen mit den Azure-Speicher für die VM-Datenträger. Die Abrechnung pro Minute-Option ermöglicht SQL Server für die Verwendung als ohne Kauf SQL Server-Lizenzen benötigen. Wenn Sie Azure SQL Server Lizenz bringen, werden für Windows Server- und Speicherkosten nur erhoben werden. Finden Sie weitere Informationen zur Lizenzierung bringen eigene [Lizenzmobilität durch Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Die gesamten Kosten berechnen

Beim Starten mit einer Cloudplattform enthält die Kosten für die Anwendung Entwicklung und Verwaltungskosten, die öffentliche Cloud-Plattform Servicekosten.

Detaillierte Berechnung der für die Anwendung, die unter VMs Azure SQL-Datenbank und SQL Server lautet

**Bei Verwendung von Azure SQL-Datenbank:**

*Gesamtkosten der Anwendung = sehr minimierten Verwaltungskosten softwareentwicklungskosten + Servicekosten SQL-Datenbank*

**Wenn Sie SQL Server auf Azure VMs verwenden:**

*Gesamtkosten der Anwendung = sehr minimierten Software Entwicklungskosten Verwaltungskosten SQL Server und Windows Server Lizenzkosten + Azure Speicherkosten*

Weitere Informationen zu Preisen finden Sie in folgenden Ressourcen:

- [SQL Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/)
- [Virtual Machine Preise](https://azure.microsoft.com/pricing/details/virtual-machines/) für [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) und [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] Es ist eine kleine Teilmenge der Features in SQL Server nicht zutreffend oder SQL-Datenbank nicht verfügbar. Weitere Informationen finden Sie unter [SQL Datenbank Allgemein und Richtlinien](sql-database-general-limitations.md) und [SQL Datenbank Transact-SQL-Informationen](sql-database-transact-sql-information.md) . Wenn Sie eine vorhandene SQL Server-Lösung in die Cloud verschieben, finden Sie unter [Migrieren von Azure SQL-Datenbank eine SQL Server-Datenbank](sql-database-cloud-migrate.md). Beim Migrieren einer vorhandenen lokalen SQL Server-Anwendung mit SQL-Datenbank sollten Sie die Aktualisierung der Anwendung Funktionen nutzen, die cloud-Services bieten. Beispielsweise sollten Sie mithilfe von [Azure App Webdienst](https://azure.microsoft.com/services/app-service/web/) oder [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) Hosten der Anwendungsschicht Kostenvorteile erhöhen.

### <a name="administration"></a>Verwaltung

Für viele Unternehmen ist die Entscheidung für den Übergang zu einem Clouddienst zur Entlastung der Komplexität der Verwaltung dieser Kosten ist. Microsoft verwaltet **SQL-Datenbank**die zugrunde liegende Hardware. Microsoft automatisch repliziert alle Daten zur Gewährleistung hoher Verfügbarkeit, konfiguriert und aktualisiert die Datenbanksoftware verwaltet Netzwerklastenausgleich und transparentes Failover ist, wenn ein Server ausfällt. Sie können weiterhin die Datenbank verwalten, aber nicht mehr Datenbankmodul, Server-Betriebssystem oder Hardware verwalten müssen.  Elemente können Sie verwalten zählen Datenbanken und Benutzernamen, Index und Abfrage optimieren, Überwachung und Sicherheit.

Mit **SQL Server auf Azure VMs**haben Sie vollständige Kontrolle über das Betriebssystem und SQL Server-Konfiguration. Mit einer VM liegt es an Ihnen, wenn das Betriebssystem und die Datenbank aktualisiert und wann zusätzlichen Software wie Anti-Virus installiert. Einige automatisierten Funktionen dienen patching, backup und hohe Verfügbarkeit erheblich vereinfachen. Darüber hinaus können Sie die Größe der VM, die Anzahl der Laufwerke und ihren Speicherkonfigurationen steuern. Azure können Sie die Größe eines virtuellen Computers nach Bedarf ändern. Informationen finden Sie unter [virtuelle Computer und Azure Cloud Service Größe](../virtual-machines/virtual-machines-linux-sizes.md). 

### <a name="service-level-agreement-sla"></a>Vereinbarung zum Servicelevel (SLA)

Für viele IT-Abteilung ist Zeit Pflichten des Service Level Agreement (SLA) oberste Priorität. In diesem Abschnitt betrachten wir was SLA für jede Datenbank Hostingoption gilt.

**SQL Datenbank** Basic, Standard und Premium Dienstebenen bietet Microsoft eine Verfügbarkeit von 99,99 % SLA. Aktuelle Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/sql-database/). Neueste Informationen zur SQL-Datenbank Dienstebenen und unterstützten Business Continuity-Plänen finden Sie unter [Dienstebenen](sql-database-service-tiers.md).

**SQL Server unter Azure VMs ausgeführt**bietet Microsoft eine Verfügbarkeits-SLA von 99,95 %, die nur der virtuelle Computer umfasst. SLA ist nicht auf dem virtuellen Computer ausgeführten Prozesse (z. B. SQL Server) und erfordert, dass mindestens zwei VM-Instanzen in einem Verfügbarkeit hosten. Aktuelle Informationen finden Sie in der [VM-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Für die Datenbank mit hoher Verfügbarkeit (HA) in VMs sollten Sie eine der Optionen unterstützt hohe Verfügbarkeit in SQL Server wie [AlwaysOn Availability](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx)konfigurieren. Unterstützt high Availability-Option bietet eine zusätzliche SLA, aber ermöglicht > datenbankverfügbarkeit von 99,99 %.

### <a name="market"></a>Zeit bis zur Marktreife

**SQL-Datenbank** wird die richtige Lösung für Applikationen Cloud entwickelt Entwicklerproduktivität und schnelle Time-to-Market entscheidend sind. Programmgesteuerte DBA-ähnliche Funktionalität ist ideal für Cloud-Architekten und Entwickler, wie die Notwendigkeit senkt für das zugrunde liegende Betriebssystem und Datenbank. Beispielsweise können Sie die [REST-API](http://msdn.microsoft.com/library/azure/dn505719.aspx) und [PowerShell-Cmdlets](http://msdn.microsoft.com/library/azure/dn546726.aspx) zum Automatisieren und verwalten Verwaltungsvorgänge für Tausende von Datenbanken. Funktionen wie [Elastische Datenbankpools](sql-database-elastic-pool.md) können konzentrieren auf Anwendungsebene und Ihre Lösung auf dem Markt schneller.

**SQL Server unter Azure VMs ausgeführt** ist perfekt vorhandenen oder neuen Anwendung benötigen große Datenbanken, zusammenhängende Datenbanken und Zugriff auf alle Funktionen in SQL Server oder Windows. Es ist auch geeignet, wenn Sie vorhandene migrieren möchten lokale Programme und Datenbanken in Azure als-ist. Da Sie keine Präsentation, Anwendung und Datenebenen ändern müssen, sparen Sie Zeit und des Budgets einer vorhandene Projektmappe. Können Sie stattdessen alle Projektmappen migrieren, Azure und einige Performance-Optimierungen, die möglicherweise von der Azure-Plattform erforderlich. Weitere Informationen finden Sie unter [Best Practices zur Performance für SQL Server auf Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel untersucht SQL-Datenbank und SQL Server auf Azure virtuelle Maschinen (VMs) und erläutert common Business Motivation, die Ihre Entscheidung beeinflussen können. Folgendes ist eine Zusammenfassung der Vorschläge berücksichtigen Sie:

Wählen Sie **SQL Azure-Datenbank** :

- Sie neue Cloud-basierte Anwendung zu Kostenersparnis und Performance-Optimierung, die cloud-Dienste bereitstellen. Dieser Ansatz bietet die Vorteile einer vollständig verwaltete Cloud-Dienst, kann niedrigere anfängliche Time-to-Market und bieten die langfristigen Kosten optimieren.

- Möchten Sie Microsoft common Management Operationen auf Datenbanken und stärker Verfügbarkeits-SLAs für Datenbanken erforderlich.



Wählen Sie **SQL Server auf Azure VMs** :

- Müssen vorhandene lokale Applikationen, die Sie in die Cloud erweitern oder migrieren möchten oder wenn Sie Anwendung größer als 1 TB. Dieser Ansatz bietet den Vorteil der 100 %-SQL-Kompatibilität, große Datenbankkapazität Vollzugriff auf SQL Server und Windows und secure tunneling lokal. Dieser Ansatz minimiert die Kosten für Entwicklung und Ändern der vorhandenen Anwendung.

- Sie können müssen vorhandene IT-Ressourcen und letztendlich patchen, Backups und Datenbank mit hoher Verfügbarkeit. Beachten Sie, dass einige automatisierten Funktionen dieser Vorgänge erheblich vereinfacht. 


## <a name="next-steps"></a>Nächste Schritte
- Siehe [SQL Lernprogramm: erstellen eine SQL-Datenbank in Minuten Azure-Portal](sql-database-get-started.md) Schritte mit SQL-Datenbank.
- Finden Sie unter [SQL-Datenbank Preis] (https://azure.microsoft.com/pricing/details/sql-database/).
- [Bereitstellen einer SQL Server-VM in Azure](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) Schritte mit SQL Server auf Azure VMs anzeigen
- Siehe [SQL Server auf einem virtuellen Computer Azure: Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
