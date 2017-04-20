<properties
    pageTitle="Virtuelle VMware-Maschinen und physische Server in Azure mit Azure Site Recovery im Azure-Portal replizieren | Microsoft Azure"
    description="Beschreibt das Bereitstellen von Azure Site Recovery, Replikation, Failover und Recovery von lokalen virtuellen VMware Maschinen und Windows/Linux Servern in Azure mithilfe von Azure Portal organisieren"
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
    ms.date="08/12/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>Virtuelle VMware-Maschinen und physischen Replikation auf Azure mit Azure Site Recovery mit Azure-portal

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmware-to-azure.md)
- [Azure Classic](site-recovery-vmware-to-azure-classic.md)
- [Azure Classic (Legacy)](site-recovery-vmware-to-azure-classic-legacy.md)

Willkommen bei Azure Site Recovery! Verwenden Sie in diesem Artikel Azure Azure-Portal Azure Site Recovery bei lokalen virtuellen VMware Maschinen oder Windows-Linux-Servern repliziert werden soll.

> [AZURE.NOTE] Azure hat zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager (ARM) und Classic. Azure verfügt außerdem über zwei Portale – klassischen Azure-Portal, das klassische Bereitstellungsmodell unterstützt und Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle.

Site Recovery im Azure-Portal bietet eine Reihe neuer Features:

- Azure Backup und Azure Site Recovery Services sind in einem Tresor Recovery Services zusammengefasst, sodass einrichten und Verwalten von Business Continuity und Disaster Recovery (BCDR) von einem einzigen Ort. Einheitliches Dashboard können Sie überwachen und Verwalten von Operationen über Ihren lokalen Sites und Azure öffentlichen Cloud.
- Benutzer mit Azure Cloud Solution Provider (CSP) Programm bereitgestellt können jetzt Site Recovery-Vorgänge im Azure-Portal verwalten.
- Site Recovery im Azure-Portal kann auf ARM-Speicherkonten Computer replizieren. Bei einem Failover erstellt Site Recovery ARM-basierten VMs in Azure.
- Site Recovery weiterhin klassische Speicherkonten Replikation. Bei einem Failover erstellt Site Recovery VMs mit der Option Klassisch.

Nach dem Lesen dieser Beitrag Kommentare unten in die Kommentare Disqus. Fragen Sie technische [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Übersicht

Organisationen benötigen eine BCDR Strategie, die bestimmt, wie apps Arbeitslasten und Daten bleiben aktiv bei geplanten und ungeplanten Ausfällen normalen Arbeitsbedingungen so bald wie möglich wiederherstellen. Die BCDR Strategie sollten schützen Sie Daten wiederhergestellt und Arbeitslasten ständig verfügbar bleiben beim Notfall.

Site Recovery ist eine Azure Service BCDR Strategie umzusetzen Replikation von lokalen Servern und virtuellen Computer in die Cloud (Azure) oder zu einem sekundären Datencenter beiträgt. Treten Ausfälle in Ihrem primären Standort Failover Sie sekundäre an apps und Arbeitslasten zur Verfügung. Sie nicht an Ihrem primären Standort zu beenden. Weitere Informationen [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md)

Dieser Artikel enthält alle Informationen, die Sie replizieren müssen virtuelle VMware-Computer und Windows-Linux-Servern in Azure lokal. Es enthält eine Architekturübersicht Informationen und Bereitstellungsschritte für die Konfiguration von Azure, lokalen Server Replikationseigenschaften und Planung planen. Nach dem Einrichten der Infrastruktur können Sie Replikation auf Computern zu schützen, und überprüfen Sie, dass ein Failover funktioniert.

## <a name="business-advantages"></a>Geschäftliche Vorteile

- Site Recovery schützt Offsite Arbeitslasten in Ihrem Unternehmen und virtuelle VMware-Computer zu physischen Servern ausgeführt.
- Das Recovery-Portal stellt eine zentrale Stelle zum Einrichten, verwalten und Überwachen von Replikation, Failover und Recovery.
- Site Recovery erkennen automatisch VMware VMs vSphere Hosts hinzugefügt.
- Sie können problemlos Failover Ihrer lokalen Infrastruktur Azure und Failback (Wiederherstellen) von Azure VM VMware Server in der lokalen Website ausführen.
- Sie ermöglichen der Multi-VM und Replikationsgruppen tiered-Arbeitslasten über mehrere Computer replizieren gleichzeitig erstellen. Alle Computer in einer Replikationsgruppe haben konsistente und app konsistente Recovery-Punkte, wenn sie Failover. Für Failover können Sie mehrere Computer im Recovery-Pläne erfassen, sodass tiered Arbeitslasten zusammen Failover.

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

Szenariokomponenten sind:

- **Konfigurationsserver**: lokalen Maschine Kommunikation koordiniert und verwaltet Daten-Replikation und Recovery-Prozesse. Auf diesem Computer wird eine einzelne Setup Installation Konfigurationsserver und weitere ausführen:
    - **Prozessserver**: fungiert als Gateway Replikation. Quelle geschützt Maschinen replizierten Daten empfängt, caching, Komprimierung und Verschlüsselung optimiert und Azure-Speicher sendet. Auch Push-Installation des Dienstes Mobilität geschützten Computern verarbeitet und führt die automatische Ermittlung für virtuelle VMware-Computer. Standard-Prozess-Server ist auf dem Konfigurationsserver installiert. Sie können zusätzliche eigenständige Prozessserver skalieren die Bereitstellung bereitstellen.
    - **Master-Zielserver**: Replikationsdaten während des Failbacks von Azure behandelt.

- **Mobilitätsservice**: Diese Komponente wird auf jedem Computer (VMware VM oder physische Server), die Sie in Azure replizieren möchten bereitgestellt. Schreibvorgänge auf dem Computer erfasst und an den Prozess-Server weiterleitet.
- **Azure**: Sie brauchen Azure VMs für Replikation und Failover in Azure erstellen.  Sie benötigen ein Azure-Abonnement Azure Speicherkonto replizierten Daten und Azure virtuelles Netzwerk speichern, damit Azure VMs nach einem Failover mit einem Netzwerk verbunden sind. Das Speicherkonto und Netzwerk muss im Bereich Recovery Services Depot.
- **Failback**: von Azure lokalen Standort nach einem Failover nicht nun, müssen eine Azure VM als temporäre Prozess-Server erstellen. Sie können es nach Abschluss der Failback. Für Failback auch benötigen Sie eine Verbindung zum VPN (oder Azure ExpressRoute) zwischen der lokalen Site und Azure Netzwerk in dem Azure-VMs befinden. Wenn Failback Datenverkehr schwer müssen Sie einen dedizierten Master Zielserver für lokale Computer einrichten. Standardmäßig master Target Server auf dem Konfigurationsserver kann leichter Datenverkehr verwendet werden.


Die Grafik zeigt, wie diese Komponenten interagieren.

![Architektur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Abbildung 1: VMware/physische in Azure**

## <a name="azure-prerequisites"></a>Azure-Komponenten

Hier ist was Sie in Azure müssen Sie dieses Szenario bereitstellen.

**Voraussetzung** | **Details**
--- | ---
**Ein Azure-Konto**| Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.
**Azure-Speicher** | Replizierte Daten in Azure-Speicher gespeichert und Azure VMs werden erstellt, wenn ein Failover auftritt. <br/><br/>Zum Speichern von Daten benötigen Sie ein Speicherkonto standard oder Premium im Bereich Recovery Services Depot.<br/><br/>Sie können ein Speicherkonto LRS oder g. Wir empfehlen g, damit Daten sowie einen regionalen Ausfall oder die primäre Region wiederhergestellt werden kann. [Erfahren Sie mehr](../storage/storage-redundancy.md).<br/><br/> [Premium-Speicher](../storage/storage-premium-storage.md) wird normalerweise für virtuelle Computer verwendet, die eine hohe e/a-Leistung und niedriger Latenz zur Host-i/o-intensive Arbeitslasten.<br/><br/> Wenn Sie Premium-Konto zum Speichern von replizierter Daten verwenden möchten, benötigen Sie auch, ein Konto Standardspeicher Replikationsprotokollen speichern, die laufenden lokalen Daten erfassen.<br/><br/> Beachten Sie, dass im Azure-Portal erstellte Speicherkonten in Ressourcengruppen verschoben werden können. Auch wird Schutz für Premium-Speicherkonten in zentralen Indien und Südindien derzeit nicht unterstützt.<br/><br/> [Informationen über](../storage/storage-introduction.md) Azure-Speicher.
**Azure-Netzwerk** | Sie benötigen Azure virtual Network, der Azure-VMs verbunden wird, wenn ein Failover auftritt. Azure virtuelle Netzwerk muss im Bereich Recovery Services Depot.
**Failback von Azure** | Sie benötigen eine temporäre Prozessserver als eine Azure-VM. Sie können diese erstellen, wenn fertig Failback und Abschluss Fehler wieder löschen.<br/><br/> Nicht müsst wieder eine VPN-Verbindung (oder Azure ExpressRoute) aus dem Azure Netzwerk zum lokalen Standort.

## <a name="configuration-server--scale-out-process-prerequisites"></a>Server Configuration / Prozess erforderliche skalieren

Sie können einen lokalen Arbeitsplatz als dem Konfigurationsserver / Skalierung Prozessserver.

**Voraussetzung** | **Details**
--- | ---
**Konfigurationsserver**| Sie benötigen lokale physische oder virtuelle Computer mit Windows Server 2012 R2. Alle lokalen Site Recovery Komponenten sind auf diesem Computer installiert.<br/><br/>VMware VM Replikation empfehlen wir den Server als eine hochverfügbare VMware VM bereitstellen. Wenn Sie physischen Replikation kann der Computer einen physischen Server.<br/><br/> Failback zum lokalen Standort von Azure werden virtuelle VMware-Computer immer, unabhängig davon, ob Sie über VMs oder Servern nicht. Wenn Sie den Konfigurationsserver als VMware VM nicht müssen Sie ein separates master Zielserver als VMware VM einrichten Failback Datenverkehr empfangen bereitstellen.<br/><br/>Wenn der Server VMware VM ist, sollte der Netzwerkadapter VMXNET3. Wenn Sie einen anderen Netzwerkadapter verwenden müssen Sie eine [VMware aktualisieren](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) auf vSphere 5.5-Server installieren.<br/><br/>Der Server sollte eine statische IP-Adresse verfügen.<br/><br/>Der Server sollte kein Domänencontroller sein.<br/><br/>Der Hostname des Servers sollte 15 Zeichen oder weniger.<br/><br/>Das Betriebssystem sollte nur Englisch.<br/><br/> Sie müssen VMware vSphere PowerCLI 6.0 installieren. auf dem Konfigurationsserver.<br/><br/>Der Konfigurationsserver benötigt Internetzugriff. Abgehenden Zugriff ist erforderlich:<br/><br/>Temporären Zugriff auf HTTP-80 während der Installation von Site Recovery Komponenten (MySQL download)<br/><br/>Laufende ausgehenden Zugriff auf HTTPS 443 für Replikations-management<br/><br/>Laufende ausgehenden Zugriff auf HTTPS-9443 für den Replikationsverkehr (dieser Port kann geändert werden)<br/><br/>Server benötigen auch Zugriff auf die folgenden URLs, damit es in Azure zugreifen kann: *. hypervrecoverymanager.windowsazure.com, *. AccessControl.Windows.NET; *. backup.windowsazure.com, *. BLOB.Core.Windows.NET; *. store.core.windows.net<br/><br/>Haben Sie Firewallregeln mit IP-Adresse auf dem Server sicher, dass Regeln in Azure Kommunikation ermöglichen. Sie müssen den [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das Protokoll HTTPS (443).<br/><br/>IP-Adressbereiche für Azure Region Ihres Abonnements und Westen der USA zulassen<br/><br/>URL für MySQL herunterladen können:.http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## <a name="vmware-vcentervsphere-host-prerequisites"></a>VMware vCenter-vSphere Host-Komponenten

**Voraussetzung** | **Details**
--- | ---
**vSphere**| Sie benötigen mindestens VMware vSphere Hypervisor.<br/><br/>Hypervisoren läuft vSphere, Version 6.0, 5.5 oder 5.1 mit den neuesten Updates.<br/><br/>Wir empfehlen, Ihre vSphere Hosts und vCenter Server im selben Netzwerk wie der Prozessserver befinden (dieses Netzwerks mit der Konfigurationsserver befindet, wenn Sie dedizierte Prozessserver einrichten werden).
**vCenter** | Wir empfehlen vCenter VMware vSphere Hosts verwalten Server bereitstellen. Es sollte vCenter, Version 6.0 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>Beachten Sie, dass Site Recovery keine neuen vCenter unterstützt und vSphere 6.0-Funktionen wie vMotion vCenter, virtuelle Volumes, und Storage DRS. Unterstützung von Recovery ist auf Features, die auch in Version 5.5 verfügbar waren.


## <a name="protected-machine-prerequisites"></a>Geschützte Computer erforderliche Komponenten

**Voraussetzung** | **Details**
--- | ---
**Lokal (VMware VMs)** | Virtuelle VMware-Computer schützen möchten, müssen VMware Tools installiert und ausgeführt.<br/><br/> Zu schützenden Computer sollten [Azure Komponenten](site-recovery-best-practices.md#azure-virtual-machine-requirements) zum Erstellen von Azure VMs entsprechen.<br/><br/>Einzelne Festplattenkapazität auf geschützten Computern sollte nicht mehr als 1023 GB sein. Eine VM können bis zu 64 Festplatten (also bis zu 64 TB). <br/><br/>Mindestens 2 GB freier Speicher auf dem Installationslaufwerk für Komponenteninstallation.<br/><br/>Schutz von virtuellen Maschinen mit verschlüsselten Datenträger wird nicht unterstützt.<br/><br/>Freigegebene Datenträger Gast Clustern nicht unterstützt.<br/><br/>**Port 20004** sollte auf geschützten virtuellen Computer lokale Firewall geöffnet werden, wenn **Anwendungskonsistenz**aktivieren möchten.<br/><br/>Computer, auf denen Unified Extensible Firmware Interface (UEFI) Extensible Firmware Interface(EFI) Boot nicht unterstützt.<br/><br/>Computernamen sollten zwischen 1 und 63 Zeichen (Buchstaben, Zahlen und Bindestriche) enthalten. Der Name muss mit einem Buchstaben oder einer Zahl beginnen und enden mit einem Buchstaben oder einer Zahl. Nach der Replikation für einen Computer aktiviert haben, können Sie Azure Namen ändern.<br/><br/>Wenn die VM NIC-teaming hat wird nach einem Failover in Azure einer NIC konvertiert.<br/><br/>Geschützte virtuelle Maschinen haben einen iSCSI-Datenträger konvertiert dann Site Recovery geschützten VM iSCSI-Datenträger in eine VHD-Datei bei die VM in Azure Failover. Wenn das iSCSI-Ziel der Azure-VM erreichbar wird es verbinden und sehen im Wesentlichen zwei Festplatten – die VHD auf Azure VM und die Quelle iSCSI. In diesem Fall müssen Sie das iSCSI-Ziel zu trennen, die auf der Azure-VM.
**Windows-Computer (physischen oder VMware)** | Sollte der Computer unterstütztes 64-Bit-Betriebssystem ausgeführt werden: Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 mit am wenigsten SP1.<br/><br/> Das Betriebssystem sollte auf Laufwerk C:\. Der Betriebssystem-Datenträger sollte eine Basisfestplatte Windows und nicht dynamisch. Der Datenträger kann dynamisch sein.<br/><br/>Site Recovery unterstützt VMs RDM Datenträger. Beim Failback wird Site Recovery Disk RDM Wiederverwenden der ursprünglichen Quelle VM und RDM Datenträger verfügbar ist. Wenn sie nicht verfügbar sind, während des Failbacks erstellt Site Recovery eine neue VMDK-Datei für jeden Datenträger.
**Linux-Maschinen** (physikalische oder VMware)|  Sie benötigen ein unterstütztes 64-Bit-Betriebssystem: Red Hat Enterprise Linux-6.7,7.1,7.2; CentOS 6.5 6.6,6.7,7.0,7.1,7.2; Oracle Enterprise Linux 6.4, 6.5 kompatibel Kernel Red Hat oder unverwüstliche Enterprise Kernel-Version 3 (UEK3), SUSE Linux Enterprise Server 11 SP3 ausgeführt.<br/><br/>/ etc/hosts-Dateien auf geschützten Computern sollten Einträge enthalten, die den lokalen Hostnamen IP-Adressen aller Netzwerkadapter zugeordnet.<br/><br/>Ein Azure virtuellen Computer mit Linux nach einem Failover mit einem Secure Shell-Client (ssh) eine Verbindung herstellen möchten, stellen Sie sicher, dass Secure Shell-Dienst auf dem geschützten Computer automatisch beim Systemstart starten und Firewallregeln zulassen einer ssh Verbindung.<br/><br/>Hostname, Bereitstellungspunkte, Device-Namen und Linux Systempfade und Dateinamen (z. B./Etc /, / usr) sollten nur in Englisch sein.<br/><br/>Schutz kann nur für Computer mit den folgenden Linux aktiviert: Dateisystem (EXT3 ETX4 ReiserFS, XFS); Multipath-Software Gerät Mapper (Multipfad)); Volume-Manager: (LVM2). Physische Server mit HP CCISS Controller werden nicht unterstützt. ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.<br/><br/>Site Recovery unterstützt VMs RDM Datenträger.  Während des Failbacks für Linux verwendet Site Recovery Disk RDM. Stattdessen wird eine neue VMDK-Datei für jeden entsprechenden RDM-Datenträger erstellt.<br/><br/>Stellen Sie sicher, dass Sie die disk.enableUUID=true in den Konfigurationsparametern der VM in VMware eingestellt. Erstellen Sie den Eintrag existiert nicht. Es muss eine konsistente UUID der VMDK-Datei angeben, sodass ordnungsgemäß bereitgestellt. Diese Einstellung hinzufügen gewährleistet, dass nur deltaänderungen beim Failback und keine vollständige Replikation zu lokalen übertragen werden.
**Mobilitätsservice** |  **Windows**: Schieben automatisch mobilitätsservice auf VMs unter Windows müssen Sie ein Administratorkonto (lokaler Administrator auf dem Computer Windows) bereitstellen, sodass der Prozess-Server eine Push-Installation möglich wird.<br/><br/>**Linux**: mobilitätsservice automatisch auf VMs mit Linux müssen ein Konto zu erstellen, die vom Prozessserver verwendet werden können, führen Sie eine Push-Installation übertragen.<br/><br/> Standardmäßig werden alle Laufwerke auf einem Computer repliziert. [Datenträger von der Replikation ausschließen](#exclude-disks-from-replication)muss Mobility-Dienst manuell auf dem Computer installiert werden, vor dem Aktivieren der Replikation.<br/>

## <a name="prepare-for-deployment"></a>Bereiten auf die Bereitstellung vor

Um für die Bereitstellung vorbereiten müssen:

1. [Einrichten einer Azure-Netzwerk](#set-up-an-azure-network) in der Azure-VMs befinden, wenn sie nach einem Failover erstellt haben. Darüber hinaus für das Failback müssen Sie eine VPN-Verbindung (oder Azure ExpressRoute) aus dem Azure Netzwerk Ihrer lokalen Website.
2. [Einrichten eines Kontos Azure-Speicher](#set-up-an-azure-storage-account) für replizierte Daten.
3. [Vorbereiten eines Kontos](#prepare-an-account-for-automatic-discovery) auf den vSphere vCenter hostet, Site Recovery automatisch virtuelle VMware-Computer erkennen, die hinzugefügt werden.
4. [Vorbereiten der Konfigurationsserver](#prepare-the-configuration-server) um sicherzustellen, dass können erforderliche URLs zugreifen und vSphere PowerCLI 6.0 installieren.


### <a name="set-up-an-azure-network"></a>Ein Azure-Netzwerk einrichten

- Das Netzwerk sollte in derselben Azure-Region, in dem Depot Recovery Services bereitgestellt werden.
- Je nach Ressource gewünschte mit Failover Azure VMs richten Sie Azure-Netzwerk im [klassischen Modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)oder [ARM](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .
- Fehlschlagen von Azure Ihrer lokalen VMware Site benötigen Sie eine VPN-Verbindung (oder eine Azure ExpressRoute-Verbindung) aus dem Azure Netzwerk replizierten Azure VMs in lokalen Netzwerk sind in denen Konfigurationsserver befindet.
- [Informationen zu](../vpn-gateway/vpn-gateway-site-to-site-create.md) unterstützten Bereitstellung Modelle für VPN-Standort-zu-Standort-Verbindung und [eine Verbindung einrichten](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- Alternativ können Sie [Azure ExpressRoute](../expressroute/expressroute-introduction.md)einrichten. [Weitere](../expressroute/expressroute-howto-vnet-portal-classic.md) Informationen zum Einrichten von Azure-Netzwerk mit ExpressRoute

> [AZURE.NOTE] [Migration von Netzwerken](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist nicht für Netzwerke für die Bereitstellung von Site Recovery unterstützt.

### <a name="set-up-an-azure-storage-account"></a>Einrichten eines Kontos Azure-Speicher

- Sie benötigen einen Standard oder ein Premium Azure-Speicher für Daten in Azure repliziert. Das Konto muss im Bereich Recovery Services Depot. Je nach Ressource gewünschte mit Failover Azure VMs richten Sie ein Konto im [klassischen Modus](../storage/storage-create-storage-account-classic-portal.md)oder [ARM](../storage/storage-create-storage-account.md) .
- Wenn Sie einen Premium-Konto von replizierten Daten, die müssen Sie erstellen eine zusätzliche Standardkonto Replikationsprotokollen speichern, die laufenden lokalen Daten erfassen verwenden.  

> [AZURE.NOTE] [Migration von Speicherkonten](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist für die Bereitstellung von Site Recovery Storage-Konten nicht unterstützt.

### <a name="prepare-an-account-for-automatic-discovery"></a>Automatische Erkennung ein Kontos vorbereiten

Site Recovery-Prozess-Server erkennen automatisch VMs VMware vSphere Hosts oder ein vCenter Server, der Hosts verwaltet. Automatisches ausführen kann Site Recovery Anmeldeinformationen Entdeckung VMware Server zugreifen. Dies ist relevant, wenn Sie physische Computer replizieren möchten.

1. Um verwenden ein dediziertes Konto für automatische Erkennung eine Rolle (z.B. Azure_Site_Recovery) Ebene vCenter mit den [erforderlichen Berechtigungen](#vmware-account-permissions)erstellen.
2. Erstellen Sie einen neuen Benutzer auf dem Host oder vCenter vSphere und dem Benutzer weisen Sie die Rolle zu. Später lassen Sie Site Recovery über diese Anmeldeinformationen, so dass sie automatische Erkennung durchführen kann.

    >[AZURE.NOTE] Ein vCenter Benutzerkonto mit Schreibschutz-Rolle kann Failover ausgeführt kann geschützte Datenquelle Computer heruntergefahren. Möchten Sie diese Computer herunterfahren benötigen Sie die Rolle [Azure_Site_Recovery](#vmware-account-permissions) . Wenn nur VMs von VMware in Azure migrieren und Failback musst ist die schreibgeschützte Rolle ausreichend.

### <a name="prepare-the-configuration-server"></a>Vorbereiten des konfigurationsservers

1.  Stellen Sie sicher, dass die [erforderliche](#configuration-server-prerequisites)Konfiguration Server verwendeten Computer entspricht. Stellen Sie insbesondere sicher, dass der Computer mit dem Internet zu:

    - Zugriff auf diese URLs: *. hypervrecoverymanager.windowsazure.com, *. AccessControl.Windows.NET; *. backup.windowsazure.com, *. BLOB.Core.Windows.NET; *. store.core.windows.net
    - Zugriff auf [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) MySQL herunterladen.
    - Firewall in Azure [Azure-Rechenzentrum IP-Adressbereiche](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das Protokoll HTTPS (443) zulassen.

2.  Downloaden Sie und installieren Sie [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) auf dem Konfigurationsserver. Andere Versionen von PowerCLI (derzeit werden nicht unterstützt einschließlich R Version 6.0,.)


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Depots Recovery Services

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2. **Klicken Sie auf** > **Management** > **Backup und Site Recovery (OMS)**. Sie können auf **Durchsuchen klicken** > **Recovery Services Vault** > **Hinzufügen**.

    ![Neues Depot](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. **Name** Geben Sie einen Anzeigenamen zu Tresor an. Haben Sie mehrere Abonnements wählen Sie davon.
4. [Eine neue Ressourcengruppe erstellen](../resource-group-template-deploy-portal.md) oder ein vorhandenes Profil auszuwählen. Angeben einer Azure-Region. Computer werden in diesem Bereich repliziert. Beachten Sie, dass Azure-Speicher und Netzwerke für die Site Recovery im selben Bereich müssen. Überprüfen Sie unterstützte Regionen finden Sie unter geografische Verfügbarkeit in [Azure Site Recovery Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Möchten Sie das Depot aus dem Dashboard zugreifen auf **Pin Dashboard** , und klicken Sie auf **Erstellen**.

    ![Neues Depot](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

Neue Depot erscheinen im **Dashboard** > **alle Ressourcen**und auf die **Recovery Services Depots** Blade.

## <a name="getting-started"></a>Erste Schritte

Site Recovery bietet einen Einstieg gelangen Sie oben und so schnell wie möglich. Erforderliche Komponenten überprüft und führt Sie durch die Schritte zu Site Recovery bereitgestellt.

Wählen Sie den Typ der Computer repliziert werden soll und auf repliziert werden soll. Die Infrastruktur, einschließlich auf lokalen Servern, Azure Einstellungen Replikations-Policies und Planung einrichten. Nachdem Ihre Infrastruktur aktivieren Sie Replikation für VMs und Servern. Sie können dann Failover für bestimmte Computer ausführen oder Recovery-Pläne nicht über mehrere Computer erstellen.

Zunächst Einsteiger auswählen wie Sie Site Recovery bereitstellen möchten. Der Einstieg Fluss ändert sich je nach Bedarf Replikation etwas.


## <a name="step-1-choose-your-protection-goals"></a>Schritt 1: Auswählen der Ziele

Wählen Sie replizieren möchten und wo Sie zu replizieren.

1. Blade **-Rückgewinnungsservice Depots** Vault und klicken Sie auf **Settings**.
2. **Einstellungen** > **Erste Schritte** klicken Sie auf **Site Recovery** > **Schritt 1: Vorbereiten Infrastruktur** > **Schutzziel**.

    ![Ziele auswählen](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. **Schutzziel** wählen Sie **, Azure**, und wählen Sie **Ja, mit VMware vSphere Hypervisor**. Klicken Sie auf **OK**.

    ![Ziele auswählen](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Schritt 2: Einrichten der Quell-Umgebung

Konfiguration eingerichtet und im Tresor Recovery Services registrieren. Wenn Sie virtuelle VMware-Computer replizieren sind Sie für die automatische Erkennung verwenden VMware-Konto angeben.

1. Klicken Sie auf **Schritt 1: Vorbereiten Infrastruktur** > **Quelle**. **Vorbereiten der Quelle** haben Sie Konfigurationsserver klicken Sie auf **+ Konfigurationsserver** hinzufügen.

    ![Quelle festlegen](./media/site-recovery-vmware-to-azure/set-source1.png)

2. In Kontrollkästchen Blade- **Server hinzufügen** wird dem **Konfigurationsserver** Server **Geben**.
3. Vor dem Einrichten der Konfigurationsserver Überprüfung der [erforderlichen Software](#configuration-server-prerequisites). In bestimmten sicher, dass der Computer die erforderlichen URLs zugreifen kann.
4.  Installation von Site Recovery Unified Setup herunterladen.
5.  Herunterladen Sie den Vault-Registrierungsschlüssel. Sie benötigen diese beim Unified Setup. Der Schlüssel gilt für 5 Tage, nachdem Sie es erstellt haben.

    ![Quelle festlegen](./media/site-recovery-vmware-to-azure/set-source2.png)

6.  Führen Sie auf dem Computer als Konfigurationsserver verwendeten Unified Setup dem Konfigurationsserver, Prozess-Server und dem master Zielserver installieren.


### <a name="run-site-recovery-unified-setup"></a>Standort-Recovery Unified Setup

1.  Führen Sie die Installationsdatei Setup Unified.
2.  Wählen Sie im **vor** **Konfigurationsserver und Prozess-Server installieren**.

    ![Bevor Sie beginnen](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. **Drittanbieter-Software-Lizenz** klicken Sie auf **ich stimme** zum Herunterladen und Installieren von MySQL.

    ![Dritte = Software anderer Hersteller](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. **Registrierung** durchsuchen Sie, und wählen Sie den Registrierungsschlüssel, die, den Sie aus dem Tresor heruntergeladen.

    ![Registrierung](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. Angegeben Sie **Internet** wie der Anbieter auf dem Konfigurationsserver Azure Site Recovery über das Internet verbunden werden.

    - Möchten Sie mit dem Proxy herstellen, die derzeit auf dem Computer eingerichtet wählen Sie **Verbindung mit vorhandenen Proxyeinstellungen**.
    - Soll den Anbieter direkt wählen Sie **Verbinden direkt ohne Proxy aus**
    - Wenn vorhandene Proxy Authentifizierung erfordert oder einen benutzerdefinierten Proxy für die Verbindung zu einem Internetdienstanbieter verwenden möchten, wählen Sie **Verbindung mit Standardeinstellungen**.
        - Verwenden Sie einen benutzerdefinierten Proxy müssen Sie die Adresse, Anschluss und Anmeldeinformationen angeben
        - Wenn Sie einen Proxy verwenden sollten Sie [Komponenten](#configuration-server-prerequisites)beschrieben URLs bereits zulässig.

    ![Firewall](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. **Erforderliche** Check führt Setup einen sicherstellen, dass die Installation ausgeführt werden kann. Erscheint eine Warnung über den **globalen Synchronisierung überprüfen** überprüfen Sie, dass die Zeit der Systemuhr (**Datum und Uhrzeit** korrekt) Zeitzone identisch.

    ![Erforderliche Komponenten](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. **MySQL** -Konfiguration erstellen Sie Anmeldeinformationen für die Anmeldung an der MySQL-Server-Instanz, die installiert werden.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. **Details zur Umgebung** wählen, ob wirst du virtuelle VMware-Computer replizieren. Sind, sucht Setup PowerCLI 6.0 installiert ist.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. **Installationsordner** wählen zu Binärdateien Cache gespeichert werden soll. Wählen Sie ein Laufwerk mit mindestens 5 GB Speicherplatz, doch empfehlen wir ein Cachelaufwerk mit mindestens 600 GB freien Speicherplatz.

    ![Installation](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. Geben Sie **Netzwerkauswahl** Listener (Netzwerkadapter und SSL-Anschluss) auf dem Konfigurationsserver senden und Empfangen von Replikationsdaten. Ändern Sie die Standard-port (9443). Neben diesen Port Port 443 von einem Webserver dienen die Replikationsvorgänge koordiniert. 443 sollte für den Empfang von Replikation verwendet werden.


    ![Netzwerkauswahl](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  **Zusammenfassung** Informationen Sie der und klicken Sie auf **Installieren**. Nach Abschluss der Installation wird eine Passphrase generiert. Sie benötigen bei Replikation aktivieren so kopieren und an einem sicheren Ort aufbewahren.

    ![Zusammenfassung](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Nachdem der Server beendet die Registrierung in den **Einstellungen**angezeigt > **Server** Blade im Tresor.



#### <a name="run-setup-from-the-command-line"></a>Führen Sie Setup von der Befehlszeile aus

Sie können dem Konfigurationsserver über die Befehlszeile einrichten:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parameter:

- / ServerMode: obligatorisch. Gibt an, ob die Konfiguration und den Prozess-Server installiert werden soll, oder den Prozess-Server. Eingabewerte: CS PS
- InstallLocation: obligatorisch. Der Ordner, in dem die Komponenten installiert sind.
- / MySQLCredsFilePath. Obligatorisch. Der Pfad in dem MySQL-Serveranmeldeinformationen gespeichert sind. Die Datei sollte im folgenden Format:
    - [MySQLCredentials]
    - MySQLRootPassword = "<Password>"
    - MySQLUserPassword = "<Password>"
- / VaultCredsFilePath. Obligatorisch. Den Speicherort der Datei vault
- / EnvType. Obligatorisch. Der Installationstyp. Werte: VMware, NonVMware
- / PSIP und /CSIP. Obligatorisch. Die IP-Adresse des Prozessservers und Konfigurationsserver.
- / PassphraseFilePath. Obligatorisch. Der Speicherort der Datei Passphrase.
- / BypassProxy. Optional. Gibt an, dass der Konfigurationsserver zu Azure einen Proxyserver herstellt.
- / ProxySettingsFilePath. Optional. Proxyeinstellungen (der standardmäßige Proxy erfordert Authentifizierung oder ein benutzerdefinierter Proxy). Die Datei sollte im folgenden Format:
    - [ProxySettings]
    - ProxyAuthentication = "Ja/Nein"
    - Proxy IP = "IP-Adresse >"
    - ProxyPort = "<Port>"
    - ProxyUserName = "<User Name>"
    - Proxypasswordmüssen = "<Password>"
- DataTransferSecurePort. Optional. Portnummer für Replikationsdaten verwendet werden.
- SkipSpaceCheck. Optional. Überspringen Sie Cache Speicherplatz überprüfen.
- AcceptThirdpartyEULA. Obligatorisch. Kennzeichnung der dritten EULA akzeptieren.
- ShowThirdpartyEULA. Obligatorisch. Drittanbieter-EULA anzeigen Als Eingabe werden alle anderen Parameter ignoriert.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>Fügen Sie für die automatische Erkennung verwendet VMware-Konto hinzu

 Wenn Sie für die Bereitstellung bereit haben [ein VMware-Konto erstellt](#prepare-an-account-for-automatic-discovery) Sie, die Site Recovery für die automatische Erkennung verwenden können. Fügen Sie dieses Konto wie folgt:

1. Öffnen Sie **CSPSConfigtool.exe**. Ist als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner \home\svsystems\bin [INSTALLATIONSORT].
2. Klicken Sie auf **Konten** > **Konto hinzufügen**.

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure/credentials1.png)

3. Fügen Sie in den **Kontodetails** Konto für automatische Erkennung hinzu Beachten Sie, dass sie mindestens 15 Minuten für den Kontonamen im Portal angezeigt werden kann. Um sofort zu aktualisieren, klicken Sie auf **Konfigurationsserver** > Servername > **Server aktualisieren**.

    ![Details](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>VSphere Hosts und vCenter Server verbinden

Wenn Sie virtuelle VMware-Computer Replikation vSphere Hosts und vCenter Server verbinden.

1. Überprüfen Sie, ob der Konfigurationsserver Netzwerkzugriff vSphere Hosts und vCenter Server verfügt.
2. Klicken Sie auf **Prepare Infrastruktur** > **Quelle**. Quelle **Vorbereiten** wählen Sie Konfigurationsserver aus und auf **+ vCenter** um vSphere Host oder vCenter Server hinzuzufügen.
3. Geben Sie einen Anzeigenamen für den vSphere Host oder vCenter Server in **vCenter hinzufügen** und geben Sie die IP-Adresse oder den FQDN des Servers. Behalten Sie den Port 443, wenn VMware Server auf einen anderen Port Abhören konfiguriert werden. Wählen Sie das Konto, das zum Verbinden mit VMware Server. Klicken Sie auf **OK**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

    >[AZURE.NOTE] Wenn Sie vCenter Server oder vSphere Host mit einem Konto, das Administratorrechte auf dem vCenter oder Host nicht hinzufügen, stellen Sie sicher, dass das Konto diese Berechtigungen aktiviert: Datacenter Datenspeicher Ordner, Server, Netzwerk, Ressource, virtuelle Maschine vSphere Switch verteilt. Darüber hinaus benötigt der Server vCenter Recht Ansichten speichern.

Site Recovery verbindet VMware Server mit Standardeinstellungen angegeben und VMs entdeckt.

## <a name="step-3-set-up-the-target-environment"></a>Schritt 3: Festlegen der Ziel-Umgebung

Überprüfen Sie haben Sie ein Speicherkonto für die Replikation und ein Azure-Netzwerk mit dem Azure VMs nach einem Failover verbunden wird.

1.  Klicken Sie auf **Prepare Infrastruktur** > **Ziel** und wählen Sie den Azure-Abonnement verwenden möchten.
2.  Geben Sie das Bereitstellungsmodell für VMs nach einem Failover verwenden möchten.
3.  Site Recovery überprüft haben kompatible Azure-Speicherkonten und Netzwerke.

    ![Ziel](./media/site-recovery-vmware-to-azure/gs-target.png)

4.  Wenn Sie noch kein Speicherkonto erstellt und soll einen ARM klicken Sie auf **+ Speicherkonto** , Inline sind.  Blade **Speicherkonto erstellen** Geben Sie einen Kontonamen, Typ, Abonnements und Speicherort. Das Konto sollte im Bereich Recovery Services Depot.

    ![Speicher](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

    Beachten Sie Folgendes:

    - Möchten Sie erstellen ein Speicherkonto der klassischen Modell tun Sie das in Azure-Portal. [Weitere Informationen](../storage/storage-create-storage-account-classic-portal.md)
    - Wenn Sie einen Premium-Speicherkonto für replizierte Daten verwenden müssen Sie eine zusätzliche standardspeicherkonto einrichten meldet Replikation des Informationsspeichers laufende Erfassung ändert für lokale Daten.

    > [AZURE.NOTE] Schutz für Premium-Speicherkonten in zentralen Indien und Südindien wird derzeit nicht unterstützt.

4.  Wählen Sie eine Azure Netzwerk. Wenn Sie noch kein Netzwerk erstellt und, dass möchten ARM klicken Sie auf **+ Netzwerk** , Inline sind. **Virtuelles Netzwerk erstellen** Blade Geben Sie einen Netzwerknamen, Adressbereich Subnetdetails, Abonnements und Speicherort. Das Netzwerk sollte am gleichen Speicherort wie das Depot Recovery Services.

    ![Netzwerk](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

    Möchten Sie ein Netzwerk mit dem klassischen Modell tun Sie das in Azure-Portal. [Erfahren Sie mehr](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Schritt 4: Einrichten von Replikationseigenschaften

1. Erstellen Sie eine neue Replikation Richtlinie klicken Sie auf **Prepare Infrastruktur** > **Replikationseigenschaften** > **+ Erstellen und zuordnen**.
2. Geben Sie **Erstellen und verknüpfen Richtlinie** eine Richtlinie an.
3. **RPO**Schwellenwert: RPO Limit angeben. Warnung werden generiert, wenn fortlaufende Replikation dieses Limit überschreitet.
5. Geben Sie im **Wiederherstellungspunkt Aufbewahrung**in Stunden Dauer der Beibehaltungsdauer für jede Wiederherstellungspunkt werden an. Geschützte Computer können zu einem beliebigen Punkt innerhalb eines Fensters wiederhergestellt werden. Bis zu 24 Stunden Aufbewahrung wird für Computer repliziert Premium-Speicher unterstützt.
6. **App-konsistente Snapshots Häufigkeit**angeben wie oft (in Minuten) anwendungskonsistente Snapshots mit Wiederherstellungspunkte erstellt.
7. Beim Erstellen einer Replikationsrichtlinie wird standardmäßig eine entsprechende Richtlinie automatisch für das Failback erstellt. Zum Beispiel ist die Replikationsrichtlinie **Rep-** dann die Failbackrichtlinie **Rep Richtlinie Failback**. Diese Richtlinie wird nicht verwendet, bis ein Failback zu initiieren.  
8. Klicken Sie auf **OK** , um die Richtlinie zu erstellen.

    ![Replikationsrichtlinie](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Beim Erstellen einer neuen Richtlinie hat es dem Konfigurationsserver automatisch zugeordnet. Klicken Sie auf **OK**.

    ![Replikationsrichtlinie](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## <a name="step-5-capacity-planning"></a>Schritt 5: Planung

Jetzt haben Sie Ihre grundlegende Infrastruktur Sie denken über Planung und herausfinden, ob zusätzliche Ressourcen benötigen.

Site Recovery bietet Kapazitätsplaner helfen die richtigen Ressourcen für Ihre quellumgebung Websitekomponenten, Netzwerke und Speicher reservieren. Sie können im Schnellmodus Einschätzung basiert auf einer durchschnittlichen Anzahl von VMs, Datenträger und Speicher oder im detaillierten Modus, in dem Zahlen Ebene Arbeitslast Eingabe werden, Planer ausführen. Bevor Sie beginnen müssen Sie:

- Informationen Sie zur replikationsumgebung, einschließlich VMs Laufwerke pro VMs und Speicher pro Datenträger.
- Schätzen Sie die tägliche (Churn) Änderungsrate für replizierte Daten müssen. [VSphere Kapazität Planen der Appliance](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) können Sie.

1.  Klicken Sie auf **herunterladen** , um das Tool herunterladen und ausführen. [Lesen Sie den Artikel](site-recovery-capacity-planner.md) , die das Tool beigefügt ist.
2.  Sie danach wählen Sie **Ja** im **haben Sie Planung?**

    ![Planen der Kapazität](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

In der folgenden Tabelle werden einige Punkte zur Unterstützung bei der Planung für dieses Szenario erfasst.


**Komponente** | **Details**
--- | --- | ---
**Replikation** | **Maximale tägliche Änderungsrate**-geschützter Computer können nur ein Prozessserver und ein einzelner Prozessserver kann täglich verarbeiten Änderung bewerten bis zu 2 TB. 2 TB ist somit maximalen täglichen Daten ändern, die für einen geschützten Computer unterstützt wird.<br/><br/> **Maximale Durchsatz**– ein Speicherkonto in Azure kann replizierter Computer gehören. Standard Speicherkonto kann bis zu 20.000 Anfragen pro Sekunde verarbeiten, und es wird empfohlen, die Anzahl von IOPS über ein Quellcomputer 20.000. Für Beispiel haben Sie einen Quellcomputer mit 5 Festplatten und jedes Laufwerk 120 IOPS (8 KB Größe) der Quelle wird dann werden in Azure pro Datenträger IOPS 500. Die Anzahl der Speicherkonten erforderlich = total Quelle IOPs-20000.
**Konfigurationsserver** | Konfigurationsserver sollen die Änderung Ratenkapazität über alle Arbeitslasten, die auf geschützten Computern behandeln und ausreichend Bandbreite kontinuierlich Datenreplikation Azure-Speicher benötigt.<br/><br/> Als bewährte Methode wird empfohlen, der Konfigurationsserver befinden sich im gleichen Netzwerk und LAN-Segment wie die Computer zu schützen. Kann in einem anderen Netzwerk befinden aber zu schützenden Computer sollten Netzwerktransparenz, L3.<br/><br/> Empfohlene Größe für den Konfigurationsserver werden in der folgenden Tabelle zusammengefasst.
**Prozessserver** | Der erste Prozessserver wird standardmäßig auf dem Konfigurationsserver installiert. Sie können weitere Server skalieren Ihrer Umgebung bereitstellen. Beachten Sie Folgendes:<br/><br/> Prozess-Server empfängt der Replikation von geschützten Computern und zwischenspeichern, Komprimierung und Verschlüsselung vor dem Senden in Azure optimiert. Prozess-Servercomputer müssen ausreichende Ressourcen zum Ausführen dieser Aufgaben.<br/><br/> Prozess-Server verwendet datenträgerbasierten Caches. Wir empfehlen eine separate Cachedatenträger 600 GB oder mehr Daten Änderungen bei einem Ausfall des Netzwerks auf Engpässe gespeichert.

### <a name="size-recommendations-for-the-configuration-server"></a>Empfohlene Größe für den Konfigurationsserver

**CPU** | **Speicher** | **Datenträger-Cachegröße** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Steckplätze * 4 Kernen @ 2,5 GHz) | 16 GB | 300 GB | 500 GB oder weniger | Weniger als 100 Computer zu replizieren.
12 vCPUs (2 Steckplätze * 6 Kerne @ 2,5 GHz) | 18 GB | 600 GB | 500 GB bis 1 TB | Zwischen 100-150 Computer replizieren.
16 vCPUs (2 Steckplätze * 8 Kernen @ 2,5 GHz) | 32 GB | 1 TB | 1 TB und 2 TB | Zwischen 150 und 200 Computern repliziert.
Bereitstellen von einem anderen Prozessserver | | | > 2 TB | Bereitstellen Sie weitere Server, sind mehr als 200 Computer replizieren oder ändert die täglichen Daten 2 TB übersteigt.

Wo:

- Jede Quellcomputer wird mit 3 100 GB-Datenträger konfiguriert.
- Benchmarking Speicher 8 SAS-Laufwerke mit 10 K u/Min verwendet mit RAID 10 für Cache Datenträger Maßeinheiten.

### <a name="size-recommendations-for-the-process-server"></a>Empfohlene Größe für den Prozess-server

Wenn Sie mehr als 200 Computer schützen oder tägliche Änderungsrate größer als 2 TB ist können Sie weitere Server behandeln die Replikationslast hinzufügen. Skalieren Sie können:

- Die Zahl der Konfigurationsserver. Beispielsweise können Sie bis zu 400 mit zwei Konfigurationsserver schützen.
- Weitere Server hinzufügen und verwenden dazu den Verkehr statt (oder zusätzlich) dem Konfigurationsserver.

Ein Szenario in der Tabelle:

- Sie planen nicht den Konfigurationsserver als Prozess-Server verwenden.
- Sie haben eine weitere Server einrichten.
- Sie haben geschützte virtuelle Maschinen mit zusätzlicher Prozess-Server konfigurieren.
- Jede geschützte Quellcomputer wird mit drei Datenträgern von 100 GB konfiguriert.

**Konfigurationsserver** | **Weitere server**| **Datenträger-Cachegröße** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Steckplätze * 4 Kernen @ 2,5 GHz), 16 GB Speicher | 4 vCPUs (2 Steckplätze * 2 Kernen @ 2,5 GHz), 8 GB Speicher | 300 GB | 250 GB oder weniger | Replizieren Sie 85 Jahre oder weniger Computer.
8 vCPUs (2 Steckplätze * 4 Kernen @ 2,5 GHz), 16 GB Speicher | 8 vCPUs (2 Steckplätze * 4 Kernen @ 2,5 GHz), 12 GB Speicher | 600 GB | 250 GB bis 1 TB | Replikation zwischen 85 150 Maschinen.
12 vCPUs (2 Steckplätze * 6 Kerne @ 2,5 GHz), 18 GB Speicher | 12 vCPUs (2 Steckplätze * 6 Kerne @ 2,5 GHz) 24 GB Arbeitsspeicher | 1 TB | 1 TB und 2 TB | Zwischen 150-225 Computer replizieren.


Die Möglichkeit, in der Sie Ihre Server skalieren, hängen von der Einstellung für eine Einrichtung oder Modell skalieren.  Sie einige High-End-Konfiguration und Prozess-Server skalieren oder Skalieren von mehr Server mit weniger Ressourcen bereitstellen. Beispiel: 220 Computer schützen soll möglich Folgendes:

- Konfigurationsserver 12vCPU 18 GB Speicher, eine weitere Server mit 12vCPU, 24 GB Speicher richten Sie ein und konfigurieren Sie geschützte Maschinen zusätzliche Prozess-Server verwenden.
- Sie können auch konfigurieren zwei Konfigurationsserver (2 X 8vCPU, 16 GB RAM) und zwei weitere Server (1 x 8vCPU) und 4vCPU x 1 135 + 85 (220) Maschinen und geschützte Maschinen verwenden, die zusätzliche Prozessserver konfigurieren.

Einrichten eines Servers zusätzliche Prozess [gehen](#deploy-additional-process-servers) .

### <a name="network-bandwidth-considerations"></a>Vorüberlegungen zum Netzwerk-Bandbreite

Die Kapazitätsplanertools können Sie berechnen die Bandbreite für die Replikation (erste Replikation und Delta) müssen. Um die bandbreitenverwendung für die Replikation steuern haben Sie einige Optionen:

- **Bandbreite**: VMware, die in Azure repliziert Netzwerkverkehr über einen bestimmten Prozess-Server. Sie können Bandbreite auf den Computern als Prozessserver einschränken.
- **Bandbreite beeinflussen**: die Bandbreite für die Replikation mit zwei Registrierungsschlüssel beeinflussen:
    - Der Registrierungswert " **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** " gibt die Anzahl der Threads, die für die Datenübertragung (Replication Initial oder Delta) einer Festplatte verwendet werden. Ein höherer Wert erhöht die Netzwerkbandbreite für die Replikation verwendet.
    - **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** gibt die Anzahl der Threads, die für die Datenübertragung während des Failbacks verwendet.

#### <a name="throttle-bandwidth"></a>Bandbreite

1. Microsoft Azure Backup-MMC-Snap-in auf dem Computer als Prozess-Server öffnen. Standardmäßig wird eine Verknüpfung für Microsoft Azure Backup auf dem Desktop oder in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klicken Sie im Snap-in- **Eigenschaften ändern**.

    ![Bandbreite](./media/site-recovery-vmware-to-azure/throttle1.png)

3. Wählen Sie auf der Registerkarte **Beschränkung** **Internet-Bandbreite für Backups Einschränkungen aktivieren**, legen Sie die Grenzwerte für die Arbeit und arbeitsfreien Stunden. Gültige Bereiche sind 512 Kbit/s bis 102 MB/s pro Sekunde.

    ![Bandbreite](./media/site-recovery-vmware-to-azure/throttle2.png)

Das Cmdlet " [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) " können Sie Einschränkung festgelegt. Es folgt ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting-NoThrottle** zeigt an, dass keine Einschränkung.


#### <a name="influence-network-bandwidth"></a>Netzwerkbandbreite beeinflussen

1. Navigieren Sie in der Registrierung **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - Beeinflussen den Verkehr Bandbreite replizieren Datenträger ändern Sie den Wert **UploadThreadsPerVM**oder erstellen Sie den Schlüssel, falls er vorhanden ist.
    - Um die Bandbreite für den Datenverkehr von Azure Failback beeinflussen, ändern Sie den Wert **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem Netzwerk "übermäßiger" sollte diese Registrierungsschlüssel aus geändert werden. Der Höchstwert beträgt 32. Überwachen Sie Optimierung des Werts-Datenverkehr.

## <a name="step-6-replicate-applications"></a>Schritt 6: Replizieren Applikationen

Stellen Sie sicher, dass Computer replizieren möchten Mobilität Service-Installation vorbereitet und Replikation aktivieren.

### <a name="install-the-mobility-service"></a>Installieren Sie den Dienst Mobilität

Der erste Schritt beim Schutz von virtuellen Maschinen und Servern aktivieren ist Mobility-Dienst installieren. Hierzu können Sie in einigen Arten:

- **Prozess Serverpush**: Wenn Sie Replikation auf einem Computer aktivieren, push und Mobility-Komponente vom Prozess-Server installieren. Beachten Sie, dass die Push-Installation durchgeführt wird, wenn Computer bereits eine Todate bis Version der Komponente ausgeführt werden.
- **Enterprise-Push**: die Komponente mithilfe des Enterprise Push-Prozesses wie WSUS oder System Center Configuration Manager oder [Azure Automation und optimalen Konfiguration](./site-recovery-automate-mobility-service-install.md)installiert. Richten Sie den Konfigurationsserver bevor Sie dies tun.
- **Manuelle Installation**: Installieren Sie die Komponente manuell auf jedem Computer, die Sie replizieren möchten. Richten Sie den Konfigurationsserver bevor Sie dies tun.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Vorbereitung für automatische Push auf Windows-Computern

Hier ist Windows-Computer so vorbereiten, dass mobilitätsservice automatisch durch den Prozess-Server installiert werden kann.

1.  Erstellen Sie ein Konto, das vom Prozessserver auf dem Computer verwendet werden kann. Das Konto muss Administratorrechte (lokal oder Domäne), und nur für die Push-Installation verwendet.

    >[AZURE.NOTE] Wenn Sie nicht über ein Domänenkonto verwenden, müssen Sie RAS-Benutzer auf dem lokalen Computer deaktivieren. Hierzu fügen Sie in der Registrierung unter HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System DWORD-Eintrag LocalAccountTokenFilterPolicy mit dem Wert 1. Registrierung von CLI-Typ zu **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Die Windows-Firewall des Computers soll zu schützen, wählen Sie **eine Anwendung zulassen oder Feature durch die Firewall**. Aktivieren Sie **die Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation**. Für Computer, die einer Domäne angehören, können Sie die Firewall mit einem Gruppenrichtlinienobjekt konfigurieren.

    ![Firewall Einstellung](./media/site-recovery-vmware-to-azure/mobility1.png)

2. Fügen Sie das Konto, das Sie erstellt haben:

    - Öffnen Sie **Cspsconfigtool**. Ist als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner \home\svsystems\bin [INSTALLATIONSORT].
    - Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
    - Fügen Sie das Konto, das Sie erstellt haben. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Vorbereitung für automatische Push auf Linux-Servern

1.  Stellen Sie sicher, dass Linux-Computer schützen möchten beschriebenen [Komponenten geschützten Computer](#protected-machine-prerequisites)unterstützt wird. Gewährleisten, dass die Netzwerkkonnektivität zwischen dem Linux-Computer und den Prozess-Server.

2.  Erstellen Sie ein Konto, das vom Prozessserver auf dem Computer verwendet werden kann. Das Konto sollte ein Root-Benutzer auf dem Linux-Quellserver und nur für die Push-Installation verwendet.

    - Öffnen Sie **Cspsconfigtool**. Ist als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner \home\svsystems\bin [INSTALLATIONSORT].
    - Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
    - Fügen Sie das Konto, das Sie erstellt haben. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.

3.  Überprüfen Sie, dass etc auf dem Linux-Quellserver Einträge enthält, die den lokalen Hostnamen IP-Adressen aller Netzwerkadapter zugeordnet.
4.  Installieren der neuesten Openssh, Openssh-Server Openssl-Pakete auf dem Computer zu replizieren.
5.  Sicherstellen Sie, dass SSH Port 22 aktiviert ist und ausgeführt wird.
6.  Aktivieren Sie SFTP-Subsystem und Kennwortauthentifizierung in der Datei Sshd_config folgendermaßen:

    - Melden Sie sich als Root an.
    - Suchen Sie in der Datei /etc/ssh/sshd_config die Zeile, die mit **Passwordauthentification**beginnt.
    - Kommentieren Sie die Zeile, und ändern Sie den Wert von **nicht** auf **Ja**.
    - Suchen Sie die Zeile, die mit **Subsystem** beginnt, und kommentieren Sie die Zeile.

        ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Mobility-Dienst manuell installieren

Installationsprogramme sind auf dem Konfigurationsserver in **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

Betriebssystem | Service-Installationsdatei Mobilität
--- | ---
Windows-Server (64 Bit) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (64 Bit) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (64 Bit) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (64 Bit) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-mobility-service-on-a-windows-server"></a>Mobilitätsdienst auf einem WindowsServer installieren


1. Herunterladen und Ausführen des entsprechenden Installers.
2. Wählen Sie bevor **Sie beginnen** **mobilitätsservice aus**

    ![Mobilitätsservice](./media/site-recovery-vmware-to-azure/mobility3.png)

3. Geben Sie in **Server-Konfigurationsdetails** die IP-Adresse des konfigurationsservers und das Kennwort, das beim Ausführen von Setup Unified generiert. Abrufen die Passphrase ausgeführt: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – V** auf dem Konfigurationsserver.

    ![Mobilitätsservice](./media/site-recovery-vmware-to-azure/mobility6.png)

4. Im **Installationsordner** lassen Sie die Standardeinstellung, und klicken Sie auf **Weiter** um die Installation zu starten.
5. Im **Verlauf der Installation** überwachen Sie Installation und starten Sie den Computer aus, wenn Sie dazu aufgefordert werden. Nach der Installation des Dienstes dauert es rund 15 Minuten Status im Portal aktualisieren.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Installieren Sie Mobility-Dienst auf einem Windows-Server mithilfe der Befehlszeile

1. Kopieren Sie das Installationsprogramm auf einem lokalen Ordner (z. B. C:\Temp) auf den Server, den Sie schützen möchten. Das Installationsprogramm kann auf dem Konfigurationsserver unter **[Installationsort] \home\svsystems\pushinstallsvc\repository**gefunden. Paket für Windows-Betriebssysteme haben einen ähnlichen Namen wie Microsoft ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe
2. **Benennen Sie** MobilitySvcInstaller.exe
3. Führen Sie den folgenden Befehl zum Extrahieren des MSI-Installationsprogramms </br>

        C:\> cd C:\Temp
        C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted
        C:\Temp> cd Extracted
        C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>

#####<a name="full-command-line-syntax"></a>Vollständige Befehlszeilensyntax

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parameter**

- **/Role:** Obligatorisch. Gibt an, ob der Mobility-Dienst installiert werden soll. Eingabewerte Agent | MasterTarget
- **/InstallLocation:** Obligatorisch. Gibt an, wo den Dienst installiert.
- **/PassphraseFilePath:** Obligatorisch. Konfiguration Server Passphrase.
- **/LogFilePath:** Obligatorisch. Speicherort, in dem die Installationsprotokolldateien erstellt werden soll.



#### <a name="uninstall-mobility-service-manually"></a>Mobility-Dienst manuell deinstallieren

Mobilitätsdienst kann hinzufügen entfernen Programm aus Bedienfeld oder Befehlszeile deinstalliert werden.

Der Befehl über Befehlszeile Mobility-Dienst deinstallieren

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### <a name="install-mobility-service-on-a-linux-server-using-command-line"></a>Installieren Sie Mobility-Dienst auf einem Linux-Server mit Befehlszeilenoptionen

1. Kopieren Sie das entsprechende Tar-Archiv basierend auf der Tabelle oben mit dem Linux-Computer replizieren möchten.
2. Öffnen Sie ein Shell-Programm und extrahieren Sie das Gezippte Tar-Archiv auf einen lokalen Pfad mit:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Erstellen Sie eine passphrase.txt-Datei im lokalen Verzeichnis, Tar-Archiv extrahiert. Soll dies kopieren die Passphrase aus C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase auf dem Konfigurationsserver und speichern Sie sie in passphrase.txt mit *`echo <passphrase> >passphrase.txt`* in der Schale.
4. Installieren Sie den Dienst Mobilität mit *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Interne IP-Adresse des konfigurationsservers und stellen Sie sicher, dass Port 443 ausgewählt ist. Nach der Installation des Dienstes dauert es rund 15 Minuten Status im Portal aktualisieren.

**Sie können auch von der Befehlszeile aus**:

1. Kopieren Sie die Passphrase aus C:\Program Files (x86) \InMage Systems\private\connection auf dem Konfigurationsserver und als "passphrase.txt" auf dem Konfigurationsserver speichern. Führen Sie diese Befehle. In diesem Beispiel wird die Server-IP-Adresse 104.40.75.37 und der HTTPS-Port 443 sollte:

Auf einem Produktionsserver installieren:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Auf dem master Zielserver zu installieren:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>Aktivieren der Replikation

#### <a name="before-you-start"></a>Bevor Sie beginnen

Wenn Replikation virtuellen VMware beachten Maschinen Folgendes:

- Virtuelle VMware-Computer entdeckt alle 15 Minuten und es kann 15 Minuten oder länger nach Entdeckung im Portal angezeigt werden. Ebenso dauert Discovery mindestens 15 Minuten einen neuen vCenter Server oder vSphere Host hinzufügen.
- Umgebung ändert sich auf dem virtuellen Computer (z. B. VMware Tools-Installation) dauert ebenfalls mindestens 15 Minuten im Portal aktualisiert werden.
- Sie können zuletzt ermittelten für virtuelle VMware-Computer im Feld **Letzte Kontakt am** vCenter Server-vSphere Host im Blade- **Server Konfiguration** überprüfen.
- Fügen Sie Computer für die Replikation ohne geplanten Erkennung markieren Server Configuration (nicht klicken), und klicken Sie auf die Schaltfläche **Aktualisieren** .
- Wenn Sie Replikation aktivieren, wenn der Computer den Prozess-Server automatisch vorbereitet installiert den Dienst Mobilität auf.

#### <a name="exclude-disks-from-replication"></a>Datenträger von der Replikation ausschließen

Wenn Sie Replikation aktivieren, werden standardmäßig alle Laufwerke auf einem Computer repliziert. Sie können Datenträger von der Replikation ausschließen. Beispielsweise sollten nicht replizieren Festplatten mit temporären Daten oder Daten, die jeder einen Computer aktualisiert wurde oder Neustarts der Anwendung (z.B. pagefile.sys oder Tempdb von SQL Server). Wenn Sie ausschließen möchten Festplatten beachten:

- Sie können nur Festplatten ausschließen, die bereits den Mobility-Dienst installiert. Sie müssen [den Mobility-Dienst manuell](#install-the-mobility-service-manually) installieren, weil der Mobilität nur Dienstinstallation Push-Mechanismus verwenden, nachdem die Replikation aktiviert ist.
- Nur Basisdatenträger können von der Replikation ausgeschlossen werden. Betriebssystem oder dynamische Datenträger kann nicht ausgeschlossen werden.
- Nachdem die Replikation aktiviert können nicht Sie hinzufügen oder Entfernen von Festplatten für die Replikation. Wenn hinzufügen oder Ausschließen einer Festplatte müssen Sie zum Schutz des Computers zu deaktivieren und dann reaktivieren.
- Wenn Sie einen Datenträger, der für eine Anwendung erforderlich ist ausschließen, nach einem Failover in Azure müssen Sie es manuell in Azure erstellen, damit die replizierte Anwendung ausführen können. Alternativ können Sie in einen Wiederherstellungsplan beim Failover des Computers erstellen Azure Automatisierung integrieren.
- Datenträger, die Sie manuell in Azure erstellen wieder fehlgeschlagen. Zum Beispiel drei Festplatten fehl und zwei direkt in Azure alle fünf wird werden Fehler zurück. Datenträger manuell erstellt aus Failback kann nicht ausgeschlossen werden.

**Aktivieren der Replikation jetzt wie folgt**:

1. Klicken Sie auf **Schritt2: replizieren Anwendung** > **Quelle**. Nachdem die Replikation erstmalig aktiviert haben klicke Sie **+ replizieren** im Tresor Replikation für zusätzliche Computer aktivieren.
2. In der **Quelle** Blade-> **Source** Konfigurationsserver wählen.
3. Wählen Sie im **Typ** **virtuelle** oder **Physische Maschinen**.
4. Wählen Sie in **vCenter/vSphere Hypervisor** vCenter-Server, der den Host vSphere verwaltet, oder des Hosts. Diese Einstellung ist nicht relevant, wenn Sie physischen Replikation.
5. Wählen Sie den Prozess-Server. Zusätzliche Prozessserver erstellen werden diese Namen dem Konfigurationsserver. Klicken Sie auf **OK**.

    ![Aktivieren der Replikation](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. **Ziel** wählen Sie Vault-Abonnement und im **Post-Failover-Bereitstellungsmodell** Modell (Classic oder Resource Management), das Sie in Azure nach einem Failover verwenden möchten.
7. Wählen Sie das Azure-Speicher, die Sie zum Replizieren von Daten verwenden. Beachten Sie Folgendes:

    - Sie können eine Premium oder standard Konto auswählen. Wenn Sie einen Premium-Konto auswählen müssen Sie zusätzliche Standardspeicher Konto für die Replikation Protokolle. Konten müssen Bereich Recovery Services Depot.
    - Möchten Sie einen anderen Speicher Konto als haben Sie [Erstellen](#set-up-an-azure-storage-account)können. Erstellen ein Konto mit dem ARM-Modell klicken Sie auf **neu erstellen**. Möchten Sie ein Speicherkonto der klassischen Modell erstellen tun Sie diese [im Azure-Portal](../storage/storage-create-storage-account-classic-portal.md).

8. Wählen Sie Azure-Netzwerk und Subnetz mit dem Azure VMs verbunden wird, wenn sie nach einem Failover erstellt haben. Das Netzwerk muss im Bereich Recovery Services Depot. Wählen Sie **konfigurieren jetzt für ausgewählte Computer** die Einstellung für alle Computer ausgewählten Schutz. Wählen Sie **später konfigurieren** Azure Netzwerk pro Computer auswählen. Haben Sie ein Netzwerk müssen Sie [eine](#set-up-an-azure-network)erstellen. Erstellen ein Netzwerks mit ARM-Modells klicken Sie auf **neu erstellen**. Möchten Sie ein Netzwerk mit dem klassischen Modell tun Sie das [in Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wählen Sie ggf. ein Subnetz. Klicken Sie auf **OK**.

    ![Aktivieren der Replikation](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. In **virtuellen Maschinen** > **virtuellen Computer auswählen** auf und wählen Sie jeden Computer, die Sie replizieren möchten. Sie können nur Computer auswählen, für die Replikation aktiviert werden kann. Klicken Sie auf **OK**.

    ![Aktivieren der Replikation](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. **Eigenschaften** > **Eigenschaften konfigurieren**, wählen Sie das Konto, die vom Prozessserver automatisch verwendet werden Mobility-Dienst auf dem Computer installieren. Standardmäßig sind alle Festplatten repliziert. Klicken Sie auf **Alle Festplatten** , und deaktivieren Sie alle Datenträger, die Sie replizieren möchten. Klicken Sie auf **OK**. Sie können später weitere Eigenschaften festlegen.

    ![Aktivieren der Replikation](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. In **Replikationseigenschaften** > **Konfigurieren Replikationseigenschaften** sicher, dass die richtige Replikationsrichtlinie ausgewählt ist. Replikationseinstellungen **Einstellungen**ändern > **Replikationsrichtlinien** > Richtlinienname > **Einstellungen bearbeiten**. Einer Richtlinie anwenden werden Änderungen zu replizieren und neuen Rechnern.

12. Aktivieren Sie, **Multi-VM Konsistenz** Maschinen in einer Replikationsgruppe sammeln und geben Sie einen Namen für die Gruppe. Klicken Sie auf **OK**. Beachten Sie Folgendes:

    - Maschinen Replikation replizieren gruppieren und konsistente und einheitliche Anwendung Wiederherstellungspunkte freigegeben haben, wenn sie Failover.
    - Wir empfehlen Sie VMs und Servern zusammenzufassen, damit sie Ihre Arbeitslasten spiegeln. Ermöglicht Multi-VM Konsistenz kann die Arbeitslast auswirken und sollte nur verwendet werden, wenn Computer die gleiche Arbeitslast ausgeführt werden und Konsistenz.

    ![Aktivieren der Replikation](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Klicken Sie auf **Replikation aktivieren**. Fortschritt des Auftrags **Schutz aktivieren** **Einstellungen**verfolgen > **Aufträge** > **Website Aufträge**. Nach **Abschließen der** Schutzauftrag die Maschine läuft kann Failover.

> [AZURE.NOTE] Wenn der Computer die Push-Installation vorbereitet ist Mobility-Komponente installiert wird, wenn der Schutz aktiviert ist. Nach die Komponente ist auf dem Computer ein Schutzauftrag beginnt und nicht installiert. Nach dem Fehler müssen Sie jeden Computer manuell neu starten. Nach dem Neustart beginnt der Schutzauftrag erneut und anfänglichen Replikation.

### <a name="view-and-manage-vm-properties"></a>Anzeigen und Verwalten von VM-Eigenschaften

Überprüfen Sie die Eigenschaften des Quellcomputers empfohlen. Beachten Sie, dass der Name Azure [Azure Virtual Machine](site-recovery-best-practices.md#azure-virtual-machine-requirements)Vorschriften entsprechen soll.

1. **Klicken Sie** > **repliziert Objekte** > und wählen Sie den Computer. **Essentials** Blade zeigt Informationen über Optionen Maschinen und Status.

2. **Eigenschaften** können Sie Informationen für die VM-Replikation und Failover anzeigen.

    ![Aktivieren der Replikation](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. **Compute**und > **Eigenschaften berechnen** die Azure-VM-Name und Ziel Größe angeben. Ändern Sie den Namen Azure erfüllen Sie benötigen.
Sie können außerdem anzeigen und Informationen zum Zielnetzwerk, Subnetzmaske und IP-Adresse der Azure-VM zugewiesen werden. Beachten Sie Folgendes:

    - Sie können die Ziel-IP-Adresse festlegen. Wenn Sie eine Adresse, die fehlerhafte Maschine angeben werden DHCP verwenden. Wenn Sie eine Adresse, die nicht beim Failover verfügbar ist festlegen, funktioniert das Failover nicht. Die gleiche IP-Zieladresse kann für testfailover verwendet werden, ist die Adresse im Testnetzwerk Failover.
    - Die Anzahl der Netzwerkadapter diktiert die Größe für der virtuelle Zielcomputer wie folgt angeben:

        - Wenn die Anzahl der Netzwerkadapter auf dem Quellcomputer kleiner oder gleich der Anzahl der Netzwerkadapter für die Zielgröße Computer zulässig ist, haben das Ziel die gleiche Anzahl von Adaptern als Quelle.
        - Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputers die Höchstzahl überschreitet für die Größe und die maximale Ziel verwendet werden.
        - Wenn beispielsweise ein Quellcomputer verfügt über zwei Netzwerkadapter die Zielgröße Computer unterstützt vier und dem Zielcomputer müssen zwei Adapter. Wenn der Quellcomputer hat zwei Adapter unterstützten Zielgröße unterstützt jedoch nur eine haben dem Zielcomputer nur eine Netzwerkkarte.     
    - Die VM hat mehrere Netzwerkadapter werden alle im gleichen Netzwerk herstellen.

    ![Aktivieren der Replikation](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. **Datenträger** des Betriebssystems und der Datenträger auf dem virtuellen Computer angezeigt, die repliziert werden.


## <a name="step-7-test-the-deployment"></a>Schritt 7: Testen der Bereitstellung

Um die Bereitstellung testen führen Sie ein testfailover einer einzigen virtuellen Maschine oder einen Wiederherstellungsplan, der einen oder mehrere virtuelle Computer enthält.


### <a name="prepare-for-failover"></a>Bereiten für Failover vor

- Führen Sie ein testfailover wird empfohlen, ein neues Azure-Netzwerk, das isoliert wurde von Azure Produktionsnetzwerk erstellen (Dies ist standardmäßig beim Erstellen eines neuen Netzwerks in Azure). [Weitere Informationen](site-recovery-failover.md#run-a-test-failover) zu Test-Failover ausgeführt.
- Um die beste Leistung bei einem zu Azure Failover, installieren Sie den Azure-Agent auf dem geschützten Computer. Es schneller starten und bei der Problembehandlung. Installieren Sie den Agent [Linux](https://github.com/Azure/WALinuxAgent) oder [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) .
- Um die Bereitstellung vollständig testen benötigen Sie eine Infrastruktur für replizierte Computer wie erwartet. Wenn Sie Active Directory und DNS testen möchten können erstellen einen virtuellen Computer als Domänencontroller mit DNS und dies mit Azure Site Recovery Azure replizieren. Lesen Sie mehr in [Test-Failover Aspekte für Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Stellen Sie sicher, dass der Konfigurationsserver ausgeführt wird. Andernfalls schlägt Failover fehl.
- Wenn Datenträger von der Replikation ausgeschlossen haben müssen Sie diese manuell in Azure nach Failover-Disketten, damit die Anwendung korrekt ausgeführt wird.
- Wenn ein ungeplantes Failovers statt ein testfailover ausführen möchten Beachten Sie Folgendes:

    - Wenn möglich sollten Sie primären Computer herunterfahren, vor dem Ausführen eines ungeplanten Failovers. So wird sichergestellt, dass Quelle und Replikat Computern gleichzeitig haben. Wenn Sie virtuelle VMware-Computer Replikation können Sie angeben, daß Site Recovery versucht, die Quelle Computer herunterfahren. Je nach Zustand des primären Standorts dies möglicherweise oder funktioniert nicht. Wenn Replikation Servern bietet Site Recovery nicht diese Option.
    - Beim Ausführen eines ungeplanten Failovers beendet so alle Delta Daten nicht übertragen werden, nachdem ein ungeplantes Failovers beginnt Datenreplikation von primären Computer. Darüber hinaus beim Ausführen eines ungeplanten Failovers auf einen Wiederherstellungsplan wird bis zum Abschluss, ausgeführt, wenn ein Fehler auftritt.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Verbindung zu Azure VMs nach einem Failover vorbereiten

Wenn Sie Azure VMs mit RDP nach einem Failover möchten, unbedingt Folgendes:

**Auf dem lokalen Computer vor dem Failover**:

- Für den Zugriff über das Internet aktivieren RDP, TCP- und UDP-Regeln für die- **Öffentliche**hinzugefügt werden, und gewährleistet die RDP in der **Windows-Firewall**kann -> **Zugelassene apps und Funktionen** für alle Profile.
- Für den Zugriff über eine Standort-zu-Standort-Verbindung aktivieren RDP auf dem Computer und sicher, dass RDP in der **Windows-Firewall** -> **Zugelassene apps und Features** für **Domäne** und **privaten** Netzwerken.
- Installieren Sie der [Azure-VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) auf dem lokalen Computer.
- [Mobility-Dienst manuell installieren](#install-the-mobility-service-manually) auf Computern anstatt den Prozess-Server den Dienst automatisch übertragen. Ist die Push-Installation nur auftritt, nachdem der Computer für die Replikation aktiviert ist.
- Sicherstellen Sie, dass das Betriebssystem SAN-Richtlinie auf OnlineAll festgelegt ist. [Weitere Informationen]( https://support.microsoft.com/kb/3031135)
- Deaktivieren des IPSec-Dienstes, bevor das Failover ausgeführt.

**Auf der Azure-VM nach dem Failover**:

- Fügen Sie einen öffentlichen Endpunkt für das RDP-Protokoll (Port 3389) und geben Sie die Anmeldeinformationen für die Anmeldung.
- Sicherstellen Sie, dass Sie Domänenrichtlinien nicht, die Verbindung mit einem virtuellen Computer eine öffentliche Adresse verhindern.
- Versuchen Sie, eine Verbindung herzustellen. Sie verbinden können sicher, dass die VM ausgeführt wird. Weitere Tipps zur Problembehandlung finden Sie in diesem [Artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


Wenn Sie eine Azure-VM unter Linux nach einem Failover mit einem Secure Shell-Client (ssh) zugreifen möchten, folgendermaßen Sie vor:

**Auf dem lokalen Computer vor dem Failover**:

- Sicherzustellen Sie, dass Secure Shell-Dienst auf der Azure-VM automatisch beim Systemstart gestartet.
- Überprüfen Sie, Firewall-Regeln eine SSH-Verbindung zu ermöglichen.

**Auf der Azure-VM nach dem Failover**:

- Netzwerksicherheitsregeln-Gruppe auf fehlgeschlagene VM und Azure Subnetz, besteht, müssen eingehende Verbindungen SSH-Port.
- Ein öffentlicher Endpunkt sollte erstellt werden, um eingehende Verbindungen SSH-Port (standardmäßig TCP-Port 22).
- Wenn die VM über eine VPN-Verbindung (Express Route oder Standort-zu-Standort-VPN) zugegriffen wird, kann Verbindung direkt für die VM über SSH Client verwendet.

**Auf das Azure Windows/Linux VM nach dem Failover**:

Haben Sie eine Netzwerk-Sicherheitsgruppe zugeordneten virtuellen Computer oder das Subnetz, zu dem der Computer gehört, achten Sie Network Security Group eine ausgehende Regel HTTP/HTTPS zu ermöglichen. Stellen Sie außerdem sicher, dass DNS im Netzwerk der virtuellen Maschine über erste ist ordnungsgemäß konfiguriert. Sonst konnte das Failover Mal mit Fehler: "PreFailoverWorkflow Aufgabe WaitForScriptExecutionTask Timeout". Um im Detail verstehen, finden Sie im Abschnitt auf [Überwachung und Fehlerbehebung](site-recovery-monitoring-and-troubleshooting.md#recovery).

## <a name="run-a-test-failover"></a>Führen Sie ein testfailover

1. Failover von einem einzigen Computer in **Einstellungen** > **Elemente repliziert**, klicken Sie auf die VM > **+ Testfailover** -Symbol.

    ![Testfailover](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Einen Wiederherstellungsplan **Einstellungen**Failover > **Recovery-Pläne**, mit der rechten Maustaste des Plans > **Test-Failover**. Erstellen Sie eine plan für die Wiederherstellung [Führen Sie diese Schritte](site-recovery-create-recovery-plans.md).

3. Wählen Sie **Test** -Failover Azure-Netzwerk mit dem Azure VMs nach einem Failover verbunden werden.
4. Klicken Sie auf **OK** , um das Failover zu beginnen. Fortschritt kann durch Klicken auf die Eigenschaften des virtuellen Computers oder für den Einzelvorgang **Test-Failover** Depotnamen > **Einstellungen** > **Aufträge** > **Website Aufträge**.
5. Erreicht das Failover Status **vollständig zu testen** , führen Sie folgende Schritte aus:

    1. Replikat VM in Azure-Portal anzeigen Stellen Sie sicher, dass die virtuellen Computer erfolgreich gestartet.
    2. Wenn Sie aus dem lokalen Netzwerk Zugriff auf virtuelle Computer eingerichtet sind, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer initiieren.
    3. Klicken Sie auf **test abschließen** , fertig zu stellen.

        ![Testfailover](./media/site-recovery-vmware-to-azure/test-failover6.png)


    4. Klicken Sie auf **Notizen** zu speichern Anmerkungen testfailover zugeordnet.
    5. Klicken Sie auf die Umgebung automatisch bereinigen **testfailover ist abgeschlossen** . Danach wird das testfailover Status **abgeschlossen** angezeigt.
    6.  In dieser Phase werden Elemente oder VMs erstellt automatisch Site Recovery bei einem testfailover gelöscht. Zusätzliche Test-Failover erstellten Elemente werden nicht gelöscht.

    > [AZURE.NOTE] Wenn ein testfailover mehr weiterhin als zwei Wochen mit abgeschlossen ist.


6. Nach Abschluss des Failovers sollten auch möglicherweise das Replikat Azure finden Computer in Azure-Portal angezeigt > **virtuelle Computer**. Sie sollten sicherstellen, dass die VM die entsprechende Größe aufweist, die mit dem entsprechenden Netzwerk verbunden ist und ausgeführt wird.
7. Sollten Sie [für Verbindungen nach einem Failover](#prepare-to-connect-to-azure-vms-after-failover) Sie Azure VM herstellen.

## <a name="monitor-your-deployment"></a>Überwachen der Bereitstellung

Hier ist, wie Sie die Konfiguration, Status und Zustand für Ihre Site Recovery-Bereitstellung überwachen können:

1. Klicken Sie auf das Depot auf dem **Essentials** -Dashboard. In diesem Dashboard können Sie Site Recovery Aufträge, Replikationsstatus Recovery-Pläne, Serverzustand und Ereignisse.  Sie können Grundlagen der Kacheln und Layouts, die Sie z. B. den Status der anderen Site Recovery und Backup-Depots besonders hilfreich sind.<br>
![Grundlagen](./media/site-recovery-vmware-to-azure/essentials.png)

2. Der **Gesundheit** Kachel können Sie Site Server (VMM oder Konfiguration) überwachen, die Problem und die in den letzten 24 Stunden von Site Recovery ausgelösten Ereignisse auftreten.
3. Verwalten und Überwachen der Replikation der **Objekte repliziert**, **Recovery-Pläne**, und **Wiederherstellungsaufträge Website** angeordnet. Drilldown können in Aufträge in **Einstellungen** -> **Aufträge** -> **Website Aufträge**.


## <a name="deploy-additional-process-servers"></a>Bereitstellung zusätzlicher Server

Haben Sie zum Skalieren der Bereitstellung über 200 Quelle Maschinen oder eine tägliche Änderung Gesamtrate von mehr als 2 TB benötigen Sie weitere Server das Volumen.

Überprüfen Sie die [empfohlene Größe für Prozessserver](#size-recommendations-for-the-process-server) und dann gehen Sie den Prozess-Server einrichten. Nach dem Einrichten des Servers werden Quelle Maschinen verwenden migrieren.

### <a name="install-an-additional-process-server"></a>Einen Weitere Server installieren

1. **Einstellungen** > **Site Recovery Server** klicken Sie auf dem Konfigurationsserver > **Process-Server**.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. Klicken Sie unter **Servertyp** auf **Prozessserver (lokal)**.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. Herunterladen Sie die Site Recovery Unified Setup-Datei und führen Sie Prozess-Server installieren und registrieren es im Tresor aus.
4. Wählen Sie bevor **Sie beginnen** **Hinzufügen zusätzlicher Server skalieren Bereitstellung aus**
5. Führen Sie den Assistenten auf die gleiche Weise habe bei Sie Konfigurationsserver [Einrichten](#step-2-set-up-the-source-environment) .

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. Geben Sie in **Server-Konfigurationsdetails** die IP-Adresse des konfigurationsservers und das Kennwort. Die Passphrase führen zu ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** auf dem Konfigurationsserver.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrieren von Maschinen für den neuen Prozessserver

1. **Einstellungen** > **Site Recovery Server** auf den Konfigurationsserver und **Prozess-Server**zu erweitern.

    ![Aktualisieren Process-server](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Aktuelle Prozess-Server und **Switch**.

    ![Aktualisieren Process-server](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. Unter **Prozess Zielserver auswählen**die Option neue Prozess-Server, und wählen Sie anschließend den virtuellen Computer, die der Prozess-Server behandelt werden soll. Klicken Sie auf das Informationssymbol, um Informationen über den Server. Beschlüsse laden zu helfen wird durchschnittliche Speicherplatz, die für jeden ausgewählten virtuellen Computer auf den neuen Prozessserver replizieren. Klicken Sie zum Starten auf den neuen Prozessserver replizieren.

## <a name="vmware-account-permissions"></a>VMware-Kontoberechtigungen

Prozess-Server erkennen automatisch VMs auf vCenter Server. Automatische Erkennung ausführen müssen definieren [eine Rolle (Azure_Site_Recovery)](#prepare-an-account-for-automatic-discovery) Sie Site Recovery auf VMware Server zugreifen können. Beachten Sie, dass nur müssen Sie VMware Maschinen in Azure migrieren und nicht von Azure Failback müssen, können Sie eine schreibgeschützte Rolle ausreichend. Erforderliche Rollenberechtigungen werden in der folgenden Tabelle zusammengefasst.

**Rolle** | **Details** | **Berechtigungen**
--- | --- | ---
Azure_Site_Recovery-Rolle | VMware VM-Ermittlung |Weisen Sie diese Berechtigungen für den Server V-Center:<br/><br/>Datenspeicher -> Allocate Speicherplatz, durchsuchen Datenspeicher, niedrige Datei Operationen., Datei entfernen, virtuellen Aktualisierungsdateien<br/><br/>Netzwerk -> Netzwerk zuweisen<br/><br/>Ressourcen -> Zuweisen von virtuellen Ressourcenpool ausgeschalteten virtuellen Computer migrieren, Migration virtueller Computer eingeschaltet<br/><br/>Aufgaben-Task erstellen, Update-Task ><br/><br/>VM-Konfiguration ><br/><br/>Virtual Machine-Interaktion > -> Frage beantworten Gerät Verbindung. konfigurieren CDs, Disketten konfigurieren, schalten Sie die Stromversorgung, VMware Tools installieren<br/><br/>Virtual Machine-Lager > -> erstellen, registrieren, Registrierung<br/><br/>VM-Bereitstellung > -> zulassen VM herunterladen, Hochladen von virtuellen Dateien zulassen<br/><br/>VM-Snapshots > -> Snapshots entfernen
vCenter Benutzerrolle | VMware VM Discovery/Failover ohne Herunterfahren der Quelle VM | Weisen Sie diese Berechtigungen für den Server V-Center:<br/><br/>Rechenzentrum-Objekt hat untergeordnete Objekt übertragen Rolle = schreibgeschützt <br/><br/>Der Benutzer Datacenter Ebene zugewiesen und hat damit Zugriff auf alle Objekte im Datencenter.  Untergeordnete Objekte (vSphere Hosts, Datenspeicher, virtuelle Computer und Netzwerke) Wenn Sie den Zugriff beschränken möchten, weisen Sie die Rolle **keinen Zugriff** **übertragen zum untergeordneten** Objekt zu.
vCenter Benutzerrolle | Failover und failback | Weisen Sie diese Berechtigungen für den Server V-Center:<br/><br/>Datacenter Objekt übertragen, untergeordnetes Objekt Rolle = Azure_Site_Recovery<br/><br/>Der Benutzer Datacenter Ebene zugewiesen und hat damit Zugriff auf alle Objekte im Datencenter.  Wenn Sie den Zugriff beschränken möchten, weisen Sie die Rolle **keinen Zugriff** **auf untergeordnete Objekt übertragen** auf das untergeordnete Objekt (vSphere Hosts, Datenspeicher, virtuelle Computer und Netzwerke).  
## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr](site-recovery-failover.md) über verschiedene Failover.
- [Erfahren Sie mehr über Failback](site-recovery-failback-azure-to-vmware.md) zu Ihrem fehlgeschlagen auf Computern in Azure zu Ihrer lokalen Umgebung.

## <a name="third-party-software-notices-and-information"></a>Drittanbieter-Software und Hinweise

Nicht übersetzen bzw. lokalisieren

Software und Firmware aus Microsoft-Produkt oder Dienst basiert oder aus der unten aufgeführten Projekte enthält (zusammen "Drittanbieter-Code).  Microsoft ist nicht die ursprünglichen Autor Drittanbieter-Code.  Ursprüngliche Urheberrechtshinweis und Lizenz unter dem Microsoft solcher Drittanbieter-Code empfangen werden unten festgelegt.

Die Informationen in Abschnitt A wird dritten Codekomponenten aufgeführten Projekte. Solche Lizenzen und Informationen dienen nur zu Informationszwecken.  Dieser dritte wird Ihnen von Microsoft unter Microsoft Software Lizenzierungsinformationen für das Microsoft-Produkt oder Dienst nachlizenziert wird.  

Die in Abschnitt B ist Drittanbieter Codekomponenten Informationen, die Ihnen von Microsoft unter der ursprünglichen Lizenzierungsinformationen bereitgestellt werden.

Die vollständige Datei finden im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft behält sich alle hierin nicht ausdrücklich gewährten Rechte durch Implikation, weder ausdrücklich noch konkludent oder auf andere Weise.
