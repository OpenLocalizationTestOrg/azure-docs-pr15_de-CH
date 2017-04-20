<properties
    pageTitle="Virtuelle VMware-Maschinen und physische Server in Azure mit Azure Site Recovery (Legacy) replizieren | Microsoft Azure" 
    description="Beschreibt, wie VMs für lokale Replikation und Windows-Linux-Servern in Azure Azure Site Recovery einer älteren Bereitstellung im klassischen Portal." 
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
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>Azure mit Azure Site Recovery Verwaltungsportal (Legacy) mit replizieren Sie virtuellen Maschinen von VMware und Servern

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmware-to-azure.md)
- [Verwaltungsportal](site-recovery-vmware-to-azure-classic.md)
- [Verwaltungsportal (Legacy)](site-recovery-vmware-to-azure-classic-legacy.md)


Willkommen bei Azure Site Recovery! Dieser Artikel beschreibt eine ältere Bereitstellung auf Azure Azure Site Recovery im klassischen Portal mit lokalen virtuellen VMware Maschinen oder Windows/Linux Servern repliziert.

## <a name="overview"></a>Übersicht

Organisationen benötigen eine BCDR Strategie, die bestimmt, wie apps Arbeitslasten und Daten bleiben aktiv bei geplanten und ungeplanten Ausfällen normalen Arbeitsbedingungen so bald wie möglich wiederherstellen. Die BCDR Strategie sollten schützen Sie Daten wiederhergestellt und Arbeitslasten ständig verfügbar bleiben beim Notfall.

Site Recovery ist eine Azure Service BCDR Strategie umzusetzen Replikation von lokalen Servern und virtuellen Computer in die Cloud (Azure) oder zu einem sekundären Datencenter beiträgt. Treten Ausfälle in Ihrem primären Standort Failover Sie sekundäre an apps und Arbeitslasten zur Verfügung. Sie nicht an Ihrem primären Standort zu beenden. Weitere Informationen [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md)


>[AZURE.WARNING] Dieser Artikel enthält **ältere Informationen**. Verwenden Sie nicht für neue Installationen. Stattdessen [gehen](site-recovery-vmware-to-azure.md) Site Recovery in Azure-Portal oder verbesserte Bereitstellung im klassischen Portal konfigurieren [lernen](site-recovery-vmware-to-azure-classic.md) bereitstellen. Bereits bereitgestellt in diesem Artikel beschriebene Methode sollten die verbesserte Bereitstellung im klassischen Portal migrieren.


## <a name="migrate-to-the-enhanced-deployment"></a>Verbesserte Bereitstellung migrieren

Dieser Abschnitt ist nur relevant, wenn Sie Site Recovery mit der in diesem Artikel bereits bereitgestellt haben.

Die vorhandene Bereitstellung migrieren zu müssen:

1. Bereitstellen Sie neue Website Komponenten in Ihrer lokalen Website.
2. Konfigurieren Sie Anmeldeinformationen für die automatische Erkennung der VMware-VMs auf dem neuen Konfigurationsserver.
3. Erkennen Sie VMware Server mit der neuen Konfigurationsserver.
3. Erstellen einer neuen Schutzgruppe mit neuen Konfigurationsserver.


Bevor Sie beginnen:

- Wir empfehlen ein Wartungsfenster für die Migration einzurichten.
- **Computer migrieren** -Option ist nur verfügbar, wenn Sie vorhandene Gruppen verfügen, die während einer älteren Bereitstellung erstellt wurden.
- Nach Abschluss der Schritte der Migration dauert es 15 Minuten oder länger die Anmeldeinformationen aktualisieren zu ermitteln und virtuelle Computer aktualisieren, sodass Sie sie einer Schutzgruppe hinzufügen können. Sie können statt manuell aktualisieren. 

Migrieren Sie folgendermaßen:

1. Informationen Sie zu [erweiterten Bereitstellung im klassischen Portal](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Überprüfen Sie verbesserte [Architektur](site-recovery-vmware-to-azure-classic.md#scenario-architecture)und [Komponenten](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
2. Deinstallieren Sie den Dienst Mobilität von Computern, die Sie derzeit Replikation. Eine neue Version des Dienstes wird auf den Computern installiert werden, beim Hinzufügen der neuen Schutzgruppe hinzu.
3. Herunterladen Sie ein [Depot Registrierungsschlüssel](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) und Konfigurationsserver, Prozessserver und master-Ziel-Serverkomponenten installieren [Führen Sie den Assistenten einheitliche](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) Weitere Informationen zur [Planung](site-recovery-vmware-to-azure-classic.md#capacity-planning).
4. [Einrichten von Anmeldeinformationen](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) , Site Recovery können auf VMware Server virtuelle VMware-Computer automatisch zu erkennen. Enthält Informationen Sie zu den [erforderlichen Berechtigungen](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. [VCenter Server oder vSphere Hosts](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts)hinzufügen. Es dauert 15 Minuten für Server in der Site Recovery-Portal.
6. Erstellen einer [neuen Schutzgruppe](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group). Es dauert bis zu 15 Minuten für das Portal aktualisieren, damit die virtuellen Computer erkannt werden und angezeigt. Möchten Sie nicht warten können, markieren Sie den Management Servernamen (nicht klicken) > **Aktualisieren**.
7. Klicken Sie unter neue Schutzgruppe **Migrieren**.

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)

8. **Wählen Sie** Computer wählen Sie die Schutzgruppe zu migrieren und die Sie migrieren möchten aus

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)

9. **Konfigurieren Ziel** angegeben Sie, ob Sie verwenden dieselbe Einstellung für alle Computer und Prozess-Server und Azure Speicherkonto auswählen möchten. Haben Sie einen separaten Prozess-Server werden die IP-Adresse des Konfigurationsserver.


    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migration von Speicherkonten](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist für die Bereitstellung von Site Recovery Storage-Konten nicht unterstützt.

10. **Konten angeben**wählen Sie das Konto für den Prozess-Server auf der Maschine drücken Sie die neue Version des Dienstes Mobilität erstellt.

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)

11. Site Recovery migrieren die replizierten Daten Konto Azure-Speicher, die Sie bereitgestellt. Optional können Sie das Speicherkonto wiederverwenden legacy-Bereitstellung verwendet.
12. Nachdem der Auftrag beendet werden virtuelle Computer automatisch synchronisiert. Nach Abschluss der Synchronisierung können Sie die virtuellen Computer aus älteren Schutzgruppe löschen.
13. Nachdem alle Computer migriert haben können Sie ältere Schutzgruppe löschen.
14. Beachten Sie die Failover-Eigenschaften für Computer und Azure Network Settings angeben, nach Abschluss der Synchronisierung.
15. Haben Sie vorhandene Recovery-Pläne, können Sie diese verbesserte Bereitstellung mit **Wiederherstellungsplan migrieren** migrieren. Sie sollten dies nur tun, wenn alle geschützten Computer migriert wurden. 

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

>[AZURE.NOTE] Nach Abschluss der Migration haben [Erweiterte Artikel](site-recovery-vmware-to-azure-classic.md)fort. Diese älteren Artikel mehr werden und nicht müssen Sie weitere Schritte in It **.




## <a name="what-do-i-need"></a>Was benötige ich?

Dieses Diagramm zeigt die bereitstellungskomponenten.

![Neues Depot](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Hier ist, was Sie benötigen:

**Komponente** | **Bereitstellung** | **Details**
--- | --- | ---
**Konfigurationsserver** | Ein Azure standard A3 virtuellen Computer in der gleichen Anmeldung als Site Recovery. | Der Konfigurationsserver koordiniert Kommunikation zwischen geschützten Computer, Prozess-Server und master Zielserver in Azure. Bei einem Failover festgelegt von Replikation und Recovery in Azure Koordinaten.
**Master-Ziel-server** | Ein Azure Virtual Machine – entweder ein Windows-Server auf Windows Server 2012 R2 Bild (zu Windows-Computer) oder als LinuxServer basierend auf OpenLogic CentOS 6.6 Bild (zum Schutz von Linux-Computern).<br/><br/> Drei Größen stehen – Standard A4 und Standard D14 Standard DS4.<br/><br/> Der Server ist mit demselben Azure Netzwerk wie der Konfigurationsserver verbunden.<br/><br/> Sie richten im Site Recovery-portal | Empfängt und speichert replizierte Daten aus Ihrem geschützten Computer angeschlossene Festplatten auf BLOB-Speicher in Azure Storage-Konto erstellt.<br/><br/> Wählen Sie Standard DS4 speziell für Schutz für Arbeitslasten benötigen gleichbleibend hohe Leistung und niedrige Latenz mit Premium-Speicherkonto konfigurieren.
**Prozessserver** | Einen lokalen virtuellen oder physischen Server unter Windows Server 2012 R2<br/><br/> Wir empfehlen befindet sich im gleichen Netzwerk und LAN-Segment wie die zu schützenden Computer, ausführen kann in einem anderen Netzwerk als geschützte Computer L3-netzwerksichtbarkeit zu haben.<br/><br/> Sie einrichten und dem Konfigurationsserver Site Recovery-Portal registrieren. | Geschützte Computer Replikationsdaten an lokalen Prozess-Server gesendet. Es wurde ein zwischenzuspeichern bandgestützte Daten. Es führt eine Reihe von Aktionen auf die Daten.<br/><br/> Durch Zwischenspeichern, komprimieren und vor dem Senden an den master Zielserver verschlüsseln optimiert Daten.<br/><br/> Push-Installation des Mobility-Dienst verarbeitet.<br/><br/> Automatische Erkennung der virtuellen VMware Maschinen ausgeführt.
**Lokalen Computer** | Lokale virtuelle VMware-Maschinen oder Servern unter Windows oder Linux. | Sie konfigurieren Replikation, die einen oder mehrere Computer anwenden. Sie können über einen einzelnen Computer oder in mehreren Computern fehl, die zusammen in einem Wiederherstellungsplan sammeln. 
**Mobilitätsservice** | Installiert auf jedem virtuellen oder physischen Server, die, den Sie schützen möchten<br/><br/> Manuell installiert oder abgelegt und installiert automatisch den Prozess-Server bei der Replikation für einen Computer aktivieren. | Mobility-Dienst sendet Daten an den Prozess-Server während der ersten Replikation (RE). Nachdem der Computer geschützt ist (nach Abschluss Resync) mobilitätsservice erfasst Schreibvorgänge auf Datenträger im Arbeitsspeicher und an den Prozess-Server gesendet werden. Applicationconsistency für Windows-Server erfolgt über VSS
**Azure Site Recovery vault** | Sie ein Depot Site Recovery mit Azure-Abonnement erstellen und Registrieren von Servern im Tresor. | Das Depot koordiniert und koordiniert die Datenreplikation, Failover und Recovery zwischen dem lokalen Standort und Azure.
**Replikation** | **Über das Internet**– kommuniziert und repliziert Daten von geschützten lokalen Server in Azure mit sicheren SSL/TLS-Kanal über das Internet. Dies ist die Standardoption.<br/><br/> **VPN-ExpressRoute**– kommuniziert und repliziert Daten zwischen lokalen Servern und Azure über eine VPN-Verbindung. Sie müssen ein Standort-zu-Standort-VPN oder eine ExpressRoute-Verbindung zwischen dem lokalen Standort und Azure-Netzwerk einrichten.<br/><br/> Sie wählen wie Sie während der Bereitstellung der Site Recovery replizieren möchten. Nach Konfiguration ohne Beeinträchtigung der Replikation von vorhandenen Computern können Sie das Verfahren ändern. | Keine Option erfordert eingehende Netzwerkports auf geschützten Computern öffnen. Gesamte Netzwerkkommunikation wird von der lokalen Website initiiert. 

## <a name="capacity-planning"></a>Planen der Kapazität

Die wichtigsten Bereiche berücksichtigen müssen:

- **Quell-Umgebung**– das VMware Infrastruktur quelleinstellungen und Vorschriften.
- **Komponentenserver**-Process-Server-, Konfigurationsserver und master Zielserver 

### <a name="considerations-for-the-source-environment"></a>Aspekte der Quell-Umgebung

- **Maximalgröße des Datenträger**-die aktuelle maximale Größe des Laufwerks zu einer virtuellen Maschine zugeordnet werden kann, ist 1 TB. Daher beträgt maximal einen Quelldatenträger repliziert werden kann auch 1 TB.
- **Maximalgröße pro Quelle**– die maximale Größe der einzelnen Quellcomputer ist 31 TB (mit 31 Festplatten) und mit einer D14 für master Zielserver bereitgestellt. 
- **Anzahl der Quellen pro master Zielserver**– mehrere Quell-Computer mit einem einzelnen master Zielserver geschützt werden können. Jedoch kann nicht einzelnen Quellcomputer über mehrere master Zielserver geschützt, da Festplatten repliziert eine virtuelle Festplatte, die die Größe des Laufwerks widerspiegelt Azure BLOB-Speicher erstellt wird und als Datenträger master Zielserver zugeordnet werden.  
- **Maximale tägliche Änderungsrate pro Quelle**– es sind drei Faktoren zu berücksichtigen, wenn die empfohlene Änderung pro Quelle. Zwei IOPS müssen Gründen basiert auf dem Zieldatenträger für jeden Vorgang auf der Quelle. Dies ist beim Lesen von Daten und Schreiben neuer Daten auf dem Zieldatenträger geschieht. 
    - **Tägliche Änderungsrate von Prozess-Server unterstützt**– ein Quellcomputer kann nicht mehrere Prozessserver umfassen. Ein einzelner Prozessserver unterstützt bis zu 1 TB tägliche Änderungsrate. 1 TB ist daher die maximalen täglichen Daten für ein Quellcomputer unterstützt ändern. 
    - **Maximale Durchsatz durch die Zieldiskette unterstützt**– maximale Änderung pro Quell-Laufwerk nicht mehr als 144 GB pro Tag (mit 8 KByte schreiben). Siehe die Tabelle im Zielabschnitt master Durchsatz und IOPs des Ziels für verschiedene Größen schreiben. Diese Nummer muss durch zwei geteilt werden, da jede Quelle IOP 2 IOPS auf dem Zieldatenträger generiert. Informationen Sie zu [Azure Skalierbarkeits- und Performance-Ziele](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) beim Ziel für Premium-Speicherkonten konfigurieren.
    - **Das Speicherkonto unterstützte maximale Durchsatz**-Quelle kann nicht mehrere Speicherkonten umfassen. Da ein Speicherkonto bis zu 20.000 Anfragen pro Sekunde dauert und jede Quelle IOP 2 IOPS auf dem master Zielserver generiert, empfehlen wir, die Anzahl von IOPS über die Quelle 10.000. Informationen Sie zu [Azure Skalierbarkeits- und Performance-Ziele](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) beim Quelle für Premium-Speicherkonten konfigurieren.

### <a name="considerations-for-component-servers"></a>Aspekte der Komponentenserver

In Tabelle 1 sind die virtuellen Computer Größen für die Konfiguration und master Zielserver.

**Komponente** | **Bereitgestellte Azure Instanzen** | **Kerne** | **Speicher** | **Max. Laufwerke** | **Festplattengröße**
--- | --- | --- | --- | --- | ---
Konfigurationsserver | Standard-A3 | 4 | 7 GB | 8 | 1023 GB
Master-Ziel-server | Standard A4 | 8 | 14 GB | 16 | 1023 GB
 | Standard D14 | 16 | 112 GB | 32 | 1023 GB
 | Standardmäßige DS4 | 8 | 28 GB | 16 | 1023 GB

**Tabelle 1**

#### <a name="process-server-considerations"></a>Prozessserver Aspekte

Im Allgemeinen hängt Prozess Größe tägliche Änderungsrate für alle geschützten Arbeitslasten.


- Sie benötigen ausreichend Compute Aufgaben wie Inline-Komprimierung und Verschlüsselung.
- Prozess-Server verwendet datenträgerbasierten Caches. Empfohlene Cachespeicher sicher und Datenträgerdurchsatz steht bei einem Ausfall des Netzwerks auf Engpässe gespeicherten Daten ändert sich zu erleichtern. 
- Daß ausreichenden Bandbreite so der Prozess-Server die Daten auf der master Zielserver kontinuierlichen Datenschutz bieten hochladen kann. 

Tabelle 2 enthält eine Zusammenfassung der Richtlinien Server.

**Datenänderungsrate** | **CPU** | **Speicher** | **Datenträger-Cachegröße**| **Cache Datenträgerdurchsatz** | **Bandbreite Eindringen/Ausgang**
--- | --- | --- | --- | --- | ---
< 300 GB | 4 vCPUs (2 Steckplätze * 2 Kernen @ 2,5 GHz) | 4 GB | 600 GB | 7 bis 10 MB pro Sekunde | 30 Mbit/s-21 Mbit/s
300 bis 600 GB | 8 vCPUs (2 Steckplätze * 4 Kernen @ 2,5 GHz) | 6 GB | 600 GB | 11 bis 15 MB pro Sekunde | 60 MB/s 42 Mbit/s
600 GB bis 1 TB | 12 vCPUs (2 Steckplätze * 6 Kerne @ 2,5 GHz) | 8 GB | 600 GB | 16 bis 20 MB pro Sekunde | 100 Mbit/s/70 Mbit/s
> 1 TB | Bereitstellen von einem anderen Prozessserver | | | | 

**Tabelle 2**

Wo: 

- Eindringen ist Download Bandbreite (Intranet zwischen der Quelle und Prozess-Server).
- Ausgang ist Bandbreite (Internet zwischen Prozessserver und master Zielserver). Ausgang Zahlen nehmen durchschnittlich 30 % Prozess Server Komprimierung an.
- Diskette eine separate OS mindestens 128 GB wird Cache für alle Prozessserver empfohlen.
- Für Cache Datenträgerdurchsatz folgende Speicher Benchmark verwendet wurde: 8 SAS-Festplatten 10 Min. mit RAID 10-Konfiguration.

#### <a name="configuration-server-considerations"></a>Verwendung von Konfiguration

Jede Konfigurationsserver unterstützt bis zu 100 Quell Computern mit 3 bis 4. Wenn die Bereitstellung mehr empfehlen wir eine andere Konfigurationsserver bereitstellen. Die Standardeigenschaften virtuellen Server Konfiguration finden Sie in Tabelle 1. 

#### <a name="master-target-server-and-storage-account-considerations"></a>Master Target Server- und Aspekte

Speicher für jeden master Zielserver enthält eine Betriebssystemdatenträger, eine Aufbewahrung Volumen und Datenträger. Aufbewahrungslaufwerk verwaltet die Erfassung der Daten Träger für die Dauer der Beibehaltungsdauer Site Recovery-Portal definiert.  Die virtuellen Eigenschaften des Zielservers master finden Sie in Tabelle 1. Tabelle 3 zeigt, wie die Laufwerke a4 verwendet werden.

**Instanz** | **Betriebssystem-Datenträger** | **Aufbewahrung** | **Datenträger**
--- | --- | --- | ---
 | | **Aufbewahrung** | **Datenträger**
Standard A4 | 1 Datenträger (1 * 1023 GB) | 1 Datenträger (1 * 1023 GB) | 15 Festplatten (15 * 1023 GB)
Standard D14 |  1 Datenträger (1 * 1023 GB) | 1 Datenträger (1 * 1023 GB) | 31 Festplatten (15 * 1023 GB)
Standardmäßige DS4 |  1 Datenträger (1 * 1023 GB) | 1 Datenträger (1 * 1023 GB) | 15 Festplatten (15 * 1023 GB)

**Tabelle 3**

Planung für den master Zielserver hängt:

- Azure-Speicher-Performance und Grenzen
    - Die maximale Anzahl von sehr Datenträger für Standard-Tier-VM verwendet ist 40 (20.000-500 IOPS pro Datenträger) in ein einzelnes Speicherkonto. Lesen Sie [Skalierbarkeitsziele für Standardspeicher Sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-standard-storage-accounts) und [Premium Speicher Sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts).
-   Tägliche Änderungsrate 
-   Aufbewahrung Volume speichern.

Beachten Sie Folgendes:

- Eine Quelle kann nicht mehrere Speicherkonten umfassen. Dies gilt für den Datenträger zu Speicherkonten ausgewählt, wenn Sie den Schutz konfigurieren. Das Betriebssystem und die Aufbewahrung Festplatten werden normalerweise automatisch bereitgestellte Speicherkonto.
- Die Aufbewahrung Speicher erforderlich hängt die tägliche Änderungsrate und die Anzahl der Aufbewahrungstage. Aufbewahrung Speicher pro master Zielserver Änderung vom pro Tag gesamt = * Anzahl der Aufbewahrungstage. 
- Jedem master Server hat nur eine aufbewahrungsvolume. Aufbewahrungsvolume von Datenträgern auf dem master-Ziel-Server gemeinsam genutzt. Zum Beispiel:
    - Wenn ein Quellcomputer mit 5 Festplatten und jedes Laufwerk 120 IOPS (8 KB Größe) der Quelle generiert, entspricht dies 240 IOPS pro Datenträger (2 Operationen auf dem Zieldatenträger pro Source IO). 240 IOPS ist in Azure pro Datenträger IOPS 500.
    - Auf dem aufbewahrungsvolume wird 120 * 5 = 600 IOPS und dadurch einen Flaschenhals werden. In diesem Szenario wäre eine gute Strategie Aufbewahrung Datenträger weitere Festplatten hinzufügen und es, als RAID-Stripeset Konfiguration umfassen. Dies verbessert die Leistung, da die IOPS auf mehrere Laufwerke verteilt sind. Die Anzahl der Laufwerke aufbewahrungsvolume hinzugefügt werden wie folgt:
        - Summe IOPS von Quell-Umgebung / 500
        - Änderung gesamt pro Tag von Quell-Umgebung (nicht komprimiert) / 287 GB. 287 GB beträgt der maximale Durchsatz Zieldatenträger pro Tag unterstützt. Diese Metrik variiert je Schreibgröße mit 8 KB, da in diesem Fall 8 KB Schreibgröße ausgegangen ist. Write-Größe ist 4 KB sein Durchsatz beispielsweise 287/2. Und wenn Schreibzugriff 16 K ist dann Durchsatz 287 * 2.
- Die Anzahl der Speicherkonten erforderlich = total Quelle IOPs-10000.


## <a name="before-you-start"></a>Bevor Sie beginnen

**Komponente** | **Vorschriften** | **Details**
--- | --- | --- 
**Ein Azure-Konto** | Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten.
**Azure-Speicher** | Sie benötigen ein Konto Azure-Speicher zum Speichern von replizierter Daten<br/><br/> Das Konto sollte ein [Standardkonto Geo-redundant Storage](../storage/storage-redundancy.md#geo-redundant-storage) oder [Premium-Speicherkonto](../storage/storage-premium-storage.md).<br/><br/> Es muss im Bereich der Azure Site Recovery-Dienst und dieselbe Abonnement zugeordnet werden. Verschieben von Ressourcengruppen über [neue Azure-Portal](../storage/storage-create-storage-account.md) erstellt Speicherkonten wird nicht unterstützt.<br/><br/> Lesen weitere [Einführung in Microsoft Azure-Speicher](../storage/storage-introduction.md)
**Azure virtual network** | Sie benötigen ein Azure virtuelles Netzwerk auf dem Konfigurationsserver und master Zielserver bereitgestellt werden. Es sollte das Depot Azure Site Recovery in dieselbe Abonnement und Region. Möchten Sie Daten über eine ExpressRoute oder VPN-Verbindung replizieren muss Azure virtuelle Netzwerk über eine ExpressRoute oder ein Standort-zu-Standort-VPN mit dem lokalen Netzwerk verbunden sein.
**Azure-Ressourcen** | Stellen Sie sicher, dass genügend Azure Ressourcen alle Komponenten bereitgestellt haben. Lesen Sie mehr in [Azure Abonnement Grenzen](../azure-subscription-service-limits.md).
**Azure virtuelle Computer** | Virtuelle Maschinen zu schützen sollten [Azure Komponenten](site-recovery-best-practices.md)entsprechen.<br/><br/> **Anzahl der Datenträger**– maximal 31 Datenträger kann auf einem geschützten Server unterstützt<br/><br/> **Datenträger**– einzelne Festplattenkapazität sollte nicht mehr als 1023 GB<br/><br/> **Clustering**-Clustered Server werden nicht unterstützt.<br/><br/> **Boot**-Unified Extensible Firmware Interface (UEFI) Extensible Firmware Interface(EFI) Boot wird nicht unterstützt.<br/><br/> **Volumes**, Bitlocker verschlüsselten Volumes werden nicht unterstützt.<br/><br/> **Namen**-Namen müssen zwischen 1 und 63 Zeichen (Buchstaben, Zahlen und Bindestriche) enthalten. Der Name muss mit einem Buchstaben oder einer Zahl beginnen und enden mit einem Buchstaben oder einer Zahl. Nachdem ein Computer geschützt können Sie Azure Namen ändern.
**Konfigurationsserver** | Basierend auf ein Galeriebild Azure Site Recovery Windows Server 2012 R2 Standard A3 VM wird in Ihrem Abonnement für Konfigurationsserver erstellt. Es wird als die erste Instanz in eine neue Cloud-Dienst erstellt. Wenn Sie Internetzugang den Verbindungstyp Konfigurationsserver Cloud-Dienst eine reservierte öffentliche IP-Adresse erstellt wird wählen.<br/><br/> Der Pfad sollte nur englische Zeichen.
**Master-Ziel-server** | Azure virtuellen Computer standard A4, D14 oder DS4.<br/><br/> Der Pfad sollte nur englische Zeichen. Beispielsweise sollte der Pfad **/usr/local/ASR** für einen master Zielserver unter Linux.
**Prozessserver** | Sie können den Prozess-Server auf physischen oder virtuellen Computer mit Windows Server 2012 R2 mit den neuesten Updates bereitstellen. Auf Laufwerk C: installieren /.<br/><br/> Wir empfehlen, den Server im gleichen Netzwerk und Subnetz wie die Computer platzieren Sie schützen möchten.<br/><br/> Installieren Sie VMware vSphere CLI 5.5.0 auf den Prozess-Server. VMware vSphere CLI-Komponente muss auf den Prozess-Server virtuelle Maschinen von einem vCenter Server verwaltet und virtuellen Computern auf einem Host ESXi entdecken.<br/><br/> Der Pfad sollte nur englische Zeichen.<br/><br/> ReFS Dateisystem wird nicht unterstützt.
**VMware** | Verwalten Ihre VMware vSphere Hypervisor VMware vCenter Server. Es sollte vCenter Version 5.1 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/> Mindestens mit virtuellen Maschinen von VMware vSphere Hypervisor, die Sie schützen möchten. Der Hypervisor läuft ESX/ESXi 5.1 oder 5.5 mit den neuesten Updates.<br/><br/> Virtuelle VMware-Computer sollte VMware Tools installiert und ausgeführt haben. 
**Windows-Computer** | Geschützten Servern oder virtuelle VMware-Computer unter Windows verfügen Anforderungen.<br/><br/> Ein unterstützt 64-Bit-Betriebssystem: **Windows Server 2012 R2**, **Windows Server 2012**oder **Windows Server 2008 R2 mit am wenigsten SP1**.<br/><br/> Der Hostname Bereitstellungspunkte Gerätenamen, Windows-Systempfad (z.B.: C:\Windows) sollte nur in Englisch sein.<br/><br/> Das Betriebssystem sollte auf Laufwerk C:\.<br/><br/> Nur Basisdatenträger werden unterstützt. Dynamische Datenträger werden nicht unterstützt.<br/><br/> Firewall-Regeln auf geschützten Computern sollten lassen zu Konfiguration und Master-Zielserver in Azure.p ><p>Sie müssen ein Administratorkonto angeben (muss ein lokaler Administrator auf dem Windows-Computer) auf Pushinstallation Mobility-Dienst von Windows Server. Wenn das angegebene Konto ein Domänenkonto ist müssen Sie RAS-Benutzer auf dem lokalen Computer deaktivieren. Hinzufügen den LocalAccountTokenFilterPolicy DWORD-Registrierungseintrag mit dem Wert 1 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Fügen Sie den Registrierungseintrag CLI öffnen Cmd oder Powershell und **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. [Erfahren Sie mehr](https://msdn.microsoft.com/library/aa826699.aspx) über die Zugriffsteuerung.<br/><br/> Nach einem Failover soll Windows virtuelle Computer in Azure mit Remotedesktop Verbindung sicherstellen Sie, dass Remotedesktop für den lokalen Computer aktiviert ist. Wenn Sie nicht über VPN herstellen, sollten Firewallregeln Remotedesktopverbindungen über das Internet zulassen.
**Linux-Maschinen** | Eine unterstützte 64-Bit-Betriebssystem: **Centos 6.4, 6.5, 6.6**; **Oracle Enterprise Linux 6.4, 6.5 unter Red Hat kompatibel Kernel oder unverwüstliche Enterprise Kernel Version 3 (UEK3)**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Firewall-Regeln auf geschützten Computern sollten sie die Konfiguration und master Zielserver in Azure zulassen.<br/><br/> / etc/hosts-Dateien auf geschützten Computern sollten Einträge enthalten, die den lokalen Hostnamen IP-Adressen aller Netzwerkkarten zuordnen <br/><br/> Ein Azure virtuellen Computer mit Linux nach einem Failover mit einem Secure Shell-Client (ssh) eine Verbindung herstellen möchten, stellen Sie sicher, dass Secure Shell-Dienst auf dem geschützten Computer automatisch beim Systemstart starten und Firewallregeln zulassen einer ssh Verbindung.<br/><br/> Hostname, Bereitstellungspunkte, Device-Namen und Linux Systempfade und Dateinamen (z. B./Etc /, / usr) sollten nur in Englisch sein.<br/><br/> Schutz für lokalen Computer mit den folgenden aktiviert werden:-<br>Dateisystem: EXT3, ETX4, ReiserFS XFS<br>Multipath-Software-Device Mapper (Multipfad)<br>Volume-Manager: LVM2<br>Physische Server mit HP CCISS Controller werden nicht unterstützt.
**Von Drittanbietern** | Einige bereitstellungskomponenten in diesem Szenario hängt von Drittanbieter-Software ordnungsgemäß funktioniert. Eine vollständige Liste finden Sie unter [Hinweise zu Software von Drittanbietern und Informationen](#third-party)


### <a name="network-connectivity"></a>Netzwerkkonnektivität

Sie haben zwei Optionen beim Konfigurieren von Netzwerkkonnektivität zwischen lokalen Standort und Azure virtuelle Netzwerk auf dem Infrastrukturkomponenten (Server Configuration, master Server) bereitgestellt werden. Sie müssen entscheiden, der Netzwerk-Konnektivitätsoption vor der Bereitstellung der Server Configuration. Sie müssen diese Option bei der Bereitstellung. Es kann später geändert werden.

**Internet:** Kommunikation und Replikation von Daten zwischen den lokalen Server (Prozessserver, geschützte Computer) und den Servern der Azure-Infrastruktur (Server Configuration, master Zielserver) erfolgt über eine sichere Verbindung SSL/TLS von lokalen öffentlichen Endpunkte auf den Zielservern Konfigurations- und Master. (Die einzige Ausnahme ist die Verbindung zwischen den Prozess-Server und dem master Zielserver auf TCP-Port 9080 verschlüsselt. Nur Steuerelement Informationen Replikationsprotokoll für Replikation für diese Verbindung stattfindet.)

![Bereitstellung Diagramm internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: Kommunikation und Replikation von Daten zwischen den lokalen Server (Prozessserver, geschützte Computer) und den Servern der Azure-Infrastruktur (Server Configuration, master Zielserver) erfolgt über eine VPN-Verbindung zwischen dem lokalen Netzwerk und Azure virtuelle Netzwerk auf dem Konfigurationsserver und master Zielserver bereitgestellt. Sicherstellen Sie, dass lokale Netzwerk durch eine ExpressRoute oder eine Standort-zu-Standort-VPN-Verbindung mit Azure virtuellen Netzwerk verbunden ist.

![Bereitstellungsdiagramm VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)


## <a name="step-1-create-a-vault"></a>Schritt 1: Erstellen eines Depots

1. Melden Sie sich im [Verwaltungsportal](https://portal.azure.com).


2. Erweitern Sie **Data Services** > **Recovery Services** und **Site Recovery Depot**auf.


3. Klicken Sie auf **neue** > **schnell erstellen**.

4. Geben Sie im Feld **Name**einen Anzeigenamen zu Tresor.

5. Wählen Sie im **Bereich**geografische Region für das Depot. Überprüfen Sie unterstützte Regionen finden Sie unter geografische Verfügbarkeit in [Azure Site Recovery Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/)

6. Klicken Sie auf **Create Tresor**.

    ![Neues Depot](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Überprüfen Sie die Statusleiste, um sicherzustellen, dass das Depot erfolgreich erstellt wurde. Das Depot wird als **aktiv** auf **Recovery Services** aufgeführt.

## <a name="step-2-deploy-a-configuration-server"></a>Schritt 2: Bereitstellen eines Konfiguration Servers

### <a name="configure-server-settings"></a>Server konfigurieren

1. Der **Recovery-** Seite klicken Sie auf das Depot zum Öffnen der Seite Quick Start. Quick-Start kann auch jederzeit auf das Symbol geöffnet werden.

    ![Schnellstart-Symbol](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)

2. Wählen Sie in der Dropdownliste **zwischen einer lokalen Website mit VMware-physischen Servern und Azure**.
3. Klicken Sie im **Target(Azure)-Ressourcen vorbereiten** **Konfigurationsserver bereitstellen**.

    ![Konfigurationsserver bereitstellen](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)

4. Geben Sie **Neue Server-Konfigurationsdetails** :

    - Ein Name für die Konfigurationsserver und die Anmeldeinformationen zum Herstellen der Verbindung.
    - In der Netzwerk-Konnektivität Dropdown-Liste Wählen Sie **Internetzugang** oder **VPN**. Beachten Sie, dass Sie diese Einstellung nach der Anwendung ändern können.
    - Wählen Sie das Azure Netzwerk auf dem Server befinden soll. Verwenden Sie VPN stellen sicher Azure-Netzwerk mit Ihrem lokalen Netzwerk besteht wie erwartet. 
    - Geben Sie die interne IP-Adresse und Subnetzmaske, die dem Server zugewiesen werden. Beachten Sie, dass die ersten vier Adressen ein Subnetz für interne Azure reserviert sind. Verwenden Sie alle verfügbaren IP-Adressen.
    
    ![Konfigurationsserver bereitstellen](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)

5. Beim Klicken auf **OK** einen standard A3 virtuellen Computer basierend auf ein Galeriebild Azure Site Recovery Windows Server 2012 R2 werden Ihr Abonnement für Konfigurationsserver erstellt. Es wird als die erste Instanz in eine neue Cloud-Dienst erstellt. Wenn Sie über das Internet verbunden wird Cloud-Dienst eine reservierte öffentliche IP-Adresse erstellt. Sie können auf der Registerkarte **Aufträge** überwachen.

    ![Überwachen des Fortschritts](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)

6.  Wenn Sie über das Internet herstellen, wird nach dem Konfigurationsserver bereitgestellte Hinweis öffentliche IP-Adresse auf der Seite **virtuelle Maschinen** in Azure-Portal zugewiesen. Dann zugeordnet auf den **Endpunkten** Registerkarte Hinweis öffentlichen HTTPS-Port privater Port 443. Sie benötigen diese Informationen später beim master Ziel und Prozess-Server mit dem Konfigurationsserver registrieren. Konfigurationsserver wird diese Endpunkte bereitgestellt:

    - HTTPS: Mit einem öffentlicher Port verwendet Komponentenserver und Azure Kommunikation über das Internet zu koordinieren. Privater Port 443 wird zum Kommunikation zwischen Komponentenserver und Azure über VPN koordinieren.
    - Benutzerdefiniert: Mit einem öffentlicher Port Failback Tool Kommunikation über das Internet dient. Privater Port 9443 Failback Tool Kommunikation über VPN dient.
    - PowerShell: Privater Port 5986
    - Remote Desktop: Private Port 3389
    
    ![VM-Endpunkte](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

    >[AZURE.WARNING] Nicht löschen Sie oder ändern Sie die öffentliche oder private Portnummer der Endpunkte, die während der Bereitstellung der Server Konfiguration erstellt.

Konfigurationsserver ist eine automatisch erstellte Azure Cloud Service eine reservierte IP-Adresse bereitgestellt. Die reservierte Adresse muss sicherstellen, dass Konfiguration Server Cloud Service-Adresse Neustart virtueller Maschinen (einschließlich den Konfigurationsserver) unverändert auf Cloud-Dienst. Reservierte öffentliche IP-Adresse müssen nicht manuell reservierte, wenn der Konfigurationsserver außer Betrieb genommen oder vorbehalten bleiben. Bis zu 20 Reservierte öffentliche IP Adressen pro Abonnement gibt. [Weitere Informationen](../virtual-network/virtual-networks-reserved-private-ip.md) zu reservierten IP-Adressen. 

### <a name="register-the-configuration-server-in-the-vault"></a>Registrieren Sie den Konfigurationsserver im Tresor

1. Klicken Sie auf **Quick Start** **Vorbereiten Ressourcen** > **Registrierungsschlüssel herunterladen**. Die Schlüsseldatei wird automatisch generiert. Es gilt für 5 Tage nach generiert wird. Kopieren sie auf dem Konfigurationsserver.
2. Wählen Sie in **virtuellen Maschinen** Konfigurationsserver virtueller Maschinen aus. Registerkarte **Dashboard** , und klicken Sie auf **Verbinden**. **Öffnen** der RDP-Datei Konfigurationsserver Remotedesktop anmelden. Verwenden Sie VPN verwenden, die interne IP-Adresse (die Adresse angegebene bei der Bereitstellung des Servers Configuration) für eine Remotedesktopverbindung von der lokalen Website. Der Azure Site Recovery Server Konfigurationsassistent wird automatisch ausgeführt, wenn Sie zum ersten Mal anmelden.

    ![Registrierung](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)

3. **Drittanbieter-Software Installation** klicken Sie auf **ich stimme** zum Herunterladen und Installieren von MySQL.

    ![MySQL installieren](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)

4. **MySQL Server** Details erstellen Sie Anmeldeinformationen anmelden der MySQL-Server-Instanz.

    ![MySQL-Anmeldeinformationen](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)

5. Geben Sie im **Internet** wie der Konfigurationsserver im Internet herstellen. Beachten Sie Folgendes:

    - Möchten Sie einen benutzerdefinierten Proxyserver sollten Sie es vor dem Installieren des Anbieters festlegen.
    - Beim Klicken auf **Weiter** wird ein Test ausgeführt, um die Proxy-Verbindung zu überprüfen.
    - Sie verwenden einen benutzerdefinierten Proxy, ob der Standardproxy erfordert Authentifizierung Sie die Proxy-Details, einschließlich Adresse, Anschluss und Anmeldeinformationen einzugeben müssen.
    - Sollte der Proxy über die folgenden URLs:
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Wenn Sie IP-Adressen basierenden daß Firewallregeln die Regeln Kommunikation vom Konfigurationsserver der IP-Adressen in [Azure Datacenter IP-Bereiche](https://msdn.microsoft.com/library/azure/dn175718.aspx) und HTTPS (443) Protokoll beschrieben. Sie müssten zu White-Liste IP-Adressbereiche der Azure-Region, die Sie verwenden möchten, West US.

    ![Proxyregistrierung](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)

6. Festlegen Sie im **Anbieter Fehler Meldung Lokalisierung** , in welcher Sprache Fehlermeldungen angezeigt werden.

    ![Fehler Meldung Registrierung](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)

7. **Azure Site Recovery-Registrierung** durchsuchen Sie, und wählen Sie die Schlüsseldatei, die auf den Server kopiert.

    ![Schlüsseldatei Registrierung](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)

8. Optionen Sie auf der Abschlussseite des Assistenten:

    - Wählen Sie **Konto Management Dialogfeld starten** an, dass das Dialogfeld Konten verwalten nach Abschluss des Assistenten geöffnet.
    - Wählen Sie **ein Desktopsymbol für Cspsconfigtool erstellen** eine desktop-Verknüpfung auf dem Konfigurationsserver hinzufügen, damit Sie jederzeit das Dialogfeld **Konten verwalten** öffnen können, ohne den Assistenten erneut ausführen.

    ![Registrierung abschließen](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)

9. Klicken Sie auf **Fertig stellen** , um den Assistenten abzuschließen. Eine Passphrase wird generiert. Kopieren Sie sie an einem sicheren Ort. Sie benötigen diese authentifizieren und Prozess und master Zielserver mit dem Konfigurationsserver registrieren. Es wird auch Channel Integrität im Konfigurations-Server-Kommunikation verwendet. Die Passphrase regenerieren, aber dann müssen master Ziel registrieren und Server neue Passphrase verarbeiten.

    ![Passphrase](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Nach der Registrierung wird der Konfigurationsserver auf der Seite **Konfigurationsserver** im Tresor aufgeführt.

### <a name="set-up-and-manage-accounts"></a>Einrichten und Verwalten von Konten

Während der Bereitstellung fordert Site Recovery Anmeldeinformationen für folgenden Aktionen:

- Ein VMware-Konto können ThatSite Wiederherstellung automatisch Discovery VMs auf vCenter Server oder vSphere Hosts. 
- Wenn Sie Computer Schutz hinzufügen, Site Recovery Service Mobilität installieren können.

Nach dem Konfigurationsserver registriert können Sie öffnen das Dialogfeld " **Konten verwalten** " hinzufügen und Verwalten von Konten für diese Aktionen verwendet werden soll. Es gibt ein paar Methoden zur Verfügung:

- Öffnen Sie die Verknüpfung zu dem Dialog auf der letzten Seite von Setup für den Server Configuration (Cspsconfigtool) entschieden werden erstellt.
- Öffnen Sie der Dialog Beenden des Server-Konfiguration.

1. **Konten verwalten** klicken Sie auf **Konto hinzufügen**. Außerdem können Sie vorhandene Konten löschen und ändern.

    ![Konten verwalten](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)

2. Geben Sie einen Kontonamen verwenden in den **Kontodetails** und Azure (Domäne/Benutzername). 

    ![Konten verwalten](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Verbindung zum Konfigurationsserver 

Verbindung zum Konfigurationsserver erhalten Sie auf zweierlei Art:

- Über eine VPN-Standort-zu-Standort- oder ExpressRoute-Verbindung
- Über das internet 

Beachten Sie Folgendes:

- Eine Internet-Verbindung verwendet die Endpunkte des virtuellen Computers in Verbindung mit der öffentliche virtuelle IP-Adresse des Servers.
- Eine VPN-Verbindung verwendet die interne IP-Adresse des Servers mit privaten Endpunkt-Ports.
- Ist eine einmalige Entscheidung entscheiden (Daten-Steuerelement und Replikation) Verbindung von lokalen Servern an die verschiedenen Komponentenserver (Server Configuration, master Zielserver) in Azure ausgeführt wird, über eine VPN-Verbindung oder das Internet. Sie können diese Einstellung später ändern. Ansonsten müssen Sie das Szenario erneut und schützt Ihre Computer.  


## <a name="step-3-deploy-the-master-target-server"></a>Schritt 3: Master Zielserver bereitstellen

1. Klicken Sie auf **Target(Azure)-Ressourcen vorbereiten** > **master Zielserver bereitstellen**.
2. Master-Ziel-Detailinformationen und Anmeldeinformationen angeben. Der Server wird im gleichen Azure Netzwerk wie der Konfigurationsserver bereitgestellt werden. Wenn Sie Azure VM ausführen klicken mit Windows oder Linux Bild entstehen.

    ![Ziel-Einstellungen](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Beachten Sie, dass die ersten vier Adressen ein Subnetz für interne Azure reserviert sind. Alle verfügbaren IP-Adressen angeben.

>[AZURE.NOTE] Wählen Sie Standard DS4 beim Schutz für Arbeitslasten konfigurieren die konsistent hohe i/o-Performance und niedrige Latenz benötigen, um e/a-intensive Arbeitslasten mit [Premium-Speicherkonto](../storage/storage-premium-storage.md)hosten.


3. Windows master Zielserver erstellten VM mit diesen Endpunkten. Beachten Sie, dass öffentliche Endpunkte erstellt werden, wenn die Verbindung über das Internet.

    - Benutzerdefiniert: Öffentliche Port zur Prozess-Server Replikationsdaten über das Internet senden. Privater Port 9443 Prozess-Server dient dem master Zielserver über VPN Replikationsdaten an.
    - Custom1: Öffentlicher Port vom Prozessserver verwendet, um Metadaten über das Internet senden. Privater Port 9080 Prozess-Server dient dem master Zielserver über VPN Metadaten an.
    - PowerShell: Privater Port 5986
    - Remote Desktop: Private Port 3389

4. Linux master Zielserver VM wird diese Endpunkte erstellt. Beachten Sie, dass öffentliche Endpunkte nur erstellt werden, wenn Sie über das Internet herstellen.

    - Benutzerdefiniert: Öffentliche Port zur Prozess-Server Replikationsdaten über das Internet senden. Privater Port 9443 Prozess-Server dient dem master Zielserver über VPN Replikationsdaten an.
    - Custom1: Öffentliche Prozess-Server dient, Metadaten über das Internet senden. Privater Port 9080 Prozess-Server dient dem master Zielserver über VPN Metadaten an
    - SSH: Private Anschluss 22

    >[AZURE.WARNING] Nicht löschen Sie oder ändern Sie die öffentliche oder private Portnummer aller Endpunkte bei serverbereitstellung master Ziel erstellt.

5. **Virtuelle Computer** -Wartezeit für den virtuellen Computer zu starten.

    - Ist dies ein Hinweis Windows Server remote desktop Einzelheiten.
    - Ein Linux-Server und Verbinden über VPN Beachten Sie die interne IP-Adresse des virtuellen Computers. Beachten Sie öffentliche IP-Adresse, wenn Sie über das Internet herstellen.

6.  Anmelden an Server Installation abzuschließen und mit dem Konfigurationsserver registrieren. 
7.  Wenn Sie Windows verwenden:

    1. Initiieren Sie eine Remotedesktopverbindung mit dem virtuellen Computer. Beim ersten eines Skripts anmelden wird im PowerShell-Fenster ausgeführt. Schließen Sie nicht. Bei Fertigstellung öffnet die Host-Agent-Konfigurationstool Server registrieren.
    2. Geben Sie **Host Agent Config** die interne IP-Adresse der Server Configuration und Port 443 an. Interne Adresse und private Port 443 können auch wenn Sie nicht über VPN eine Verbindung herstellen, da Azure demselben Netzwerk wie der Konfigurationsserver der virtuellen Maschine zugeordnet ist. **Mit HTTPS** aktiviert lassen. Geben Sie das Kennwort für den zuvor notierten Konfigurationsserver. Klicken Sie auf **OK** , um den Server zu registrieren. Die NAT-Optionen können Sie ignorieren. Sie werden nicht verwendet.
    3. Wenn der geschätzte Aufbewahrung Laufwerk mehr als 1 TB ist können Sie Aufbewahrung (R:) mit einem virtuellen Laufwerk [Speicherplätze](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx) Konfiguration
    
    ![Windows masterzielserver](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)

8. Wenn Sie Linux verwenden:
    1. Stellen Sie sicher, dass Sie die neuesten Linux Integration Services (LIS) vor der master Zielserver installiert installiert haben. Finden Sie die neueste Version von LIS sowie eine Anleitung zum Installieren [hier](https://www.microsoft.com/download/details.aspx?id=46842). Starten Sie den Computer nach dem Installieren der LIS.
    2. **Vorbereiten (Azure) Ressourcen** klicken Sie auf **zusätzlichen Software (nur für Linux-Master-Zielserver) herunterladen und installieren**. Kopieren der heruntergeladenen Tar-Datei auf den virtuellen Computer mit einem Sftp-Client. Sie master Zielserver bereitgestellte Linux anmelden und verwenden Sie *Wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* zum Herunterladen der Datei.
    2. Melden Sie sich mit dem Server über einen Secure Shell-Client. Verwenden Sie den Azure-Netzwerk über VPN verbunden sind die interne IP-Adresse. Verwenden Sie andernfalls die externe IP-Adresse und den öffentlichen SSH-Endpunkt.
    3. Extrahieren Sie die Dateien aus dem gezippte Installer mit: **-Xvzf Microsoft tar-ASR_UA_8.4.0.0_RHEL6-64***
    ![Linux master Zielserver](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
    4. Stellen Sie sicher, dass Sie im Verzeichnis sind, der Inhalt der Tar-Datei extrahiert.
    5. Passphrase Server Konfiguration in einer lokalen Datei mit dem Befehl Kopieren * *Echo* `<passphrase>` * > passphrase.txt**
    6. Führen Sie den Befehl "**Sudo. / install -t sowohl – ein host -R MasterTarget -d-/usr/local/ASR -i* `<Configuration server internal IP address>` * -p 443 s - y - C Https -P passphrase.txt**".

    ![Ziel-Server registrieren](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)

9. Warten Sie ein paar Minuten (10-15) und überprüfen Sie auf der Seite master Zielserver aufgeführt, wie **Server**registriert > Registerkarte**Konfiguration** **Serverdetails** . Führen Sie Wenn Sie Linux und nicht registriert den Host Konfigurationstool erneut aus /usr/local/ASR/Vx/bin/hostconfigcli aus. Sie müssen Berechtigungen festlegen Chmod als Stamm.

    ![Zielserver überprüfen](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

>[AZURE.NOTE] Es dauert bis zu 15 Minuten nach Registrierung für master Zielserver im Portal angezeigt werden. Sofort aktualisieren, klicken Sie auf der Seite **Konfigurationsserver** **Aktualisieren** .

## <a name="step-4-deploy-the-on-premises-process-server"></a>Schritt 4: Bereitstellen von lokalen Prozess-server

Bevor Sie beginnen wird empfohlen, eine statische IP-Adresse auf den Prozess-Server konfigurieren, damit sie garantiert über Neustarts hinweg beibehalten werden.

1. Klicken Sie auf Quick Start > **Installation Process Server lokal** > **herunter und installieren Sie den Prozess-Server**.

    ![Prozess-Server installieren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)

2.  Kopieren Sie die heruntergeladene Zip-Datei auf den Server, auf dem Sie den Prozess-Server installiert werden. Zip-Datei enthält zwei Installationsdateien:

    - Microsoft-ASR_CX_TP_8.4.0.0_Windows*
    - Microsoft-ASR_CX_8.4.0.0_Windows*

3. Entpacken Sie das Archiv und kopieren Sie die Installationsdateien auf den Server.
4. Ausführen der **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** Installation Datei und folgen. Drittanbieter-Komponenten für die Bereitstellung installiert.
5. Führen Sie **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. Wählen Sie auf der Seite **Server-Modus** **Prozessserver**.
7. Auf der Seite **Umgebung** folgendermaßen vor:


    - Wenn Sie virtuelle VMware schützen möchten klicken Sie **Ja**
    - Wenn Sie nur physische Server schützen möchten und daher VMware vCLI auf den Prozess-Server installiert brauchen. Klicken Sie auf **Nein** , und fortfahren.

8. Beachten Sie Folgendes bei der Installation von VMware vCLI:

    - **Nur VMware vSphere CLI 5.5.0 unterstützt**. Prozess-Server funktioniert nicht mit anderen Versionen oder Updates von vSphere CLI.
    - VSphere CLI 5.5.0 herunterladen von [hier.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
    - Wenn Sie vSphere CLI installiert vor Installation Prozess gestartet und Setup erkennt nicht, warten Sie bis zu fünf Minuten, bevor Sie Setup erneut versuchen. Dies gewährleistet, dass alle Umgebungsvariablen für vSphere CLI Erkennung richtig initialisiert wurden benötigt.

9.  **NIC** -Auswahl für Prozess-Server wählen Sie den Netzwerkadapter, den der Prozess-Server verwenden soll.

    ![Wählen Sie adapter](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)

10. **Konfigurationsdetails für Server**:

    - Für IP-Adresse und Port Wenn über VPN herstellen Geben Sie interne IP-Adresse des konfigurationsservers und 443 für den Port an. Andernfalls geben Sie öffentliche virtuelle IP-Adresse und zugeordnete Öffentliche HTTP-Endpunkt.
    - Geben Sie die Passphrase Konfigurationsserver.
    - Deaktivieren Sie **Überprüfen Mobilität Software Signatur** ggf. Überprüfung zu deaktivieren, wenn automatische Push verwenden, um den Dienst zu installieren. Überprüfung der Signatur benötigt Internetkonnektivität vom Prozess-Server.
    - Klicken Sie auf **Weiter**.

    ![Konfigurationsserver registrieren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)


11. Wählen Sie in **Wählen Sie Installationslaufwerk** Cachelaufwerk. Prozess-Server benötigt ein Cachelaufwerk mit mindestens 600 GB freien Speicherplatz. Klicken Sie auf **Installieren**. 

    ![Konfigurationsserver registrieren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)

12. Beachten Sie, dass der Server zum Abschließen der Installation neu gestartet werden müssen. **Konfiguration**Server > **Server Details** prüfen, der Prozess-Server wird im Tresor erfolgreich registriert.

>[AZURE.NOTE]Es dauert bis zu 15 Minuten nach der Registrierung für den Prozess-Server angezeigt werden, wie unter dem Konfigurationsserver abgeschlossen ist. Sofort aktualisieren, aktualisieren Sie den Konfigurationsserver durch Klicken auf die Schaltfläche "Aktualisieren" am unteren Rand der Seite Konfiguration server
 
![Prozess überprüfen](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Deaktivieren nicht überprüfen von Signaturen für mobilitätsdienst beim Prozess-Server registrieren können Sie dies später wie folgt tun:

1. Prozess-Server als Administrator anmelden und die Datei C:\pushinstallsvc\pushinstaller.conf zur Bearbeitung zu öffnen. Fügen Sie diese Zeile im Abschnitt **[PushInstaller.transport]** : **SignatureVerificationChecks = "0"**. Speichern Sie und schließen Sie die Datei.
2. Starten Sie den Dienst InMage PushInstall.


## <a name="step-5-update-site-recovery-components"></a>Schritt 5: Update Website Komponenten

Site Recovery Komponenten werden von Zeit zu Zeit aktualisiert. Wenn neue Updates verfügbar sind, sollten Sie sie in der folgenden Reihenfolge installieren:

1. Konfigurationsserver
2. Prozessserver
3. Master-Ziel-server
4. Failback Tool (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Beziehen und Installieren von updates


1. Updates für Konfiguration, Prozess und master Server erhalten Sie von der Site Recovery- **Dashboard**. Extrahieren Sie die Dateien für Linux gezippte Installer und führen den Befehl "Sudo. / install" das Update installieren.
2. [Download](http://go.microsoft.com/fwlink/?LinkID=533813) die neueste Aktualisierung für das Failback-tool(vContinuum).
3. Wenn Sie virtuellen Computern oder Servern, auf denen bereits die mobilitätsservice installiert ausführen, können Sie die Updates für den Dienst erhalten:

    - **Option 1**: Downloaden von Updates:
        - [Windows-Server (64 Bit)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
        - [CentOS 6.4,6.5,6.6 (64 Bit)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
        - [Oracle Enterprise Linux 6.4,6.5 (64 Bit)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
        - [SUSE Linux Enterprise Server SP3 (64 Bit)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
        - Nach dem Aktualisieren Process-Server wird die aktualisierte Version des Dienstes Mobilität im Ordner C:\pushinstallsvc\repository auf den Prozess-Server verfügbar sein.
    - **Option 2**: Wenn Sie einen Computer mit einer älteren Version der Mobility-Dienst installiert haben, können Mobility-Dienst auf dem Computer automatisch aus dem Verwaltungsportal aktualisieren.

        1. Sicherstellen Sie, dass der Prozess-Server aktualisiert wird.
        2. Stellen Sie sicher, dass der geschützte Computer die [Komponenten](#install-the-mobility-service-automatically) automatisch Push Service Mobilität entspricht, damit das Update funktioniert wie erwartet.
        2. Wählen Sie die Schutzgruppe, markieren Sie geschützten Computer und klicken Sie auf **Update Mobilität**. Diese Schaltfläche ist nur verfügbar, wenn eine neuere Version des Dienstes Mobilität. 

            ![Wählen Sie vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Wählen Sie Konten Geben Sie das Administratorkonto mit mobilitätsdienst auf dem geschützten Server aktualisiert werden an. Klicken Sie auf OK, und warten Sie ausgelöste Auftrag abgeschlossen.


## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Schritt 6: VCenter Server oder vSphere Hosts hinzufügen

1. Klicken Sie auf **Server** > **Konfigurationsserver** > Configuration Server >**Add vCenter Server** einen vCenter Server oder vSphere Host hinzufügen.

    ![Wählen Sie vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)

2. Angaben für den Server oder Host, und wählen Sie den Prozess-Server, der verwendet wird, erkannt.

    - Wenn vCenter Server nicht den Standardport 443 läuft Geben Sie die Anschlussnummer auf der vCenter Server ausgeführt wird an.
    - Prozess-Server muss in demselben Netzwerk wie der Host vCenter Server-vSphere und VMware vSphere CLI 5.5.0 installiert haben.

    ![vCenter Server konfigurieren](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)


3. Nach Abschluss der Erkennung vCenter Server unter Server Konfigurationsdetails finden.

    ![vCenter Server konfigurieren](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)

4. Wenn Sie den Server oder Host ein Administratorkonto verwenden, stellen Sie sicher, dass das Konto die folgenden Berechtigungen verfügt:

    - vCenter Konten müssen Datacenter Datenspeicher, Ordner, Host, Netzwerk, Ressourcen, Ansichten, virtuellen und vSphere Switch verteilt berechtigt.
    - vSphere Host Konten müssen Datacenter, Datenspeicher, Ordner, Server, Netzwerk, Ressource, Virtual Machine und vSphere Switch verteilt berechtigt



## <a name="step-7-create-a-protection-group"></a>Schritt 7: Erstellen einer Schutzgruppe

1. Öffnen Sie **Geschützte Objekte** > **Schutzgruppe** > **Schutzgruppe erstellen**.

    ![Schutzgruppe erstellen](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)

2. Geben Sie auf der Seite **Einstellungen Schutz geben** einen Namen für die Gruppe, und wählen Sie Konfigurationsserver erstellen soll.

    ![Schutz Einstellung](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)

3. Auf der Seite **Geben Sie Replikation** Konfigurieren der Replikation, die für alle Computer in der Gruppe verwendet werden.

    ![Gruppenreplikation Schutz](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)

4. Einstellungen:
    - **Mehrere VM Konsistenz**: Wenn Sie diese Option aktivieren wird freigegebenen anwendungskonsistente Recovery-Punkte auf den Computern in der Schutzgruppe erstellt. Diese Einstellung ist relevantesten, wenn alle Computer in der Schutzgruppe die gleiche Arbeitslast ausgeführt werden. Alle Computer werden auf die gleichen Daten wiederhergestellt. Nur verfügbar für Windows-Server.
    - **RPO-Schwellenwert**: Warnung werden generiert, wenn kontinuierlichen Schutz Replikation RPO konfigurierten RPO-Schwellenwert überschreitet.
    - **Wiederherstellungspunkt Aufbewahrung**: Gibt die Beibehaltungsdauer. Geschützte Computer können zu einem beliebigen Punkt innerhalb dieses Fenster wiederhergestellt werden.
    - **Anwendungskonsistente Snapshots Häufigkeit**: Gibt an, wie oft Wiederherstellungspunkte mit anwendungskonsistente Snapshots erstellt werden.

Sie auf **Geschützte Elemente** erstellt haben, können Sie die Schutzgruppe überwachen.

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Schritt 8: Einrichten von Computern zu schützen

Sie müssen installieren Service Mobilität virtueller Maschinen zu physischen Servern zu schützen. Dies ist auf zwei Arten möglich:

- Automatisch drücken und den Dienst auf jedem Computer vom Prozess-Server installieren.
- Installieren Sie den Dienst manuell. 

### <a name="install-the-mobility-service-automatically"></a>Mobility-Dienst automatisch installieren

Beim Hinzufügen von Computern zu einer Schutzgruppe mobilitätsservice automatisch abgelegt und Prozess-Server auf jedem Computer installiert. 

**Automatisch Push mobilitätsdienst von Windows Server installieren:** 

1. Die neuesten Updates für den Prozess-Server unter [Schritt 5: Installieren der neuesten Updates](#step-5-install-latest-updates), und stellen Sie sicher, dass der Prozessserver verfügbar ist. 
2. Sicher Netzwerkkonnektivität zwischen dem Quellcomputer und Prozess-Server vorhanden ist und der Quellcomputer vom Prozess-Server zugegriffen werden kann.  
3. Konfigurieren der Windows-Firewall zur **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation**. Unter Windows Firewall Settings die Option "Eine app oder Feature durch die Firewall zulassen" und wählen Sie die Anwendung wie in der folgenden Abbildung dargestellt. Für Computer, die einer Domäne angehören, können Sie die Firewallrichtlinie mit einem Gruppenrichtlinienobjekt konfigurieren.

    ![Firewall Einstellung](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)

4. Zur Durchführung der Pushinstallation verwendete Konto muss in der Administratorgruppe auf dem Computer zu schützen. Diese Anmeldeinformationen werden nur für Push-Installation des Dienstes Mobilität verwendet und Sie geben sie beim Hinzufügen eines Computers zu einer Schutzgruppe.
5. Wenn das angegebene Konto ein Domänenkonto ist müssen Sie RAS-Benutzer auf dem lokalen Computer deaktivieren. Hinzufügen den LocalAccountTokenFilterPolicy DWORD-Registrierungseintrag mit dem Wert 1 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Fügen Sie den Registrierungseintrag CLI öffnen Cmd oder Powershell und **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. 

**Push installiert den mobilitätsdienst auf Linux-Servern:**

1. Die neuesten Updates für den Prozess-Server unter [Schritt 5: Installieren der neuesten Updates](#step-5-install-latest-updates), und stellen Sie sicher, dass der Prozessserver verfügbar ist.
2. Sicher Netzwerkkonnektivität zwischen dem Quellcomputer und Prozess-Server vorhanden ist und der Quellcomputer vom Prozess-Server zugegriffen werden kann.  
3. Stellen Sie sicher, das Konto Root-Benutzer auf dem Linux-Quellserver.
4. Sicherstellen Sie, dass die/etc/hosts-Datei auf dem Linux-Quellserver Einträge enthält, die den lokalen Hostnamen IP-Adressen aller Netzwerkkarten zuordnen.
5. Installieren der neuesten Openssh, Openssh-Server Openssl-Pakete auf dem Computer zu schützen.
6. Sicherstellen Sie, dass SSH Port 22 aktiviert ist und ausgeführt wird. 
7. Aktivieren Sie SFTP-Subsystem und Kennwortauthentifizierung in der Datei Sshd_config folgendermaßen: 

    - (a) melden Sie sich als Root.
    - (b) in der Datei /etc/ssh/sshd_config suchen Sie die Zeile, die mit **Passwordauthentification**beginnt.
    - (c) kommentieren Sie die Zeile, und ändern Sie den Wert von "No" auf "yes".

        ![Linux-Mobilität](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)

    - (d) suchen Sie die Zeile, die mit Subsystem beginnt, und kommentieren Sie die Zeile.
    
        ![Linux Push Mobilität](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    

8. Sicherstellen Sie, dass die Quelle Computer Linux Variante unterstützt wird. 
 
### <a name="install-the-mobility-service-manually"></a>Mobility-Dienst manuell installieren

Softwarepakete installieren des Dienstes Mobilität werden auf dem Prozessserver C:\pushinstallsvc\repository. Prozess-Server anmelden und das entsprechende Installationspaket auf dem Quellcomputer basierend auf Tabelle kopieren:-

| Betriebssystem                               | Mobility-Servicepaket für Prozess-server                                                            |
|---------------------------------------------------    |------------------------------------------------------------------------------------------------------ |
| Windows-Server (64 Bit)                          | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe`         |
| CentOS 6.4, 6.5, 6.6 (64 Bit)                    | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz`     |
| SUSE Linux Enterprise Server 11 SP3 (64 Bit)     | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz`|
| Oracle Enterprise Linux 6.4, 6.5 (64 Bit)        | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz`       |


**Mobility-Dienst manuell auf einem Windows-Server installieren**, gehen Sie folgendermaßen vor:

1. Kopieren des **Microsoft ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** -Pakets aus Prozess Verzeichnispfad der in der Tabelle oben auf dem Quellcomputer aufgeführt.
2. Der Mobility-Dienst durch Ausführen der ausführbaren Datei auf dem Quellcomputer installiert.
3. Gehen Sie auf Installer.
4. Wählen Sie **Mobility-Dienst** die Rolle aus und auf **Weiter**.
    
    ![Mobilitätsdienst installieren](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)

5. Lassen Sie das Installationsverzeichnis als den standardmäßigen Installationspfad und klicken Sie auf **Installieren**.
6. **Host Agent Config** angeben der IP-Adresse und HTTPS-Port des konfigurationsservers.

    - Wenn Sie über das Internet herstellen Geben Sie öffentliche virtuelle IP-Adresse und öffentliche HTTPS-Endpunkt als Port an.
    - Wenn Sie über VPN herstellen Geben Sie die interne IP-Adresse und den Port 443. Lassen Sie **Verwenden HTTPS** aktiviert.

    ![Mobilitätsdienst installieren](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)

7. Geben Sie die Passphrase Konfiguration Server und **OK** mobilitätsservice Configuration Server registrieren.

**Von der Befehlszeile aus ausführen:**

1. Die Passphrase von der CX auf die Datei "C:\connection.passphrase" auf dem Server kopieren und diesen Befehl ausführen. In unserem Beispiel CX i 104.40.75.37 und den HTTPS-Port ist 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Mobility-Dienst manuell auf einem Linux-Server installieren**:

1. Anhand der obigen Tabelle vom Prozess-Server auf den Quellcomputer entsprechende Tar-Archiv zu kopieren.
2. Öffnen Sie ein Shell-Programm und extrahieren Sie das Gezippte Tar-Archiv auf einen lokalen Pfad durch Ausführen`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Erstellen Sie eine passphrase.txt-Datei im lokalen Verzeichnis, Sie die Tar-Archiv eingeben extrahiert *`echo <passphrase> >passphrase.txt`* von Shell.
4. Installieren Sie den Dienst Mobilität durch Eingabe *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Geben Sie die IP-Adresse und den Port:

    - Wenn Sie mit dem Konfigurationsserver über das Internet Verbindung geben Konfiguration Server virtuelle öffentliche IP-Adresse und öffentliche HTTPS-Endpunkt in `<IP address>` und `<port>`.
    - Wenn Sie über eine VPN-Verbindung herstellen Geben Sie die interne IP-Adresse und 443.

**Von der Befehlszeile aus ausführen**:

1. Die Passphrase von der CX auf die Datei "passphrase.txt" auf dem Server kopieren und diese Befehle ausführen. In unserem Beispiel CX i 104.40.75.37 und den HTTPS-Port ist 62519:

Auf einem Produktionsserver installieren:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt
 
Auf dem Zielserver zu installieren:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

>[AZURE.NOTE] Wenn Sie die Computer einer Schutzgruppe, die bereits eine geeignete Version des Dienstes Mobilität dann die Push-Installation übersprungen hinzufügen.


## <a name="step-9-enable-protection"></a>Schritt 9: Aktivieren Schutz

Um Schutz aktivieren Sie virtuellen Computern und Servern eine Schutzgruppe hinzufügen. Bevor Sie beginnen, beachten Sie Folgendes:

- Virtuelle Computer werden alle 15 Minuten entdeckt und dauert bis zu 15 Minuten nach der Erkennung in Azure Site Recovery angezeigt werden.
- Umgebung ändert sich auf dem virtuellen Computer (z. B. VMware Tools-Installation) dauert auch bis zu 15 Minuten in Site Recovery aktualisiert werden.
- Sie können zuletzt ermittelten im Feld **Letzte Kontakt am** vCenter Server-ESXi Host auf der Seite **Konfigurationsserver** überprüfen.
- Wenn Sie einer bereits erstellten Schutzgruppe vCenter Server oder ESXi Host, hinzufügen Minuten 15 Azure Site Recovery-Portal aktualisieren und virtuellen Computer das Dialogfeld **Computer zu einer Schutzgruppe hinzufügen** aufgelistet werden.
- Sie unverzüglich Schutzgruppe ohne geplanten Erkennung Computer hinzufügen möchten, markieren Sie den Konfigurationsserver (nicht klicken), und klicken Sie auf die Schaltfläche **Aktualisieren** .
- Wenn Sie virtuelle oder physische Maschinen zu einer Schutzgruppe hinzufügen, legt der Prozess-Server automatisch und Mobility-Dienst auf dem Quellserver installiert, wenn It bereits installiert ist.
- Automatische Push funktioniert sicherstellen, dass Sie die geschützten Computer eingerichtet haben, wie im vorherigen Schritt beschrieben.

Hinzufügen von Computern wie folgt:

1. Klicken Sie auf **geschützte Elemente** > **Schutzgruppe** > **Maschinen** > **Computer hinzufügen**. Als bewährte Methode empfiehlt Schutzgruppen Workloads gespiegelt werden soll, sodass Computern einer bestimmten Anwendung derselben Gruppe hinzufügen.
2. **Virtuelle Computer auswählen** , wenn Sie den physischen Schutz stellen Server, Assistenten für **Physische Computer hinzufügen** die IP-Adresse und den Anzeigenamen. Wählen Sie Betriebssystem.

    ![V-Center Server hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)

3. In **virtuelle Computer auswählen** Wenn Sie virtuelle VMware-Computer schützen, wählen Sie einen vCenter Server, der die virtuellen Computer (oder EXSi Host sie ausführen) verwaltet und dann Computer.

    ![V-Center Server hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png) 
4. **Geben Sie Ressourcen** wählen Sie die master Server und Speicher für die Replikation und wählen, ob die Einstellungen für alle Arbeitslasten verwendet werden soll. Wählen Sie [Premium-Speicherkonto](../storage/storage-premium-storage.md) beim Konfigurieren des Schutzes für Arbeitslasten die konsistent hohe i/o-Performance und niedrige Latenz benötigen, um e/a-intensive Arbeitslasten host aus Wenn Sie ein Premium-Speicherkonto für Ihre Arbeitslast Datenträger verwenden möchten, müssen Sie Master-Ziel der DS-Serie verwenden. Premium-Datenträger können keine Master-Ziel der DS-Serie.

    >[AZURE.NOTE] Verschieben von Ressourcengruppen über [neue Azure-Portal](../storage/storage-create-storage-account.md) erstellt Speicherkonten wird nicht unterstützt.

    ![vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)

5. Wählen Sie das Konto für den Dienst Mobilität auf geschützten Computern installieren möchten, Konten **Angeben** . Die Anmeldeinformationen werden für die automatische Installation des Dienstes Mobilität. Wenn Sie ein Konto müssen nicht richten Sie eine wie in Schritt2 beschrieben. Beachten Sie, dass dieses Konto von Azure zugegriffen werden kann. Für Windows müssen Server das Konto Administratorrechte auf dem Quellserver. Das Konto muss für Linux Stamm sein.

    ![Linux-Anmeldeinformationen](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)

6. Klicken Sie auf das Häkchen Maschinen zur Schutzgruppe hinzufügen und erste Replikation für jeden Computer starten. Sie können den Status auf der Seite **Projekte** überwachen.

    ![V-Center Server hinzufügen](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)

7. Darüber hinaus können Sie überwachen Schutzstatus auf **Geschützte Elemente** > Schutzgruppenname > **virtuelle Computer** . Nachdem die erste Replikation abgeschlossen ist und der Computer Daten synchronisieren zeigen sie **Protected** Status.

    ![Virtual Machine-Aufträge](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)


### <a name="set-protected-machine-properties"></a>Set geschützte Eigenschaften

1. Nachdem ein Computer **geschützten** Status hat können Sie ihre Failover-Eigenschaften konfigurieren. Gruppendetails Schutz wählen Sie den Computer, und öffnen Sie die Registerkarte **Konfigurieren** .
2. Sie können den Namen ändern, der Computer in Azure nach Failover und Azure VM-Größe angegeben werden. Sie können auch Azure Netzwerk auswählen, mit dem der Computer nach einem Failover verbunden werden.

    > [AZURE.NOTE] [Migration von Netzwerken](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist nicht für Netzwerke für die Bereitstellung von Site Recovery unterstützt.

    ![Festlegen von Eigenschaften für virtuelle Computer](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Beachten Sie Folgendes:

- Der Name der Azure-Computer muss Azure erfüllen.
- Standardmäßig sind nicht replizierte virtuelle Computer in Azure Azure-Netzwerk verbunden. Wenn Sie replizieren möchten sicherstellen VMs zu demselben Azure Netzwerk festgelegt.
- Wenn Sie die Größe eines Volumes auf einem physischen Server geht in einen kritischen Zustand oder virtuellen VMware-Maschine. Folgendermaßen Sie müssen die Größe ändern, vor:

    - (a) ändern Sie die Einstellung.
    - b) auf der Registerkarte **virtuelle Computer** den virtuellen Computer, und klicken Sie auf **Entfernen**.
    - (c) im **virtuellen Computer entfernen** die Option **Deaktivieren Schutz (verwendet für Recovery Bohren und Volume-Größe)**. Diese Option behält Wiederherstellungspunkte in Azure jedoch deaktiviert den Schutz.

        ![Festlegen von Eigenschaften für virtuelle Computer](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)

    - (d) aktivieren Sie Schutz für den virtuellen Computer. Beim Schutz aktivieren werden die Daten für die Größe Datenträger in Azure übertragen.

    

## <a name="step-10-run-a-failover"></a>Schritt 10: Ausführen ein Failovers

Derzeit können nur ungeplante Failovers geschützte virtuelle VMware-Maschinen zu physischen Servern ausgeführt werden. Beachten Sie Folgendes:



- Bevor Sie einen Failover initiieren, sicherstellen Sie, dass Konfiguration und Master Server ausgeführt werden und fehlerfrei sind. Andernfalls schlägt Failover fehl.
- Quell-Computer sind nicht Bestandteil einer ungeplanten Failover heruntergefahren. Durchführen eines ungeplanten Failovers beendet Datenreplikation für die geschützten Server. Sie müssen die Computer aus der Schutzgruppe löschen und erneut hinzufügen, um geschützt Maschinen erneut nach dem ungeplante Failover beendet ist.
- Ggf. Failover ohne Datenverlust unbedingt Primärdatenbank virtuelle Computer deaktiviert werden, bevor das Failover initiieren.

1. **Recovery-Pläne** auf der Seite, und fügen Sie einen Wiederherstellungsplan. Geben Sie Details für den Plan und wählen Sie **Azure** als Ziel aus.

    ![Konfigurieren von Recovery-Plans](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)

2. Wählen Sie im **virtuellen Computer auswählen** eine Schutzgruppe, und wählen Sie Computer aus der Gruppe der Wiederherstellungsplan hinzu. [Mehr](site-recovery-create-recovery-plans.md) über Pläne.

    ![Hinzufügen von virtuellen Computern](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)

3. Wenn Anpassen des Plans zu Gruppen und Reihenfolge der Reihenfolge benötigt der Computer bei der Wiederherstellung Plan Failover werden. Sie können auch Abfragen für manuelle Aktionen und Skripts hinzufügen. Skripts in Azure Wiederherstellung können mithilfe von [Azure Automatisierung Runbooks](site-recovery-runbook-automation.md)hinzugefügt werden.

4. Wählen Sie auf der Seite **Wiederherstellungspläne** den Plan und **Ungeplanten Failover**auf.
5. Failover **Bestätigen** überprüfen Sie Richtung Failover (auf Azure) und wählen Sie Wiederherstellungspunkt Failover zu.
6. Warten Sie dafür Failover und stellen Sie sicher, dass das Failover erwartungsgemäß und replizierte virtuelle Computer in Azure gestartet.




## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Schritt 11: Fehler wieder aus Azure Failover

[Weitere](site-recovery-failback-azure-to-vmware-classic-legacy.md) Informationen zu der fehlgeschlagenen auf Computern Ihrer lokalen Umgebung Azure zurück.


## <a name="manage-your-process-servers"></a>Verwalten von Servern Prozess

Prozess-Server sendet Replikationsdaten zum master Zielserver in Azure und erkennt neuen virtuelle VMware-Maschinen vCenter Server hinzugefügt. In folgenden Fällen sollten Sie den Prozess-Server in der Bereitstellung ändern:

- Wenn der aktuelle Prozessserver ausfällt
- Steigt die Recovery Point Objective (RPO) auf ein inakzeptables Maß für Ihre Organisation.

Ggf. verschieben die Replikation einiger oder aller lokalen virtuellen VMware Maschinen und physischen Server auf einen anderen Prozess-Server. Zum Beispiel:

- **Fehler**, wenn ein Prozess-Server ausfällt oder nicht verfügbar Verschieben Sie geschützte Computer Replikation auf einen anderen Prozess. Metadaten Quelle und replikatcomputer auf den neuen Prozessserver verschoben und Daten synchronisiert. Neue Prozess-Server wird automatisch mit der vCenter Server für automatische Erkennung verbinden. Sie können den Zustand des Prozessserver Site Recovery-Dashboard überwachen.
- **Lastenausgleich anpassen RPO**– verbesserte Lastenausgleich Sie wählen Sie einen anderen Prozessserver im Portal Site Recovery und Replikation von einen oder mehrere Computer ans, manueller Lastenausgleich kann. In diesem Fall wird Metadaten der ausgewählten Quelle und Replikat zum neuen Prozessserver verschoben. Der ursprüngliche Prozessserver bleibt vCenter Server an. 

### <a name="monitor-the-process-server"></a>Überwachen Sie den Prozessserver

Ist ein Prozessserver in einem kritischen Zustand wird eine Warnung auf der Site Recovery-Dashboard angezeigt. Sie können den Status die Registerkarte Ereignisse öffnen und dann bestimmte Projekte auf der Registerkarte Aufträge Drilldown klicken. 

### <a name="modify-the-process-server-used-for-replication"></a>Ändern Sie den Prozess-Server für die Replikation

1. Öffnen Sie **Server** > **Konfigurationsserver** > Server Configuration > **Serverdetails**.
2. Klicken Sie auf **Prozess-Server** > **Prozessserver ändern** neben dem Server, die Sie ändern möchten.

    ![Prozessserver 1 ändern](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)

3. **Änderung Prozess**Server > **Prozess Zielserver** auswählen den neuen Server zu verwenden, wählen Sie die virtuellen Computer, die Sie auf den neuen Server replizieren möchten. Klicken Sie auf das Informationssymbol neben dem Servernamen Speicherplatz und Arbeitsspeicher verwendet. Der durchschnittliche Platz zu jedem ausgewählten virtuellen Computer auf den neuen Prozessserver repliziert wird zu Entscheidungen Laden angezeigt.

    ![Prozessserver 2 ändern](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)

4. Klicken Sie zunächst auf den neuen Prozessserver replizieren. Hinweis: Wenn Sie alle virtuellen Computer von einem Prozessserver entfernen, die kritisch es mehr wichtige Warnung im Dashboard angezeigt werden soll.


## <a name="third-party-software-notices-and-information"></a>Drittanbieter-Software und Hinweise

Nicht übersetzen bzw. lokalisieren

Software und Firmware aus Microsoft-Produkt oder Dienst basiert oder aus der unten aufgeführten Projekte enthält (zusammen "Drittanbieter-Code).  Microsoft ist nicht die ursprünglichen Autor Drittanbieter-Code.  Ursprüngliche Urheberrechtshinweis und Lizenz unter dem Microsoft solcher Drittanbieter-Code empfangen werden unten festgelegt.

Die Informationen in Abschnitt A wird dritten Codekomponenten aufgeführten Projekte. Solche Lizenzen und Informationen dienen nur zu Informationszwecken.  Dieser dritte wird Ihnen von Microsoft unter Microsoft Software Lizenzierungsinformationen für das Microsoft-Produkt oder Dienst nachlizenziert wird.  

Die in Abschnitt B ist Drittanbieter Codekomponenten Informationen, die Ihnen von Microsoft unter der ursprünglichen Lizenzierungsinformationen bereitgestellt werden.

Die vollständige Datei finden im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft behält sich alle hierin nicht ausdrücklich gewährten Rechte durch Implikation, weder ausdrücklich noch konkludent oder auf andere Weise.
