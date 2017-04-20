<properties
    pageTitle="Azure Site Recovery schützen Sie welche Arbeitslasten?"
    description="Azure Site Recovery schützt Ihre Arbeitslasten und Applikationen durch Replikation, Failover und Recovery von lokalen virtuellen Computern und Servern Azure oder einem lokalen sekundären koordinieren"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Azure Site Recovery schützen Sie welche Arbeitslasten?


Dieser Artikel beschreibt Arbeitslasten und Programme, die mit dem Azure Site Recovery-Dienst repliziert werden können.

Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Übersicht

Organisationen brauchen eine Business Continuity und Disaster Recovery (BCDR) Strategie Arbeitslasten und Daten sicher und verfügbar bei geplanten und ungeplanten Ausfällen sowie auf reguläre so bald wie möglich wiederherstellen.

Site Recovery ist ein Azure die BCDR Strategie beteiligt. Mit Site Recovery können Sie anwendungsspezifische Replikation zur Cloud oder an einem sekundären Standort bereitstellen. Führen Sie Ihre apps sind Windows oder Linux-basierten, auf physische Server, VMware und Hyper-V Sie Site Recovery Replikation orchestrieren können, Disaster Recovery-Tests, und zur Failover und Failback.


Site Recovery integriert für Microsoft-Programme, einschließlich SharePoint, Exchange Dynamics, SQL Server und Active Directory. Microsoft arbeitet außerdem eng mit führenden Anbietern wie Oracle, SAP, IBM und Red Hat. Sie können die Replikation zu app app.

## <a name="why-use-site-recovery-for-application-replication"></a>Wozu Site Recovery Anwendung Replikation?

Site Recovery zur Anwendungsebene Schutz und Wiederherstellung wie folgt:

- App-unabhängig, Replikation für alle Arbeitslasten auf einem unterstützten Computer bereitstellen.
- Bei synchronen Replikation mit RPOs so niedrig wie 30 Sekunden, um den Bedürfnissen der wichtigsten Business-apps.
- App-konsistente Snapshots für einzelnen oder mehreren Ebenen.
- Integration mit SQL Server AlwaysOn und mit anderen auf Replikationstechnologien, einschließlich Active Directory-Replikation SQL AlwaysOn Availability-Gruppen für Exchange-Datenbank (DAGs) und Oracle Data Guard.
- Flexible Recovery-Pläne, können Sie einen gesamte Anwendungsstapel mit einem einzigen Mausklick wiederhergestellt, und um externe Skripts und manuelle Aktionen im Plan enthalten.
- Erweitertes Netzwerk-Management in Site Recovery und Azure app Anforderungen, einschließlich der Möglichkeit, IP-Adressen reservieren vereinfachen konfigurieren Lastenausgleich und Integration mit Azure Traffic Manager für niedrigen RTO Netzwerk Switchover.
-  Ein rich-Bibliothek, die Produktion, anwendungsspezifische Skripts enthält, die heruntergeladen und Recovery-Pläne integriert werden können.



## <a name="workload-summary"></a>Arbeitslast-Zusammenfassung

Site Recovery kann jede Anwendung auf einem unterstützten Computer replizieren. Außerdem haben wir mit Produktteams zusätzliche anwendungsspezifische Tests durchführen können.

**Arbeitslast** | **Hyper-V VMs auf einem sekundären Standort replizieren** | **Replikation von Hyper-V VMs in Azure** | **Virtuelle VMware-Computer an einem sekundären Standort replizieren** | **Virtuelle VMware-Computer in Azure replizieren**
---|---|---|---|---
Active Directory, DNS | Y | Y | Y | Y
Webapps (IIS, SQL) | Y | Y | Y | Y
System Center Operations Manager | Y | Y | Y | Y
SharePoint | Y | Y | Y | Y
SAP<br/><br/>Replizieren SAP Website in Azure-cluster | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet)
Exchange (nicht so) | Y | Demnächst | Y | Y
Remote Desktop-VDI | Y | Y | Y | N/A
Linux (Betriebssystem und apps) | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet)
Dynamics AX | Y | Y | Y | Y
Dynamics CRM | Y | Demnächst | Y | Demnächst
Oracle | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet)
Windows-Dateiserver | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Replizieren von Active Directory und DNS

Ein Active Directory und DNS-Infrastruktur sind für die meisten Unternehmen apps. Während der Wiederherstellung müssen Sie schützen und Wiederherstellen dieser Infrastrukturkomponenten vor dem Wiederherstellen der Arbeitslasten und apps.

Site Recovery können einen vollständig automatisierte Disaster Recovery-Plan für Active Directory und DNS zu erstellen. Z. B. nicht über SharePoint- und SAP vom primären zum sekundären Standort lassen einen Wiederherstellungsplan, die zunächst nicht über Active Directory und eine zusätzliche anwendungsspezifische Wiederherstellungsplan Failover von anderen apps, die auf Active Directory.

[Weitere](site-recovery-active-directory.md) Informationen zum Schützen von Active Directory und DNS

## <a name="protect-sql-server"></a>Schützen von SQL Server

SQL Server bietet eine Grundlage Data Services für Datendienste für viele Business-apps in einem Rechenzentrum vor Ort.  Site Recovery kann SQL Server HA/DR-Technologien mehrstufige Enterprise apps schützen, die mit SQL Server verwendet werden. Site Recovery bietet:

- Eine einfache und kostengünstige Disaster Recovery-Lösung für SQL Server. Mehrere Versionen und Editionen von SQL Server eigenständiger Server und Cluster, Azure oder an einem sekundären Standort repliziert.  
- Integration mit SQL AlwaysOn Verfügbarkeit, Failover und Failback Azure Site Recovery Recovery-Pläne verwalten.
- End-to-End-Recovery-Pläne für alle Ebenen in einer Anwendung, einschließlich der SQL Server-Datenbanken.
- Skalieren von SQL Server für maximale lädt mit Site Recovery werden in IaaS Virtual Machine größere in Azure "Trennen".
- Der SQL Server-Wiederherstellung einfach testen. Sie können Test-Failover zum Analysieren von Daten und Ausführen von Compliance-Kontrollen ohne Beeinträchtigung Ihrer produktiven Umgebung ausführen.

[Erfahren Sie mehr](site-recovery-sql.md) zu SQL Server zu schützen.

##<a name="protect-sharepoint"></a>Schützen von SharePoint

Azure Site Recovery schützt SharePoint-Installationen wie folgt:

- Beseitigt die Notwendigkeit und Infrastrukturkosten für einen Stand-by-Betrieb für Disaster Recovery. Verwenden Sie Site Recovery eine gesamte Farm (Web-, Anwendung und Ebenen) in Azure oder an einem sekundären Standort repliziert.
- Vereinfacht die Bereitstellung und Verwaltung. Vom primären Standort bereitgestellten Updates automatisch repliziert und sind daher nach dem Failover und Wiederherstellung der Farm an einem sekundären Standort verfügbar. Senkt die Management-Komplexität und Kosten Stand-by-Betrieb auf dem neuesten Stand.
- Vereinfacht die SharePoint-Anwendungsentwicklung und Tests durch Erstellen einer Produktion kopieren bei Bedarf Replikat Umgebung zum Testen und Debuggen.
- Übergang zur Cloud vereinfacht SharePoint-Bereitstellungen zu Azure Migrieren mithilfe von Site Recovery.

[Weitere](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) Informationen zum Schützen von SharePoint


## <a name="protect-dynamics-ax"></a>Schützen von Dynamics AX

Azure Site Recovery schützt Ihre Dynamics AX ERP-Lösung durch:

- Replikation der gesamten Dynamics AX Umgebung (Web- und AOS Tiers, Datenbankebene SharePoint) in Azure oder an einem sekundären Standort orchestriert.
- Vereinfachung der Migration von Dynamics AX-Bereitstellung in der Cloud (Azure).
- Vereinfachung der Dynamics AX-Anwendungsentwicklung und Tests durch Erstellen einer produktionsähnlichen Kopie auf Anforderung, zum Testen und Debuggen.

[Weitere](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) Informationen zum Schützen von Dynamics AX

## <a name="protect-rds"></a>Schützen von RDS

Remote Desktop Services (RDS) können virtual desktop Infrastructure (VDI), sitzungsbasierten Desktops und Applikationen Benutzer überall arbeiten. Mit Azure Site Recovery können Sie:

- Replizieren Sie verwalteten oder nicht verwalteten Pool virtueller Desktops zu einem sekundären Standort und Remoteanwendungen Sessions zu einem sekundären Standort Azure
- Hier ist replizieren können:

**RDS** | **Hyper-V VMs auf einem sekundären Standort replizieren** | **Replikation von Hyper-V VMs in Azure** | **Virtuelle VMware-Computer an einem sekundären Standort replizieren** | **Virtuelle VMware-Computer in Azure replizieren** | **Replizieren von Servern an einem sekundären Standort** | **Physische Server in Azure replizieren**
---|---|---|---|---|---|---
**Pool virtueller Desktop (nicht verwaltet)** | Ja | Nein | Ja | Nein | Ja | Nein
**Pool virtueller Desktop (verwaltete und ohne UPD)** | Ja | Nein | Ja | Nein | Ja | Nein
**Remoteanwendungen und Desktop-Sitzungen (ohne UPD)** | Ja | Ja | Ja | Ja | Ja | Ja


[Weitere](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) Informationen zum Schützen von RDS.


## <a name="protect-exchange"></a>Schützen von Exchange

Site Recovery schützt Exchange, wie folgt:

- Für kleinen Exchange-Installationen wie einzelne oder Standalone-Server Site Recovery replizieren und ein Failover zu Azure oder an einem sekundären Standort.
- Exchange-DAGS für größere Installationen integriert Site Recovery.
- Exchange-DAGs sind die empfohlene Lösung für Exchange Disaster Recovery in einem Unternehmen.  Site Recovery Recovery-Pläne zählen DAGs, um so Failover über Standorte hinweg koordinieren.


[Weitere](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) Informationen zum Schützen von Exchange.

## <a name="protect-sap"></a>Schutz von SAP

Site Recovery der SAP-Bereitstellung wie folgt zu verwenden:

- Aktivieren des Schutzes der gesamten SAP-Bereitstellung von verschiedenen Ebenen in Azure oder an einem sekundären Standort repliziert.
- Vereinfachen Sie die Cloud Migration mithilfe von Site Recovery der SAP-Umgebung zu Azure migrieren.
- Vereinfachen die SAP-Entwicklung und Tests durch Erstellen einer produktionsähnlichen kopieren bei Bedarf zum Testen und Debuggen.

[Erfahren Sie mehr](http://aka.ms/asr-sap) über den Schutz von SAP.

## <a name="next-steps"></a>Nächste Schritte

[Bereiten Sie auf die Bereitstellung von Site Recovery vor](site-recovery-best-practices.md) 
