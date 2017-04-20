<properties
    pageTitle="Zuordnen von Speicher in Azure Site Recovery für Hyper-V-VM Replikation zwischen lokalen Rechenzentren | Microsoft Azure"
    description="Zuordnung von Storage für Hyper-V-VM Replikation zwischen zwei lokalen Rechenzentren mit Azure Site Recovery vorzubereiten."
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
    ms.date="07/06/2016"
    ms.author="raynew"/>


# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Zuordnung von Storage für Hyper-V-VM Replikation zwischen zwei lokalen Rechenzentren mit Azure Site Recovery vorbereiten


Azure Site Recovery trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern und Servern orchestriert. Dieser Artikel beschreibt Speicher zuordnen, welche hilft Ihnen, optimalen Speicher verwenden, wenn Sie Hyper-V virtuelle Computer zwischen zwei lokalen VMM Rechenzentren repliziert Site Recovery verwenden.

Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Übersicht

Speicher-Zuordnung ist nur relevant, wenn Hyper-V virtuelle Computer repliziert sind, die in VMM-Clouds aus einem primären Rechenzentrum in einem sekundären Datencenter mit Hyper-V Replica oder SAN-Replikation wie folgt gespeichert:


- **Lokal lokale Replikation mit Hyper-V Replica)** – Sie Speicher Zuordnung durch Zuordnung Speicher Klassifikationen auf Quelle und Ziel VMM-Server Folgendes:

    - **Identifizieren Zielspeicher für Replikat virtuelle Computer**, virtuelle Computer auf einem Speicher (SMB freigeben oder cluster freigegebenen Volumes (CSVs)) repliziert, die Sie auswählen.
    - **Platzierung virtueller Maschinen Replikat**– Speicher-Zuordnung verwendet, um optimal Replikat virtuelle Computer in Hyper-V-Host-Servern. Replikat virtuelle Computer wird auf Hosts platziert werden, die die speicherklassifizierung zugeordneten zugreifen können.
    - **Keine Speicher-Zuordnung**, wenn Sie Speicher-Zuordnung nicht konfigurieren, virtuelle Computer auf den Standardspeicherort zu Hyper-V-Hostserver zugeordnete VM Replikat angegeben repliziert.

- **Räume für die lokale Replikation mit SAN**– Sie Speicher Zuordnung durch Zuordnung Speicherpools Arrays auf Quelle und Ziel VMM-Server.
    - **Pool angeben**, gibt die sekundären Speicherpool primären Pool replizierten Daten empfängt.
    - **Ziel-Speicher-Pools identifizieren**– stellt sicher, dass die LUNs in einer Replikationsgruppe Quelle zugeordneten Zielpool Ihrer Wahl repliziert werden.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Einrichten von Branchen Storage für Hyper-V-Replikation

Wenn Sie Hyper-V Replica verwenden mit Standort repliziert, ordnen Sie zwischen Storage Klassifikationen Quell- und VMM-Server und einem einzelnen VMM-Server Standorte von VMM-Server verwaltet werden. Beachten Sie Folgendes:

- Zuordnung ordnungsgemäß konfiguriert ist und die Replikation wird Speicher zugeordnete Zielspeicherort virtuelle Festplatte des virtuellen Computers am primären Standort repliziert.
- Storage Klassifikationen müssen der Gruppen befindet sich im Quell- und Wolken verfügbar sein.
- Klassifikationen müssen dieselbe Art von Speicher haben. Beispielsweise können Sie eine Klassifikation Quelle zuordnen mit SMB-Freigaben für eine zielklassifizierung, die CSVs enthält.
- Mehr [Speicher Klassifikationen in VMM erstellen](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Beispiel

Klassifikationen bei Auswahl von Quelle und Ziel VMM-Server bei der Zuordnung von Speicher ordnungsgemäß in VMM konfiguriert, werden die Quell- und Klassifikationen angezeigt. Hier ist ein Beispiel der Storage-Dateifreigaben und Klassifikationen für eine Organisation mit zwei Standorten in New York und Chicago.

**Speicherort** | **VMM-server** | **Dateifreigabe (Quelle)** | **Klassifizierung (Quelle)** | **Zugeordnet zu** | **Dateifreigabe (Ziel)**
---|---|--- |---|---|---
München | VMM_Source| SourceShare1 | GOLD | GOLD_TARGET | TargetShare1
 |  | SourceShare2 | SILBER | SILVER_TARGET | TargetShare2
 | | SourceShare3 | BRONZE | BRONZE_TARGET | TargetShare3
Chicago | VMM_Target |  | GOLD_TARGET | Nicht zugeordnet |
| | | SILVER_TARGET | Nicht zugeordnet |
 | | | BRONZE_TARGET | Nicht zugeordnet

Sie würden auf die Registerkarte **Serverspeicher** in der Site Recovery-Portal-Seite **Ressourcen** konfigurieren.

![Konfigurieren von Speicher-Zuordnung](./media/site-recovery-storage-mapping/storage-mapping1.png)

Mit diesem Beispiel:
- Wenn ein GOLD_TARGET Speicher (TargetShare1) repliziert, eine Replikat VM virtueller Maschinen auf GOLD-Speicher (SourceShare1) erstellt.
- Beim Erstellen eine VM Replikat virtueller Maschinen auf Silber-Speicher (SourceShare2) wird in einem SILVER_TARGET (TargetShare2) Speicher repliziert werden und so weiter.

Im nächsten Screenshot der tatsächlichen Dateifreigaben und ihre zugeordneten Klassifikationen in VMM angezeigt.

![Speicher-Klassifikationen in VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Mehrere Speicherorte

Wenn die gewünschte Klassifizierung mehrere SMB-Freigaben oder CSVs zugewiesen ist, wird der optimalen Speicherort, wenn der virtuelle Computer geschützt ist automatisch ausgewählt. Ist kein passendes Suchziel Speicher mit der angegebenen Klassifizierung ist der Standardspeicherort für Hyper-V-Hosts angegeben verwendet, virtuellen Festplatten Replikat platzieren.

Die folgende Tabelle zeigt, wie speicherklassifizierung und freigegebene Clustervolumes in unserem Beispiel eingerichtet werden.

**Speicherort** | **Klassifizierung** | **Zugehörige Speicher**
---|---|---
München | GOLD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p>
 | SILBER | <p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p>
Chicago | GOLD_TARGET | <p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p>
 | SILVER_TARGET| <p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p>

In dieser Tabelle wird das Verhalten zusammengefasst, wenn Sie Schutz für virtuelle Maschinen (VM1 - VM5) in dieser beispielumgebung aktivieren.

**Virtual machine** | **Ursprünglicher Speicher** | **Quelle-Klassifizierung** | **Zugeordnetes Zielspeicher**
---|---|---|---
VM1 | C:\ClusterStorage\SourceVolume1 | GOLD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Beide GOLD_TARGET</p>
VM2 | \\FileServer\SourceShare1 | GOLD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Beide GOLD_TARGET</p>
VM3 | C:\ClusterStorage\SourceVolume2 | SILBER | <p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p>
VM4 | \FileServer\SourceShare2 | SILBER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Beide SILVER_TARGET</p>
VM5 | C:\ClusterStorage\SourceVolume3 | N/A | Keine Zuordnung verwendet der Standardspeicherort des Hyper-V-Hosts

## <a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie ein besseres Verständnis der Speicher-Zuordnung [erhalten Azure Site Recovery bereitstellen](site-recovery-best-practices.md).
