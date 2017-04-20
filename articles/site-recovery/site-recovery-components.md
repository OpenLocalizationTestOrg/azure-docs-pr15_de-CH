<properties
    pageTitle="Funktionsweise von Site Recovery | Microsoft Azure"
    description="Dieser Artikel enthält eine Übersicht über die Site Recovery-Architektur"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="how-does-azure-site-recovery-work"></a>Funktionsweise von Azure Site Recovery

Diesem Artikel verstehen, die zugrunde liegende Architektur von Azure Site Recovery-Dienst und die Komponenten dieser arbeiten. 

Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Übersicht

Organisationen benötigen eine BCDR Strategie, die bestimmt, wie apps Arbeitslasten und Daten bleiben aktiv bei geplanten und ungeplanten Ausfällen normalen Arbeitsbedingungen so bald wie möglich wiederherstellen. Die BCDR Strategie sollten schützen Sie Daten wiederhergestellt und Arbeitslasten ständig verfügbar bleiben beim Notfall. 

Site Recovery ist eine Azure Service BCDR Strategie umzusetzen Replikation von lokalen Servern und virtuellen Computer in die Cloud (Azure) oder zu einem sekundären Datencenter beiträgt. Treten Ausfälle in Ihrem primären Standort Failover Sie sekundäre an apps und Arbeitslasten zur Verfügung. Sie nicht an Ihrem primären Standort zu beenden. Weitere Informationen [Neuigkeiten von Site Recovery?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery im Azure-portal

Azure hat zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: der Azure-Ressourcen-Manager und klassischen Services Management. Azure hat ebenfalls zwei Portale – [Azure-Verwaltungsportal](https://manage.windowsazure.com/) , die das klassische Bereitstellungsmodell unterstützt und [Azure-Portal](https://portal.azure.com) mit Unterstützung für beide Bereitstellungsmodelle.

Site Recovery ist in dem Verwaltungsportal und Azure-Portal. Im klassischen Azure-Portal können Sie Site Recovery mit klassischen Service Management-Modell unterstützen. Azure-Portal können Sie Klassisch oder Ressourcenmodell Bereitstellung unterstützen. [Mehr](site-recovery-overview.md#site-recovery-in-the-azure-portal) über Azure Portal bereitstellen.

Die Informationen in diesem Artikel gilt für klassische und Azure Portal-Bereitstellung. Unterschiede werden entsprechend gekennzeichnet.

## <a name="deployment-scenarios"></a>Bereitstellungsszenarien

Site Recovery kann zum orchestrieren Replikation in eine Reihe von Szenarios bereitgestellt werden:

- **Virtuellen Maschinen von VMware replizieren**: lokalen virtuellen VMware Maschinen können Azure oder sekundären Rechenzentrum repliziert.
- - **Physische Computer replizieren**: Rechner unter Windows oder Linux Azure oder sekundären Rechenzentrum repliziert. Der Prozess für das Replizieren von physischen Computern ist fast identisch mit dem Prozess für virtuelle VMware-Computer replizieren
- **Replizieren von Hyper-V VMs (ohne VMM)**: Hyper-V-VMs, die von VMM in Azure verwaltet werden nicht repliziert.
- **Replizieren von Hyper-V VMs in System Center VMM-Clouds verwaltet**: lokale Hyper-V virtuelle Maschinen auf Hyper-V-Hostservern in VMM-Clouds Azure oder sekundären Rechenzentrum repliziert. Sie können standardmäßige Hyper-V Replica oder SAN-Replikation replizieren.
- **Migrieren von VMs**: Site Recovery zwischen [Azure IaaS VMs migrieren](site-recovery-migrate-azure-to-azure.md) oder [Migrieren Instanzen AWS Windows](site-recovery-migrate-aws-to-azure.md) Azure IaaS VMs verwenden. Derzeit nur Migration wird unterstützt die VMs Failover, Sie aber nicht möglich sie zurück.

Site Recovery kann die meisten apps auf diese VMs und physische Server replizieren. Sie erhalten eine vollständige Zusammenfassung der unterstützten apps [welche Arbeitslasten können Azure Site Recovery schützen?](site-recovery-workload.md)


## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Replizieren in Azure: VMware virtuellen Maschinen oder Windows/Linux Servern

Gibt es auf verschiedene Weise mit Site Recovery virtuelle VMware-Computer repliziert.

- **Mithilfe des Azure-Portals**-Site Recovery beim im Azure-Portal VMs classic-Dienst-Manager Speicher oder zum Ressourcen-Manager Failover können bereitstellen. Replizieren virtuelle VMware-Computer in Azure-Portal bringt eine Reihe von Vorteilen und Ressourcenmanager Speicher in Azure replizieren. [Erfahren Sie mehr](site-recovery-vmware-to-azure.md).
- **Das klassische Portal**-Site Recovery können im klassischen Portal mit Funktionen bereitstellen. Dies sollte für alle neuen Installationen im klassischen Portal verwendet werden. Diese Bereitstellung kann nur über VMs klassische Speicher in Azure und Ressourcenmanager Speicher nicht durchgeführt werden. [Erfahren Sie mehr](site-recovery-vmware-to-azure-classic.md). Es gibt auch eine [ältere Erfahrung](site-recovery-vmware-to-azure-classic-legacy.md) für VMware-Replikation im klassischen Portal einrichten. Dies sollte bei Neuinstallationen verwendet werden.  Wenn Sie bereits die legacy Erfahrung [lernen Sie migrieren](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) die verbesserte Bereitstellung bereitgestellt haben.



Architektonischen Anforderungen für die Bereitstellung von Site Recovery VMware VMs/physische Server in Azure-Portal oder klassischen Azure-Portal (Erweitert) replizieren ähneln, mit einer Reihe von unterschieden:

- Wenn Sie zum Ressourcen-Manager-basierte Replikation und Ressourcenmanager Netzwerken für die Verbindung der Azure-VMs nach einem Failover verwenden Azure-Portal bereitstellen.
- Wenn Sie beide LRS in Azure-Portal bereitstellen und g-Speicher unterstützt. Im klassischen Portal GRS ist erforderlich.
- Der Bereitstellungsprozess ist vereinfacht und benutzerfreundlichere im Azure-Portal.


Hier ist, was Sie benötigen:

- **Ein Azure-Konto**: Sie benötigen ein Microsoft Azure-Konto.
- **Azure-Speicher**: benötigen Sie ein Konto Azure-Speicher zum Speichern von replizierter Daten. Sie können ein klassisches oder ein Speicherkonto Ressourcenmanager. Das Konto kann LRS oder g in Azure-Portal bereitstellen. Replizierte Daten in Azure-Speicher gespeichert und Azure VMs werden erstellt, wenn ein Failover auftritt. 
- **Azure-Netzwerk**: benötigen Sie eine Azure virtual Network, der Azure-VMs verbunden wird, wenn sie beim Failover erstellt werden. Im Azure-Portal können sie Netzwerke im klassischen Service Manager-Modell oder im Ressourcen-Manager-Modell erstellt werden.
- **Lokalen Konfigurationsserver**: benötigen Sie einen lokale Windows Server 2012 R2 Computer, auf dem Konfigurationsserver und andere Site Recovery ausgeführt. Wenn Sie virtuelle VMware-Computer replizieren sind sollte dies eine hochverfügbare VMware VM. Wenn Sie physische Server replizieren möchten kann den Computer physische handeln. Diese Site Recovery-Komponenten werden auf dem Computer installiert:
    - **Konfigurationsserver**: Kommunikation zwischen lokalen Umgebung und Azure koordiniert und verwaltet die Replikation und Recovery.
    - **Prozessserver**: fungiert als Gateway Replikation. Diese geschützte Datenquelle Maschinen replizierten Daten empfängt, zwischenspeichern, Komprimierung und Verschlüsselung optimiert und sendet die Daten an den Azure-Speicher. Auch Push-Installation des Dienstes Mobilität geschützten Computern verarbeitet und führt die automatische Ermittlung für virtuelle VMware-Computer. Wachstum die Bereitstellung können Sie zusätzliche separaten dedizierten Prozessserver wachsende Datenmengen Replikationsverkehr Behandlung hinzufügen.
    - **Master-Zielserver**: Replikationsdaten während des Failbacks von Azure behandelt. 
- **Virtuelle VMware-Computer oder physischen Servern replizieren**: jeder Computer replizieren in Azure benötigen die Mobility-Dienstkomponente installiert. Dieser Dienst Schreibvorgänge auf dem Computer erfasst und an den Prozess-Server weiterleitet. Diese Komponente kann manuell installiert werden oder können werden abgelegt und installiert automatisch den Prozess-Server bei der Replikation für einen Computer aktivieren.
- **vSPhere Hosts-vCenter Server**: Sie benötigen mindestens vSphere Hostservern virtuelle VMware-Computer. Wir empfehlen einen vCenter Server zum Verwalten dieser Hosts bereitstellen.
- **Failback**: Sie benötigen:
    - **Physisch zu physisch-Failback wird nicht unterstützt**: Dies bedeutet, dass wenn physische Server in Azure Failover und Failback möchten Sie VMware VM Failback müssen. Sie können nicht auf einem physischen Server ausgeführt. Benötigen Sie ein Failback Azure VM, und wenn nicht den Konfigurationsserver als VMware VM bereitstellen müssen ein separates master Zielserver als VMware VM einrichten. Dies ist erforderlich, da master Zielserver interagiert und fügt an VMware Storage VMware VM Datenträger wiederherstellen.
    - - **Temporäre Prozessserver in Azure**: Wenn Sie nach einem Failover von Azure fehlschlagen eine Azure VM als Prozessserver konfiguriert Replizieren von Azure einrichten müssen möchten. Sie können diese VM nach Abschluss Failback.
    - **VPN-Verbindung**: Failbacks Sie müssen eine VPN-Verbindung (oder Azure ExpressRoute) Einrichten von Azure Netzwerk auf der lokalen Website.
    - **Separate lokale master Zielserver**: master Zielserver lokale Failback behandelt. Master Zielserver wird standardmäßig auf dem Verwaltungsserver installiert sollten, wenn wieder größere Volumes des Datenverkehrs nicht sind Sie jedoch master separaten lokalen Zielserver für diesen Zweck.

**Allgemeine Architektur**

![Erweiterte](./media/site-recovery-components/arch-enhanced.png)

**Von bereitstellungskomponenten**

![Erweiterte](./media/site-recovery-components/arch-enhanced2.png)

**Failback**

![Erweiterte failback](./media/site-recovery-components/enhanced-failback.png)


- [Weitere Informationen](site-recovery-vmware-to-azure.md#azure-prerequisites) zu Azure-Bereitstellung an.
- [Erfahren Sie mehr](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) über verbesserte Bereitstellung im klassischen Portal.
- [Erfahren Sie mehr](site-recovery-failback-azure-to-vmware.md) über Failback im Portal Auzre.
- [Erfahren Sie mehr](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) über Failback im klassischen Portal.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Replizieren in Azure: Hyper-V virtuelle Computer nicht von VMM verwaltet

Sie können Hyper-V virtuelle Computer replizieren, die vom System Center VMM in Azure mit Site wie folgt nicht verwaltet:

- **Mithilfe des Azure-Portals**-Site Recovery beim im Azure-Portal VMs klassische Speicher oder zum Ressourcen-Manager Failover können bereitstellen. [Erfahren Sie mehr](site-recovery-hyper-v-site-to-azure.md).
- **Das klassische Portal**-Site Recovery können im klassischen Portal bereitstellen. Diese Bereitstellung kann nur über VMs klassische Speicher in Azure und Ressourcenmanager Speicher nicht durchgeführt werden. [Erfahren Sie mehr](site-recovery-hyper-v-site-to-azure-classic.md).

Die Architektur für beide Bereitstellungen ist ähnlich, außer dass:

- Wenn Sie in Azure-Portal bereitstellen, Ressourcenmanager Speicher replizieren und Ressourcenmanager Netzwerken für die Verbindung der Azure-VMs nach einem Failover verwenden.
- Der Bereitstellungsprozess ist vereinfacht und benutzerfreundlichere im Azure-Portal.

Hier ist, was Sie benötigen:

- **Ein Azure-Konto**: Sie benötigen ein Microsoft Azure-Konto.
- **Azure-Speicher**: benötigen Sie ein Konto Azure-Speicher zum Speichern von replizierter Daten. Im Azure-Portal können Sie ein klassisches oder ein Speicherkonto Ressourcenmanager. Im klassischen Portal können Sie eine klassische Konto. Replizierte Daten in Azure-Speicher gespeichert und Azure VMs werden erstellt, wenn ein Failover auftritt.
- **Azure-Netzwerk**: benötigen Sie eine Azure Netzwerk Azure VMs herstellen können, wenn sie nach einem Failover erstellt werden. 
- **Hyper-V-Host**: Sie benötigen mindestens Windows Server 2012 R2 Hyper-V Hostserver. Während der Site Recovery-Bereitstellung installieren Sie Azure Site Recovery-Anbieter und Microsoft Azure Recovery Services-Agent auf dem Host.
- **Hyper-V VMs**: benötigen Sie eine oder mehrere VMs auf dem Hyper-V-Hostserver. Azure Site Recovery Anbieter und Azure Recovery Services-Agent auf dem Hyper-V-Host während der Site Recovery-Bereitstellung. Der Anbieter koordiniert und orchestriert Replikation mit Site Recovery-Dienst über das Internet. Der Agent verarbeitet Daten-Replikation über HTTPS 443. Kommunikation mit dem Anbieter und dem Agent sind verschlüsselt. Replizierte Daten in Azure-Speicher ist ebenfalls verschlüsselt.

**Allgemeine Architektur**

![Hyper-V-Website in Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Weitere Informationen](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) zu Azure-Bereitstellung an.
- [Erfahren Sie mehr](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) zu klassischen Bereitstellung an.



## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Replizieren in Azure: Hyper-V-VMs von VMM verwaltet

Sie können Hyper-V virtuelle Computer in VMM-Clouds in Azure mit Site Recovery folgendermaßen replizieren:

- **Mithilfe des Azure-Portals**-Site Recovery beim im Azure-Portal VMs klassische Speicher oder zum Ressourcen-Manager Failover können bereitstellen. [Erfahren Sie mehr](site-recovery-vmm-to-azure.md).
- **Das klassische Portal**-Site Recovery können im klassischen Portal bereitstellen. Diese Bereitstellung kann nur über VMs klassische Speicher in Azure und Ressourcenmanager Speicher nicht durchgeführt werden. [Erfahren Sie mehr](site-recovery-vmm-to-azure-classic.md).

Die Architektur für beide Bereitstellungen ist ähnlich, außer dass:

- Wenn Sie zum Ressourcen-Manager-basierte Replikation und Ressourcenmanager Netzwerken für die Verbindung der Azure-VMs nach einem Failover verwenden Azure-Portal bereitstellen.
- Der Bereitstellungsprozess ist vereinfacht und benutzerfreundlichere im Azure-Portal.


Hier ist, was Sie benötigen:

- **Ein Azure-Konto**: Sie benötigen ein Microsoft Azure-Konto.
- **Azure-Speicher**: benötigen Sie ein Konto Azure-Speicher zum Speichern von replizierter Daten. Im Azure-Portal können Sie ein klassisches oder ein Speicherkonto Ressourcenmanager. Im klassischen Portal können Sie eine klassische Konto. Replizierte Daten in Azure-Speicher gespeichert und Azure VMs werden erstellt, wenn ein Failover auftritt.
- **Azure-Netzwerk**: Sie müssen einrichten Netzwerk zuordnen, sodass Azure VMs mit entsprechenden Netzwerken verbunden sind, wenn sie nach einem Failover erstellt werden. 
- **VMM-Server**: Sie benötigen mindestens eine lokale VMM Server auf System Center 2012 R2 und mit einer oder mehreren private Clouds einrichten. Wenn Sie in Azure bereitstellen Portal müssen logische und VM-Netzwerke einrichten, damit Netzwerkzuordnung zu konfigurieren. Im klassischen Portal ist optional.  VM-Netzwerk sollte ein logisches Netzwerk verbunden, das mit der Cloud verbunden ist.
- **Hyper-V-Host**: Sie benötigen mindestens Windows Server 2012 R2 Hyper-V-Hostservern in der VMM-Cloud.
- **Hyper-V VMs**: benötigen Sie eine oder mehrere VMs auf dem Hyper-V-Hostserver.

**Allgemeine Architektur**

![VMM in Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Weitere Informationen](site-recovery-vmm-to-azure.md#azure-requirements) zu Azure-Bereitstellung an.
- [Erfahren Sie mehr](site-recovery-vmm-to-azure-classic.md#before-you-start) zu klassischen Bereitstellung an.




## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>An einem sekundären Standort replizieren: VMware virtuellen Computern oder Servern 

Virtuelle VMware-Computer oder physischen Servern an einem sekundären Standort als Download InMage Scout repliziert, die in Azure Site Recovery-Abonnement enthalten. Sie können von Azure-Portal oder klassischen Azure-Portal herunterladen. 

Sie Komponentenserver in jedem Standort (Konfiguration, Prozess master Ziel) einrichten und Installieren der Unified Agent auf Computern, die Sie replizieren möchten. Nach der anfänglichen Replikation sendet der Agent auf jedem Computer Delta replikationsänderungen an den Prozess-Server. Prozess-Server optimiert die Daten und überträgt diese an den master Zielserver am sekundären Standort. Der Konfigurationsserver managt den Replikationsprozess.

Hier ist, benötigen Sie:

**Ein Azure-Konto**: Dieses Szenario mit InMage Scout bereitstellen. Sie erhalten erforderlich ein Azure-Abonnement. Nach dem Erstellen eines Depots Site Recovery InMage Scout downloaden und installieren Sie die neuesten Updates für die Bereitstellung einrichten.
**Prozessserver (primärer Standort)**: Prozess-Server-Komponente im primären Standort Zwischenspeichern und Komprimierung datenoptimierung Behandlung eingerichtet. Es behandelt auch Pushinstallation des Agenten Unified Maschinen zu schützen. 
**VMware ESX/ESXi und vCenter Server (primärer Standort)**: Wenn Sie virtuelle VMware-Computer schützen benötigen Sie VMware ex/ESXi Hypervisor und optional ein VMware vCenter Server-Hypervisoren zu verwalten.
- **VMs-physische Server (primärer Standort)**: virtuelle VMware-Computer oder Windows/Linux Servern zu schützen wird benötigen die Unified Agent installiert. Unified Agent wird auch auf den Computern als master Zielserver installiert. Der Agent fungiert als Kommunikationsanbieter zwischen allen Komponenten. 
- - **Konfigurationsserver (sekundäre Standort)**: der Konfigurationsserver ist die erste Komponente, die Sie installieren und auf den sekundären Standort verwalten, konfigurieren und Überwachen der Bereitstellung mit der Management-Website oder der vContinuum-Konsole installiert ist. Gibt es nur eine Konfiguration für einzelne Server in einer Bereitstellung und muss auf einem Computer mit Windows Server 2012 R2 installiert sein.
- **vContinuum-Server (sekundäre Standort)**: wird am selben Speicherort (sekundäre Standort) als Konfigurationsserver installiert. Es ist eine Konsole zur Verwaltung und Überwachung Ihrer geschützten Umgebung. Bei einer Standardinstallation vContinuum Server den ersten master Zielserver und Unified Agent installiert.
- **Master-Ziel-Server (sekundäre Standort)**: master Zielserver replizierte Daten enthält. Es empfängt Daten vom Prozess-Server erstellt eine Replikat-Rechner in den sekundären Standort und Aufbewahrung Datenpunkte enthält. Die Anzahl der benötigten master Zielserver hängt von der Anzahl der Computer, den, die Sie schützen. Möchten Sie mit dem primären Standort fehl benötigen Sie auch master Zielserver vorhanden. 

**Allgemeine Architektur**

![VMware VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>An einem sekundären Standort replizieren: Hyper-V-VMs von VMM verwaltet


Sie können Hyper-V virtuelle Computer replizieren, die vom System Center VMM in einem sekundären Datencenter mit Site wie folgt verwaltet werden:

- **Mithilfe des Azure-Portals**-Site Recovery beim im Azure-Portal bereitstellen. [Erfahren Sie mehr](site-recovery-hyper-v-site-to-azure.md).
- **Das klassische Portal**-Site Recovery können im klassischen Portal bereitstellen. [Erfahren Sie mehr](site-recovery-hyper-v-site-to-azure-classic.md).

Die Architektur für beide Bereitstellungen ist ähnlich, außer dass:

- Wenn Sie in Azure-Portal bereitstellen müssen Sie Netzwerkübersicht einrichten. Dies ist optional das Verwaltungsportal.
- Der Bereitstellungsprozess ist vereinfacht und benutzerfreundlichere im Azure-Portal.
- - Bereitstellung in Azure ist klassischen Portal [Speicher zuordnen](site-recovery-storage-mapping.md) verfügbar.

Hier ist, was Sie benötigen:

- **Ein Azure-Konto**: Sie benötigen ein Microsoft Azure-Konto.
- **VMM-Server**: Wir empfehlen VMM-Server am primären Standort und in den sekundären Standort jeweils mindestens eine private Cloud von VMM. Der Server sollte mindestens ausgeführt System Center 2012 SP1 mit neuesten Updates und mit dem Internet verbunden. Wolken müssen das Hyper-V-Funktion Profil. Installieren Sie den Azure Site Recovery-Anbieter auf dem VMM-Server. Der Anbieter koordiniert und orchestriert Replikation mit Site Recovery-Dienst über das Internet. Kommunikation zwischen dem Anbieter und Azure sind verschlüsselt.
- **Hyper-V Server**: Hyper-V-Hostservern in primäre und sekundäre VMM-Clouds befinden soll. Die Host Server ausführen soll Windows Server 2012 mit den neuesten Updates installiert und mit dem Internet verbunden. Daten werden zwischen den primären und sekundären Hyper-V-Hostservern über LAN oder VPN mit Kerberos oder Zertifikat repliziert.  
- **Geschützt Maschinen**: Quelle Hyper-V-Host-Server muss mindestens ein virtueller Computer, die Sie schützen möchten.

**Allgemeine Architektur**

![Lokal zu lokalen](./media/site-recovery-components/arch-onprem-onprem.png)


- [Erfahren Sie mehr](site-recovery-vmm-to-vmm.md#azure-prerequisites) über Bereitstellung im Azure-Portal.
- - [Erfahren Sie mehr](site-recovery-vmm-to-vmm-classic.md#before-you-start) über Bereitstellung im klassischen Azure-Portal.




## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Ein sekundärer Standort mit SAN-Replikation replizieren: Hyper-V-VMs von VMM verwaltet

Sie können Hyper-V virtuelle Computer in VMM-Clouds an einem sekundären Standort mit SAN-Replikation mit klassischen Azure-Portal verwaltet replizieren. Dieses Szenario wird im neuen Azure-Portal derzeit nicht unterstützt. 

In diesem Szenario während der Site Recovery-Bereitstellung installieren Sie Azure Site Recovery Provider in VMM-Server. Der Anbieter koordiniert und orchestriert Replikation mit Site Recovery-Dienst über das Internet. Daten werden zwischen der primären und sekundären Speicher-Arrays über synchrone SAN-Replikation repliziert.

Hier ist, was Sie benötigen:

**Ein Azure-Konto**: Sie benötigen ein Azure-Abonnement
- **SAN-Array**: [unterstützt SAN-Array](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) von primären VMM-Server verwaltet. SAN gemeinsam mit einem anderen SAN-Array am sekundären Standort eine Netzwerkinfrastruktur.
- **VMM-Server**: Wir empfehlen VMM-Server am primären Standort und in den sekundären Standort jeweils mindestens eine private Cloud von VMM. Der Server sollte mindestens ausgeführt System Center 2012 SP1 mit neuesten Updates und mit dem Internet verbunden. Wolken müssen das Hyper-V-Funktion Profil.
- **Hyper-V Server**: Hyper-V-Host-Servern in der primären und sekundären VMM-Clouds. Die Host Server ausführen soll Windows Server 2012 mit den neuesten Updates installiert und mit dem Internet verbunden.
- **Geschützt Maschinen**: Quelle Hyper-V-Host-Server muss mindestens ein virtueller Computer, die Sie schützen möchten.

**SAN-Replikation-Architektur**

![SAN-Replikation](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Erfahren Sie mehr](site-recovery-vmm-san.md#before-you-start) über Bereitstellung.
### <a name="on-premises"></a>Vor Ort



## <a name="hyper-v-protection-lifecycle"></a>Hyper-V-Schutz-Lebenszyklus

Dieser Workflow zeigt den Prozess zum Schutz, Replikation und Failover Hyper-V virtuelle Computer. 

1. **Schutz**: Site Recovery Depot einrichten, Konfigurieren der Replikation für eine VMM-Cloud oder Hyper-V-Website und Schutz für virtuelle Computer. Ein Projekt mit der Bezeichnung **Schutz aktivieren** initiiert, und auf der Registerkarte **Aufträge** überwacht werden. Der Auftrag überprüft, der Computer ruft dann die [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) -Methode setzt Replikation in Azure mit den konfigurierten Komponenten entspricht. **Schutz** Auftrag ruft auch die [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) -Methode, um eine vollständige Replikation VM initialisieren.
2. **Erste Replikation**: Momentaufnahme einer virtuellen und virtueller Festplatten werden repliziert, bis sie alle Azure oder sekundären Datencenter kopiert werden. Die Zeit für dieses hängt virtueller Speicher, Netzwerkbandbreite und die erste Replikation-Methode. Datenträger Änderungen während der ersten Replikation verfolgt Hyper-V Replica Replikation Tracker Änderungen als Hyper-V-Replikationsprotokollen (.hrl), die in demselben Ordner wie die Datenträger befinden. Jeder Datenträger hat eine zugeordnete .hrl-Datei, die in den sekundären Speicher gesendet werden. Beachten Sie, dass die Momentaufnahme und-Protokolldateien während der ersten Replikation Datenträgerressourcen nutzen. Nach Abschluss die anfängliche Replikation der VM-Snapshot gelöscht und Delta Datenträger Protokoll sind synchronisiert und zusammengeführt.
3. **Finalize Schutz**: nach Abschluss Erstreplikation **Finalize** Schutzauftrag Netzwerk- und weitere nach der Replikation so konfiguriert, dass der virtuelle Computer geschützt ist. Wenn Sie Azure Replikation müssen Sie die Einstellung für den virtuellen Computer zu optimieren, so dass für Failover bereit ist. An dieser Stelle führen Sie ein testfailover überprüfen, dass alles wie erwartet funktioniert.
4. **Replikation**: nach der anfänglichen Replikation Delta-Synchronisierung beginnt nach Replikationseigenschaften. 
    - **Replikationsfehler**: Wenn Deltareplikation fehlschlägt, eine vollständige Replikation beansprucht Bandbreite und Zeit wäre und erneute Synchronisierung auftritt. Für Beispiel erreicht .hrl Dateien 50 % der Größe der Festplatte dann die VM für die erneute Synchronisierung markiert wird. Resynchronisation minimiert Kontrollsummen Quell- und virtuellen Computern und senden das Delta gesendeten Daten. Nach Abschluss der erneuten Synchronisierung wird Deltareplikation fortgesetzt. Standardmäßig Resynchronisation soll automatisch außerhalb der Geschäftszeiten ausgeführt, jedoch können Sie einen virtuellen Computer manuell synchronisieren.
    - **Replikationsfehler**: tritt ein Replikationsfehler ist eine integrierte wiederholen. Es ist ein nicht behebbarer Fehler wie eine Authentifizierung oder Autorisierung, ob ein replikatcomputer ist in einem ungültigen Zustand, wird keine Wiederholung versucht. Es ist ein behebbarer Fehler oder einen Netzwerkfehler, genügend Speicherplatz oder dem, wenn eine Wiederholung tritt immer zwischen Wiederholungsversuchen (1, 2, 4, 8, 10, und dann alle 30 Minuten).
4. **Geplante/ungeplante Failovers**: geplanten oder ungeplanten Failover läuft nach Bedarf. Ausführen einer geplanten Failover und Quelle VMs heruntergefahren werden sicher keine Daten verloren. Erstellung von Replikat VMs sind sie ein Commit aussteht platziert. Müssen Sie zum Abschließen des Failovers ein Commit ausgeführt, wenn Replikation mit SAN in diesem Fall Commit automatisch. Failback kann auftreten, wenn der primäre Standort ausgeführt wird. In Azure reverse repliziert haben ist Replikation automatisch. Andernfalls starten Sie reverse Replikation manuell.
 

![Workflow](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Nächste Schritte

[Bereiten auf die Bereitstellung vor](site-recovery-best-practices.md)
