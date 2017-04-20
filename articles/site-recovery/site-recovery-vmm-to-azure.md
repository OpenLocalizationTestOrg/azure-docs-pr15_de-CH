<properties
    pageTitle="Replizieren von Hyper-V virtuelle Computer in VMM-Clouds in Azure Azure-Portal Site Recovery mit | Microsoft Azure"
    description="Beschreibt das Bereitstellen von Azure Site Recovery, Replikation, Failover und Recovery von Hyper-V virtuelle Computer in VMM-Clouds in Azure mithilfe von Azure Portal organisieren"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-azure-site-recovery-with-the-azure-portal--microsoft-azure"></a>Replizieren von Hyper-V virtuelle Computer in VMM-Clouds in Azure Azure-Portal mit Azure Site Recovery | Microsoft Azure

> [AZURE.SELECTOR]
- [Azure-portal](site-recovery-vmm-to-azure.md)
- [Klassische Azure](site-recovery-vmm-to-azure-classic.md)
- [PowerShell-Ressourcen-Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [PowerShell classic](site-recovery-deploy-with-powershell.md)

Willkommen bei Azure Site Recovery! Verwenden Sie in diesem Artikel möchten Sie lokale Hyper-V virtuelle Computer replizieren in System Center Virtual Machine Manager (VMM) Wolken in Azure-Portal Azure Site Recovery bei Azure verwaltet.

> [AZURE.NOTE]Azure hat zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model
> ) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager und Classic. Azure verfügt außerdem über zwei Portale – klassischen Azure-Portal, das klassische Bereitstellungsmodell unterstützt und Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle.


Azure Site Recovery im Azure-Portal bietet einige neue Features:

- Azure Backup und Azure Site Recovery Services sind im Azure-Portal in einem Tresor Recovery Services zusammengefasst, sodass einrichten und Verwalten von Business Continuity und Disaster Recovery (BCDR) von einem einzigen Ort. Ein einheitliches Dashboard ermöglicht das Überwachen und Verwalten von Operationen über Ihren lokalen Sites und Azure öffentlichen Cloud.
- Benutzer mit Azure Cloud Solution Provider (CSP) Programm bereitgestellt können jetzt Site Recovery-Vorgänge im Azure-Portal verwalten.
- Site Recovery im Azure-Portal kann Computer in Azure Ressourcenmanager Speicherkonten replizieren. Bei einem Failover erstellt Site Recovery Ressourcenmanager basierende VMs in Azure.
- Site Recovery weiterhin klassische Speicherkonten Replikation. Bei einem Failover erstellt Site Recovery VMs mit der Option Klassisch.


Dieser Artikel alle Kommentare unten in die Kommentare Disqus. Fragen Sie technische [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Übersicht

Organisationen benötigen eine BCDR Strategie, die bestimmt, wie apps Arbeitslasten und Daten bleiben aktiv bei geplanten und ungeplanten Ausfällen normalen Arbeitsbedingungen so bald wie möglich wiederherstellen. Die BCDR Strategie sollten schützen Sie Daten wiederhergestellt und Arbeitslasten ständig verfügbar bleiben beim Notfall.

Site Recovery ist eine Azure Service BCDR Strategie orchestrieren Replikation von lokalen Servern und virtuellen Computer in die Cloud (Azure) oder zu einem sekundären Datencenter beiträgt. Treten Ausfälle in Ihrem primären Standort Failover Sie sekundäre an apps und Arbeitslasten zur Verfügung. Sie nicht an Ihrem primären Standort zu beenden. Weitere Informationen [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md)

Dieser Artikel enthält alle Informationen, die Sie replizieren möchten Hyper-V virtuelle Computer in VMM-Clouds in Azure lokal. Es enthält eine Architekturübersicht Informationen und Bereitstellungsschritte für die Konfiguration von Azure, lokalen Server Replikationseigenschaften und Planung planen. Nach dem Einrichten der Infrastruktur können Sie Replikation auf Computern zu schützen, und überprüfen Sie, dass ein Failover funktioniert.


## <a name="business-advantages"></a>Geschäftliche Vorteile

- Site Recovery schützt externen Unternehmen Arbeitslasten und Hyper-V VMs ausgeführt.
- Das Recovery-Portal stellt eine zentrale Stelle zum Einrichten, verwalten und Überwachen von Replikation, Failover und Recovery.
- Sie können problemlos Failover Ihrer lokalen Infrastruktur Azure und Failback (Wiederherstellen) von Azure Hyper-V-Host-Server in der lokalen Website ausführen.
- Sie können Recovery-Pläne mit mehreren Computern konfigurieren, sodass tiered Arbeitslasten zusammen Failover.


## <a name="scenario-architecture"></a>Szenario-Architektur


Szenariokomponenten sind:

- **VMM-Server**: eine lokale VMM-Server mit einer oder mehreren Wolken.
- **Hyper-V-Host oder Cluster**: Hyper-V-Hostserver oder Cluster verwaltet VMM-Clouds.
- **Azure Site Recovery und Recovery Services Agent**: während der Bereitstellung Sie Azure Site Recovery-Anbieter auf dem VMM-Server und Microsoft Azure Recovery Services Agent auf Hyper-V-Host-Server installieren. Der Anbieter auf dem VMM-Server kommuniziert mit Site Recovery über HTTPS 443 Orchestrierung replizieren. Der Agent auf dem Hyper-V-Hostserver repliziert Daten standardmäßig Azure-Speicher über HTTPS 443.
- **Azure**: Sie benötigen Azure-Abonnement, ein Konto Azure-Speicher repliziert Daten und Azure virtual Network, Azure VMs nach einem Failover mit einem Netzwerk verbunden sind.

![E2A Topologie](./media/site-recovery-vmm-to-azure/architecture.png)


## <a name="azure-prerequisites"></a>Azure-Komponenten

Hier ist Sie in Azure, benötigen Sie dieses Szenario bereitstellen.

**Voraussetzung** | **Details**
--- | ---
**Ein Azure-Konto**| Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.
**Azure-Speicher** | Sie benötigen ein Konto standard Azure-Speicher zum Speichern von replizierter Daten. Sie können ein Speicherkonto LRS oder g. Wir empfehlen g, damit Daten sowie einen regionalen Ausfall oder die primäre Region wiederhergestellt werden kann. [Erfahren Sie mehr](../storage/storage-redundancy.md). Das Konto muss im Bereich Recovery Services Depot.<br/><br/>Premium-Speicher wird nicht unterstützt.<br/><br/> Replizierte Daten in Azure-Speicher gespeichert und Azure VMs werden erstellt, wenn ein Failover auftritt. <br/><br/> [Informationen über](../storage/storage-introduction.md) Azure-Speicher.
**Azure-Netzwerk** | Sie benötigen ein Azure virtuelles Netzwerk, dem Azure VMs Verbindung bei einem Failover. Das Netzwerk muss im Bereich Recovery Services Depot.

## <a name="on-premises-prerequisites"></a>Lokalen Komponenten

Sie benötigen lokale

**Voraussetzung** | **Details**
--- | ---
**VMM**| Eine oder mehrere VMM-Server auf System Center 2012 R2 ausgeführt. Jede VMM-Server müssen einer oder mehreren Wolken konfiguriert. Eine Wolke sollte enthalten:<br/><br/> Eine oder mehrere VMM-Hostgruppen.<br/><br/> Hyper-V-Hostserver oder Cluster in jede Hostgruppe.<br/><br/>[Weitere](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) Informationen zum Einrichten von VMM-Clouds.
**Hyper-V** | Hyper-V-Hostservern laufen mindestens **Windows Server 2012 R2** mit Hyper-V-Rolle oder **Microsoft Hyper-V Server 2012 R2** und die neuesten Updates installiert haben.<br/><br/> Hyper-V Server sollte eine oder mehrere VMs.<br/><br/> Einem Hyper-V-Hostserver oder Cluster, die VMs enthält Sie replizieren möchten, muss in der VMM-Cloud verwaltet werden.<br/><br/>Hyper-V Server sollte entweder direkt oder über einen Proxyserver mit dem Internet verbunden werden.<br/><br/>Hyper-V Server sollten gemäß Artikel [2961977](https://support.microsoft.com/kb/2961977) installiert ist.<br/><br/>Hyper-V-Hostserver benötigen Internetzugang Datenreplikation in Azure.
**Anbieter und agent** | Bei Azure Site Recovery-Bereitstellung installieren Sie Azure Site Recovery-Anbieter auf dem VMM-Server und Recovery Services Agent auf Hyper-V-Hosts. Der Anbieter und der Agent müssen Azure über das Internet direkt oder über einen Proxyserver herstellen. Beachten Sie, dass ein HTTPS-basierte Proxy nicht unterstützt. Proxy-Server auf dem VMM-Server und Hyper-V-Hosts sollte Zugriff auf: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blob.core.windows.net <br/><br/> *. store.core.windows.net<br/><br/>Haben Sie Firewallregeln mit IP-Adresse auf dem VMM-Server sicher, dass Regeln in Azure Kommunikation ermöglichen. Sie müssen [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/confirmation.aspx?id=41653) und HTTPS (443)-Anschluss.<br/><br/>IP-Adressbereiche für Azure Region Ihres Abonnements und Westen der USA zulassen<br/><br/>Außerdem. der Proxyserver auf dem VMM-Server benötigt Zugriff auf https://www.msftncsi.com/ncsi.txt


## <a name="protected-machine-prerequisites"></a>Geschützte Computer erforderliche Komponenten


**Voraussetzung** | **Details**
--- | ---
**Geschützte VMs** | Failover einer VM Vergewissern Sie erfüllt der Namen, der Azure-VM zugewiesen, [Azure Komponenten](site-recovery-best-practices.md#azure-virtual-machine-requirements). Sie können den Namen ändern, nachdem Sie die Replikation für die VM aktiviert haben. <br/><br/> Einzelne Festplattenkapazität auf geschützten Computern sollte nicht mehr als 1023 GB sein. Eine VM können bis zu 16 Festplatten (also bis zu 16 TB).<br/><br/> Freigegebene Datenträger Gast Clustern nicht unterstützt.<br/><br/> Extensible Firmware Interface (UEFI) Unified Extensible Firmware Interface(EFI) Boot wird nicht unterstützt.<br/><br/> Wenn die VM NIC-teaming hat wird nach einem Failover in Azure einer NIC konvertiert.<br/><br/>Schützen von VMs mit Linux und eine statische IP-Adresse wird nicht unterstützt.

## <a name="prepare-for-deployment"></a>Bereiten auf die Bereitstellung vor

Für die Bereitstellung vorbereiten möchten:

1. [Einrichten einer Azure-Netzwerk](#set-up-an-azure-network) in der Azure-VMs nach einem Failover befinden.
2. [Einrichten eines Kontos Azure-Speicher](#set-up-an-azure-storage-account) für replizierte Daten.
4. [Vorbereiten der VMM-Server](#prepare-the-vmm-server) Site Recovery-Bereitstellung.
5. [Vorbereitung der Netzwerkübersicht](#prepare-for-network-mapping). Netzwerke so richten Sie ein, dass Netzwerkzuordnung während der Site Recovery-Bereitstellung konfigurieren können.

### <a name="set-up-an-azure-network"></a>Ein Azure-Netzwerk einrichten

Sie benötigen eine Azure-Netzwerk, Azure-VMs nach Failover Verbindung wird erstellt.

- Das Netzwerk sollte im selben Bereich wie der in dem Depot Recovery Services bereitstellen.
- Je nach Ressource gewünschte mit Failover Azure VMs richten Sie Azure-Netzwerk im [Ressourcenmanager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder [klassischen Modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Es wird empfohlen, dass Sie ein Netzwerk einrichten, bevor Sie beginnen. Andernfalls müssen Sie es während der Bereitstellung der Site Recovery.

> [AZURE.NOTE] [Migration von Netzwerken](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist nicht für Netzwerke für die Bereitstellung von Site Recovery unterstützt.


### <a name="set-up-an-azure-storage-account"></a>Einrichten eines Kontos Azure-Speicher

- Sie benötigen ein Konto standard Azure-Speicher für Daten in Azure repliziert. Das Konto muss im Bereich Recovery Services Depot.
- Je nach Ressource gewünschte mit Failover Azure VMs richten Sie ein Konto im [Ressourcenmanager](../storage/storage-create-storage-account.md) oder [klassischen Modus](../storage/storage-create-storage-account-classic-portal.md).
- Es wird empfohlen, dass Sie ein Konto einrichten, bevor Sie beginnen. Andernfalls müssen Sie es während der Bereitstellung der Site Recovery.

> [AZURE.NOTE] [Migration von Speicherkonten](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist für die Bereitstellung von Site Recovery Storage-Konten nicht unterstützt.

### <a name="prepare-the-vmm-server"></a>Vorbereiten des VMM-Servers

- Stellen Sie sicher, dass [Komponenten](#on-premises-prerequisites)der VMM-Server entspricht.
- Während der Bereitstellung der Site Recovery können alle Wolken auf dem VMM-Server in der Azure-Portal verfügbar sein sollen. Wenn nur bestimmte Wolken im Portal angezeigt werden soll, können Sie diese Einstellung in der Cloud in der VMM-Verwaltungskonsole aktivieren.


### <a name="prepare-for-network-mapping"></a>Vorbereitung der Netzwerkübersicht

Netzwerkzuordnung während der Bereitstellung der Site Recovery eingerichtet werden müssen. Netzwerkzuordnung ordnet zwischen VMM VM-Netzwerke und Azure Netzwerke, um Folgendes Ziel:

- Computer im selben Netzwerk Failover können, auch wenn sie nicht auf die gleiche Weise oder im gleichen Wiederherstellungsplan Failover sind.
- Ein Netzwerk-Gateway auf dem Zielcomputer Azure-Netzwerk eingerichtet ist, können virtuelle Azure-Computer lokalen virtuellen Maschinen.
- Um Netzwerk ist hier zuordnen müssen vorbereiten:

    - Stellen Sie sicher, dass VMs auf der Hyper-V-Host mit einem VMM VM-Netzwerk verbunden sind. Das Netzwerk sollte ein logisches Netzwerk verbunden, das der Cloud zugeordnet ist.
    - Ein Azure-Netzwerk wie beschrieben [oben](#set-up-an-azure-network)

- [Erfahren Sie mehr](site-recovery-network-mapping.md) über die Funktionsweise von Netzwerkzuordnung.


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Depots Recovery Services

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2. **Klicken Sie auf** > **Management** > **Recovery Services**. Sie können auf **Durchsuchen klicken** > **Recovery Services** Depots > **Hinzufügen**.

    ![Neues Depot](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. Geben Sie im Feld **Name**einen Anzeigenamen zu Tresor an Haben Sie mehrere Abonnements wählen Sie davon.
4. [Erstellen einer Ressourcengruppe](../resource-group-template-deploy-portal.md), oder wählen Sie eine vorhandene. Angeben einer Azure-Region. Computer werden in diesem Bereich repliziert. Überprüfen Sie unterstützte Regionen finden Sie unter geografische Verfügbarkeit in [Azure Site Recovery Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Wenn Sie das Depot aus dem Dashboard zugreifen möchten, klicken Sie auf **Pin zum Dashboard** > **Depot erstellen**.

    ![Neues Depot](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Neue Depot wird im **Dashboard** > **alle Ressourcen**und auf die **Recovery Services Depots** Blade.

## <a name="getting-started"></a>Erste Schritte

Site Recovery bietet einen Einstieg Erfahrung, mit der Sie so schnell wie möglich bereitstellen. Erste Schritte überprüft Komponenten und Site Recovery-Bereitstellung in der richtigen Reihenfolge Schritte durchläuft.

Erste Schritte Sie wählen den Computer repliziert werden soll und auf repliziert werden soll. Einrichten von lokalen Servern, Azure-Speicherkonten und Netzwerke. Sie erstellen Replikations-Policies und Planung durchführen. Nach dem Einrichten der Infrastruktur können Sie Replikation für virtuelle Computer. Sie können ausführen Failover für bestimmte Computer oder Recovery-Pläne nicht über mehrere Computer erstellen.

Zunächst Einsteiger auswählen wie Sie Site Recovery bereitstellen möchten. Der Einstieg Fluss ändert sich je nach Bedarf Replikation etwas.



## <a name="step-1-choose-your-protection-goals"></a>Schritt 1: Auswählen der Ziele

Wählen Sie replizieren möchten und wo Sie zu replizieren.

1. Blade **-Rückgewinnungsservice Depots** Vault und klicken Sie auf **Settings**.
2. Klicken Sie auf der **Einstieg** **Site Recovery** > **Schritt 1: Vorbereiten Infrastruktur** > **Schutzziel**.

    ![Ziele auswählen](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. **Schutzziel** wählen Sie **, Azure**, und wählen Sie **Ja, mit Hyper-V**. Wählen Sie **Ja** zu bestätigen, dass Sie VMM verwenden zum Verwalten von Hyper-V-Hosts und Recovery-Standort. Klicken Sie auf **OK**.

    ![Ziele auswählen](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## <a name="step-2-set-up-the-source-environment"></a>Schritt 2: Einrichten der Quell-Umgebung

Installieren Sie Azure Site Recovery-Anbieter auf dem VMM-Server und registrieren Sie den Server im Tresor. Installieren Sie den Agent Azure Recovery Services auf Hyper-V-Hosts.

1. Klicken Sie auf **Schritt 2: Vorbereiten der Infrastruktur** > **Quelle**.

    ![Quelle festlegen](./media/site-recovery-vmm-to-azure/set-source1.png)

2. **Vorbereiten der Datenquelle** klicken Sie auf **+ VMM** VMM-Server hinzufügen.

    ![Quelle festlegen](./media/site-recovery-vmm-to-azure/set-source2.png)

3. Im Scheck Blade- **Server hinzufügen** , den **Typ** und **System Center VMM-Server** angezeigt wird entspricht der VMM-Server [erforderliche und URL-Anforderungen](#on-premises-prerequisites).
4. Installation von Azure Site Recovery Anbieter herunterladen.
5. Downloaden Sie den Registrierungsschlüssel. Diese benötigen Sie, wenn Sie Setup ausführen. Der Key gilt für fünf Tage, nachdem Sie es erstellt haben.

    ![Quelle festlegen](./media/site-recovery-vmm-to-azure/set-source3.png)

6. Installieren Sie Azure Site Recovery-Anbieter auf dem VMM-Server.


### <a name="set-up-the-azure-site-recovery-provider"></a>Einrichten von Azure Site Recovery-Anbieter

1.  Führen Sie den Anbieter Setupdatei.
2. **Microsoft** Update können Sie Updates anmelden, damit Microsoft Update Richtlinie Anbieter Updates installiert werden.
3. **Installation**akzeptieren Sie oder ändern Sie den Standardspeicherort für Anbieter und klicken Sie auf **Installieren**.

    ![Installation](./media/site-recovery-vmm-to-azure/provider2.png)

4. Nach Abschluss der Installation klicken Sie auf **Registrieren** , um das Depot VMM-Server anmelden.
5. **Depot** Seite klicken Sie auf **Durchsuchen** , um Vault-Schlüsseldatei auszuwählen. Azure Site Recovery-Abonnement und den Vault-Namen angeben.

    ![Server-Registrierung](./media/site-recovery-vmm-to-azure/provider10.PNG)

6. **Internetzugang**Geben Sie wie auf dem VMM-Server Anbieter Site Recovery über das Internet verbunden werden an.

    - Soll den Anbieter direkt wählen Sie **direkt mit Azure Site Recovery ohne Proxy verbinden**.
    - Wenn Ihre vorhandene Proxy Authentifizierung erfordert oder benutzerdefinierten Proxy Select **Azure Site Recovery verwenden einen Proxyserver**verwenden möchten.
    - Verwenden Sie einen benutzerdefinierten Proxy, geben Sie Adresse, Anschluss und Anmeldeinformationen an.
    - Wenn Sie einen Proxy verwenden sollten Sie [Komponenten](#on-premises-prerequisites)beschrieben URLs bereits zulässig.
    - Verwenden Sie einen benutzerdefinierten Proxy wird automatisch mit den angegebenen Proxy-Anmeldeinformationen VMM RunAs-Konto (DRAProxyAccount) erstellt werden. Konfigurieren Sie den Proxyserver dieses Konto erfolgreich authentifizieren kann. Konteneinstellungen VMM RunAs können in der VMM-Konsole geändert werden. Erweitern Sie **Einstellungen** **Sicherheit** > **Konten ausgeführt**, und ändern Sie das Kennwort für DRAProxyAccount. Sie müssen den VMM-Dienst neu starten, damit diese Einstellung wirksam wird.

    ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)

7. Akzeptieren Sie oder ändern Sie den Speicherort eines SSL-Zertifikats, das zum Verschlüsseln von Daten generiert. Dieses Zertifikat wird verwendet, wenn Daten-Verschlüsselung eine Wolke geschützt von Azure Azure Site Recovery-Portal aktivieren. Bewahren Sie dieses Zertifikat. Beim Ausführen eines Failovers in Azure benötigen Sie es entschlüsselt, wenn Verschlüsselung aktiviert ist.


8. Geben Sie im Feld **Servername**Anzeigename zum Identifizieren des VMM-Servers im Tresor. Geben Sie in einer Clusterkonfiguration der Rollenname VMM Cluster.
9. Aktivieren **von Synchronisierungsmetadaten Cloud** mit den Metadaten für alle Wolken auf dem VMM-Server synchronisiert. Dieser Vorgang muss nur einmal auf jedem Server ausgeführt. Möchten Sie alle Wolken synchronisieren, können diese Einstellung deaktiviert lassen und Synchronisieren jedes Cloud in die Cloud-Eigenschaften in der VMM-Konsole. Klicken Sie auf **Registrieren** , um den Vorgang abzuschließen.

    ![Server-Registrierung](./media/site-recovery-vmm-to-azure/provider16.PNG)

10. Registrierung wird gestartet. Nach Abschluss der Registrierung wird der Server auf **Einstellungen** > **Server** Blade im Tresor.


#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Installation der Befehlszeile Azure Site Recovery-Anbieter

Azure Site Recovery-Anbieter können über die Befehlszeile installiert werden. Diese Methode kann verwendet werden, den Anbieter auf Server Core für Windows Server 2012 R2 installieren.

1. Den Key-Datei und Registrierung Anbieter in einen Ordner herunterladen Beispielsweise C:\ASR.
2. Führen Sie diese Befehle Anbieter Installer Extrahieren aus ein Eingabeaufforderungsfenster mit erhöhten Rechten aus:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Führen Sie diesen Befehl, um die Komponenten zu installieren:

            C:\ASR> setupdr.exe /i

4. Führen Sie diese Befehle zum Registrieren des Servers in dem Depot:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Wo:

- **/Credentials**: Erforderlicher Parameter, der angibt, wo die Schlüssel Datei befindet.  
- **/FriendlyName**: Erforderlicher Parameter für den Namen des Hostservers Hyper-V, die in Azure Site Recovery-Portal angezeigt.
- - **/EncryptionEnabled**: beim Replizieren von Hyper-V virtuelle Computer in VMM sind Optionaler Parameter in Azure Wolken. Geben Sie ggf. virtuelle Computer in Azure (bei Rest Verschlüsselung) verschlüsselt. Stellen Sie sicher, dass der Name der Datei Erweiterung **PFX** . Verschlüsselung ist standardmäßig deaktiviert.
- **/ProxyAddress**: Optionaler Parameter, der die Adresse des Proxyservers.
- **/ProxyPort**: Optionaler Parameter, der den Port des Proxyservers.
- **/proxyUsername**: Optionaler Parameter, der den Proxy-Benutzernamen gibt (wenn Proxy eine Authentifizierung erfordert).
- **/proxyPassword**: Optionaler Parameter, der gibt das Kennwort für den Proxyserver authentifizieren (wenn der Proxyserver Authentifizierung erfordert).


### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Installieren Sie den Agent Azure Recovery Services auf Hyper-V-hosts

1. Nachdem Sie den Anbieter eingerichtet haben, müssen Sie download der Installationsdatei für den Agent Azure Recovery Services. Führen Sie Setup auf jedem Hyper-V Server in der VMM-Cloud.

    ![Hyper-V-sites](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. Klicken Sie auf der Seite **Erforderliche Komponenten prüfen** auf **Weiter**. Alle fehlenden erforderlichen Komponenten werden installiert.

    ![Erforderliche Dienste Wiederherstellungsagenten](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. Akzeptieren Sie auf der Seite **Installation Settings** oder ändern Sie den Installationsort und den Cachespeicherort. Konfigurieren des Caches auf ein Laufwerk mit mindestens 5 GB Speicherplatz, aber wir empfehlen Cachelaufwerk mit mindestens 600 GB freien Speicherplatz. Klicken Sie auf **Installieren**.
4. Nach Abschluss der Installation klicken Sie auf **Schließen** beendet.

    ![MARS-Agent registrieren](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Installation der Befehlszeile Azure Site Recovery Services Agent

Sie können Microsoft Azure Recovery Services Agent über die Befehlszeile mit dem folgenden Befehl:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Zugriff auf Internet Site Recovery von Hyper-V-Hosts einrichten

Der Agent Recovery Services auf Hyper-V-Hosts benötigt Internetzugriff zum Azure VM-Replikation. Wenn Zugriff auf das Internet über einen Proxy richten sie wie folgt:

1. Öffnen Sie Microsoft Azure Backup-MMC-Snap-in auf dem Hyper-V-Host. Standardmäßig wird eine Verknüpfung für Microsoft Azure Backup auf dem Desktop oder in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klicken Sie im Snap-in- **Eigenschaften ändern**.
3. Geben Sie auf der Registerkarte **Proxy-Konfiguration** Proxyserver-Informationen.

    ![MARS-Agent registrieren](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Sicherstellen Sie, dass der Agent die URLs in [Komponenten](#on-premises-prerequisites)beschrieben erreichen.


## <a name="step-3-set-up-the-target-environment"></a>Schritt 3: Festlegen der Ziel-Umgebung

Geben Sie das Konto Azure-Speicher für Replikation und Azure-Netzwerk mit dem Azure VMs nach einem Failover verbunden wird.

1.  Klicken Sie auf **Prepare Infrastruktur** > **Ziel** und wählen Sie den Azure-Abonnement verwenden möchten.
2.  Geben Sie das Bereitstellungsmodell für VMs nach einem Failover verwenden möchten.
3.  Site Recovery überprüft haben kompatible Azure-Speicherkonten und Netzwerke.

    ![Speicher](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4.  Wenn Sie noch kein Speicherkonto erstellt und soll einen Ressourcen-Manager, klicken Sie auf **+ Speicherkonto** , Inline sind.  Blade **Speicherkonto erstellen** Geben Sie einen Kontonamen, Typ, Abonnements und Speicherort. Das Konto sollte in demselben Speicherort wie das Depot Recovery Services.

    ![Speicher](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

    Beachten Sie Folgendes:

    - Wenn Sie ein Speicherkonto der klassischen Modell erstellen möchten, tun Sie im Azure-Portal. [Weitere Informationen](../storage/storage-create-storage-account-classic-portal.md)
    - Wenn Sie ein Speicherkonto Premium von replizierten Daten verwenden, müssen Sie ein Konto zusätzliche Standardspeicher Replikationsprotokollen speichern, die laufenden lokalen Daten erfassen einrichten.

4.  Wenn Sie noch nicht Azure-Netzwerk erstellt und soll eine Ressourcen-Manager klicken Sie auf **+ Netzwerk** dazu, Inline. **Virtuelles Netzwerk erstellen** Blade Geben Sie einen Netzwerknamen, Adressbereich Subnetdetails, Abonnements und Speicherort. Das Netzwerk sollte am gleichen Speicherort wie das Depot Recovery Services.

    ![Netzwerk](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Möchten Sie ein Netzwerk mit dem klassischen Modell tun Sie das in Azure-Portal. [Erfahren Sie mehr](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Konfigurieren von Netzwerkzuordnung

- [Lesen](#prepare-for-network-mapping) ist eine schnelle Übersicht über welche Netzwerkzuordnung. Für eine genauere Erklärung [Lesen](site-recovery-network-mapping.md) .
- Überprüfen Sie virtueller Maschinen auf dem VMM-Server mit einem VM-Netzwerk verbunden sind und mindestens ein virtuelles Netzwerk Azure erstellt haben. Mehrere VM-Netzwerke können ein Azure Netzwerk zugeordnet werden.

Konfigurieren Sie Mappings wie folgt:

1. **Einstellungen** > **Website Infrastruktur** > **Netzwerk-Zuordnung** > **Netzwerk zuordnen**, klicken Sie auf das Symbol **+ Netzwerk zuordnen** .

    ![Netzwerkzuordnung](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Der **Netzwerkübersicht hinzufügen** wählen Sie VMM Quellserver und **Azure** als Ziel.
3. Überprüfen Sie das Abonnement und das Bereitstellungsmodell nach einem Failover.
4. **Quellnetzwerk**wählen Sie das lokale VM Quellnetzwerk aus dem VMM-Server zuordnen möchten.
5. Wählen Sie **Zielnetzwerk**Azure-Netzwerk in der, das Replikatgruppe Azure VMs befinden wird bei ihrer Erstellung. Klicken Sie auf **OK**.

    ![Netzwerkzuordnung](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Hier geschieht Netzwerkübersicht beginnt:

- Vorhandene VMs VM Quellnetzwerk sind beginnt Zuordnung zum Zielnetzwerk verbunden. Neue VMs mit VM Quellnetzwerk verbunden sind bei der Replikation mit zugeordneten Azure-Netzwerk verbunden.
- Ändern eine vorhandenen Netzwerkzuordnung wird Replikat virtuelle Computer mit neuen Standardeinstellungen verbunden.
- Wenn das Zielnetzwerk mehrere Subnetze und von diesen Subnetzen hat denselben Namen wie Subnetz auf dem virtuellen Quellcomputers befindet, wird dann VM Replikat an Ziel Subnetz nach einem Failover erfolgen.
- Ist kein Ziel Subnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.



## <a name="step-4-set-up-replication-settings"></a>Schritt 4: Einrichten von Replikationseigenschaften


1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie **Infrastruktur vorbereiten** > **Replikationseigenschaften** > **+ Erstellen und zuordnen**.

    ![Netzwerk](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. Geben Sie in **Erstellen und zuordnen Richtlinie**einen Richtliniennamen ein.
3. Festlegen Sie im **Kopieren Häufigkeit**wie oft Delta Datenreplikation nach der ersten Replikation (alle 30 Sekunden 5 oder 15 Minuten).
4. Geben Sie im **Wiederherstellungspunkt Aufbewahrung**in Stunden Dauer der Beibehaltungsdauer für jede Wiederherstellungspunkt werden an. Geschützte Computer können zu einem beliebigen Punkt innerhalb eines Fensters wiederhergestellt werden.
6. **App-konsistente Snapshots Häufigkeit**angeben Häufigkeit (1 bis 12 Stunden) Recovery mit anwendungskonsistente Snapshots werden erstellt. Hyper-V verwendet zwei Arten von Snapshots – eines Standardsnapshots, die ein inkrementelles Snapshots der gesamte virtuelle Computer bereitstellt und ein anwendungskonsistente Snapshots, die einen Point-in-Time Snapshot der Anwendungsdaten innerhalb des virtuellen Computers annimmt. Anwendungskonsistente Snapshots verwenden Volume Shadow Copy Service (VSS), um sicherzustellen, dass Anwendung in einem konsistenten Zustand der Momentaufnahme. Beachten Sie, dass aktivieren anwendungskonsistente Snapshots beeinträchtigen sie die Leistung der Anwendung in Quelle virtuellen Maschinen ausgeführt. Sicherstellen Sie, dass der Wert kleiner als die Anzahl der zusätzlichen Wiederherstellungspunkte, die Sie konfigurieren.
3. **Erste Replikation Startzeit**zeigen an, wann die erste Replikation starten. Die Replikation erfolgt über die Internetbandbreite so außerhalb der Stoßzeiten planen möchten.
5. **Verschlüsseln von Daten auf Azure**Geben Sie an, ob Daten in Azure Storage Rest verschlüsselt Klicken Sie auf **OK**.

    ![Replikationsrichtlinie](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Beim Erstellen einer neuen Richtlinie hat es die VMM-Cloud automatisch zugeordnet. Klicken Sie auf **OK**. Stehen weitere VMM-Clouds (und die VMs werden) mit dieser Replikationsrichtlinie **Einstellungen** > **Replikation** > Richtlinienname > **VMM-Cloud zuordnen**.

    ![Replikationsrichtlinie](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Schritt 5: Planung

Jetzt haben Sie Ihre grundlegende Infrastruktur Sie denken über Planung und herausfinden, ob zusätzliche Ressourcen benötigen.

Site Recovery bietet Kapazitätsplaner helfen die richtigen Ressourcen für Ihre quellumgebung Websitekomponenten, Netzwerke und Speicher reservieren. Sie können im Schnellmodus Einschätzung basiert auf einer durchschnittlichen Anzahl von VMs, Datenträger und Speicher oder im detaillierten Modus, in dem Zahlen Ebene Arbeitslast Eingabe werden, Planer ausführen. Bevor Sie beginnen müssen Sie:

- Informationen Sie zur replikationsumgebung, einschließlich VMs Laufwerke pro VMs und Speicher pro Datenträger.
- Schätzen Sie die tägliche (Churn) Änderungsrate für replizierte Daten müssen. Der [Kapazitätsplaner für Hyper-V Replica](https://www.microsoft.com/download/details.aspx?id=39057) können Ihnen dabei helfen.

1.  Klicken Sie auf **herunterladen** , um das Tool herunterladen und ausführen. [Lesen Sie den Artikel](site-recovery-capacity-planner.md) , die das Tool beigefügt ist.
2.  Sie wählen Sie **Ja** , **haben Sie Capacity Planner**ausführen?

    ![Planen der Kapazität](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Vorüberlegungen zum Netzwerk-Bandbreite

Die Kapazitätsplanertools können Sie berechnen die Bandbreite für die Replikation (erste Replikation und Delta) müssen. Um die bandbreitenverwendung für die Replikation steuern haben Sie einige Optionen:

- **Bandbreite**: Hyper-V, die an einem sekundären Standort repliziert Netzwerkverkehr über einen bestimmten Hyper-V-Host. Sie können Bandbreite auf dem Hostserver einschränken.
- **Optimieren der Bandbreite**: die Bandbreite für die Replikation mit zwei Registrierungsschlüssel beeinflussen.

#### <a name="throttle-bandwidth"></a>Bandbreite

1. Microsoft Azure Backup-MMC-Snap-in auf Hyper-V-Host-Server öffnen. Standardmäßig wird eine Verknüpfung für Microsoft Azure Backup auf dem Desktop oder in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klicken Sie im Snap-in- **Eigenschaften ändern**.
3. Wählen Sie auf der Registerkarte **Beschränkung** **Internet-Bandbreite für Backups Einschränkungen aktivieren**, legen Sie die Grenzwerte für die Arbeit und arbeitsfreien Stunden. Gültige Bereiche sind 512 Kbit/s bis 102 MB/s pro Sekunde.

    ![Bandbreite](./media/site-recovery-vmm-to-azure/throttle2.png)

Das Cmdlet " [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) " können Sie Einschränkung festgelegt. Es folgt ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting-NoThrottle** zeigt an, dass keine Einschränkung.


#### <a name="influence-network-bandwidth"></a>Netzwerkbandbreite beeinflussen

Der Registrierungswert **UploadThreadsPerVM** steuert die Anzahl der Threads, die für die Datenübertragung (Replication Initial oder Delta) einer Festplatte verwendet werden. Ein höherer Wert erhöht die Netzwerkbandbreite für die Replikation verwendet. Der Registrierungswert " **DownloadThreadsPerVM** " gibt die Anzahl der Threads, die während des Failbacks für die Datenübertragung verwendet.

1. Navigieren Sie in der Registrierung **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

    - Ändern Sie den Wert **UploadThreadsPerVM** (oder erstellen Sie den Schlüssel, falls er vorhanden ist) zur Steuerung von Threads zur Laufwerksreplikation.
    - Ändern Sie den Wert **DownloadThreadsPerVM** (oder erstellen Sie den Schlüssel, falls er vorhanden ist) zur Threadsteuerung Failback Verkehr von Azure.
2. Der Standardwert ist 4. In einem Netzwerk "übermäßiger" sollte diese Registrierungsschlüssel aus geändert werden. Der Höchstwert beträgt 32. Überwachen Sie Optimierung des Werts-Datenverkehr.

## <a name="step-6-enable-replication"></a>Schritt 6: Ermöglichen Replikation

Aktivieren Sie Replikation jetzt wie folgt:

1. Klicken Sie auf **Schritt2: replizieren Anwendung** > **Quelle**. Nachdem die Replikation erstmalig aktiviert haben klicken Sie auf **+ replizieren** im Tresor Replikation für zusätzliche Computer aktivieren.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. In der **Quelle** Blade-> Wählen Sie den VMM-Server und der Cloud in der Hyper-V-Hosts befinden. Klicken Sie auf **OK**.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. **Ziel** wählen Sie das Abonnement, Bereitstellungsmodell Post-Failover und Speicherkonto für replizierte Daten verwendeten.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Wählen Sie das Speicherkonto, das Sie verwenden möchten. Möchten Sie einen anderen Speicher Konto als haben Sie [Erstellen](#set-up-an-azure-storage-account)können. Erstellen Sie ein Konto mit dem Ressourcen-Manager-Modell klicken Sie auf **neu erstellen**. Wenn Sie ein Speicherkonto der klassischen Modell erstellen möchten, führen Sie das [in Azure-Portal](../storage/storage-create-storage-account-classic-portal.md). Klicken Sie auf **OK**.
5. Wählen Sie Azure-Netzwerk und Subnetz mit dem Azure VMs verbunden wird, wenn sie nach einem Failover erstellt haben. Wählen Sie **konfigurieren jetzt für ausgewählte Computer** die Einstellung für alle Computer ausgewählten Schutz. Wählen Sie **später konfigurieren** Azure Netzwerk pro Computer auswählen. Wenn Sie ein anderes Netzwerk die verwenden möchten haben Sie [Erstellen](#set-up-an-azure-network)können. Klicken Sie zum Erstellen ein Netzwerks mit dem Ressourcen-Manager-Modell auf **neu erstellen**. Möchten Sie ein Netzwerk mit dem klassischen Modell tun Sie das [in Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wählen Sie ggf. ein Subnetz. Klicken Sie auf **OK**.
6. In **virtuellen Maschinen** > **virtuellen Computer auswählen** auf und wählen Sie jeden Computer, die Sie replizieren möchten. Sie können nur Computer auswählen, für die Replikation aktiviert werden kann. Klicken Sie auf **OK**.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. **Eigenschaften** > **Eigenschaften konfigurieren**, wählen Sie das Betriebssystem für die ausgewählten virtuellen Computer und Betriebssystem-Datenträger. Klicken Sie auf **OK**. Sie können später weitere Eigenschaften festlegen.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-azure/enable-replication6.png)


12. In **Replikationseigenschaften** > **Replikationseigenschaften konfigurieren**, wählen Sie die Replikationsrichtlinie für die geschützten virtuellen Computer anwenden möchten. Klicken Sie auf **OK**. Die Replikationsrichtlinie **Einstellungen**ändern > **Replikationsrichtlinien** > Richtlinienname > **Einstellungen bearbeiten**. Änderungen, die Sie zum Computer bereits repliziert werden und neue Maschinen.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Fortschritt des Auftrags **Schutz aktivieren** **Einstellungen**verfolgen > **Aufträge** > **Website Aufträge**. Nach **Abschließen der** Schutzauftrag die Maschine läuft kann Failover.

### <a name="view-and-manage-vm-properties"></a>Anzeigen und Verwalten von VM-Eigenschaften

Überprüfen Sie die Eigenschaften des Quellcomputers empfohlen. Beachten Sie, dass der Name Azure [Azure Virtual Machine](site-recovery-best-practices.md#azure-virtual-machine-requirements)Vorschriften entsprechen soll.

1. **Klicken Sie** > **Geschützte Objekte** > **Repliziert Objekte** > und wählen Sie den Computer um seine Details anzuzeigen.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. **Eigenschaften** können Sie Informationen für die VM-Replikation und Failover anzeigen.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. **Compute**und > **Eigenschaften berechnen** die Azure-VM-Name und Ziel Größe angeben. Ändern Sie den Namen [Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen Sie benötigen. Sie können auch anzeigen und Informationen zum Zielnetzwerk, Subnetzmaske und der Azure-VM zugewiesene IP-Adresse ändern. Beachten Sie Folgendes:

    - Sie können die Ziel-IP-Adresse festlegen. Wenn Sie eine Adresse, die fehlerhafte Maschine angeben werden DHCP verwenden. Wenn Sie eine Adresse, die nicht beim Failover verfügbar ist festlegen, wird das Failover fehl. Die gleiche IP-Zieladresse kann für testfailover verwendet werden, ist die Adresse im Testnetzwerk Failover.
    - Die Anzahl der Netzwerkadapter diktiert die Größe für der virtuelle Zielcomputer wie folgt angeben:

        - Wenn die Anzahl der Netzwerkadapter auf dem Quellcomputer kleiner oder gleich der Anzahl der Netzwerkadapter für die Zielgröße Computer zulässig ist, haben das Ziel die gleiche Anzahl von Adaptern als Quelle.
        - Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputers für die Größe und die Zielgröße maximal zulässige Anzahl überschreitet wird verwendet.
        - Wenn beispielsweise ein Quellcomputer verfügt über zwei Netzwerkadapter die Zielgröße Computer unterstützt vier und dem Zielcomputer müssen zwei Adapter. Wenn der Quellcomputer hat zwei Adapter unterstützten Zielgröße unterstützt jedoch nur eine haben dem Zielcomputer nur eine Netzwerkkarte.     
        - Die VM hat mehrere Netzwerkadapter werden alle im gleichen Netzwerk herstellen.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  **Datenträger** des Betriebssystems und der Datenträger auf dem virtuellen Computer angezeigt, die repliziert werden.



## <a name="step-7-test-your-deployment"></a>Schritt 7: Testen der Bereitstellung

Zum Testen der Bereitstellung führen Sie ein testfailover einer einzigen virtuellen Maschine oder einen Wiederherstellungsplan, der einen oder mehrere virtuelle Computer enthält.


### <a name="prepare-for-failover"></a>Bereiten für Failover vor

- Führen Sie ein testfailover wird empfohlen, ein neues Azure-Netzwerk, das isoliert wurde von Azure Produktionsnetzwerk erstellen (Dies ist standardmäßig beim Erstellen eines neuen Netzwerks in Azure). [Weitere Informationen](site-recovery-failover.md#run-a-test-failover) zu Test-Failover ausgeführt.
- Um die beste Leistung bei einem zu Azure Failover, installieren Sie den Azure-Agent auf dem geschützten Computer. Es schneller starten und bei der Problembehandlung. Installieren Sie den Agent [Linux](https://github.com/Azure/WALinuxAgent) oder [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) .
- Um Ihre Bereitstellung vollständig testen benötigen Sie eine Infrastruktur für replizierte Computer wie erwartet. Wenn Sie Active Directory und DNS testen möchten können erstellen einen virtuellen Computer als Domänencontroller mit DNS und dies mit Azure Site Recovery Azure replizieren. Lesen Sie mehr in [Test-Failover Aspekte für Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Wenn ein ungeplantes Failovers statt ein testfailover ausführen möchten Beachten Sie Folgendes:

    - Wenn möglich sollten Sie primären Computer herunterfahren, vor dem Ausführen eines ungeplanten Failovers. So wird sichergestellt, dass Quelle und Replikat Computern gleichzeitig haben.
    - Beim Ausführen eines ungeplanten Failovers beendet so alle Delta Daten nicht übertragen werden, nachdem ein ungeplantes Failovers beginnt Datenreplikation von primären Computer. Darüber hinaus beim Ausführen eines ungeplanten Failovers auf einen Wiederherstellungsplan wird bis zum Abschluss, ausgeführt, wenn ein Fehler auftritt.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Verbindung zu Azure VMs nach einem Failover vorbereiten

Wenn Sie Azure VMs mit RDP nach einem Failover möchten, unbedingt Folgendes:

**Auf dem lokalen Computer vor dem Failover**:

- Für den Zugriff über das Internet aktivieren RDP, TCP- und UDP-Regeln für die- **Öffentliche**hinzugefügt werden, und gewährleistet die RDP in der **Windows-Firewall**kann -> **Zugelassene apps und Funktionen** für alle Profile.
- Für den Zugriff über eine Standort-zu-Standort-Verbindung aktivieren RDP auf dem Computer und sicher, dass RDP in der **Windows-Firewall** -> **Zugelassene apps und Features** für **Domäne** und **privaten** Netzwerken.
- Installieren Sie der [Azure-VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) auf dem lokalen Computer.
- Sicherstellen Sie, dass das Betriebssystem SAN-Richtlinie auf OnlineAll festgelegt ist. [Weitere Informationen]( https://support.microsoft.com/kb/3031135)
- Deaktivieren des IPSec-Dienstes, bevor das Failover ausgeführt.

**Auf der Azure-VM nach dem Failover**:

- Fügen Sie einen öffentlichen Endpunkt für das RDP-Protokoll (Port 3389) und geben Sie die Anmeldeinformationen für die Anmeldung.
- Sicherstellen Sie, dass Sie Domänenrichtlinien nicht, die Verbindung mit einem virtuellen Computer eine öffentliche Adresse verhindern.
- Versuchen Sie, eine Verbindung herzustellen. Wenn Sie verbinden können, überprüfen Sie VM ausgeführt wird. Weitere Tipps zur Problembehandlung finden Sie in diesem [Artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Wenn Sie eine Azure-VM unter Linux nach einem Failover mit einem Secure Shell-Client (ssh) zugreifen möchten, folgendermaßen Sie vor:

**Auf dem lokalen Computer vor dem Failover**:

- Sicherzustellen Sie, dass Secure Shell-Dienst auf der Azure-VM automatisch beim Systemstart gestartet.
- Überprüfen Sie, Firewall-Regeln eine SSH-Verbindung zu ermöglichen.

**Auf der Azure-VM nach dem Failover**:

- Netzwerksicherheitsregeln-Gruppe auf fehlgeschlagene VM und Azure Subnetz, besteht, müssen eingehende Verbindungen SSH-Port.
- Ein öffentlicher Endpunkt sollte erstellt werden, um eingehende Verbindungen SSH-Port (standardmäßig TCP-Port 22).
- Wenn die VM über eine VPN-Verbindung (Express Route oder Standort-zu-Standort-VPN) zugegriffen wird, kann Verbindung direkt für die VM über SSH Client verwendet.


### <a name="run-a-test-failover"></a>Führen Sie ein testfailover

Test-Failover führen Folgendes ausführen:

1. Failover einer einzigen VM **Einstellungen** > **Elemente repliziert**, klicken Sie auf die VM > **+ Testfailover**.
2. Einen Wiederherstellungsplan **Einstellungen**Failover > **Recovery-Pläne**, mit der rechten Maustaste des Plans > **Test-Failover**. Erstellen Sie eine plan für die Wiederherstellung [Führen Sie diese Schritte](site-recovery-create-recovery-plans.md).

3. **Test** -Failover der Azure-VMs Verbindung nach einem Failover Azure-Netzwerk auswählen
4. Klicken Sie auf **OK** , um das Failover zu beginnen. Fortschritt kann durch Klicken auf die Eigenschaften des virtuellen Computers oder für den Einzelvorgang **Test-Failover** **Settings** > **Website Aufträge**.
5. Erreicht das Failover **abgeschlossen** -Testphase, führen Sie folgende Schritte aus:

    1. Replikat VM in Azure-Portal anzeigen Stellen Sie sicher, dass die virtuellen Computer erfolgreich gestartet.
    2. Wenn Sie aus dem lokalen Netzwerk Zugriff auf virtuelle Computer eingerichtet sind, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer initiieren.
    3. Klicken Sie auf **vollständig den Test** fertig zu stellen.
    4. Klicken Sie auf **Notizen** zu speichern Anmerkungen testfailover zugeordnet.
    5. Klicken Sie auf **testfailover ist abgeschlossen**. Bereinigen der Umgebung automatisch ausschalten und den Test virtuellen Computer löschen.
    6. In dieser Phase werden Elemente oder VMs erstellt automatisch Site Recovery bei einem testfailover gelöscht. Zusätzliche Test-Failover erstellten Elemente werden nicht gelöscht.

    > [AZURE.NOTE] Wenn ein testfailover mehr weiterhin als zwei Wochen mit abgeschlossen ist.

6. Nach Abschluss des Failovers sollten auch möglicherweise das Replikat Azure finden Computer in Azure-Portal angezeigt > **virtuelle Computer**. Sie sollten sicherstellen, dass die VM die entsprechende Größe aufweist, die mit dem entsprechenden Netzwerk verbunden ist und ausgeführt wird.
7. Sollten Sie [für Verbindungen nach einem Failover](#prepare-to-connect-to-Azure-VMs-after-failover) Sie Azure VM herstellen.


## <a name="monitor-your-deployment"></a>Überwachen der Bereitstellung

Hier ist, wie Sie die Konfiguration, Status und Zustand für Ihre Site Recovery-Bereitstellung überwachen können:

1. Klicken Sie auf das Depot auf dem **Essentials** -Dashboard. In diesem Dashboard können Sie Site Recovery Aufträge, Replikationsstatus Recovery-Pläne, Serverzustand und Ereignisse.  Sie können Grundlagen der Kacheln und Layouts, die Sie z. B. den Status der anderen Site Recovery und Backup-Depots besonders hilfreich sind.

    ![Grundlagen](./media/site-recovery-vmm-to-azure/essentials.png)

2. Der **Gesundheit** Kachel können Sie Site Server (VMM oder Konfiguration) überwachen, die Problem und die in den letzten 24 Stunden von Site Recovery ausgelösten Ereignisse auftreten.
3. Verwalten und Überwachen der Replikation der **Objekte repliziert**, **Recovery-Pläne**, und **Wiederherstellungsaufträge Website** angeordnet. Aufträge in **Einstellungen**Drillinto -> **Aufträge** -> **Website Aufträge**.


## <a name="next-steps"></a>Nächste Schritte

Nach der Bereitstellung eingerichtet ist und ausgeführt wird, zu verschiedenen Failover [mehr](site-recovery-failover.md) .
