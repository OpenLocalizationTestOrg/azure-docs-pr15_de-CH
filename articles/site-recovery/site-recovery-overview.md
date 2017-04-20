<properties
    pageTitle="Was ist Website-Wiederherstellung? | Microsoft Azure"
    description="Übersicht über Azure Site Recovery Service und Bereitstellungsszenarien zusammengefasst."
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
    ms.date="10/13/2016"
    ms.author="raynew"/>

#  <a name="what-is-site-recovery"></a>Was ist Website-Wiederherstellung?

Willkommen bei Azure Site Recovery! Dieser Artikel bietet einen Überblick der Site Recovery Service und wie in Ihrem Unternehmen beitragen.

Ihre Organisation benötigt einen Business Continuity und Disaster Recovery (BCDR)-Strategie, die apps und den Arbeitslasten verfügbar bei geplanten und ungeplanten Ausfällen zu und so bald wie möglich auf normale wiederhergestellt. Site Recovery ist ein Azure, die dieser Strategie beiträgt.

Site Recovery orchestriert Replikation von Arbeitslasten mit lokalen Servern und virtuellen Maschinen. Sie können Server und VMs aus einem primären Rechenzentrum Cloud (Azure) oder sekundären Rechenzentrum replizieren. Bei Ausfall des primären Standorts Failover Sie an den sekundären Standort zu apps und Arbeitslasten zugänglich. Sie nicht an Ihrem primären Standort zu beenden.

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery im Azure-portal

Azure hat zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen. Azure-Ressourcen-Manager-Modell und klassischen Services Management-Modell. Azure hat zwei Portale – auch im [klassischen Azure-Portal](https://manage.windowsazure.com/) , die das klassische Bereitstellungsmodell unterstützt und [Azure-Portal](https://portal.azure.com) klassischen und Ressourcenmanager Modelle.

- Site Recovery ist in dem Verwaltungsportal und Azure-Portal.
- In der Azure-Verwaltungsportal unterstützen Sie Site Recovery mit klassischen Service Management-Modell.
- In Azure-Portal können Sie das klassische Modell oder Ressourcenmanager Bereitstellung unterstützen. 

Die Informationen in diesem Artikel gilt für klassische und Azure Portal-Bereitstellung. Unterschiede werden entsprechend gekennzeichnet.


## <a name="why-deploy-site-recovery"></a>Gründe für die Bereitstellung von Site Recovery

Hier ist was Site Recovery für Ihr Unternehmen:

- **BCDR vereinfachen**– Sie können behandeln, Replikation, Failover und Recovery mehrerer Arbeitslasten in einem Standort im Azure-Portal. Site Recovery koordiniert, Replikation und Failover jedoch nicht Ihre Anwendung Daten oder Informationen darüber.
- **Bereitstellen flexibler Replikation**– mithilfe von Site Recovery können Arbeitslasten auf unterstützten Hyper-V virtuelle Computer, virtuelle VMware-Computer und Windows-Linux-Servern repliziert.
- **Einfache Replikation durchführen testen**– Site Recovery bietet Test-Failover Unterstützung Disaster Recovery Bohrer ohne Produktion.
- **Failover und Recovery**– Ausführen von geplanten Failover für erwartete Ausfälle mit Null Datenverlust oder ungeplante Failovers mit minimalem Datenverlust (je nach Häufigkeit der Replikation) für unerwarteten Notfällen. Können Sie nach einem Failover Failback primäre Standorte. Site Recovery bietet Recovery-Pläne, die Skripts und Azure Automation Arbeitsmappen können Failover- und Multi-Tier-Anwendung anpassen können.
- **Entfernen einer sekundären Datencenter**-Workloads in Azure nicht an einem sekundären Standort replizieren. Dadurch Kosten und Komplexität ein sekundäres Rechenzentrum verwalten. Replizierte Daten werden in Azure-Speicher mit der Stabilität gespeichert, die bereitstellt. VMs werden bei einem Failover mit replizierten Daten erstellt.
- **Integration mit vorhandenen BCDR**-Site Recovery integriert mit anderen BCDR. Site Recovery können Sie um SQL Server-Back-End-Unternehmens Arbeitslasten, einschließlich Unterstützung für SQL Server AlwaysOn Failover Verfügbarkeitsgruppen zu schützen.

## <a name="what-can-i-replicate"></a>Was kann ich replizieren?

Hier ist eine Zusammenfassung der replizieren können mithilfe von Site Recovery.

![Lokal zu lokalen](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLIZIEREN** | **REPLIZIEREN IN** 
---|---
Arbeitslasten auf lokale virtuelle VMware-Computer | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundärstandort](site-recovery-vmware-to-vmware.md)
Arbeitslasten auf lokalen Hyper-V virtuelle Computer verwaltet in VMM-clouds  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Sekundärstandort](site-recovery-vmm-to-vmm.md) 
Arbeitslast auf lokalen Hyper-V VMs verwaltet VMM-Clouds mit SAN-Speicher|  [Sekundärstandort](site-recovery-vmm-san.md)
Arbeitslasten auf lokalen Hyper-V VMs ohne VMM | [Azure](site-recovery-hyper-v-site-to-azure.md)
Arbeitslasten auf lokalen Servern Windows/Linux | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundärstandort](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect"></a>Welche Arbeitslasten können werden geschützt?

Site Recovery kann anwendungsspezifische BCDR damit Arbeitslasten und apps beim Auftreten von Ausfällen in konsistenter Weise ausgeführt. Site Recovery bietet:

- **Anwendungskonsistente Snapshots**– Computer mithilfe von anwendungskonsistenten Snapshots für einzelne oder mehrere Ebenen apps replizieren. Neben Daten erfassen, anwendungskonsistente Snapshots capture alle Daten im Speicher und alle Transaktionen im Prozess.
- **In der Nähe synchrone Replikation**– Site Recovery bietet Replikationsrate beträgt 30 Sekunden für Hyper-V und die fortlaufende Replikation für VMware.
- **Flexible Recovery-Pläne**– erstellen und Anpassen von Recovery-Plänen mit externen Skripts und manuelle Aktionen. Integration in Azure Automation Runbooks können Sie einen gesamte Anwendungsstapel mit einem einzigen Mausklick wiederhergestellt.
- **Integration mit SQL Server AlwaysOn**-verwalten das Failover der Verfügbarkeitsgruppen in Site Recovery Recovery-Pläne.
- **Benutzeroberflächenautomatisierungs-Bibliothek**– bietet eine umfangreiche Bibliothek von Azure Automation Produktion, anwendungsspezifische Skripts, die heruntergeladen und Site Recovery integriert werden können.
- **Simple Network Management**– Erweitertes Netzwerk-Management-Website und Azure anwendungsanforderungen, einschließlich IP-Adressen reserviert, Lastenausgleich konfigurieren und Integrieren von Azure Traffic Manager für Netzwerk-Serverarrays vereinfacht.


## <a name="next-steps"></a>Nächste Schritte

- Lesen mehr [welche Arbeitslasten können Site Recovery schützen?](site-recovery-workload.md)
- Weitere Informationen zu Site Recovery-Architektur in [Site Recovery funktioniert?](site-recovery-components.md)
 
