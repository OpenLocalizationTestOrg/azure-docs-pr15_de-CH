<properties
    pageTitle="Vorbereiten auf die Bereitstellung von Site Recovery | Microsoft Azure"
    description="Dieser Artikel beschreibt die Replikation mit Azure Site Recovery Bereitstellung bereit."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Bereiten Sie auf die Bereitstellung von Azure Site Recovery vor

Dieser Artikel eine umfassende Übersicht über Bereitstellung an jedes Szenario Replikation von Azure Site Recovery-Dienst. Nach dem Lesen der allgemeinen Vorschriften für jedes Szenario können Sie mit speziellen Bereitstellungsdetails für jede Bereitstellung verknüpfen.

Nach dem Lesen dieser Beitrag Kommentare oder Fragen am Ende des Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Übersicht

Organisationen benötigen eine BCDR Strategie, die bestimmt, wie apps Arbeitslasten und Daten bleiben aktiv bei geplanten und ungeplanten Ausfällen normalen Arbeitsbedingungen so bald wie möglich wiederherstellen. Die BCDR Strategie sollten schützen Sie Daten wiederhergestellt und Arbeitslasten ständig verfügbar bleiben beim Notfall. 

Site Recovery ist eine Azure Service BCDR Strategie umzusetzen Replikation von lokalen Servern und virtuellen Computer in die Cloud (Azure) oder zu einem sekundären Datencenter beiträgt. Treten Ausfälle in Ihrem primären Standort Failover Sie sekundäre an apps und Arbeitslasten zur Verfügung. Sie nicht an Ihrem primären Standort zu beenden. Weitere Informationen [Neuigkeiten von Site Recovery?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Wählen Sie Ihr Bereitstellungsmodell

Azure hat zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: der Azure-Ressourcen-Manager und klassischen Services Management. Azure hat ebenfalls zwei Portale – [Azure-Verwaltungsportal](https://manage.windowsazure.com/) , die das klassische Bereitstellungsmodell unterstützt und [Azure-Portal](https://ms.portal.azure.com/) mit Unterstützung für beide Bereitstellungsmodelle.

Site Recovery ist in dem Verwaltungsportal und Azure-Portal. Im klassischen Azure-Portal können Sie Site Recovery mit klassischen Service Management-Modell unterstützen. Im Azure-Portal können Sie das klassische Modell oder Ressourcenmanager Bereitstellung unterstützen. [Mehr](site-recovery-overview.md#site-recovery-in-the-azure-portal) über Azure Portal bereitstellen.

Wenn Sie Modellnotizen Bereitstellung, die die Wahl:

- Wir empfehlen Sie möglichst [Azure-Portal](https://portal.azure.com) und der Ressourcen-Manager-Modell verwenden.
- Site Recovery bietet einfacher und intuitiver Einstieg Erfahrung im Azure-Portal.
- Azure-Portal können Sie Maschinen Classic und Ressourcenmanager Speicher in Azure replizieren. Darüber hinaus können Sie in Azure-Portal LRS oder GRS Speicherkonten verwenden.
- Azure-Portal kombiniert Site Recovery und Backup-Services in einem Tresor Recovery Services, sodass einrichten und BCDR Dienste von einem Standort aus verwalten.
- Benutzer mit Azure Cloud Solution Provider (CSP) Programm bereitgestellt können jetzt Site Recovery-Vorgänge im Azure-Portal verwalten.
- Replizieren virtuelle VMware-Computer oder physischen Computern in Azure in Azure-Portal bietet eine Reihe neuer Features einschließlich Unterstützung für Premium-Speicher und die Möglichkeit, bestimmte Festplatten von der Replikation ausschließen.


## <a name="select-your-deployment-scenario"></a>Wählen Sie Ihr Bereitstellungsszenario

**Bereitstellung** | **Details** | **Azure-portal** | **Verwaltungsportal** | **PowerShell**
---|---|---|---|---
**Virtuelle VMware-Computer in Azure** | Replikation von VMware VMs Azure-Speicher | In der Azure kann Portal VMs und Ressourcenmanager Speicher Failover<br/><br/> Bereitstellung in [Azure-Portal](site-recovery-vmware-to-azure.md) bietet eine optimierte bereitstellungserfahrung und eine Anzahl von Funktionen profitieren. | Im klassischen Azure-Portal können Sie mit [erweiterten Modell](site-recovery-vmware-to-azure-classic.md) bereitstellen und ein Failover zu klassischen Azure-Speicher.<br/><br/> Es gibt auch ein legacy-Modell, das bei Neuinstallationen verwendet werden sollte. |  PowerShell wird derzeit nicht unterstützt.
**Hyper-V virtuelle Computer Azure** | Hyper-V VMs Azure-Speicher. VMs können auf Hyper-V-Hosts in System Center VMM-Clouds oder ohne VMM verwaltet werden. | In der Azure kann Portal VMs und Ressourcenmanager Speicher Failover.<br/><br/> Azure-Portal bietet eine optimierte Bereitstellung. [Weitere Informationen](site-recovery-vmm-to-azure.md) zur Replikation von Hyper-V virtuelle Computer in VMM-Clouds. [Weitere Informationen](site-recovery-hyper-v-site-to-azure.md) zur Replikation von Hyper-V VMs (ohne VMM).| Im klassischen Azure-Portal können Sie VMs klassischen Azure-Speicher Failover | PowerShell-Bereitstellung wird unterstützt.
**Physische Server in Azure** | Replizieren Sie Windows-Linux-Servern, Azure-Speicher | In der Azure können Portalservern und Ressourcenmanager Speicher Failover.<br/><br/> Bereitstellung in [Azure-Portal](site-recovery-vmware-to-azure.md) bietet eine optimierte bereitstellungserfahrung und eine Anzahl von Funktionen profitieren. | Im klassischen Azure-Portal können Sie mit [erweiterten Modell](site-recovery-vmware-to-azure-classic.md) bereitstellen und ein Failover zu klassischen Azure-Speicher.<br/><br/> Es gibt auch ein legacy-Modell, das bei Neuinstallationen verwendet werden sollte. | PowerShell-Bereitstellung wird derzeit nicht unterstützt.
**VMware VMs/physische Server auf sekundären Standort* | Replizieren Sie virtuelle VMware-Computer oder Windows-Linux-Servern an einem sekundären Standort. [Weitere Informationen und Downloads](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) InMage Scout-Benutzerhandbuch. | Nicht verfügbar in Azure-portal | Im klassischen Portal werden Sie ein Depot Site Recovery InMage Scout herunterladen. | PowerShell-Bereitstellung wird derzeit nicht unterstützt
**Hyper-V virtuelle Computer einen sekundären Standort** | Hyper-V virtuelle Computer in VMM-Clouds in sekundären Cloud replizieren | In der [Azure-Portal](site-recovery-vmm-to-vmm.md) können Sie Hyper-V virtuelle Computer in VMM-Clouds an einem sekundären Standort nur mit Hyper-V Replica replizieren. SAN wird derzeit nicht unterstützt. | Im klassischen Azure-Portal können Sie Hyper-V virtuelle Computer in VMM-Clouds an einem sekundären Standort mit [Hyper-V Replica](site-recovery-vmm-to-vmm-classic.md) oder [SAN-Replikation](site-recovery-vmm-san.md) replizieren. | PowerShell-Bereitstellung wird unterstützt



## <a name="check-what-you-need-for-deployment"></a>Überprüfen Sie benötigen für die Bereitstellung

### <a name="replicate-to-azure"></a>Azure replizieren

**Anforderung** | **Details** 
---|---
**Ein Azure-Konto** | Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/) -Konto.<br/><br/> Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.
**Azure-Speicher** | Replizierte Daten in Azure-Speicher gespeichert und Azure VMs werden erstellt, wenn ein Failover auftritt. Replikation in Azure benötigen Sie ein [Speicherkonto Azure](../storage/storage-introduction.md).<br/><br/>Wenn Sie das klassische Portal Site Recovery bereitstellen benötigen Sie eine oder mehrere [standard GRS Speicherkonten](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Wenn Sie in Azure-Portal bereitstellen können Sie g oder LRS Speicher.<br/><br/> Wenn Sie virtuelle VMware-Computer oder physische Server in der Azure Portal Premium Replikation wird unterstützt. Beachten Sie, dass bei Verwendung einer Premium-Speicherkonto benötigen Sie außerdem ein Konto Standardspeicher Replikationsprotokollen speichern, die laufenden lokalen Daten erfassen. [Premium-Speicher](../storage/storage-premium-storage.md) wird normalerweise für virtuelle Computer verwendet, die eine hohe e/a-Leistung und niedriger Latenz zur Host-i/o-intensive Arbeitslasten.<br/><br/> Wenn Sie Premium-Konto zum Speichern von replizierter Daten verwenden möchten, benötigen Sie auch, ein Konto Standardspeicher Replikationsprotokollen speichern, die laufenden lokalen Daten erfassen.
**Azure-Netzwerk** | Replikation in Azure benötigen Sie ein Azure-Netzwerk, der Azure-VMs verbunden wird, wenn sie nach einem Failover erstellt werden.<br/><br/> Wenn Sie das klassische Portal bereitstellen verwenden Sie klassische Netzwerk. Wenn Sie in Azure-Portal bereitstellen, können Sie einen klassischen oder Ressourcenmanager Netzwerk.<br/><br/> Das Netzwerk muss in derselben Region als Depot.
**Netzwerkzuordnung (VMM in Azure)** | Wenn Sie von VMM in Azure replizieren, sorgt [Netzwerkübersicht](site-recovery-network-mapping.md) Azure VMs nach einem Failover mit richtigen Netzwerken verbunden sind.<br/><br/> Zum Einrichten der Netzwerkzuordnung müssen Sie VM-Netzwerke in VMM-Portal zu konfigurieren.
**Vor Ort** | **Virtuelle VMware-Computer**: Sie benötigen einen lokalen Computer mit Site Recovery Komponenten, VMware vSphere Hosts-vCenter Server und VMs, die Sie replizieren möchten. [Mehr](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Physische Server**: Wenn Replikation physische Server benötigen Sie einen lokalen Computern Website Komponenten und Servern, die Sie replizieren möchten. [Mehr](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Möchten Sie [ein Failback](site-recovery-failback-azure-to-vmware.md) nach Failover auf Azure benötigen Sie eine VMware-Infrastruktur zu tun.<br/><br/> **Hyper-V VMs**: möchten Sie Hyper-V virtuelle Computer in VMM-Clouds replizieren VMM-Server benötigen und Hyper-V-Hosts auf die VMs schützen soll befinden. [Mehr](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Wenn Sie Hyper-V virtuelle Computer ohne VMM replizieren möchten benötigen Sie Hyper-V-Hosts auf denen virtuellen Computer befinden. [Mehr](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Geschützte Computer** | Geschützte Computer in Azure replizieren müssen [Azure Komponenten](#azure-virtual-machine-requirements) beschrieben.


### <a name="replicate-to-a-secondary-site"></a>Auf einem sekundären Standort replizieren

**Anforderung** | **Details** 
---|---
**Ein Azure-Konto** | Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/) -Konto.<br/><br/> Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.
**Vor Ort** | **Virtuelle VMware-Computer**: am primären Standort benötigen Sie einen Prozess-Server zum Zwischenspeichern, komprimieren und Verschlüsseln von replizierten Daten vor dem Senden an den sekundären Standort. In den sekundären Standort installieren Sie Konfigurationsserver zum Verwalten und Überwachen der Bereitstellung und master Zielserver. Es wird empfohlen, einen vContinuum Server für einfachere Verwaltung. Darüber hinaus benötigen Sie mindestens VMware vSphere Host und optional vCenter Server. [Lesen Sie mehr](site-recovery-vmware-to-vmware.md)<br/><br/> **Hyper-V virtuelle Computer in VMM-Clouds**: Sie müssen VMM Server und virtuelle Computer mit Hyper-V-Hosts, die Sie replizieren möchten. [Mehr](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Netzwerkübersicht (VMM VMM)** | Wenn Sie von VMM VMM replizieren, sorgt [Netzwerkübersicht](site-recovery-network-mapping.md) Replikat VMs mit entsprechenden Netzwerken verbunden sind, nach einem Failover und sind optimal auf Hyper-V-Hosts. Zum Einrichten der Netzwerkzuordnung müssen Sie VM-Netzwerke in VMM-Server konfigurieren.
**Speicher-Zuordnung** | Wenn Sie von VMM VMM replizieren können Sie optional [Speicher Zuordnung](site-recovery-storage-mapping.md) Einrichten der Speicherziel für replizierte VMs an. Speicher-Zuordnung kann Hyper-V Replica und SAN-Replikation eingerichtet werden.<br/><br/> Speicher-Zuordnung wird im Azure-Portal derzeit nicht unterstützt.


## <a name="verify-url-access"></a>Überprüfen Sie den URL-Zugriff

Stellen Sie sicher, dass diese URLs Server zugegriffen werden kann.

**URL** | **VMM VMM** | **VMM in Azure** | **Hyper-V in Azure** | **VMware in Azure**
---|---|---|---|---
*. accesscontrol.windows.net | Zulassen | Zulassen | Zulassen | Zulassen
*. backup.windowsazure.com | Nicht erforderlich | Zulassen | Zulassen | Zulassen
*. hypervrecoverymanager.windowsazure.com | Zulassen | Zulassen | Zulassen | Zulassen
*. store.core.windows.net | Zulassen | Zulassen | Zulassen | Zulassen
*. blob.core.windows.net | Nicht erforderlich | Zulassen | Zulassen | Zulassen
https://www.msftncsi.com/ncsi.txt | Zulassen | Zulassen | Zulassen | Zulassen
https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Nicht erforderlich | Nicht erforderlich | Nicht erforderlich | Zulassen

## <a name="azure-virtual-machine-requirements"></a>Azure Virtual Machine Vorschriften

Sie können Site Recovery zum Replizieren von virtuellen Computern und Servern mit einem beliebigen Betriebssystem unterstützt von Azure bereitstellen. Dazu gehören die meisten Versionen von Windows und Linux. Sie müssen sicherstellen, die lokale virtuelle Computer, die zu schützenden Azure Vorschriften entsprechen.


**Funktion** | **Vorschriften** | **Details**
---|---|---
Hyper-V-host | Windows Server 2012 R2 läuft | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht Betriebssystem unterstütztes
VMware hypervisor  | Unterstütztes Betriebssystem | [Überprüfen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Gast-Betriebssystem | Hyper-V Azure Replikation: Site Recovery unterstützt alle Betriebssysteme, die [von Azure unterstützt](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> VMware und physische Server-Replikation: Überprüfen Sie die Windows- und Linux- [Komponenten](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht unterstützt.
Gast-Architektur | 64-bit | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht unterstützt
Betriebssystem-Datenträgergröße |  1023 GB | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht unterstützt
Zahl der Betriebssystem-Laufwerke | 1 | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht unterstützt.
Anzahl der Datenträger Daten | 16 oder weniger (Maximalwert hängt von der Größe des virtuellen Computers erstellt wird. 16 = XL) | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht unterstützt
Daten VHD Festplattengröße | Maximal 1023 GB | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht unterstützt
Netzwerkadapter | Mehrere Adapter unterstützt |
Statische IP-Adresse | Unterstützt | Wenn der primäre virtuelle Computer eine statische IP-Adresse verwenden können Sie die statische IP-Adresse für den virtuellen Computer angeben, die in Azure erstellt werden. Beachten Sie, dass statische IP-Adresse für einen virtuellen Linux-Maschine auf Hyper-V nicht unterstützt wird.
iSCSI-Datenträger | Nicht unterstützt | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht unterstützt
Freigegebene VHD | Nicht unterstützt | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht unterstützt
Festplatte | Nicht unterstützt | Prüfung der erforderlichen Komponenten schlägt fehl, wenn nicht unterstützt
Festplatte formatieren| VIRTUELLE FESTPLATTE <br/><br/> VHDX | Obwohl VHDX derzeit in Azure unterstützt wird, wandelt Site Recovery VHDX, VHD bei einem in Azure Failover. Wenn Sie nicht zu lokalen weiterhin die virtuellen Computer VHDX-Format.
BitLocker | Nicht unterstützt | BitLocker muss deaktiviert werden, bevor Sie einen virtuellen Computer schützen.
Name des virtuellen Computers| Zwischen 1 und 63 Zeichen. Nur Buchstaben, Zahlen und Bindestriche. Starten und beenden mit einem Buchstaben oder einer Zahl | Aktualisiert die Eigenschaften virtueller Computer Site Recovery
Virtuelle Maschine | <p>Generation 1</p> <p>Generation 2 - Windows</p> |  Generation 2 virtueller Computer mit OS Datenträgertyp Basisfestplatte mit 1 oder 2 Datenmengen mit Festplatte als VHDX ist weniger als 300GB unterstützt wird. Linux Generation 2 virtuelle Computer werden nicht unterstützt. [Weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>Optimierung der Bereitstellung

Verwenden Sie die folgenden Tipps zur Optimierung und Skalierung der Bereitstellung.

- **Betriebssystem-Volume-Größe**: Wenn Sie einen virtuellen Computer in Azure replizieren Betriebssystem muß weniger als 1 TB. Haben Sie mehrere Volumes als können Sie manuell verschieben auf einen anderen Datenträger vor der Bereitstellung.
- **Größe der Festplatte Daten**: Replikation in Azure kann man bis zu 32 Datenträger auf einer virtuellen Maschine mit bis zu 1 TB. Sie können effektiv replizieren und Failover für einen virtuellen Computer ~ 32 TB.
- **Recovery Plan Grenzwerte**: Site Recovery skalierbar auf Tausende von virtuellen Maschinen. Recovery-Pläne dienen als Modell für Programme, die zusammen Failover sollten wir die Anzahl der Computer in einen Wiederherstellungsplan 50 beschränken.
- **Azure Service Grenzwerte**: Jede Azure-Abonnement enthält eine Reihe von Standard-Kontingentgrenzen Kerne, cloud-Dienstleistungen. Wir empfehlen, ein testfailover zum Überprüfen der Verfügbarkeit der Ressourcen in Ihrem Abonnement ausführen. Sie können diese Grenzwerte über Azure-Support.
- **Planung**: für Site Recovery [Planung](site-recovery-capacity-planner.md) lesen.
- **Bandbreite für die Replikation**: Wenn Sie kurz auf die Bandbreite für die Replikation beachten:
    - **ExpressRoute**: Site Recovery arbeitet mit Azure ExpressRoute und WAN-Optimierer wie Flussbett. [Mehr](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) über ExpressRoute.
    - **Replikation**: Site Recovery verwendet eine intelligente Erstreplikation mit nur Datenblöcke und nicht die gesamte virtuelle Festplatte führt. Nur werden während der Replikation repliziert.
    - **Netzwerkverkehr**: Steuern des Netzwerkverkehrs zur Replikation von [Windows-QoS-](https://technet.microsoft.com/library/hh967468.aspx) Richtlinie auf Grundlage der IP-Adresse und Port einrichten.  Ferner Wenn Replikation Azure Site Recovery mit dem Azure Backup-Agent können Sie konfigurieren Drosselung für diesen Agenten. [Mehr](https://support.microsoft.com/kb/3056159).
- **RTO**: Recovery Time Objective (RTO) messen mit Site Recovery erwarten wir empfehlen Sie ein testfailover und Ansicht Website Wiederherstellungsaufträge analysieren wie viel Zeit nimmt, um den Vorgang abzuschließen. Wenn Sie über Azure fehlschlägt, empfiehlt für optimales RTO von Azure Automatisierung und Recovery-Pläne integrieren alle manuelle Aktionen zu automatisieren.
- **RPO**: Site Recovery unterstützt eine bei synchronen Recovery Point Objective (RPO) beim Replizieren auf Azure. Ausreichenden Bandbreite zwischen dem Rechenzentrum und Azure wird vorausgesetzt.


##<a name="service-urls"></a>Dienst-URLs
Sicherstellen Sie, dass diese URLs vom Server zugegriffen werden kann


**URLs** | **VMM VMM** | **VMM in Azure** | **Hyper-V-Website in Azure** | **VMware in Azure**
---|---|---|---|---
 \*. accesscontrol.windows.net | Erforderlich  | Erforderlich  | Erforderlich  | Erforderlich
 \*. backup.windowsazure.com |  | Erforderlich  | Erforderlich  | Erforderlich
 \*. hypervrecoverymanager.windowsazure.com | Erforderlich  | Erforderlich  | Erforderlich  | Erforderlich
 \*. store.core.windows.net | Erforderlich  | Erforderlich  | Erforderlich  | Erforderlich
 \*. blob.core.windows.net |  | Erforderlich  | Erforderlich  | Erforderlich
 https://www.msftncsi.com/ncsi.txt | Erforderlich  | Erforderlich  | Erforderlich  | Erforderlich
 https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Erforderlich


## <a name="next-steps"></a>Nächste Schritte

Nachdem und allgemeine Bereitstellung vergleichen Sie detaillierten Komponenten lesen und Bereitstellen jedes Szenario.

- [Virtuelle VMware-Maschinen in Azure replizieren](site-recovery-vmware-to-azure-classic.md)
- [Physische Server in Azure replizieren](site-recovery-vmware-to-azure-classic.md)
- [Replizieren Sie Hyper-V Server in VMM-Clouds in Azure](site-recovery-vmm-to-azure.md)
- [Replizieren Sie Hyper-V virtuelle Computer (ohne VMM) in Azure](site-recovery-hyper-v-site-to-azure.md)
- [Hyper-V VMs auf einem sekundären Standort replizieren](site-recovery-vmm-to-vmm.md)
- [Hyper-V VMs mit SAN an einem sekundären Standort replizieren](site-recovery-vmm-san.md)
- [Hyper-V virtuelle Computer mit einem einzigen VMM-Server replizieren](site-recovery-single-vmm.md)
