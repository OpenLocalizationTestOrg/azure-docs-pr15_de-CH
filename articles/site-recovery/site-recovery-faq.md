<properties
    pageTitle="Azure Site Recovery: Häufig gestellte Fragen | Microsoft Azure"
    description="Dieser Artikel beschreibt häufig gestellte Fragen zur Azure Site Recovery."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>


# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: Häufig gestellte Fragen (FAQ)

Dieser Artikel enthält häufig gestellte Fragen zur Azure Site Recovery. Fragen nach dem Lesen dieses Artikels stellen Sie sie in [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Allgemein

### <a name="what-does-site-recovery-do"></a>Wozu dient die Site Recovery?

Site Recovery trägt der Business Continuity und Disaster Recovery (BCDR) Strategie umzusetzen und automatisierte Replikation von lokalen virtuellen Computern und Servern Azure oder sekundären Datencenter. [Erfahren Sie mehr](site-recovery-overview.md).


### <a name="what-can-site-recovery-protect"></a>Was kann Site Recovery schützen?

- **Hyper-V virtuelle Computer**: Site Recovery können alle Arbeitslasten auf einem Hyper-V-VM schützen.
- **Physische Server**: Site Recovery kann unter Windows oder Linux Servern schützen.
- **Virtuelle VMware-Maschinen**: Site Recovery kann jeder Vorgang in VMware VM schützen.

### <a name="does-site-recovery-support-the-azure-resource-manager-model"></a>Unterstützt Site Recovery Azure-Ressourcen-Manager-Modell?

Neben Site Recovery im klassischen Azure-Portal steht Site Recovery in Azure-Portal mit Unterstützung für Ressourcen-Manager. Für die meisten Bereitstellungsszenarien Site Recovery in Azure Portal bietet eine optimierte Bereitstellung und Replikation von VMs und Servern klassische Speicher oder Ressourcen-Manager-Speicher. Hier sind die unterstützten Bereitstellung:

- [Replizieren Sie virtuelle VMware-Computer oder physische Server in Azure in Azure-portal](site-recovery-vmware-to-azure.md)
- [Replizieren von Hyper-V virtuelle Computer in VMM-Clouds in Azure in Azure-portal](site-recovery-vmm-to-azure.md)
- [Replikation von Hyper-V VMs (ohne VMM) in Azure in Azure-portal](site-recovery-hyper-v-site-to-azure.md)
- [Replizieren von Hyper-V virtuelle Computer in VMM-Clouds an einem sekundären Standort im Azure-portal](site-recovery-vmm-to-vmm.md)


### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Was müssen ich in Hyper-V orchestrieren Replikation mit Site Recovery?

Für die Hyper-V-Host-Server hängt müssen Sie das Bereitstellungsszenario. Hyper-V-Komponenten in Auschecken:

- [Hyper-V VMs (ohne VMM) replizieren in Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Hyper-V VMs (mit VMM) replizieren in Azure](site-recovery-vmm-to-azure.md#before-you-start)
- [Hyper-V VMs auf einem sekundären Rechenzentrum repliziert](site-recovery-vmm-to-vmm.md#before-you-start)

- Wenn Sie in einem zweiten Datencenter [unterstützte Gastbetriebssysteme für Hyper-V virtuelle Computer](https://technet.microsoft.com/library/mt126277.aspx)Informationen replizieren.
- Wenn Sie in Azure replizieren, unterstützt Site Recovery alle Gastbetriebssysteme, die [von Azure unterstützt](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Kann ich virtuelle Computer schützen, wenn Hyper-V auf einem Client-Betriebssystem ausgeführt wird?

Nein, müssen VMs auf einem Hyper-V-Hostserver befinden, die auf einem unterstützten Windows Server ausgeführt wird. Benötigen Sie einen Clientcomputer schützen konnte Sie als physische Computer [Azure](site-recovery-vmware-to-azure.md) oder [sekundären Rechenzentrum](site-recovery-vmware-to-vmware.md)repliziert werden.


### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Welche Arbeitslasten können mit Site Recovery werden geschützt?

Site Recovery können zu den meisten Arbeitslasten auf einem physischen Server oder unterstützten virtuellen Computer ausgeführt. Site Recovery unterstützt anwendungsorientierte Replikation, dass apps intelligent Zustand wiederhergestellt werden können. Mit Microsoft SharePoint Exchange, Dynamik, SQL Server und Active Directory integriert und arbeitet eng mit führenden Anbietern wie Oracle, SAP, IBM und Red Hat. [Erfahren Sie mehr](site-recovery-workload.md) zu Arbeitslast Schutz.


### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Müssen Hyper-V-Hosts in VMM-Clouds werden?

Möchten Sie einen sekundären Rechenzentrum repliziert dann Hyper-V VMs auf hostet Hyper-V Server in VMM-Cloud. Wenn Sie in Azure replizieren möchten, können Sie die VMs auf Hyper-V-Host-Server mit oder ohne VMM-Clouds replizieren. [Mehr](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Kann ich Site Recovery mit VMM bereitstellen, wenn ich nur eine VMM-Server?

Ja. Sie können entweder VMs in Hyper-V-Servern in der Cloud VMM Azure, oder zwischen VMM-Clouds auf demselben Server replizieren können. Für lokal für die lokale Replikation empfehlen wir haben Sie VMM-Server am primären und sekundären Standorten.  [Lesen Sie mehr](site-recovery-single-vmm.md)

### <a name="what-physical-servers-can-i-protect"></a>Welche physischen Servern kann ich schützen?

Sie können physische Server unter Windows und Linux in Azure oder an einem sekundären Standort replizieren. [Erfahren Sie mehr über](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) Betriebssystem-Vorschriften.  Es gelten die gleichen Vorschriften, ob physische Server in Azure oder an einem sekundären Standort repliziert sind.

Beachten Sie, dass die physische Servern als VMs in Azure ausgeführt werden, wenn der lokale Server ausfällt. Failback auf einen lokalen physischen Server wird derzeit nicht unterstützt, aber Sie können nicht auf einem virtuellen Computer mit Hyper-V und VMware.


### <a name="what-vmware-vms-can-i-protect"></a>Welche VMware-VMs können werden geschützt?

Virtuelle VMware-Computer Schutz benötigen Sie vSphere Hypervisor und virtuellen Computern VMware Tools. Wir empfehlen auch Sie VMware vCenter Server die Hypervisoren zu verwalten. [Erfahren Sie mehr](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) über Anforderungen für VMware Server und virtuelle Computer in Azure oder an einem sekundären Standort repliziert.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Kann ich für meine Zweigstellen mit Site Recovery Wiederherstellung verwalten?

Ja. Verwendung Site Recovery Replikation und Failover in Ihren Zweigstellen orchestrieren, erhalten eine einheitliche Orchestrierung und Anzeigen aller Branch Office Arbeitslasten auf einem zentralen Sie. Sie können Failover ausgeführt und Wiederherstellung aller von Ihrem Hauptsitz ohne Zweige verwalten.

## <a name="security"></a>Sicherheit

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Werden Replikationsdaten werden auf der Site Recovery-Dienst gesendet?

Nein, Site Recovery nicht replizierte Daten abfangen, und nicht auf virtuellen Computern oder physischen Servern ausgeführten Informationen.
Replikation zwischen lokalen Hyper-V-Hosts, VMware-Hypervisoren Servern und Azure-Speicher oder sekundären Standort Datenaustausch. Site Recovery hat keine Möglichkeit, die Daten abzufangen. Replikation und Failover orchestrieren erforderlichen Metadaten wird die Site Recovery Service gesendet.

Site Recovery ist ISO 27001:2013, 27018, HIPAA, DPA zertifiziert, und bei der Bewertung von SOC2 und FedRAMP JAB.


### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Aus Gründen der Kompatibilität müssen auch unsere lokalen Metadaten derselben geographischen Region. Helfen Site Recovery uns?

Ja. Beim Erstellen einer Site Recovery Depots in einer Region wir sicherstellen, dass alle Metadaten, die wir und koordinieren Replikation und Failover bleibt in diesem Bereich geografische Begrenzung.

### <a name="does-site-recovery-encrypt-replication"></a>Verschlüsselt Site Recovery Replikation?

Replikation zwischen lokalen Sites Verschlüsselung-Transitlager wird für virtuelle Computer und Servern unterstützt. Für virtuelle Computer und physischen Servern replizieren in Azure Verschlüsselung in Transit und Verschlüsselung at Rest (in Azure) unterstützt.


## <a name="replication"></a>Replikation


### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Sind alle erforderlichen Komponenten für die Replikation von virtuellen Maschinen in Azure?

Virtuelle Computer in Azure replizieren möchten sollten [Azure](site-recovery-best-practices.md#virtual-machines)erfüllen.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Kann Hyper-V Generation 2 virtuelle Computer in Azure werden repliziert?

Ja. Site Recovery konvertiert von Generation 2 in Generation 1 bei einem Failover. Der Computer wird am Failback zu Generation 2 konvertiert. [Mehr](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Wenn ich in Azure replizieren wie Azure VMs Bezahle ich?

Während der normalen Replikation Daten Geo redundante Azure-Speicher repliziert und benötigen keine Gebühren virtuellen Azure IaaS Zahlen einen erheblichen Vorteil. Beim Ausführen eines Failovers in Azure Site Recovery erstellt automatisch Azure IaaS virtuellen Maschinen und danach werden Sie Rechnung für computeressourcen, die in Azure nutzen.


### <a name="is-there-an-sdk-i-can-use-to-automate-the-asr-workflow"></a>Gibt es eine SDK kann ich mit den ASR-Workflow automatisieren?

Ja. Die Rest-API, PowerShell oder in Azure SDK Site Recovery-Workflows automatisieren. Derzeit unterstützte Szenarien für die Bereitstellung von Site Recovery mit PowerShell:

- [Replikation von Hyper-V VMs VMMs Wolken Azure PowerShell classic](site-recovery-deploy-with-powershell.md)
- [Replikation von Hyper-V VMs VMMs Wolken zu Azure PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Replikation von Hyper-V VMs ohne VMM Azure PowerShell classic](site-recovery-hyper-v-site-to-azure-classic.md)
- [Replikation von Hyper-V VMs ohne VMM Azure PowerShell Ressourcenmanager](site-recovery-deploy-with-powershell-resource-manager.md)


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Wenn ich in Azure welches Speicherkonto replizieren muss?

- **Azure-Verwaltungsportal**: Wenn Sie Site Recovery im klassischen Azure-Portal bereitstellen, benötigen Sie einen [standard Geo redundante Speicherkonto](../storage/storage-redundancy.md#geo-redundant-storage). Premium-Speicher wird derzeit nicht unterstützt. Das Konto muss im Bereich der Site Recovery-Tresor.

- **Azure-Portal**: Wenn Sie in Azure-Portal Site Recovery bereitstellen, benötigen Sie ein Speicherkonto LRS oder g. Wir empfehlen g, damit Daten sowie einen regionalen Ausfall oder die primäre Region wiederhergestellt werden kann. Das Konto muss im Bereich Recovery Services Depot. Premium-Speicher wird nur unterstützt, wenn virtuelle VMware-Computer oder physischen Servern repliziert sind.

### <a name="how-often-can-i-replicate-data"></a>Wie oft können Daten replizieren?

- **Hyper-v** Hyper-V VMs können alle 30 Sekunden, Minuten oder 15 Minuten repliziert werden. SAN-Replikation festlegen ist Replikation synchron.
- **VMware und physische Server:** Hier ist ein Replikationsrate relevant. Replikation ist.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Kann Replication aus vorhandenen Recovery-Standort zu einem anderen dritten Standort erweitern?

Erweiterte oder verkettete Replikation wird nicht unterstützt. Dieses Feature [Bewertungsportal](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication)anfordern


### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Tun eine offline-Replikation beim ersten, die ich in Azure replizieren?

Dies wird nicht unterstützt. Dieses Feature [Bewertungsportal](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)anfordern


### <a name="can-i-exclude-specific-disks-from-replication"></a>Können bestimmte Datenträger von der Replikation werden ausgeschlossen?

Dies ist bei Azure Azure-Portal mit [VMware VMs und Servern repliziert](site-recovery-vmware-to-azure.md#exclude-disks-from-replication) werden unterstützt.


### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Können virtuelle Maschinen mit dynamischen Festplatten werden repliziert?

Dynamische Datenträger werden bei Hyper-V virtuelle Computer. Sie werden auch bei VMware-VMs und physische Computer in Azure. Betriebssystem-Datenträger muss einen Basisdatenträger sein.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Kann ich einen neuen Computer einen vorhandenen Replikationsgruppe hinzufügen?

Hinzufügen neuer Computer zu vorhandenen Replikation wird unterstützt. Dazu wählen Sie die Replikationsgruppe (aus "replizierte Objekte Blade) und im Kontextmenü Klicken Sie mit der rechten Maustaste auf/Select Replikationsgruppe und die entsprechende Option.

![Replikationsgruppe hinzufügen](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Können für Hyper-V-Replikationsverkehr zugeteilte Bandbreite einschränken?

Ja. Erfahren Sie mehr zum Einschränken der Bandbreite in den Artikeln der Bereitstellung:

- [Planung für virtuelle VMware-Computer und physischen Servern replizieren](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Planung für die Replikation von Hyper-V virtuelle Computer in VMM-clouds](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [Planung für die Replikation von Hyper-V virtuelle Computer ohne VMM](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## <a name="failover"></a>Failover


### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Wenn ich in Azure Failover bin, wie greife ich auf Azure virtuelle Computer nach dem Failover?

Sie können den Azure-VMs über eine sichere Internetverbindung über ein Standort-zu-Standort-VPN oder über Azure ExpressRoute zugreifen. Sie müssen eine Reihe von Dingen Verbindung vorzubereiten. Weitere Informationen finden Sie in:

- [Verbinden Sie nach dem Failover für virtuelle VMware-Computer oder physischen Servern Azure VMs](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment)
- [Verbinden Sie nach dem Failover von Hyper-V virtuelle Computer in VMM-Clouds Azure VMs](site-recovery-vmm-to-azure.md#step-7-test-your-deployment)
- [Verbinden Sie nach dem Failover von Hyper-V virtuelle Computer ohne VMM Azure VMs](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Wenn ich in Azure Failover wie Azure sicher sind meine Daten stabil?

Azure dient zur Stabilität. Site Recovery ist bereits für Failover zu einem sekundären Azure Datencenter gemäß SLA Azure ausgelegt, bei Bedarf. In diesem Fall wir achten Metadaten und Depots bleiben in derselben geographischen Region für den Tresor ausgewählt haben.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Wenn ich zwischen zwei Rechenzentren was geschieht repliziert bin, wenn meine primäre Datencenter ein unerwarteten Ausfall auftritt?

Sie können ein ungeplantes Failovers vom sekundären auslösen. Site Recovery benötigt kein Konnektivität vom primären Standort Failover ausführen.

### <a name="is-failover-automatic"></a>Ist Failover automatische?

Failover ist nicht automatisch. Initiieren Failover mit einem Klick in das Portal oder mithilfe von [Site Recovery PowerShell](https://msdn.microsoft.com/library/dn850420.aspx) einen Failover ausgelöst. Failback ist eine einfache Aktion im Portal Site Recovery.

Sie automatisieren konnte mit der lokalen Orchestrator oder Operations Manager einen virtuellen Computer Fehler erkennen und mit dem SDK Failover auslösen.

- [Mehr](site-recovery-create-recovery-plans.md) über Pläne.
- [Mehr](site-recovery-failover.md) über Failover.
- [Mehr](site-recovery-failback-azure-to-vmware.md) über fehlerhafte virtuelle VMware-Computer und physischen Servern sichern


## <a name="service-providers"></a>Dienstanbieter


### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Ich bin ein Dienstanbieter. Funktioniert Site Recovery für dedizierte und gemeinsame infrastrukturmodelle

Ja, Site Recovery unterstützt beide Modelle dedizierte und gemeinsame Infrastruktur.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Für Service Provider ist die Identität des Mandanten gemeinsam mit Site Recovery-Service?

Nein. Mieter Identität anonymisiert. Der Mieter zugreifen nicht auf Site Recovery-Portal. Nur Anbieter Dienstadministrator interagiert mit dem Portal.


### <a name="will-tenant-application-data-ever-go-to-azure"></a>Gehen Daten Mieter immer in Azure?

Bei der Replikation zwischen Service Provider im Besitz geht Daten nie in Azure. In Transit und replizierte direkt zwischen den Standorten Service Provider verschlüsselt.

Wenn Sie in Azure replizieren, werden Daten Azure-Speicher, jedoch nicht die Site Recovery-Dienst gesendet. Daten werden verschlüsselt in Transit und bleibt in Azure verschlüsselt.


### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Erhalten Meine Mieter Bill für Azure Dienste?

Nein. Abrechnungsinformationen Azure ist direkt mit dem Dienstanbieter. Dienstanbieter sind bestimmte Rechnungen für ihre Mieter generiert.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Wenn ich in Azure replizieren bin, müssen wir virtuelle Computer jederzeit in Azure ausgeführt?

Nein, werden Daten auf ein Azure-Speicher Ihr Abonnement repliziert. Beim Ausführen von testfailover (DR Drill) oder eine tatsächliche Failover wird der Site Recovery virtuelle Computer in Ihrem Abonnement erstellt.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Gewährleisten Sie Isolierung Mieter auf, wenn ich in Azure replizieren?

Ja.

### <a name="what-platforms-do-you-currently-support"></a>Welche Plattformen werden derzeit unterstützt?

Wir unterstützen Azure Pack Cloud-Plattformsystem und System Center-basierte Installationen (2012 und höhere). [Weitere Informationen](https://technet.microsoft.com/library/dn850370.aspx) zur Integration von Azure Pack und Site Recovery.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Unterstützen einzelne Azure Pack und einzelnen Bereitstellung von VMM-Server?

Ja, Sie Hyper-V virtuelle Computer in Azure replizieren oder replizieren zwischen Service Provider.  Beachten Sie, dass Replizieren zwischen Service Provider Azure Runbook Integration verfügbar ist.


## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Übersicht über Website](site-recovery-overview.md)
- Erfahren Sie mehr über [Site Recovery-Architektur](site-recovery-components.md)  
