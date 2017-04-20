<properties
    pageTitle="Virtuelle VMware-Maschinen und physische Server in Azure mit Azure Site Recovery replizieren | Microsoft Azure"
    description="Dieser Artikel beschreibt das Bereitstellen von Azure Site Recovery, Replikation, Failover und Recovery von lokalen virtuellen VMware Maschinen und Windows-Linux-Servern in Azure koordinieren."
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

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>Replizieren Sie virtuelle VMware-Maschinen und physische Server in Azure mit Azure Site Recovery

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmware-to-azure.md)
- [Verwaltungsportal](site-recovery-vmware-to-azure-classic.md)
- [Verwaltungsportal (Legacy)](site-recovery-vmware-to-azure-classic-legacy.md)


Azure Site Recovery-Dienst trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern und Servern orchestriert. Computer können Azure oder einem lokalen sekundären Rechenzentrum repliziert werden. Finden Sie einen schnellen Überblick über [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md).

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie Sie:

- **Replizieren von VMware virtuelle Computer in Azure**-Bereitstellung Site Recovery, Replikation, Failover und Recovery von lokalen virtuellen VMware Maschinen Azure-Speicher zu koordinieren.
- **Replizieren von physischen Servern in Azure**-Bereitstellung Azure Site Recovery, Replikation, Failover und Recovery von lokalen Windows und Linux Servern in Azure koordinieren.

>[AZURE.NOTE] Dieser Artikel beschreibt das in Azure replizieren. Wenn Sie virtuelle VMware-Computer oder Windows-Linux-Servern auf einem sekundären Rechenzentrum replizieren möchten, gehen Sie in [diesem Artikel](site-recovery-vmware-to-vmware.md).

Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="enhanced-deployment"></a>Verbesserte Bereitstellung

Dieser Artikel enthält eine Anleitung für eine verbesserte Bereitstellung im klassischen Azure-Portal. Wir empfehlen diese Version für alle neuen Installationen verwenden. Früheren legacy-Version bereits bereitgestellt haben sollten, auf die neue Version migrieren. Lesen Sie [Weitere](site-recovery-vmware-to-azure-classic-legacy.md##migrate-to-the-enhanced-deployment) Informationen zur Migration.

Verbesserte Bereitstellung ist eine wichtige Aktualisierung. Hier ist eine Zusammenfassung der Verbesserungen haben:

- **Keine Infrastruktur VMs in Azure**: repliziert Daten direkt an ein Konto der Azure-Speicher. Replikation und Failover ist außerdem müssen Infrastruktur VMs (Server Configuration, master Zielserver) einrichten mussten in älteren Bereitstellung.  
- **Einheitliche Installation**: eine einzelne Installation bietet einfache Installation und Skalierbarkeitslösungen für lokalen Komponenten.
- **Sichere Bereitstellung**: der gesamte Datenverkehr wird verschlüsselt und Replikation Kommunikation über HTTPS 443 gesendet werden.
- **Wiederherstellungspunkte**: Unterstützung für Absturz und anwendungskonsistente Recovery Punkte für Windows und Linux-Umgebungen und unterstützt sowohl single-VM und Multi-VM konsistente Konfigurationen.
- **Testen des Failoververhaltens**: Unterstützung für unterbrechungsfreie testfailover in Azure ohne Beeinträchtigung der Produktion Replikation anhalten.
- **Ungeplantes Failover**: Unterstützung für ungeplanten Failover in Azure mit einer erweiterten Option VMs automatisch vor dem Failover Herunterfahren.
- **Failback**: integrierte Failback, die Delta-Änderungen an den lokalen Standort repliziert.
- **vSphere 6.0**: eingeschränkte Unterstützung für VMware Vsphere 6.0 Bereitstellungen.


## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Wie zum Site Recovery Schutz von virtuellen Computern und Servern?


- VMware-Administratoren können externe Schutz in Azure Business Arbeitslasten und auf virtuellen VMware Maschinen ausgeführt. Server-Manager können in Azure physischen lokalen Windows- und Linux-Server replizieren.
- Azure Site Recovery-Konsole bietet eine zentrale Stelle für einfache Einrichtung und Verwaltung der Replikation, Failover und Recovery-Prozesse.
- Wenn Sie virtuelle VMware-Computer, die von einem vCenter Server verwaltet werden replizieren, können Site Recovery die VMs automatisch erkennen. Wenn Computer auf ESXi Host ermittelt Site Recovery VMs auf dem Host.
- Führen Sie einfach Failover Ihrer lokalen Infrastruktur Azure und Failback (Wiederherstellen) von Azure VM VMware Server am lokalen Standort.
- Konfigurieren Sie das Gruppieren von Arbeitslasten, die auf mehreren Computern tiered Recovery-Pläne. Diese Pläne Failover und Site Recovery bietet Multi-VM, Computern der gleiche Arbeitslast gemeinsam auf konsistente Daten wiederhergestellt werden können.


## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

### <a name="windows64-bit-only"></a>Windows-(64 Bit)
- Windows Server 2008 R2 SP1 +
- Windows Server 2012
- Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (64 Bit)
- Red Hat Enterprise Linux 6.7, 7.1, 7.2
- CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2
- Oracle Enterprise Linux 6.4, 6.5 unter Red Hat kompatibel Kernel oder unverwüstliche Enterprise Kernel-Version 3 (UEK3)
- SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Szenario-Architektur

Szenariokomponenten:

- **Einen lokalen Verwaltungsserver**: der Verwaltungsserver führt Site Recovery Komponenten:
    - **Konfigurationsserver**: koordiniert Kommunikation und Daten-Replikation und Recovery-Prozesse.
    - **Prozessserver**: fungiert als Gateway Replikation. Er empfängt Daten von geschützten Datenquelle Maschinen, zwischenspeichern, Komprimierung und Verschlüsselung optimiert und Azure-Speicher Replikationsdaten an. Auch Push-Installation des mobilitätsservice geschützten Computern behandelt und führt die automatische Ermittlung für virtuelle VMware-Computer.
    - **Master-Zielserver**: Replikationsdaten während des Failbacks von Azure behandelt.
    Sie können auch einen Management-Server bereitstellen, der nur ein Prozessserver fungiert, um Ihre Bereitstellung zu skalieren.
- **Die Mobility Service**: Diese Komponente wird auf jedem Computer (VMware VM oder physische Server), die Sie in Azure replizieren möchten bereitgestellt. Schreibvorgänge auf dem Computer erfasst und an den Prozess-Server weiterleitet.
- **Azure**: Sie brauchen Azure VMs für Replikation und Failover erstellen. Site Recovery Service behandelt Datenmanagement und repliziert Daten direkt in Azure-Speicher. Replizierte Azure VMs werden automatisch erstellt wird, nur bei einem Failover in Azure. Möchten Sie von Azure auf der lokalen Website fehl müssen Sie eine Azure-VM als Prozessserver einrichten.


Die Grafik zeigt, wie diese Komponenten interagieren.

![Architektur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Abbildung 1: VMware/physische Azure** (Henry Robalino erstellt)


## <a name="capacity-planning"></a>Planen der Kapazität

Wenn Sie Kapazität, hier planen müssen überlegen:

- **Der Quell-Umgebung**– Planung oder die Anforderungen an VMware Infrastruktur und Quelle.
- **Der Management-Server**– Planung für die lokalen Verwaltungsserver, die Website Komponenten ausgeführt.
- **Netzwerkbandbreite Quelle und Ziel**-Netzwerkbandbreite für die Replikation zwischen Quell- und Azure planen

### <a name="source-environment-considerations"></a>Aspekte der Quell-Umgebung

- **Maximale tägliche Änderungsrate**-geschützter Computer können nur ein Prozessserver und ein einzelnen Prozess-Server kann bis zu 2 TB pro Tag ändern behandeln. 2 TB ist somit maximalen täglichen Daten ändern, die für einen geschützten Computer unterstützt wird.
- **Maximale Durchsatz**– ein Speicherkonto in Azure kann replizierter Computer gehören. Standard Speicherkonto kann bis zu 20.000 Anfragen pro Sekunde verarbeiten, und es wird empfohlen, die Anzahl von IOPS über ein Quellcomputer 20.000. Für Beispiel haben Sie einen Quellcomputer mit 5 Festplatten und jedes Laufwerk 120 IOPS (8 KB Größe) der Quelle wird dann werden in Azure pro Datenträger IOPS 500. Die Anzahl der Speicherkonten erforderlich = total Quelle IOPs-20000.


### <a name="management-server-considerations"></a>Gesichtspunkte der server

Der Verwaltungsserver führt Site Komponenten, die datenoptimierung, Replikation und Management behandelt. Soll die Änderung Ratenkapazität über alle Arbeitslasten, die auf geschützten Computern behandeln und ausreichend Bandbreite kontinuierlich Datenreplikation Azure-Speicher. Insbesondere:

- Prozess-Server empfängt der Replikation von geschützten Computern und zwischenspeichern, Komprimierung und Verschlüsselung vor dem Senden in Azure optimiert. Der Verwaltungsserver müssen ausreichende Ressourcen zum Ausführen dieser Aufgaben.
- Prozess-Server verwendet datenträgerbasierten Caches. Wir empfehlen eine separate Cachedatenträger 600 GB oder mehr Daten Änderungen bei einem Ausfall des Netzwerks auf Engpässe gespeichert. Während der Bereitstellung können Sie den Cache auf jedem Laufwerk mit mindestens 5 GB Speicherplatz, aber 600 GB ist die minimale Empfehlung.
- Als bewährte Methode empfiehlt der Verwaltungsserver befinden sich im gleichen Netzwerk und LAN-Segment wie die Computer zu schützen. Kann in einem anderen Netzwerk befinden aber zu schützenden Computer sollten Netzwerktransparenz, L3.

Empfohlene Größe für den Management-Server werden in der folgenden Tabelle zusammengefasst.

**Management-Server-CPU** | **Speicher** | **Datenträger-Cachegröße** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Steckplätze * 4 Kernen @ 2,5 GHz) | 16 GB | 300 GB | 500 GB oder weniger | Bereitstellen von einem Verwaltungsserver zu weniger als 100 Computer replizieren.
12 vCPUs (2 Steckplätze * 6 Kerne @ 2,5 GHz) | 18 GB | 600 GB | 500 GB bis 1 TB | Bereitstellen Sie Verwaltungsserver zu Replikation zwischen 100-150 Computern.
16 vCPUs (2 Steckplätze * 8 Kernen @ 2,5 GHz) | 32 GB | 1 TB | 1 TB und 2 TB | Bereitstellen Sie Verwaltungsserver zu Replikation zwischen 150-200 Computer.
Bereitstellen von einem anderen Prozessserver | | | > 2 TB | Bereitstellen Sie weitere Server, sind mehr als 200 Computer replizieren oder ändert die täglichen Daten 2 TB übersteigt.

Wo:

- Jede Quellcomputer wird mit 3 100 GB-Datenträger konfiguriert.
- Benchmarking Speicher 8 SAS-Laufwerke mit 10 K u/Min verwendet mit RAID 10 für Cache Datenträger Maßeinheiten.

### <a name="network-bandwidth-from-source-to-target"></a>Netzwerkbandbreite Quelle Ziel
Sicherstellen Sie, dass die Bandbreite zu berechnen, die für die erste Replikation und Deltareplikation mit [Kapazitätsplanertools](site-recovery-capacity-planner.md)

#### <a name="throttling-bandwidth-used-for-replication"></a>Einschränken der Bandbreite für die Replikation

VMware Netzwerkverkehr auf Azure repliziert über einen bestimmten Prozess-Server. Sie können die Bandbreite einschränken, die für die Site Recovery-Replikation auf dem Server wie folgt:

1. Öffnen bereitgestellt Microsoft Azure Backup MMC-Snap-in auf dem Haupt-Server oder auf einem Verwaltungsserver Ausführen zusätzlicher Prozess-Server. Standardmäßig eine Verknüpfung für Microsoft Azure Backup auf Desktop erstellt oder finden sie in: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klicken Sie im Snap-in- **Eigenschaften ändern**.

    ![Bandbreite](./media/site-recovery-vmware-to-azure-classic/throttle1.png)

3. Geben Sie auf der Registerkarte **Beschränkung** der Bandbreite für Site Recovery Replikation und die entsprechende Planung verwendet werden kann.

    ![Bandbreite](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Optional können Sie auch festlegen, mit PowerShell-Drosselung. Hier ist ein Beispiel:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Maximierung der Nutzung der Netzwerkbandbreite
Um die Bandbreite für die Replikation von Azure Site Recovery verwendet, müssen Sie einen Registrierungsschlüssel ändern.

Der folgende Schlüssel steuert die Anzahl der Threads pro Datenträger replizieren, die bei der Replikation verwendet werden

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 Dieser Registrierungsschlüssel muss in einem Netzwerk "übermäßiger" die Standardwerte geändert werden. Wir unterstützen maximal 32.  


[Erfahren Sie mehr](site-recovery-capacity-planner.md) über detaillierte Planung.

### <a name="additional-process-servers"></a>Weitere Server

Wenn Sie mehr als 200 Computer schützen oder tägliche Änderungsrate größer ist als 2 TB können Sie Hinzufügen zusätzlicher Server, damit die Last bewältigt. Skalieren Sie können:

- Die Zahl der Verwaltungsserver. Beispielsweise können Sie bis zu 400 Maschinen mit zwei Verwaltungsservern schützen.
- Weitere Server hinzufügen und diese den Verkehr statt (oder zusätzlich) verwenden den Verwaltungsserver.

Ein Szenario in der Tabelle:

- Ursprüngliche Management Server einrichten für die Verwendung als Konfigurationsserver nur.
- Ein zusätzlicher Server einrichten.
- Geschützte virtuelle Maschinen Verwendung zusätzlicher Prozess-Server konfigurieren.
- Jede geschützte Quellcomputer wird mit drei Datenträgern von 100 GB konfiguriert.

**Ursprüngliche Verwaltungsserver**<br/><br/>(Configuration Server) | **Weitere server**| **Datenträger-Cachegröße** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Steckplätze * 4 Kernen @ 2,5 GHz), 16 GB Speicher | 4 vCPUs (2 Steckplätze * 2 Kernen @ 2,5 GHz), 8 GB Speicher | 300 GB | 250 GB oder weniger | Sie können 85 oder weniger Computer replizieren.
8 vCPUs (2 Steckplätze * 4 Kernen @ 2,5 GHz), 16 GB Speicher | 8 vCPUs (2 Steckplätze * 4 Kernen @ 2,5 GHz), 12 GB Speicher | 600 GB | 250 GB bis 1 TB | Sie können zwischen 85 150 Computer replizieren.
12 vCPUs (2 Steckplätze * 6 Kerne @ 2,5 GHz), 18 GB Speicher | 12 vCPUs (2 Steckplätze * 6 Kerne @ 2,5 GHz) 24 GB Arbeitsspeicher | 1 TB | 1 TB und 2 TB | Sie können zwischen 150-225 Computer replizieren.


Die Möglichkeit, in der Sie Ihre Server skalieren, hängen von der Einstellung für eine Einrichtung oder Modell skalieren.  Sie einige High-End-Management und Prozess-Server skalieren oder Skalieren von mehr Server mit weniger Ressourcen bereitstellen. Beispiel: 220 Computer schützen soll möglich Folgendes:

- Konfigurieren Sie den ursprünglichen Verwaltungsserver mit 12vCPU, 18 GB Speicher, eine weitere Server mit 12vCPU, 24 GB Speicher und konfigurieren Sie geschützten Computer Verwendung zusätzlicher Prozess-Server.
- Oder auch Sie konnte zwei Verwaltungsservern (2 X 8vCPU, 16 GB RAM) und zwei weitere Server (1 x 8vCPU) und 4vCPU x 1 135 + 85 (220) Maschinen konfigurieren und geschützten Computer, die zusätzliche Prozessserver verwenden.


Einrichten eines Servers zusätzliche Prozess [gehen](#deploy-additional-process-servers) .

## <a name="before-you-start-deployment"></a>Vor der Bereitstellung

Tabellen werden die erforderlichen Komponenten für die Bereitstellung dieses Szenario zusammengefasst.


### <a name="azure-prerequisites"></a>Azure-Komponenten

**Voraussetzung** | **Details**
--- | ---
**Ein Azure-Konto**| Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.
**Azure-Speicher** | Sie benötigen ein Konto Azure-Speicher zum Speichern von replizierter Daten. Replizierte Daten in Azure-Speicher gespeichert und Azure VMs werden erstellt, wenn ein Failover auftritt. <br/><br/>Sie benötigen ein [standard Geo redundante Speicherkonto](../storage/storage-redundancy.md#geo-redundant-storage). Das Konto muss im Bereich der Site Recovery-Dienst sein und dieselbe Abonnement zugeordnet werden. Beachten Sie, dass die Replikation auf Premium-Speicherkonten wird derzeit nicht unterstützt und sollte nicht verwendet werden.<br/><br/>Verschieben von Ressourcengruppen über [neue Azure-Portal](../storage/storage-create-storage-account.md) erstellt Speicherkonten wird nicht unterstützt. [Informationen über](../storage/storage-introduction.md) Azure-Speicher.<br/><br/>
**Azure-Netzwerk** | Sie benötigen Azure virtual Network, der Azure-VMs verbunden wird, wenn ein Failover auftritt. Azure virtuelle Netzwerk muss im Bereich der Site Recovery-Tresor.<br/><br/>Beachten Sie, dass nicht nach Failover auf Azure VPN benötigen Verbindung (oder Azure ExpressRoute) eingerichtet Azure Netzwerk zum lokalen Standort.


### <a name="on-premises-prerequisites"></a>Lokalen Komponenten

**Voraussetzung** | **Details**
--- | ---
**Verwaltungsserver** | Sie benötigen einen lokale Windows 2012 R2 Server auf einem virtuellen oder physischen Server ausgeführt. Alle lokalen Site Recovery Komponenten werden auf diesem Verwaltungsserver installiert.<br/><br/> Wir empfehlen, den Server als eine hochverfügbare VMware VM bereitstellen. Failback zum lokalen Standort von Azure ist immer auf virtuelle VMware-Computer unabhängig davon, ob Sie VMs oder physischen Servern ausgeführt werden. Wenn den Verwaltungsserver nicht als VMware VM konfigurieren müssen Sie ein separates master Zielserver als VMware VM einrichten Failback Datenverkehr empfangen.<br/><br/>Der Server sollte kein Domänencontroller sein.<br/><br/>Der Server sollte eine statische IP-Adresse verfügen.<br/><br/>Der Hostname des Servers sollte 15 Zeichen oder weniger.<br/><br/>Gebietsschema des Betriebssystems sollte nur Englisch.<br/><br/>Der Verwaltungsserver ist Internetzugriff erforderlich.<br/><br/>Sie benötigen ausgehend vom Server wie folgt: temporären Zugriff auf HTTP-80 während der Installation von Site Recovery Komponenten (MySQL herunterladen); Laufende ausgehenden Zugriff auf HTTPS 443 für Replikations-Management; Laufende ausgehenden Zugriff auf HTTPS-9443 für den Replikationsverkehr (dieser Port kann geändert werden)<br/><br/> Stellen Sie sicher, dass diese URLs vom Verwaltungsserver zugegriffen werden: <br/>- \*. hypervrecoverymanager.windowsazure.com<br/>- \*. accesscontrol.windows.net<br/>- \*. backup.windowsazure.com<br/>- \*. blob.core.windows.net<br/>- \*. store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi]( https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Haben Sie Firewallregeln mit IP-Adresse auf dem Server sicher, dass Regeln in Azure Kommunikation ermöglichen. Sie müssen [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653) und HTTPS (443)-Anschluss. Sie müssen auch auf die weiße Liste IP-Adressbereiche für Azure Region Ihres Abonnements und Westen der USA. URL- [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") ist für den Download von MySQL.
**VMware vCenter-ESXi Host**: | Benötigen Sie eine oder mehrere vMware vSphere ESX/ESXi Hypervisoren Verwalten Ihrer virtuellen VMware Maschinen unter ESX/ESXi Version 6.0, 5.5 oder 5.1 mit den neuesten Updates.<br/><br/> Wir empfehlen vCenter VMware ESXi Hosts verwalten Server bereitstellen. Es sollte vCenter, Version 6.0 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>Beachten Sie, dass Site Recovery keine neuen vCenter unterstützt und vSphere 6.0-Funktionen wie vMotion vCenter, virtuelle Volumes, und Storage DRS. Unterstützung von Recovery ist auf Features, die auch in Version 5.5 verfügbar waren.
**Geschützt Maschinen**: | **AZURE**<br/><br/>Zu schützenden Computer sollten [Azure Komponenten](site-recovery-best-practices.md#azure-virtual-machine-requirements) zum Erstellen von Azure VMs entsprechen.<br><br/>Azure-VMs nach einem Failover herstellen möchten, müssen Sie Remotedesktopverbindungen lokale Firewall aktivieren.<br/><br/>Einzelne Festplattenkapazität auf geschützten Computern sollte nicht mehr als 1023 GB sein. Eine VM können bis zu 64 Festplatten (also bis zu 64 TB). Wenn Sie Erwägung größer als 1 TB Laufwerke Replikation oder SQL Server immer auf Oracle Data Guard.<br/><br/>Mindestens 2 GB freier Speicher auf dem Installationslaufwerk für Komponenteninstallation.<br/><br/>Freigegebene Datenträger Gast Clustern nicht unterstützt. Wenn Sie eine gruppierte Bereitstellung Erwägung Replikation oder SQL Server immer auf Oracle Data Guard.<br/><br/>Extensible Firmware Interface (UEFI) Unified Extensible Firmware Interface(EFI) Boot wird nicht unterstützt.<br/><br/>Computernamen sollten zwischen 1 und 63 Zeichen (Buchstaben, Zahlen und Bindestriche) enthalten. Der Name muss mit einem Buchstaben oder einer Zahl beginnen und enden mit einem Buchstaben oder einer Zahl. Nachdem ein Computer geschützt können Sie Azure Namen ändern.<br/><br/>**Virtuelle VMware-Computer**<br/><br>Sie müssen VMware vSphere PowerCLI 6.0 installieren. auf den Management Server (Konfiguration).<br/><br/>Virtuelle VMware-Computer schützen möchten, müssen VMware Tools installiert und ausgeführt.<br/><br/>Wenn die VM NIC-teaming hat wird nach einem Failover in Azure einer NIC konvertiert.<br/><br/>Haben geschützte VMs iSCSI-Datenträger konvertiert dann Site Recovery geschützten VM iSCSI-Datenträger in eine VHD-Datei bei die VM in Azure Failover. ISCSI-Ziel von Azure VM erreichbar wird es verbinden iSCSI-Ziel und sehen im Wesentlichen zwei Festplatten-VHD Datenträger auf der Azure-VM und iSCSI Quelllaufwerk. In diesem Fall müssen Sie das iSCSI-Ziel zu trennen, das fehlgeschlagenen über Azure VM angezeigt wird.<br/><br/>[Erfahren Sie mehr](#vmware-permissions-for-vcenter-access) über VMware Benutzerberechtigungen, die Site Recovery erforderlich sind.<br/><br/> **WINDOWS SERVER-Computer (auf VMware VM oder physischen Server)**<br/><br/>Sollte der Server unterstütztes 64-Bit-Betriebssystem ausgeführt werden: Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 mit am wenigsten SP1.<br/><br/>Das Betriebssystem sollte auf Laufwerk C:\ und Betriebssystem-Datenträger sollte eine Windows-Basisfestplatte (OS sollte nicht auf einer dynamischen Festplatte Windows installiert.)<br/><br/>Für Windows Server 2008 R2-Computern müssen Sie.NET Framework 3.5.1 installiert haben.<br/><br/>Sie müssen ein Administratorkonto angeben (muss ein lokaler Administrator auf dem Computer Windows) für die Push-Installation des Mobility auf Windows-Servern. Wenn das angegebene Konto ein Domänenkonto ist müssen Sie RAS-Benutzer auf dem lokalen Computer deaktivieren. [Erfahren Sie mehr](#install-the-mobility-service-with-push-installation).<br/><br/>Site Recovery unterstützt VMs mit RDM-Datenträger.  Während des Failbacks wird Site Recovery Disk RDM Wiederverwenden der ursprünglichen Quelle VM und RDM Datenträger verfügbar ist. Wenn sie nicht verfügbar sind, während des Failbacks erstellt Site Recovery eine neue VMDK-Datei für jeden Datenträger.<br/><br/>**LINUX-MASCHINEN**<br/><br/>Sie benötigen einen unterstützten 64-Bit-Betriebssystem: Red Hat Enterprise Linux 6.7; CentOS 6.5 6.6,6.7; Oracle Enterprise Linux 6.4, 6.5 kompatibel Kernel Red Hat oder unverwüstliche Enterprise Kernel-Version 3 (UEK3), SUSE Linux Enterprise Server 11 SP3 ausgeführt.<br/><br/>/ etc/hosts-Dateien auf geschützten Computern sollten Einträge enthalten, die den lokalen Hostnamen IP-Adressen aller Netzwerkadapter zugeordnet. <br/><br/>Ein Azure virtuellen Computer mit Linux nach einem Failover mit einem Secure Shell-Client (ssh) eine Verbindung herstellen möchten, stellen Sie sicher, dass Secure Shell-Dienst auf dem geschützten Computer automatisch beim Systemstart starten und Firewallregeln zulassen einer ssh Verbindung.<br/><br/>Schutz kann nur für Computer mit den folgenden Linux aktiviert: Dateisystem (EXT3 ETX4 ReiserFS, XFS); Multipath-Software Gerät Mapper (Multipfad)); Volume-Manager: (LVM2). Physische Server mit HP CCISS Controller werden nicht unterstützt. ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.<br/><br/>Site Recovery unterstützt VMs mit RDM-Datenträger.  Während des Failbacks für Linux verwendet Site Recovery Disk RDM. Stattdessen wird eine neue VMDK-Datei für jeden entsprechenden RDM-Datenträger erstellt.

Nur für Linux VM - sicherstellen Sie, dass Sie die disk.enableUUID=true im von der VM in VMware eingestellt. Wenn diese Zeile nicht vorhanden ist, fügen Sie sie hinzu. Dies ist erforderlich, eine einheitliche UUID der VMDK-Datei bereitstellen, damit ordnungsgemäß bereitgestellt. Beachten Sie auch, dass ohne diese Einstellung Failback einen vollständigen Download führt wird die VM auf Prem verfügbar. Hinzufügen von dieser Einstellung wird sichergestellt, dass nur Delta ändert sich beim Failback übertragen werden.

## <a name="step-1-create-a-vault"></a>Schritt 1: Erstellen eines Depots

1. Melden Sie sich im [Verwaltungsportal](https://manage.windowsazure.com/).
2. Erweitern Sie **Data Services** > **Recovery Services** und **Site Recovery Depot**auf.
3. Klicken Sie auf **neue** > **schnell erstellen**.
4. Geben Sie im Feld **Name**einen Anzeigenamen zu Tresor.
5. Wählen Sie im **Bereich**geografische Region für das Depot. Überprüfen Sie unterstützte Regionen finden Sie unter geografische Verfügbarkeit in [Azure Site Recovery Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Klicken Sie auf **Create Tresor**.
    ![Neues Depot](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Überprüfen Sie die Statusleiste, um sicherzustellen, dass das Depot erfolgreich erstellt wurde. Das Depot wird als **aktiv** auf **Recovery Services** aufgeführt.

## <a name="step-2-set-up-an-azure-network"></a>Schritt 2: Einrichten von Azure-Netzwerk

Ein Azure Netzwerk richten Sie ein, damit Azure VMs nach einem Failover mit einem Netzwerk verbunden wird, und Failback zum lokalen Standort erwartungsgemäß kann.

1. Im Azure-Portal > **virtuelles Netzwerk erstellen** Geben Sie den Namen. IP-Adressbereich und Subnetzmaske Adressnamen.
2. Sie müssten VPN-ExpressRoute zum Netzwerk hinzufügen, wenn Sie Failback müssen. VPN-ExpressRoute können auch nach einem Failover zum Netzwerk hinzugefügt werden.

[Mehr](../virtual-network/virtual-networks-overview.md) über Azure-Netzwerke.

> [AZURE.NOTE] [Migration von Netzwerken](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist nicht für Netzwerke für die Bereitstellung von Site Recovery unterstützt.

## <a name="step-3-install-the-vmware-components"></a>Schritt 3: Installieren Sie VMware-Komponenten

Wenn Sie virtuelle VMware replizieren möchten installieren Maschinen die folgenden VMware-Komponenten auf dem Verwaltungsserver:

1. [Herunterladen](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) und installieren VMware vSphere PowerCLI 6.0.
2. Starten Sie den Server neu.


## <a name="step-4-download-a-vault-registration-key"></a>Schritt 4: Herunterladen Sie einen Vault-Registrierungsschlüssel

1. Vom Management Server Site Recovery Console in Azure öffnen. Der **Recovery-** Seite klicken Sie auf das Depot zum Öffnen der Seite Quick Start. Quick-Start kann auch jederzeit auf das Symbol geöffnet werden.

    ![Schnellstart-Symbol](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)

2. Klicken Sie auf **Quick Start** **Vorbereiten Ressourcen** > **Registrierungsschlüssel herunterladen**. Die Datei wird automatisch generiert. Es gilt für 5 Tage nach generiert wird.


## <a name="step-5-install-the-management-server"></a>Schritt 5: Installieren des Management-Servers
> [AZURE.TIP] Stellen Sie sicher, dass diese URLs vom Verwaltungsserver zugegriffen werden:
>
- *. hypervrecoverymanager.windowsazure.com
- *. accesscontrol.windows.net
- *. backup.windowsazure.com
- *. blob.core.windows.net
- *. store.core.windows.net
- https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi
- https://www.msftncsi.com/ncsi.txt




[AZURE.VIDEO enhanced-vmware-to-azure-setup-registration]

1. Downloaden Sie die einheitliche Installationsdatei auf **Schnellstart** zum Server.

2. Führen Sie die Installationsdatei Setup Site Recovery Unified Setup-Assistenten starten.

3.  Wählen Sie im **vor** **Konfigurationsserver und Prozess-Server installieren**.

    ![Bevor Sie beginnen](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. **Drittanbieter-Software-Lizenz** klicken Sie auf **ich stimme** zum Herunterladen und Installieren von MySQL. 

    ![Dritte = Software anderer Hersteller](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)

5. **Registrierung** durchsuchen Sie, und wählen Sie den Registrierungsschlüssel, die, den Sie aus dem Tresor heruntergeladen.

    ![Registrierung](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)

6. Angegeben Sie **Internet** wie der Anbieter auf dem Konfigurationsserver Azure Site Recovery über das Internet verbunden werden.

    - Möchten Sie mit dem Proxy herstellen, die derzeit auf dem Computer eingerichtet wählen Sie **Verbindung mit vorhandenen Proxyeinstellungen**.
    - Soll den Anbieter direkt wählen Sie **Verbinden direkt ohne Proxy aus**
    - Wenn vorhandene Proxy Authentifizierung erfordert oder einen benutzerdefinierten Proxy für die Verbindung zu einem Internetdienstanbieter verwenden möchten, wählen Sie **Verbindung mit Standardeinstellungen**.
        - Verwenden Sie einen benutzerdefinierten Proxy müssen Sie die Adresse, Anschluss und Anmeldeinformationen angeben
        - Wenn Sie einen Proxy verwenden sollten Sie die folgenden URLs bereits zulässig:
            - *. hypervrecoverymanager.windowsazure.com;    
            - *. accesscontrol.windows.net; 
            - *. backup.windowsazure.com; 
            - *. blob.core.windows.net; 
            - *. store.core.windows.net
            

    ![Firewall](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

7. **Erforderliche** Check führt Setup einen sicherstellen, dass die Installation ausgeführt werden kann. 

    
    ![Erforderliche Komponenten](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Erscheint eine Warnung über den **globalen Synchronisierung überprüfen** überprüfen Sie, dass die Zeit der Systemuhr (**Datum und Uhrzeit** korrekt) Zeitzone identisch.

    ![TimeSyncIssue](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

8. **MySQL** -Konfiguration erstellen Sie Anmeldeinformationen für die Anmeldung an der MySQL-Server-Instanz, die installiert werden.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)

9. **Details zur Umgebung** wählen, ob wirst du virtuelle VMware-Computer replizieren. Sind, sucht Setup PowerCLI 6.0 installiert ist.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)

10. **Installationsordner** wählen zu Binärdateien Cache gespeichert werden soll. Wählen Sie ein Laufwerk mit mindestens 5 GB Speicherplatz, doch empfehlen wir ein Cachelaufwerk mit mindestens 600 GB freien Speicherplatz.

    ![Installation](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)

11. Geben Sie **Netzwerkauswahl** Listener (Netzwerkadapter und SSL-Anschluss) auf dem Konfigurationsserver senden und Empfangen von Replikationsdaten. Ändern Sie die Standard-port (9443). Neben diesen Port Port 443 von einem Webserver dienen die Replikationsvorgänge koordiniert. 443 sollte für den Empfang von Replikation verwendet werden.


    ![Netzwerkauswahl](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



12.  **Zusammenfassung** Informationen Sie der und klicken Sie auf **Installieren**. Nach Abschluss der Installation wird eine Passphrase generiert. Sie benötigen bei Replikation aktivieren so kopieren und an einem sicheren Ort aufbewahren.

    ![Zusammenfassung](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)



13.  **Zusammenfassung** Informationen Sie der.

    ![Zusammenfassung](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)

>[AZURE.WARNING]Microsoft Azure Service Wiederherstellungsagenten Proxy benötigt werden.
>Nach Abschluss die Installation starten Sie eine Anwendung namens "Microsoft Azure Recovery Services Shell" im Windows Startmenü. Führen Sie im Befehlsfenster öffnet die folgende Gruppe von Befehlen Proxyeinstellungen einrichten.
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine



### <a name="run-setup-from-the-command-line"></a>Führen Sie Setup von der Befehlszeile aus

Sie können auch den einheitlichen Assistenten über die Befehlszeile wie folgt:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Wo:

- / ServerMode: obligatorisch. Gibt an, ob die Installation der Konfiguration und Prozess oder den Prozess-Server installieren (zum Installieren zusätzlicher Server). Eingabewerte: CS PS
- InstallDrive: obligatorisch. Gibt den Ordner an, in dem die Komponenten installiert sind.
- / MySQLCredFilePath. Obligatorisch. Gibt den Pfad zu einer Datei, MySQL Serveranmeldeinformationen Geschichte. Rufen Sie die Vorlage zum Erstellen der Datei ab.
- / VaultCredFilePath. Obligatorisch. Speicherort der Datei vault
- / EnvType. Obligatorisch. Installationstyp. Werte: VMware, NonVMware
- / PSIP und /CSIP. Obligatorisch. IP-Adresse des Prozessservers und Konfigurationsserver.
- / PassphraseFilePath. Obligatorisch. Speicherort der Datei Passphrase.
- / ByPassProxy. Optional. Gibt an, dass der Verwaltungsserver in Azure einen Proxyserver herstellt.
- / ProxySettingsFilePath. Optional. Gibt Optionen für einen benutzerdefinierten Proxy (Standardproxy auf dem Server, der Authentifizierung erfordert) oder benutzerdefinierten proxy




## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Schritt 6: Anmeldeinformationen für den vCenter Server einrichten

> [AZURE.VIDEO enhanced-vmware-to-azure-discovery]

Prozess-Server kann automatisch virtuelle VMware-Computer erkennen, die von einem vCenter Server verwaltet werden. Automatische Erkennung Bedarf Site Recovery ein Konto und die Anmeldeinformationen, die vCenter Server zugreifen können. Dies ist nicht relevant, wenn physische Server repliziert werden.

Hierzu wie folgt:

1. Auf dem vCenter Server eine Rolle (**Azure_Site_Recovery**) Ebene vCenter mit den [erforderlichen Berechtigungen](#vmware-permissions-for-vcenter-access)erstellen.
2. Einem Benutzer vCenter **Azure_Site_Recovery** Rolle zuweisen.

    >[AZURE.NOTE] Eine vCenter Benutzerkonto mit Schreibschutz-Rolle kann Failover ohne Quelle geschützt Maschinen ausführen. Möchten Sie diese Computer herunterfahren benötigen Sie die Rolle Azure_Site_Recovery. Beachten Sie, dass nur VMs von VMware in Azure migrieren und Failback musst dann die schreibgeschützte Rolle ausreichend ist.

3. Fügen Sie das Konto **Cspsconfigtool**öffnen. Ist als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner \home\svsystems\bin [INSTALLATIONSORT].
2. Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic/credentials1.png)

3. Fügen Sie in den **Kontodetails** Anmeldeinformationen Zugriff auf den Server vCenter hinzu Beachten Sie, dass es mehr als 15 Minuten für den Kontonamen im Portal angezeigt werden kann. Um sofort zu aktualisieren, klicken Sie auf der Registerkarte **Konfiguration** aktualisieren.

    ![Details](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Schritt 7: Hinzufügen von vCenter Server und Hosts mit ESXi

Wenn Sie virtuelle VMware-Computer Sie ein vCenter Server (oder ESXi Host) hinzufügen Replikation müssen.

1. Auf den **Server** > **Konfigurationsserver** Registerkarte, wählen Sie den Server Configuration > **vCenter Server hinzufügen**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)

2. Fügen Sie vCenter Server oder ESXi Hostdetails, den Namen des Kontos der angegebene Zugriff auf vCenter Server im vorherigen Schritt und den Prozess-Server zu von vCenter Server verwaltet virtuelle VMware-Computer verwendet wird. Beachten Sie, dass vCenter Server oder ESXi Host im gleichen Netzwerk wie der Server befinden sollte mit der Prozess-Server installiert ist.

    >[AZURE.NOTE] Wenn Sie vCenter Server oder ESXi Host mit einem Konto, das Administratorrechte auf dem vCenter oder Host nicht hinzufügen, sicherstellen vCenter oder ESXi Konten diese Berechtigungen aktiviert: Datacenter Datenspeicher Ordner, Jost, Netzwerk, Ressource, virtuelle Maschine vSphere Switch verteilt. Darüber hinaus benötigt der Server vCenter Recht Ansichten speichern.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)

3. Nach Abschluss der Suche werden in der Registerkarte **Konfiguration** vCenter Server aufgeführt.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)


## <a name="step-8-create-a-protection-group"></a>Schritt 8: Erstellen einer Schutzgruppe

> [AZURE.VIDEO enhanced-vmware-to-azure-protection]


Gruppen sind logische Gruppen von virtuellen Computern oder Servern mit den gleichen Einstellvorgaben Schutz schützen möchten. Eine Schutzgruppe Einstellungen zuweisen und diese Einstellungen gelten für alle virtuellen Computer-physische Computer, die Sie der Gruppe hinzufügen.

1. Öffnen Sie **Geschützte Objekte** > **Schutzgruppe** und auf eine Schutzgruppe hinzufügen.

    ![Schutzgruppe erstellen](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)

2. Geben Sie auf der Seite **Einstellungen Schutz angeben** einen Namen für die Gruppe und wählen Sie **aus** den Konfigurationsserver erstellen soll. **Ziel** ist Azure.

    ![Schutz Einstellung](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)

3. Auf der Seite **Geben Sie Replikation** Konfigurieren der Replikation, die für alle Computer in der Gruppe verwendet werden.

    ![Gruppenreplikation Schutz](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

    - **Mehrere VM Konsistenz**: Wenn Sie diese Option aktivieren wird freigegebenen anwendungskonsistente Recovery-Punkte auf den Computern in der Schutzgruppe erstellt. Diese Einstellung ist relevantesten, wenn alle Computer in der Schutzgruppe die gleiche Arbeitslast ausgeführt werden. Alle Computer werden auf die gleichen Daten wiederhergestellt. Dies ist, ob Sie virtuelle VMware-Computer oder Windows/Linux Servern replizieren.
    - **RPO Schwellenwert**: Legt das RPO. Warnung werden generiert, wenn kontinuierlichen Schutz Replikation konfigurierten RPO-Schwellenwert überschreitet.
    - **Wiederherstellungspunkt Aufbewahrung**: Gibt die Beibehaltungsdauer. Geschützte Computer können zu einem beliebigen Punkt innerhalb dieses Fenster wiederhergestellt werden.
    - **Anwendungskonsistente Snapshots Häufigkeit**: Gibt an, wie oft Wiederherstellungspunkte mit anwendungskonsistente Snapshots erstellt werden.

Wenn Sie auf das Häkchen klicken wird eine Schutzgruppe mit dem Namen erstellt angegebene. Darüber hinaus wird eine zweite Schutzgruppe erstellt, mit dem Namen < Schutz Group-Namen Failback). Dieser Schutzgruppe wird verwendet, wenn nicht an den lokalen Standort nach einem Failover in Azure. Sie auf **Geschützte Elemente** erstellt haben, können Sie die Gruppen überwachen.

## <a name="step-9-install-the-mobility-service"></a>Schritt 9: Installieren Sie den Dienst Mobilität

Der erste Schritt beim Schutz von virtuellen Maschinen und Servern aktivieren ist Mobility-Dienst installieren. Dies ist auf zwei Arten möglich:

- Automatisch drücken und den Dienst auf jedem Computer vom Prozess-Server installieren. Beachten Sie, dass Computer einer Schutzgruppe hinzufügen und sie sind bereits mit einer entsprechenden Version der Mobilität Service Push-Installation durchgeführt wird.
- Mit Ihrem Unternehmen Push-Methode WSUS oder System Center Configuration Manager automatisch installiert. Stellen Sie sicher, dass Sie den Verwaltungsserver eingerichtet haben, bevor Sie dies tun.
- Installieren Sie manuell auf jedem Computer zu schützen. Seitennamen haben sicher Sie den Verwaltungsserver einrichten bevor Sie dies tun.


### <a name="install-the-mobility-service-with-push-installation"></a>Installieren Sie den Dienst Mobilität mit Push-installation

Beim Hinzufügen von Computern zu einer Schutzgruppe mobilitätsservice automatisch abgelegt und Prozess-Server auf jedem Computer installiert.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Vorbereitung für automatische Push auf Windows-Computern

Hier ist Windows-Computer so vorbereiten, dass mobilitätsservice automatisch durch den Prozess-Server installiert werden kann.

1.  Erstellen Sie ein Konto, das vom Prozessserver auf dem Computer verwendet werden kann. Das Konto muss über Administratorrechte (lokal oder Domäne) verfügen. Beachten Sie, dass diese Anmeldeinformationen nur für Push-Installation des Mobility-Services verwendet werden.

    >[AZURE.NOTE] Wenn Sie nicht über ein Domänenkonto verwenden, müssen Sie RAS-Benutzer auf dem lokalen Computer deaktivieren. Fügen Sie hierzu in der Registrierung unter HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System DWORD-Eintrag LocalAccountTokenFilterPolicy mit dem Wert 1 unter. Hinzufügen die Registrierung Eintrag eine CLI öffnen oder PowerShell geben **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Die Windows-Firewall des Computers soll zu schützen, wählen Sie **eine Anwendung zulassen oder Feature durch die Firewall** Aktivieren der **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation**. Für Computer, die einer Domäne angehören, können Sie die Firewallrichtlinie mit einem Gruppenrichtlinienobjekt konfigurieren.

    ![Firewall Einstellung](./media/site-recovery-vmware-to-azure-classic/mobility1.png)

2. Fügen Sie das Konto, das Sie erstellt haben:

    - Öffnen Sie **Cspsconfigtool**. Ist als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner \home\svsystems\bin [INSTALLATIONSORT].
    - Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
    - Fügen Sie das Konto, das Sie erstellt haben. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie einen Computer zu einer Schutzgruppe hinzufügen.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Vorbereitung für automatische Push auf Linux-Servern

1.  Stellen Sie sicher, dass Linux-Computer schützen möchten beschriebenen [lokalen Komponenten](#on-premises-prerequisites)unterstützt wird. Sorgen für Netzwerkkonnektivität zwischen den Computer zu schützen und den Verwaltungsserver ausgeführt, die den Prozess-Server.

2.  Erstellen Sie ein Konto, das vom Prozessserver auf dem Computer verwendet werden kann. Das Konto sollte ein Root-Benutzer auf dem Linux-Quellserver. Beachten Sie, dass diese Anmeldeinformationen nur für Push-Installation des Mobility-Services verwendet werden.

    - Öffnen Sie **Cspsconfigtool**. Ist als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner \home\svsystems\bin [INSTALLATIONSORT].
    - Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
    - Fügen Sie das Konto, das Sie erstellt haben. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie einen Computer zu einer Schutzgruppe hinzufügen.

3.  Überprüfen Sie, dass etc auf dem Linux-Quellserver Einträge enthält, die den lokalen Hostnamen IP-Adressen aller Netzwerkadapter zugeordnet.
4.  Installieren der neuesten Openssh, Openssh-Server Openssl-Pakete auf dem Computer zu schützen.
5.  Sicherstellen Sie, dass SSH Port 22 aktiviert ist und ausgeführt wird.
6.  Aktivieren Sie SFTP-Subsystem und Kennwortauthentifizierung in der Datei Sshd_config folgendermaßen:

    - Melden Sie sich als Root an.
    - Suchen Sie in der Datei /etc/ssh/sshd_config die Zeile, die mit Passwordauthentification beginnt.
    - Kommentieren Sie die Zeile, und ändern Sie den Wert von **nicht** auf **Ja**.
    - Suchen Sie die Zeile, die mit **Subsystem** beginnt, und kommentieren Sie die Zeile.

        ![Linux](./media/site-recovery-vmware-to-azure-classic/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Mobility-Dienst manuell installieren

Installationsprogramme sind in C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

Betriebssystem | Service-Installationsdatei Mobilität
--- | ---
Windows-Server (64 Bit) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (64 Bit) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (64 Bit) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (64 Bit) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-manually-on-a-windows-server"></a>Installieren Sie manuell auf einem WindowsServer


1. Herunterladen und Ausführen des entsprechenden Installers.
2. Wählen Sie bevor **Sie beginnen ** **mobilitätsservice aus**

    ![Mobilitätsservice](./media/site-recovery-vmware-to-azure-classic/mobility3.png)

3. In **Server-Konfigurationsdetails** angeben der IP-adressedes der Management-Server und das Kennwort, das bei der Installation von Management-Server-Komponenten generiert wurde. Abrufen die Passphrase ausgeführt: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** auf dem Verwaltungsserver.

    ![Mobilitätsservice](./media/site-recovery-vmware-to-azure-classic/mobility6.png)

4. Lassen Sie im **Installationsordner** am Standardspeicherort und klicken Sie auf **Weiter** um die Installation zu starten.
5. Im **Verlauf der Installation** überwachen Sie Installation und starten Sie den Computer aus, wenn Sie dazu aufgefordert werden.

Sie können auch von der Befehlszeile aus:

UnifiedAgent.exe [/ Rolle < Agent/MasterTarget >] [/ InstallLocation <Installation Directory>] [/ zerebraler Kinderlähmung <IP address of CS to be registered with>] [/ PassphraseFilePath <Passphrase file path>] [/ LogFilePath <Log File Path>]

Wo:

- Rolle: obligatorisch. Gibt an, ob der Mobility-Dienst installiert werden soll.
- / InstallLocation: obligatorisch. Gibt an, wo den Dienst installiert.
- / PassphraseFilePath: obligatorisch. Gibt die Konfiguration Server Passphrase.
- / LogFilePath: obligatorisch. Speicherort für Protokolldateien Setup gibt

#### <a name="uninstall-mobility-service-manually"></a>Mobility-Dienst manuell deinstallieren

Mobilitätsdienst kann hinzufügen entfernen Programm aus Bedienfeld oder Befehlszeile deinstalliert werden.

Der Befehl über Befehlszeile Mobility-Dienst deinstallieren

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="modify-the-ip-address-of-the-management-server"></a>Ändern Sie die IP-Adresse des Verwaltungsservers

Nach Ausführung des Assistenten können Sie die IP-Adresse des Management-Servers wie folgt ändern:

1. Öffnen Sie die Datei hostconfig.exe (befindet sich auf dem Desktop).
2. Ändern Sie auf der Registerkarte **globale** IP-Adresse des Verwaltungsservers.

    >[AZURE.NOTE] Sie sollten nur die IP-Adresse des Verwaltungsservers ändern. Die Portnummer für den Management-Server-Kommunikation muss 443 und HTTPS verwenden sollte aktiviert bleiben. Das Kennwort sollte nicht geändert werden.

    ![Management-Server-IP-Adresse](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-manually-on-a-linux-server"></a>Installieren Sie auf einem Linux-Server manuell:

1. Kopieren Sie das entsprechende Tar-Archiv basierend auf der Tabelle oben mit dem Linux-Computer schützen möchten.
2. Öffnen Sie ein Shell-Programm und extrahieren Sie das Gezippte Tar-Archiv auf einen lokalen Pfad mit:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Erstellen Sie eine passphrase.txt-Datei im lokalen Verzeichnis, Tar-Archiv extrahiert. Soll dies kopieren die Passphrase aus C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase auf dem Verwaltungsserver und speichern Sie sie in passphrase.txt mit *`echo <passphrase> >passphrase.txt`* in der Schale.
4. Installieren Sie den Dienst Mobilität durch Eingabe *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Interne IP-Adresse des Verwaltungsservers und stellen Sie sicher, dass Port 443 ausgewählt ist.

**Sie können auch von der Befehlszeile aus**:

1. Die Passphrase aus C:\Program Files (x86) \InMage Systems\private\connection auf dem Server kopieren und als "passphrase.txt" auf dem Server speichern. Führen Sie diese Befehle. In unserem Beispiel ist die Management-Server-IP-Adresse 104.40.75.37 und der HTTPS-Port 443 sollte:

Auf einem Produktionsserver installieren:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Auf dem master Zielserver zu installieren:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Schritt 10: Schutz für einen Computer aktivieren

Um Schutz aktivieren Sie virtuellen Computern und Servern eine Schutzgruppe hinzufügen. Bevor Sie beginnen, beachten Sie Folgendes, wenn Sie virtuelle VMware-Computer schützen:

- Virtuelle VMware-Computer entdeckt alle 15 Minuten und es könnte mehr als 15 Minuten nach der Erkennung in Site Recovery-Portal angezeigt werden.
- Umgebung ändert sich auf dem virtuellen Computer (z. B. VMware Tools-Installation) können auch Site Recovery aktualisiert werden mehr als 15 Minuten dauern.
- Sie können virtuelle VMware-Computer im Feld **Letzte Kontakt am** vCenter Server-ESXi Host auf der Registerkarte **Konfiguration** zuletzt ermittelten prüfen.
- Wenn Sie eine bereits erstellte Schutzgruppe und danach vCenter Server oder ESXi Host hinzufügen, dauert es mehr als 15 Minuten für Azure Site Recovery-Portal aktualisieren und virtuellen Computern im Dialogfeld **Computer Hinzufügen einer Schutzgruppe** aufgelistet werden.
- Sie unverzüglich Schutzgruppe ohne geplanten Erkennung Computer hinzufügen möchten, markieren Sie den Konfigurationsserver (nicht klicken), und klicken Sie auf die Schaltfläche **Aktualisieren** .

Beachten Sie außerdem, dass:

- Wir empfehlen Ihren Schutzgruppen Architekt, sodass sie Ihre Arbeitslasten spiegeln. Beispielsweise fügen Sie Computern einer bestimmten Anwendung derselben Gruppe hinzu.
- Beim Hinzufügen von Computern zu einer Schutzgruppe legt der Prozess-Server automatisch und mobilitätsdienst installiert, wenn es bereits installiert ist. Beachten Sie, dass den Push-Mechanismus wie im vorherigen Schritt beschrieben haben müssen.


Hinzufügen von Computern zu einer Schutzgruppe:

1. Klicken Sie auf **geschützte Elemente** > **Schutzgruppe** > **Maschinen** > Computer hinzufügen. \As empfohlen
2. In **virtuelle Computer auswählen** Wenn Sie virtuelle VMware-Computer schützen, wählen Sie einen vCenter Server, der die virtuellen Computer (oder EXSi Host sie ausführen) verwaltet und dann Computer.

    ![Schutz aktivieren](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)

3.  **Virtuelle Computer auswählen** , wenn Sie den physischen Schutz stellen Server, Assistenten für **Physische Computer hinzufügen** die IP-Adresse und den Anzeigenamen. Wählen Sie Betriebssystem.

    ![Schutz aktivieren](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)

4. **Geben Sie Ressourcen** versehen Sie das Speicherkonto für die Replikation verwenden und auswählen, ob die Einstellungen für alle Arbeitslasten verwendet werden soll. Beachten Sie, dass Premium-Speicherkonten derzeit nicht unterstützt.

    >[AZURE.NOTE] 1. wir unterstützen keine Verschiebung Speicherkonten Ressourcengruppen über [neue Azure-Portal](../storage/storage-create-storage-account.md) erstellt.                           2.[Migration Speicherkonten](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements nicht für Speicherkonten für die Bereitstellung von Site Recovery unterstützt.

    ![Schutz aktivieren](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)

5. Wählen Sie das Konto **Konten festlegen** für automatische Installation des Mobility-Dienstes [konfiguriert](#install-the-mobility-service-with-push-installation) .

    ![Schutz aktivieren](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)

6. Klicken Sie auf das Häkchen Maschinen zur Schutzgruppe hinzufügen und erste Replikation für jeden Computer starten.

    >[AZURE.NOTE] Wenn Push-Installation Service Mobilität vorbereitet wurde, wird automatisch auf Computern installiert, die nicht zur Schutzgruppe hinzugefügt werden. Nach der Service Installation startet ein Schutzauftrag und schlägt fehl. Nach dem Fehler müssen Sie jeden Computer manuell neu starten, die mobilitätsdienst installiert wurde. Nach dem Neustart beginnt der Schutzauftrag erneut und anfänglichen Replikation.

Sie können den Status auf der Seite **Projekte** überwachen.

![Schutz aktivieren](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Darüber hinaus Schutzstatus in **Geschützte Objekte**überwacht werden kann > <protection group name> > **virtuellen Maschinen**. Nachdem die erste Replikation abgeschlossen ist und Daten synchronisiert, ändert Systemstatus** geschützt**.

![Schutz aktivieren](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)


## <a name="step-11-set-protected-machine-properties"></a>Schritt 11: Set geschützte Eigenschaften

1. Nachdem ein Computer **geschützten** Status hat können Sie ihre Failover-Eigenschaften konfigurieren. Gruppendetails Schutz wählen Sie den Computer, und öffnen Sie die Registerkarte **Konfigurieren** .
2. Site Recovery schlägt Eigenschaften für Azure VM automatisch und lokalen Einstellungen Netzwerk erkennt.

    ![Festlegen von Eigenschaften für virtuelle Computer](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)

3. Sie können diese Einstellung ändern:

    -  **Azure VM-Name**: der Name, der nach einem Failover in Azure Computer gewährt wird. Der Name muss Azure erfüllen.

    -  **Azure VM-Größe**: die Anzahl der Netzwerkadapter bestimmt die Größe für virtuelle Zielcomputer angeben. [Mehr](../virtual-machines/virtual-machines-linux-sizes.md/#size-tables) über Größe und Adapter. Beachten Sie Folgendes:

        - Ändern Sie die Größe für einen virtuellen Computer und dem Speichern ändert die Anzahl der Netzwerkadapter beim nächsten die Registerkarte **Konfigurieren öffnen** . Die Anzahl der Netzwerkadapter Ziel virtueller Computer ist mindestens der Anzahl der Netzwerkadapter auf virtuellen Quellcomputers und maximal unterstützte Größe des virtuellen Computers ausgewählten Netzwerkadapter.
            - Wenn die Anzahl der Netzwerkadapter auf dem Quellcomputer kleiner oder gleich der Anzahl der Netzwerkadapter für die Zielgröße Computer zulässig ist, haben das Ziel die gleiche Anzahl von Adaptern als Quelle.
            - Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputers die Höchstzahl überschreitet für die Größe und die maximale Ziel verwendet werden.
            - Wenn beispielsweise ein Quellcomputer verfügt über zwei Netzwerkadapter die Zielgröße Computer unterstützt vier und dem Zielcomputer müssen zwei Adapter. Wenn der Quellcomputer hat zwei Adapter unterstützten Zielgröße unterstützt jedoch nur eine haben dem Zielcomputer nur eine Netzwerkkarte.
        - Wenn der virtuelle Computer verbunden mehrere Netzwerkadapter sollten alle Adapter denselben Azure Netzwerk.
    - **Azure-Netzwerk**: Geben Sie ein Azure Netzwerk nach einem Failover Azure VMs verbunden werden soll. Wenn Sie nicht angeben, wird nicht Azure VMs mit keinem Netzwerk verbunden. Darüber hinaus müssen Sie eine Azure-Netzwerk angeben, Failback von Azure auf der lokalen Website. Failback erfordert eine VPN-Verbindung zwischen Azure-Netzwerk und einem lokalen Netzwerk.
    - **Azure-IP-Adresse, Subnetzmaske**: für jeden Netzwerkadapter auswählen das Subnetz, Azure VM herstellen soll. Beachten Sie Folgendes:
        - Wenn der Netzwerkadapter des Quellcomputers eine statische IP-Adresse konfiguriert können Sie eine statische IP-Adresse für den Azure-VM angeben. Wenn Sie eine statische IP-Adresse angeben, werden alle verfügbaren IP-Adressen zugeteilt. Wenn die Ziel-IP-Adresse angegeben, aber es bereits von einer anderen VM in Azure wird wird Failover fehl. Wenn der Netzwerkadapter des Quellcomputers für DHCP konfiguriert ist müssen DHCP als Einstellung für Azure Sie.

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Schritt 12: Einen Wiederherstellungsplan erstellen und Ausführen eines Failovers

> [AZURE.VIDEO enhanced-vmware-to-azure-failover]

Sie können ein Failover für einen einzelnen Computer oder mehrere virtuelle Computer, der dieselbe Aufgabe durchführen, oder führen Sie die gleiche Arbeitslast Failover. Mehrere Computer gleichzeitig Failover Sie einen Wiederherstellungsplan hinzufügen.

### <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

1. Auf der Seite **Wiederherstellungspläne** auf **Wiederherstellungsplan hinzufügen** , und fügen Sie einen Wiederherstellungsplan. Geben Sie Details für den Plan und wählen Sie **Azure** als Ziel aus.

    ![Konfigurieren von Recovery-Plans](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)

2. Wählen Sie im **virtuellen Computer auswählen** eine Schutzgruppe, und wählen Sie Computer aus der Gruppe der Wiederherstellungsplan hinzu.

    ![Hinzufügen von virtuellen Computern](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Sie können den Plan zu Gruppen und Reihenfolge der Reihenfolge in der Computer in den Wiederherstellungsplan Failover werden. Sie können auch Skripts und Abfragen für manuelle Aktionen hinzufügen. Skripts können manuell oder mithilfe von [Azure Automatisierung Runbooks](site-recovery-runbook-automation.md)erstellt werden. [Weitere](site-recovery-create-recovery-plans.md) Informationen zum Anpassen von Recovery-Plänen.

## <a name="run-a-failover"></a>Ausführen eines Failovers

Beachten Sie, dass, bevor Sie einen Failover ausführen:


- Stellen Sie sicher, dass der Verwaltungsserver ausgeführt wird und verfügbar – andernfalls Failover fehl.
- Wenn Sie einer Notiz ungeplanten Failover ausführen:

    - Wenn möglich sollten Sie primären Computer herunterfahren, vor dem Ausführen eines ungeplanten Failovers. So wird sichergestellt, dass Quelle und Replikat Computern gleichzeitig haben. Wenn Sie virtuelle VMware-Computer replizieren sind können dann bei ein ungeplanten Failover ausführen soll Site Recovery bemühen, die Quelle Computer herunterfahren soll. Je nach Zustand des primären Standorts dies möglicherweise oder funktioniert nicht. Wenn Replikation Servern bietet Site Recovery nicht diese Option.
    - Bei einem ungeplanten Failover beendet so alle Delta Daten nicht übertragen werden, nachdem ein ungeplantes Failovers beginnt Datenreplikation von primären Computer.

- Wunsch nach einem Failover Replikat virtuellen Computer in Azure verbinden aktivieren Sie Remotedesktopverbindung auf dem Quellcomputer, bevor das Failover ausgeführt und RDP-Verbindung über die Firewall zulassen. Sie müssen auch RDP öffentlichen Endpunkt der Azure Virtual Machine nach dem Failover zu ermöglichen. Verwenden Sie diese [Tipps](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) um sicherzustellen, dass nach einem Failover RDP funktioniert.

>[AZURE.NOTE] Um die beste Leistung bei Failover in Azure tun, daß der Azure-Agent auf dem geschützten Computer installiert haben. Dies hilft schneller starten und Diagnose bei Problemen hilft. Linux-Agent kann finden [hier](https://github.com/Azure/WALinuxAgent) - und Windows Agent finden Sie [hier](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="run-a-test-failover"></a>Führen Sie ein testfailover

Führen Sie ein testfailover-Failover und Recovery-Prozesse in einem isolierten Netzwerk simulieren, die Ihre produktionsumgebung nicht beeinträchtigt und regelmäßige Replikation weiterhin normal. Testfailover der Quelle initiiert, und in Möglichkeiten ausführen:

- **Geben Sie ein Azure-Netzwerk**: Wenn ein testfailover ohne Netzwerk Ausführen der Test einfach überprüft virtuellen Computer starten und korrekt in Azure angezeigt. Virtuelle Computer werden nicht nach einem Failover mit einem Azure Netzwerk verbunden.
- **Angeben einer Azure Netzwerk**: Diese Art von Failover überprüft, ob die gesamten Umgebung wie erwartet wird und Azure virtuelle Computer mit dem angegebenen Netzwerk verbunden sind.


1. Die Seite **Wiederherstellungspläne** Plan und klicken Sie auf **Test-Failover**.

    ![Hinzufügen von virtuellen Computern](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)

2. **Testen** der Failover-bestätigen wählen Sie **keine** zeigt an, dass ein Azure-Netzwerk für den testfailover, oder wählen Sie das Netzwerk mit dem Test VMs nach einem Failover verbunden werden soll. Klicken Sie auf das Häkchen, um das Failover zu starten.

    ![Hinzufügen von virtuellen Computern](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)

3. Überwachen Sie Failover-Status auf der Registerkarte **Aufträge** .

    ![Hinzufügen von virtuellen Computern](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)

4. Nach Abschluss des Failovers sollten auch möglicherweise das Replikat Azure finden Computer in Azure-Portal angezeigt > **virtuelle Computer**. Möchten Sie eine RDP-Verbindung der Azure-VM müssen Sie Port 3389 VM-Endpunkt zu öffnen.

5. Nachdem Sie haben fertig erreicht Failover abschließen testen, klicken Sie auf vollständige Test abgeschlossen. Notizen aufzeichnen und Speichern von Anmerkungen testfailover zugeordnet.

6. Klicken Sie auf die Umgebung automatisch bereinigen **testfailover ist abgeschlossen** . Danach wird das testfailover Status **abgeschlossen** angezeigt. Alle Elemente oder VMs automatisch während des Failovers Test erstellt werden gelöscht. Beachten Sie, dass ein testfailover hält länger als zwei Wochen mit abgeschlossen ist.



### <a name="run-an-unplanned-failover"></a>Führen Sie ein ungeplantes Failovers

Ungeplantes Failover von Azure initiiert und ausgeführt werden kann, auch wenn der primäre Standort nicht verfügbar ist.


1. Die **Recovery-Pläne** wählen Sie den Plan auf Seite und **Failover** > **Ungeplanten Failover**.

    ![Hinzufügen von virtuellen Computern](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)

2. Wenn Sie virtuelle VMware-Maschinen Replikation können zu lokalen virtuellen Computer herunterfahren. Dies ist die beste Leistung und Failover weiterhin, ob der Aufwand oder nicht erfolgreich ist. Wenn nicht erfolgreich erscheint auf der Registerkarte **Aufträge **Fehlerdetails > **Ungeplanten Failover Aufträge**.

    ![Hinzufügen von virtuellen Computern](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

    >[AZURE.NOTE] Diese Option ist nicht verfügbar, wenn Sie physische Server replizieren können. Sie müssen zu den manuell herunterfahren, wenn möglich.

3. Failover **Bestätigen** überprüfen Sie Failover Richtung (Azure) und wählen Sie den Wiederherstellungspunkt für das Failover verwenden möchten. Wenn Sie bei der Konfiguration von Replikationseigenschaften Multi-VM aktiviert können Sie die aktuelle Anwendung oder absturzsicheren Wiederherstellungspunkt wiederherstellen. Sie können auch zu einem früheren Zeitpunkt wiederherstellen **benutzerdefinierte Wiederherstellungspunkt** auswählen. Klicken Sie auf das Häkchen, um das Failover zu starten.

    ![Hinzufügen von virtuellen Computern](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)

3. Warten des ungeplanten Failover Auftrags abzuschließen. Sie können Failover auf der Registerkarte **Aufträge** überwachen. Beachten Sie, dass auch bei Fehlern bei ungeplanten Failover der Wiederherstellungsplan bis zum Abschluss führt. Sie sollte auch das Replikat Azure finden Computer in virtuellen Computern in Azure-Portal angezeigt.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Verbinden Sie mit repliziert Azure virtuelle Computer nach dem failover

Um die Verbindung repliziert virtuelle Computer in Azure Failover hier ist was Sie benötigen:

1. Eine Remotedesktopverbindung sollte auf dem primären Computer aktiviert werden.
2. Die Windows-Firewall auf dem primären Computer zu RDP.
3. Nach einem Failover müssen Sie den öffentlichen Endpunkt für Azure Virtual Machine RDP hinzufügen.

[Mehr](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) über das einrichten.


## <a name="deploy-additional-process-servers"></a>Bereitstellung zusätzlicher Server

Haben Sie aus skalieren die Bereitstellung über 200 Quelle Maschinen oder Ihre täglichen Abwanderung Gesamtrate überschreitet 2 TB benötigen Sie weitere Server das Volumen. Richten Sie ein zusätzlicher Server überprüfen die [Weitere](#additional-process-servers) Server und gehen Sie hier, um den Prozess-Server einzurichten. Konfigurieren Sie nach dem Einrichten des Servers Quelle Maschinen verwenden.

### <a name="set-up-an-additional-process-server"></a>Ein zusätzlicher Server einrichten

Richten Sie eine weitere Server wie folgt:

- Führen Sie einheitliche als Prozess-Server nur einen Management-Server konfigurieren.
- Datenreplikation mit den neuen Prozessserver verwalten soll müssen Sie dazu die geschützten Computer migrieren.

### <a name="install-the-process-server"></a>Prozess-Server installieren

1. Herunterladen Sie auf der Seite Schnellstart einheitliche Installationsdatei für die Komponenteninstallation Site Recovery Führen Sie Setup.
2. Wählen Sie bevor **Sie beginnen** **Hinzufügen zusätzlicher Server skalieren Bereitstellung aus**

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)

3. Führen Sie den Assistenten auf die gleiche Weise habe bei [richten](#step-5-install-the-management-server) Sie den ersten Verwaltungsserver.

4. Geben Sie in **Server-Konfigurationsdetails** die IP-Adresse des ursprünglichen Management-Servers auf dem Konfigurationsserver und die Passphrase installiert. Auf dem ursprünglichen Server ausführen ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** um das Passwort zu erhalten.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrieren von Maschinen für den neuen Prozessserver

1. Öffnen Sie **Konfigurationsserver** > **Server** > Name des ursprünglichen Verwaltungsserver > **Serverdetails**.

    ![Aktualisieren Process-server](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)

2. Klicken Sie in der Liste **Prozess** **Prozessserver ändern** neben dem Server, den Sie ändern möchten.

    ![Aktualisieren Process-server](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)

3. **Änderung Prozess**Server > **Prozess Zielserver** wählen Sie den neuen Verwaltungsserver und dann den virtuellen Computern, die der Prozess-Server behandelt. Klicken Sie auf das Informationssymbol, um Informationen über den Server. Durchschnittliche Speicherplatz, die für jeden ausgewählten virtuellen Computer auf den neuen Prozessserver repliziert wird zu Entscheidungen laden. Klicken Sie zum Starten auf den neuen Prozessserver replizieren.

    ![Aktualisieren Process-server](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)




## <a name="vmware-permissions-for-vcenter-access"></a>Berechtigungen auf vCenter VMware

Prozess-Server erkennen automatisch VMs auf vCenter Server. Automatische Erkennung ausführen müssen Sie eine Rolle (Azure_Site_Recovery) Ebene vCenter Site Recovery vCenter berechtigt zu definieren. Beachten Sie, dass nur müssen Sie VMware Maschinen in Azure migrieren und nicht von Azure Failback müssen, können Sie eine schreibgeschützte Rolle ausreichend. Legen Sie die Berechtigungen gemäß [Schritt 6: Richten Sie Anmeldeinformationen für den vCenter Server](#step-6-set-up-credentials-for-the-vcenter-server) Rollenberechtigungen werden in der folgenden Tabelle zusammengefasst.

**Rolle** | **Details** | **Berechtigungen**
--- | --- | ---
Azure_Site_Recovery-Rolle | VMware VM-Ermittlung |Weisen Sie diese Berechtigungen für den Server V-Center:<br/><br/>Datenspeicher -> Allocate Speicherplatz, durchsuchen Datenspeicher, niedrige Datei Operationen., Datei entfernen, virtuellen Aktualisierungsdateien<br/><br/>Netzwerk -> Netzwerk zuweisen<br/><br/>Ressourcen -> Zuweisen von virtuellen Ressourcenpool ausgeschalteten virtuellen Computer migrieren, Migration virtueller Computer eingeschaltet<br/><br/>Aufgaben-Task erstellen, Update-Task ><br/><br/>VM-Konfiguration ><br/><br/>Virtual Machine-Interaktion > -> Frage beantworten Gerät Verbindung. konfigurieren CDs, Disketten konfigurieren, schalten Sie die Stromversorgung, VMware Tools installieren<br/><br/>Virtual Machine-Lager > -> erstellen, registrieren, Registrierung<br/><br/>VM-Bereitstellung > -> zulassen VM herunterladen, Hochladen von virtuellen Dateien zulassen<br/><br/>VM-Snapshots > -> Snapshots entfernen
vCenter Benutzerrolle | VMware VM Discovery/Failover ohne Herunterfahren der Quelle VM | Weisen Sie diese Berechtigungen für den Server V-Center:<br/><br/>Rechenzentrum-Objekt hat untergeordnete Objekt übertragen Rolle = schreibgeschützt <br/><br/>Der Benutzer Datacenter Ebene zugewiesen und hat damit Zugriff auf alle Objekte im Datencenter.  Untergeordnete Objekte (ESX-Hosts, Datastores VMs und Netzwerke) Wenn Sie den Zugriff beschränken möchten, weisen Sie die Rolle **keinen Zugriff** **übertragen zum untergeordneten** Objekt zu.
vCenter Benutzerrolle | Failover und failback | Weisen Sie diese Berechtigungen für den Server V-Center:<br/><br/>Datacenter Objekt übertragen, untergeordnetes Objekt Rolle = Azure_Site_Recovery<br/><br/>Der Benutzer Datacenter Ebene zugewiesen und hat damit Zugriff auf alle Objekte im Datencenter.  Wenn Sie den Zugriff beschränken möchten, untergeordnetes Objekt (ESX-Hosts, Datastores VMs und Netzwerke) weisen Sie die Rolle **keinen Zugriff ** **auf untergeordnete Objekt übertragen zu** .  



## <a name="third-party-software-notices-and-information"></a>Drittanbieter-Software und Hinweise

Nicht übersetzen bzw. lokalisieren

Software und Firmware aus Microsoft-Produkt oder Dienst basiert oder aus der unten aufgeführten Projekte enthält (zusammen "Drittanbieter-Code).  Microsoft ist nicht die ursprünglichen Autor Drittanbieter-Code.  Ursprüngliche Urheberrechtshinweis und Lizenz unter dem Microsoft solcher Drittanbieter-Code empfangen werden unten festgelegt.

Die Informationen in Abschnitt A wird dritten Codekomponenten aufgeführten Projekte. Solche Lizenzen und Informationen dienen nur zu Informationszwecken.  Dieser dritte wird Ihnen von Microsoft unter Microsoft Software Lizenzierungsinformationen für das Microsoft-Produkt oder Dienst nachlizenziert wird.  

Die in Abschnitt B ist Drittanbieter Codekomponenten Informationen, die Ihnen von Microsoft unter der ursprünglichen Lizenzierungsinformationen bereitgestellt werden.

Die vollständige Datei finden im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft behält sich alle hierin nicht ausdrücklich gewährten Rechte durch Implikation, weder ausdrücklich noch konkludent oder auf andere Weise.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Failback](site-recovery-failback-azure-to-vmware-classic.md) zu Ihrem fehlgeschlagen auf Computern in Azure zu Ihrer lokalen Umgebung.
