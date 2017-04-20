<properties
    pageTitle="Vorbereiten der Netzwerkübersicht für Hyper-V virtuelle Computer Schutz mit VMM in Azure Site Recovery | Microsoft Azure"
    description="Netzwerkzuordnung für Hyper-V-VM Replikation aus einem lokalen Rechenzentrum in Azure oder an einem sekundären Standort einrichten."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Netzwerkzuordnung für Hyper-V virtuelle Computer Schutz mit VMM in Azure Site Recovery vorbereiten

Azure Site Recovery trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern und Servern orchestriert.

Dieser Artikel beschreibt die Netzwerkübersicht in VMM-Clouds zwischen zwei lokalen Datencenter oder zwischen einer lokalen Datencenter und Azure geführtes Einstellungen optimal konfigurieren, wenn Sie Hyper-V virtuelle Computer replizieren Site Recovery verwenden können. Beachten Sie, dass Hyper-V-VMs sind ohne eine VMM-Cloud repliziert oder virtuelle VMware-Computer oder physischen Servern replizieren, dieser Artikel ist nicht relevant.

Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Übersicht

Netzwerkzuordnung wird verwendet, wenn Hyper-V virtuelle Computer in Azure oder sekundären Rechenzentrum replizieren Hyper-V Replica oder SAN-Replikation Azure Site Recovery bereitgestellt wird.

- **Replizieren von Hyper-V virtuelle Computer in VMM-Clouds zwischen zwei lokalen Rechenzentren**– Netzwerk-Zuordnung Karten zwischen VM auf einem Quellserver VMM und VM-Netzwerken auf einem Zielserver VMM Folgendes:

    - **Verbinden virtuelle Computer nach dem Failover**– stellt sicher, dass die virtuellen Computer nach einem Failover mit entsprechenden Netzwerken verbunden wird. Replikat virtuelle Computer wird zum Zielnetzwerk verbunden werden, die das Quellnetzwerk zugeordnet ist.
    - **Direkte Replikat virtuelle Computer auf Host-Server**-Replikat virtuelle Computer optimal auf Hyper-V-Host-Server platzieren. Replikat virtuelle Computer wird auf Hosts platziert werden, die die zugeordneten VM-Netzwerke zugreifen können.
    - **Keine Netzwerkzuordnung**– Wenn Sie Netzwerkzuordnung nicht konfigurieren, wird nicht replizierten virtuellen Maschinen VM Netzwerken verbunden sein, nach einem Failover.

- **Replizieren von Hyper-V virtuelle Computer in einem lokalen VMM in Azure cloud**– Netzwerk-Zuordnung Karten zwischen VM auf dem Quellserver VMM und Azure Netzwerke, um Folgendes Ziel:
    - **Verbinden der virtuellen Computer nach einem Failover**– alle Computer der Failover im gleichen Netzwerk können, unabhängig davon, welche Wiederherstellungsplan in sind.
    - **Netzwerk-Gateway**– Wenn ein Netzwerk-Gateway auf dem Zielcomputer Azure-Netzwerk eingerichtet ist, können virtuelle Computer zu anderen lokalen virtuellen Maschinen.
    - **Keine Netzwerkzuordnung**– Wenn Sie Netzwerk-Mapping konfigurieren, nur virtuelle Computer, Failover in derselben Wiederherstellungsplan werden nach Fehler in Azure miteinander.


## <a name="network-mapping-example"></a>Beispiel-Zuordnung

Netzwerk-Mapping kann konfiguriert werden, zwischen VM zwei VMM-Server und einen einzelnen VMM Server Standorte von demselben Server verwaltet werden. Zuordnung ordnungsgemäß konfiguriert ist, die Replikation am primären Standort ein virtuellen Computer mit einem Netzwerk verbunden, und sein Replikat am Zielspeicherort zugeordneten Netzwerk verbunden.

Wenn Netzwerke ordnungsgemäß in VMM eingerichteten bei Auswahl einer VM Zielnetzwerk während Netzwerkübersicht, werden VMM Quelle Wolken, mit dem das Quellnetzwerk VM, mit verfügbaren VM Zielnetzwerke für Wolken Ziel angezeigt, die für den Schutz verwendet werden.

Hier ist ein Beispiel zur Veranschaulichung dieser Mechanismus. Nehmen Sie eine Organisation mit zwei Standorten in New York und Chicago.

**Speicherort** | **VMM-server** | **VM-Netzwerke** | **Zugeordnet zu**
---|---|---|---
München | VMM NewYork| VMNetwork1 New York | VMNetwork1 Chicago zugeordnet
 |  | VMNetwork2 New York | Nicht zugeordnet
Chicago | VMM Chicago| VMNetwork1-Chicago | VMNetwork1 NewYork zugeordnet
 | | VMNetwork2-Chicago | Nicht zugeordnet

Mit diesem Beispiel:

- Beim Erstellen eine VM Replikat für virtuelle Computer, die mit VMNetwork1 NewYork verbunden wird er mit VMNetwork1 Chicago verbunden.
- Wenn eine Replikat VM VMNetwork2 NewYork oder VMNetwork2 Chicago erstellt wird, wird er nicht mit einem Netzwerk verbunden.

Hier ist wie VMM-Clouds in unserer Beispielorganisation und logische Netzwerke Wolken zugeordnet sind.

### <a name="cloud-protection-settings"></a>Cloudeinstellungen

**Geschützte cloud** | **Schutz der cloud** | **Logisches Netzwerk (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 New York</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 New York</p><p>LogicalNetwork1-Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Logische und VM Network settings

**Speicherort** | **Logisches Netzwerk** | **Zugeordnete VM-Netzwerk**
---|---|---
München | LogicalNetwork1 New York | VMNetwork1 New York
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

### <a name="target-networks"></a>Zielnetzwerke

Anhand dieser Einstellung wählen Sie Zielnetzwerk VM, zeigt die folgende Tabelle die Auswahlmöglichkeiten zur Verfügung stehen.

**Wählen Sie** | **Geschützte cloud** | **Schutz der cloud** | **Netz verfügbar**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Verfügbar
 | GoldCloud1 | GoldCloud2 | Verfügbar
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Nicht verfügbar
 | GoldCloud1 | GoldCloud2 | Verfügbar



## <a name="multiple-subnets"></a>Mehrere Subnetze

Wenn Zielnetzwerk mehrere Subnetze und von diesen Subnetzen hat den gleichen Namen wie das Subnetz auf dem virtuellen Quellcomputers befindet, wird dann VM Replikat an Ziel Subnetz nach einem Failover erfolgen. Ist kein Ziel Subnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.


### <a name="failback"></a>Failback

Um was geschieht bei Failback (reverse Replikation), angenommen, VMNetwork1 New York VMNetwork1 Chicago folgendermaßen zugeordnet ist.


**Virtual machine** | **VM-Netzwerk verbunden**
---|---
VM1 | VMNetwork1 Netzwerk
VM2 (Replikat VM1) | VMNetwork1-Chicago

Mit dieser Einstellung was passiert in einigen Szenarien.

**Szenario** | **Ergebnis**
---|---
Keine Änderung in den Eigenschaften von VM-2 nach dem Failover. | VM-1 bleibt das Quellnetzwerk an.
Netzwerkeigenschaften des VM-2 nach dem Failover geändert und wird getrennt. | VM-1 besteht.
Netzwerkeigenschaften des VM-2 nach dem Failover geändert und mit VMNetwork2 Chicago. | Wenn VMNetwork2 Chicago zugeordnet ist, werden WM 1 getrennt.
Netzwerkzuordnung von Chicago VMNetwork1 wird geändert. | VM-1 wird jetzt zugeordnet VMNetwork1 Chicago Netzwerk verbunden.


## <a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie ein besseres Verständnis der Netzwerkübersicht [mit Site Recovery-Bereitstellung beginnen](site-recovery-best-practices.md).
