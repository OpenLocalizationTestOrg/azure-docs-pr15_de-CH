<properties
    pageTitle="Replizieren von Hyper-V virtuelle Computer in VMM-Clouds in eine Azure-Portal mit VMM-Sekundärstandort | Microsoft Azure"
    description="Beschreibt das Bereitstellen von Azure Site Recovery, Replikation, Failover und Recovery von Hyper-V virtuelle Computer in VMM-Clouds zu einem Azure-Portal mit VMM-Sekundärstandort koordinieren."
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
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Replizieren Sie Hyper-V virtuelle Computer in VMM-Clouds auf einen sekundären Azure-Portal mit VMM-Standort

> [AZURE.SELECTOR]
- [Azure-portal](site-recovery-vmm-to-vmm.md)
- [Verwaltungsportal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - Ressourcen-Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Willkommen bei Azure Site Recovery! Verwenden Sie in diesem Artikel möchten Sie lokale Hyper-V virtuelle Computer replizieren in System Center Virtual Machine Manager (VMM) Wolken an einem sekundären Standort verwaltet. Dieser Artikel beschreibt das Einrichten der Replikation mithilfe von Azure Site Recovery in Azure-Portal.

> [AZURE.NOTE] Azure hat zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager und Classic. Azure verfügt außerdem über zwei Portale – klassischen Azure-Portal, das klassische Bereitstellungsmodell unterstützt und Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle.


Azure Site Recovery im Azure-Portal bietet einige neue Features:

- In der Azure sind Portal, Azure Backup und Azure Site Recovery Services in einem Tresor Recovery Services zusammengefasst, so dass einrichten und Verwalten von Business Continuity und Disaster Recovery (BCDR) von einem einzigen Ort. Ein einheitliches Dashboard ermöglicht das Überwachen und Verwalten von Operationen über Ihren lokalen Sites und Azure öffentlichen Cloud.
- Benutzer mit Azure Cloud Solution Provider (CSP) Programm bereitgestellt können jetzt Site Recovery-Vorgänge im Azure-Portal verwalten.


Dieser Artikel alle Kommentare unten in die Kommentare Disqus. Fragen Sie technische [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Übersicht

Organisationen benötigen eine BCDR Strategie, die bestimmt, wie apps Arbeitslasten und Daten bleiben aktiv bei geplanten und ungeplanten Ausfällen normalen Arbeitsbedingungen so bald wie möglich wiederherstellen. Die BCDR Strategie sollten schützen Sie Daten wiederhergestellt und Arbeitslasten ständig verfügbar bleiben beim Notfall.

Site Recovery ist eine Azure Service BCDR Strategie umzusetzen Replikation von lokalen Servern und virtuellen Computer in die Cloud (Azure) oder zu einem sekundären Datencenter beiträgt. Treten Ausfälle in Ihrem primären Standort Failover Sie sekundäre an apps und Arbeitslasten zur Verfügung. Sie nicht an Ihrem primären Standort zu beenden. Weitere Informationen [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md)

Dieser Artikel enthält alle Informationen, die Sie replizieren möchten Hyper-V virtuelle Computer in VMM-Clouds an einem sekundären Standort VMM lokal. Es enthält eine Architekturübersicht Informationen und Bereitstellungsschritte zum Konfigurieren von lokalen Servern Replikationseigenschaften und Planung planen. Nach dem Einrichten der Infrastruktur können Sie Replikation auf Computern zu schützen, und überprüfen Sie, dass ein Failover funktioniert.

## <a name="business-advantages"></a>Geschäftliche Vorteile

- Site Recovery bietet Schutz für Business-Arbeitslasten und Programme auf Hyper-V virtuelle Computer von einem sekundären Hyper-V Server replizieren.
- Das Recovery-Portal stellt eine zentrale Stelle zum Einrichten, verwalten und Überwachen von Replikation, Failover und Recovery.
- Sie können problemlos zur Failover Ihrer lokalen primären Infrastruktur sekundären Standort und Failback (Wiederherstellen) vom sekundären zum primären ausführen.
- Sie können Recovery-Pläne mit mehreren Computern konfigurieren, sodass tiered Arbeitslasten zusammen Failover.

## <a name="scenario-architecture"></a>Szenario-Architektur

Szenariokomponenten sind:

- **Primären Standort**: Primärer Standort vorhanden sind oder mehr Hyper-V-Hostservern Quelle VMs, die Sie replizieren möchten. Diese primären Hostserver befinden sich in einer privaten Cloud VMM.
- **Sekundärstandort**: am sekundären Standort sind ein oder mehrere Hyper-V-Hostservern Ziel VMs, die primäre VMs replizieren. Diese Hostserver befinden sich in einer privaten Cloud VMM. Die Cloud ist auf dem primären Server (Wenn Sie nur einen einzelnen VMM-Server) oder auf einem sekundären VMM-Server.
- **Anbieter**: Installation Site Recovery Azure Site Recovery-Anbieter auf dem VMM-Server installieren, und die Server in einem Recovery-Services registrieren. Der Anbieter auf dem VMM-Server kommuniziert mit Site Recovery über HTTPS 443 Orchestrierung replizieren. Daten-Replikation zwischen primären und sekundären Server von Hyper-V-Host. Replizierte Daten werden in lokalen Standorten und Netzwerken bleibt und nicht in Azure. Weitere Informationen zum [Datenschutz](#privacy-information-for-site-recovery).

![E2E-Topologie](./media/site-recovery-vmm-to-vmm/architecture.png)

### <a name="data-privacy-overview"></a>Übersicht über Daten-Datenschutz

Die folgende Aufstellung wie der Daten in diesem Szenario:
****
Aktion | **Details** | **Daten** | **Verwendung** | **Erforderlich**
--- | --- | --- | --- | ---
**Registrierung** | Sie registrieren einen VMM-Server in einem Recovery-Services. Wenn Sie später einen Server abmelden möchten, möchten, löschen die Informationen von Azure-Portal. | Nach dem VMM-Server registriert ist Site Recovery sammelt, Prozesse und überträgt die Metadaten der VMM-Server und die Namen der VMM-Clouds von Site Recovery erkannt. | Daten zum Identifizieren und mit den entsprechenden VMM-Server kommunizieren und Einstellungen für entsprechende VMM-Clouds. | Diese Funktion ist erforderlich. Möchten Sie diese Site Recovery übermitteln sollte nicht den Site Recovery-Dienst verwenden.
**Aktivieren der Replikation** | Azure Site Recovery-Anbieter auf dem VMM-Server installiert und ist für die Kommunikation mit der Site Recovery-Dienst. Der Anbieter ist eine Dynamic Link Library (DLL) in VMM-Prozess gehostet. Nach der Installation des Anbieters wird das Feature "Datacenter Recovery" in der VMM-Verwaltungskonsole aktiviert. Neue und vorhandene VMs können dieses Feature aktivieren Sie den Schutz für einen VM. | Mit dieser Eigenschaft sendet der Anbieter den Namen und die VM Site Recovery.  Windows Server 2012 oder Windows Server 2012 R2 Hyper-V Replica ist Replikation aktiviert. Die Daten der virtuellen Maschine repliziert von Hyper-V-Hosts (in der Regel befindet sich in einem anderen "Recovery"). | Site Recovery verwendet Metadaten zum Auffüllen der VM-Informationen im Azure-Portal. | Dieses Feature ist Bestandteil des Dienstes und kann nicht deaktiviert werden. Wenn Sie diese Informationen senden möchten, aktivieren Sie Site Recovery-Schutz für VMs nicht. Beachten Sie, dass alle Daten vom Anbieter Site Recovery über HTTPS gesendet wird.
**Recovery-Plans** | Recovery-Pläne helfen erstellen einen Orchestrierung Plan für das Recovery-Rechenzentrum. Sie können die Reihenfolge definieren, in der virtuelle Computer oder eine Gruppe von virtuellen Computern am Recovery-Standort gestartet werden soll. Sie können auch automatisierte Skripts ausführen oder manuell auszuführende Aktion, bei der Wiederherstellung für jede VM zu. Failover wird in der Regel auf Recovery planebene koordinierte Recovery ausgelöst. | Site Recovery erfasst, verarbeitet und überträgt Metadaten für den Wiederherstellungsplan virtuellen Metadaten und Metadaten von Scripts zur Automatisierung und Notizen manuell. | Metadaten wird verwendet, um den Wiederherstellungsplan in Azure-Portal erstellen. | Dieses Feature ist Bestandteil des Dienstes und kann nicht deaktiviert werden. Möchten Sie diese Site Recovery übermitteln, erstellen Sie keine Recovery-Pläne.
**Netzwerkzuordnung** | Netzwerkinformationen Karten im primären Rechenzentrum Recovery-Rechenzentrum Wenn VMs auf Recovery-Standort wiederhergestellt werden, kann Netzwerk-Mapping Verbindung zum Netzwerk herstellen. | Site Recovery erfasst, verarbeitet und überträgt die Metadaten der logische Netzwerke für jede Site (Primär- und Datacenter). | Metadaten zum Network Settings füllen verwendet, damit Sie die Netzwerkinformationen zuordnen können. | Dieses Feature ist Bestandteil des Dienstes und kann nicht deaktiviert werden. Möchten Sie diese Site Recovery übermitteln, verwenden Sie nicht Netzwerkzuordnung.
**Failover (geplante/ungeplante/Test)** | Failover Failover VMs von VMM verwaltet werden wollen. Das Failover manuell im Azure-Portal Ereignisaktion. | Der Anbieter auf dem VMM-Server Failover-Ereignis von Site Recovery benachrichtigt und führt eine Failover Aktion auf Hyper-V-Hosts über VMM Schnittstellen. Tatsächliche Failover eines virtuellen Computers ist von einem Hyper-V-Host und von Windows Server 2012 oder Windows Server 2012 R2 Hyper-V Replica. Nach dem Failover abgeschlossen ist, sendet des Anbieters auf dem VMM-Server im Rechenzentrum Recovery Erfolg Informationen Site Recovery. | Site Recovery verwendet die Informationen gesendet, um den Status der Informationen im Azure-Portal Failover zu füllen. | Dieses Feature ist Bestandteil des Dienstes und kann nicht deaktiviert werden. Wenn Sie diese Site Recovery übermitteln möchten, verwenden Sie Failover nicht.


## <a name="azure-prerequisites"></a>Azure-Komponenten

Hier ist Sie in Azure, benötigen Sie dieses Szenario bereitstellen:

**Erforderliche Komponenten** | **Details**
--- | ---
**Azure**| Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.


## <a name="on-premises-prerequisites"></a>Lokalen Komponenten

Hier ist die primären und sekundären lokale Standorte um benötigen Sie dieses Szenario bereitstellen:

**Erforderliche Komponenten** | **Details**
--- | ---
**VMM** | Wir empfehlen Sie VMM-Server am primären Standort und VMM-Server am sekundären Standort bereitstellen.<br/><br/> Sie können auch [zwischen auf einem einzelnen VMM-Server replizieren](site-recovery-single-vmm.md). Dazu benötigen Sie mindestens zwei Wolken auf dem VMM-Server konfiguriert.<br/><br/> VMM-Server sollte mindestens ausführen System Center 2012 SP1 mit den neuesten Updates.<br/><br/> Jede VMM-Server müssen in einer oder mehr Wolken konfiguriert und alle Wolken müssen des Kapazität von Hyper-V-Profils. <br/><br/>Wolken enthalten eine oder mehrere VMM-Hostgruppen.<br/><br/>Weitere Informationen zum Einrichten von VMM-Clouds in [VMM-Cloud-Struktur konfigurieren](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)und [Exemplarische Vorgehensweise: Erstellen von privaten Clouds mit System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM-Server benötigen Zugriff auf das Internet.
**Hyper-V** | Hyper-V Server laufen mindestens Windows Server 2012 mit Hyper-V-Rolle und die neuesten Updates installiert haben.<br/><br/> Hyper-V Server sollte eine oder mehrere VMs.<br/><br/>  Hyper-V-Host-Server sollte in Gruppen in der primären und sekundären VMM-Clouds befinden.<br/><br/> Wenn Sie Windows Server 2012 R2 Hyper-V in einem Cluster ausführen sollten Sie [update 2961977](https://support.microsoft.com/kb/2961977) installieren<br/><br/> Wenn Sie Hyper-V in Windows Server 2012 in einem Cluster ausführen, beachten Sie, dass dieser Cluster Broker haben einen statischen IP-Adresse-basierten Cluster automatisch erstellt wird. Sie müssen Cluster Broker manuell konfigurieren. [Mehr](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Anbieter** | Während der Bereitstellung der Site Recovery installiert Azure Site Recovery Provider auf VMM-Server. Der Anbieter kommuniziert mit Site Recovery über HTTPS 443 Replikation orchestrieren. Daten-Replikation zwischen den primären und sekundären Hyper-V-Servern über das LAN oder VPN-Verbindung.<br/><br/> Der Anbieter auf dem VMM-Server benötigt Zugriff auf diese URLs: *. hypervrecoverymanager.windowsazure.com, *. AccessControl.Windows.NET; *. backup.windowsazure.com, *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Außerdem lässt Firewall Kommunikation zwischen VMM-Server und [Azure-Rechenzentrum IP-Adressbereiche](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das Protokoll HTTPS (443).

## <a name="prepare-for-deployment"></a>Bereiten auf die Bereitstellung vor

Für die Bereitstellung vorbereiten möchten:

1. [Vorbereiten der VMM-Server](#prepare-the-vmm-server) Site Recovery-Bereitstellung.
2. [Vorbereitung der Netzwerkübersicht](#prepare-for-network-mapping). Netzwerke so richten Sie ein, dass Netzwerkzuordnung zu konfigurieren.

### <a name="prepare-the-vmm-server"></a>Vorbereiten des VMM-Servers

Stellen Sie sicher, dass VMM-Server [erforderliche Komponenten](#on-premises-prerequisites) und aufgeführten URLs zugreifen kann.


### <a name="prepare-for-network-mapping"></a>Vorbereitung der Netzwerkübersicht

Netzwerk-Zuordnung Karten zwischen VMM VM auf die primären und sekundären VMM Server:

- Platzieren Sie optimal Replikat VMs auf sekundäre Hyper-V-Hosts nach einem Failover.
- Replikat VMs entsprechende VM-Netzwerken verbinden.
- Wenn VMs nach einem Failover mit einem Netzwerk verbunden wird nicht Netzwerk Zuordnung Replikat nicht konfigurieren.
- Möchten Sie Netzwerk ist während der Wiederherstellung Sites zuordnen Bereitstellung hier benötigen Sie:

    - Stellen Sie sicher, dass VMs auf der Hyper-V-Host mit einem VMM VM-Netzwerk verbunden sind. Das Netzwerk sollte ein logisches Netzwerk verbunden, das der Cloud zugeordnet ist.
    - Überprüfen Sie, ob die sekundäre Cloud für die Wiederherstellung verwendet ein entsprechende VM-Netzwerk konfiguriert wurde. Das VM-Netzwerk sollte ein logisches Netzwerk verbunden, die sekundäre Cloud zugeordnet ist.

- [Erfahren Sie mehr](site-recovery-network-mapping.md) über die Funktionsweise von Netzwerkzuordnung.

## <a name="prepare-for-deployment-with-a-single-vmm-server"></a>Bereiten Sie auf die Bereitstellung mit einem einzelnen VMM-Server vor

Wenn Sie nur einen einzelnen VMM-Server können Sie virtuelle Computer in Hyper-V-Hosts in VMM-Cloud [Azure](site-recovery-vmm-to-azure.md) oder sekundären VMM-Cloud replizieren. Wir empfehlen die erste Option da Replikation zwischen Wolken nahtlose ist jedoch ggf. hier müssen Sie:

1. **Einrichten von VMM auf einem Hyper-V-VM**. Dazu empfehlen wir Installieren von VMM auf dem gleichen virtuellen Computer verwendete SQL Server-Instanz. Dies spart Zeit, wie nur ein virtueller Computer erstellt werden. Wenn Sie möchten Remoteinstanz von SQL Server und einem Ausfall auftritt, müssen Sie dieser Instanz wiederherstellen, bevor Sie VMM wiederherstellen können.
2. **Sicherstellen, dass der VMM-Server hat mindestens zwei Wolken konfiguriert**. Eine Wolke enthalten die VMs replizieren möchten und die Cloud dient als sekundären Standort. Die Cloud, die VMs enthält zu schützenden sollten [Komponenten](#on-premises-prerequisites)entsprechen.
3. Einrichten von Site Recovery wie in diesem Artikel beschrieben. Erstellen das Depot VMM-Server anmelden, eine Replikation Richtlinie und Replikation aktivieren. Geben Sie die anfängliche Replikation über das Netzwerk erfolgt.
4. Beim Einrichten von Netzwerkzuordnung das VM-Netzwerk für die primäre Cloud VM-Netzwerk für die sekundäre Cloud zugeordnet.
5. Aktivieren Sie in Hyper-V-Manager-Konsole Hyper-V Replica auf Hyper-V-Host, der die VMM-VM enthält und Replikation des virtuellen Computers. Stellen Sie sicher, dass Sie VMM Virtual Machine Wolken nicht hinzufügen, die von Site Recovery sicherstellen, dass Hyper-V Replica Einstellungen Site Wiederherstellung überschrieben werden nicht geschützt werden.
6. Erstellen von Recovery-Plänen für Failover verwenden Sie dieselbe VMM-Server für Quelle und Ziel.
7. Ein Failover zu wie folgt wiederherstellen:

    - Manuelles Failover VMM VM an den sekundären Standort mit Hyper-V-Manager mit einem geplanten Failover.
    - Ein Failover der VMs.
    - Nach dem primären VMM VM wiederhergestellt wurde, melden Sie sich bei Azure-Portal -> Recovery Services Depot und ungeplanten Failover der VMs vom sekundären zum primären Standort ausführen.
    - Nach Abschluss das ungeplante Failover aller Ressourcen vom primären Standort wieder möglich.


### <a name="create-a-recovery-services-vault"></a>Erstellen eines Depots Recovery Services

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2. **Klicken Sie auf** > **Management** > **Recovery Services**. Sie können auf **Durchsuchen klicken** > **Recovery Services** Depots > **Hinzufügen**.

    ![Neues Depot](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. **Name** Geben Sie einen Anzeigenamen zu Tresor an. Haben Sie mehrere Abonnements wählen Sie davon.
4. [Erstellen einer neuen Ressource](../resource-group-template-deploy-portal.md) oder wählen Sie eine vorhandene Azure Region angeben. Computer werden in diesem Bereich repliziert. Überprüfen Sie unterstützte Regionen finden Sie unter geografische Verfügbarkeit in [Azure Site Recovery Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Wenn Sie das Depot aus dem Dashboard zugreifen möchten, klicken Sie auf **Pin Dashboard** > **Depot erstellen**.

    ![Neues Depot](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Neue Depot erscheinen im **Dashboard** > **alle Ressourcen**und auf die **Recovery Services Depots** Blade.




## <a name="getting-started"></a>Erste Schritte

Site Recovery bietet einen Einstieg Erfahrung, mit der Sie so schnell wie möglich bereitstellen. Erste Schritte überprüft Komponenten und Site Recovery-Bereitstellung in der richtigen Reihenfolge Schritte durchläuft.

Erste Schritte Sie wählen den Computer repliziert werden soll und auf repliziert werden soll. Lokalen Server eingerichtet, Replikations-Policies erstellen und Planung durchführen. Nach dem Einrichten der Infrastruktur können Sie Replikation für virtuelle Computer. Sie können ausführen Failover für bestimmte Computer oder Recovery-Pläne nicht über mehrere Computer erstellen.

Zunächst Einsteiger auswählen wie Sie Site Recovery bereitstellen möchten. Der Einstieg Fluss ändert sich je nach Bedarf Replikation etwas.

## <a name="step-1-choose-your-protection-goal"></a>Schritt 1: Wählen Sie Ihr Schutzziel

Wählen Sie replizieren möchten und wo Sie zu replizieren.

1. Blade **-Rückgewinnungsservice Depots** Vault und klicken Sie auf **Settings**.
2. **Einstellungen** > **Erste Schritte** klicken Sie auf **Site Recovery** > **Schritt 1: Vorbereiten Infrastruktur** > **Schutzziel**.
3. **Schutzziel** **Recovery-Standort**wählen Sie aus, und wählen Sie **Ja, mit Hyper-V**.
4. Wählen Sie **Ja,** um anzugeben, dass die Verwendung von VMM die Hyper-V-Hosts verwalten und wählen **Ja** Wenn Sie einen sekundären VMM-Server. Wenn Sie Replikation zwischen auf einem einzelnen VMM-Server bereitstellen klicken Sie auf **Nein**. Klicken Sie auf **OK**.

    ![Ziele auswählen](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## <a name="step-2-set-up-the-source-environment"></a>Schritt 2: Einrichten der Quell-Umgebung

Installieren Sie das Azure Site Recovery in VMM-Server und registrieren Sie Server im Tresor.

1. Klicken Sie auf **Schritt 2: Vorbereiten der Infrastruktur** > **Quelle**.

    ![Quelle festlegen](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. **Vorbereiten der Datenquelle** klicken Sie auf **+ VMM** VMM-Server hinzufügen.

    ![Quelle festlegen](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. Im Scheck Blade- **Server hinzufügen** , den **Typ** und **System Center VMM-Server** angezeigt wird entspricht der VMM-Server [erforderliche und URL-Anforderungen](#on-premises-prerequisites).
4. Installation von Azure Site Recovery Anbieter herunterladen.
5. Downloaden Sie den Registrierungsschlüssel. Diese benötigen Sie, wenn Sie Setup ausführen. Der Schlüssel gilt für 5 Tage, nachdem Sie es erstellt haben.

    ![Quelle festlegen](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. Installieren Sie Azure Site Recovery-Anbieter auf dem VMM-Server.

> [AZURE.NOTE] Sie müssen nicht explizit etwas auf Hyper-V-Host-Server installieren.


### <a name="set-up-the-azure-site-recovery-provider"></a>Einrichten von Azure Site Recovery-Anbieter

1. Der Anbieter Setupdatei auf jede VMM-Server ausführen. Wenn VMM in einem Cluster bereitgestellt wird, und installieren Sie den Anbieter für erstmals auf einem aktiven Knoten installieren und schließen Sie die Installation im Tresor VMM-Server registrieren. Installieren Sie den Provider auf den anderen Knoten. Clusterknoten sollten alle die gleiche Version des Anbieters ausführen.
2. Setup führt einige prerequirement Überprüfung und Zustimmung zu dem VMM-Dienst. VMM-Dienst wird automatisch gestartet, wenn Setup abgeschlossen ist. Bei Installation auf einem Cluster VMM werden Sie aufgefordert, die Clusterrolle beenden.

2.  **Microsoft** Update können Sie Updates anmelden, damit Microsoft Update Richtlinie Anbieter Updates installiert werden.
3. **Installation** akzeptieren Sie oder ändern Sie den Standardspeicherort für Anbieter und klicken Sie auf **Installieren**.

    ![Installation](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Nach Abschluss der Installation klicken Sie auf **Registrieren** , Registrieren des Servers im Tresor.

    ![Installation](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. **Depotnamen**überprüfen Sie, ob das Depot in dem der Server registriert werden. Klicken Sie auf *Weiter*.

    ![Server-Registrierung](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. Geben Sie **Internetzugang** wie der Anbieter auf dem VMM-Server mit dem Internet verbunden. Wählen Sie **Verbindung mit vorhandenen Proxyeinstellungen** , die Internet-Verbindung auf dem Server konfigurierten Standardeinstellungen.

    ![Internet-Einstellungen](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Möchten Sie einen benutzerdefinierten Proxyserver sollten Sie es vor dem Installieren des Anbieters festlegen. Wenn Sie benutzerdefinierte Proxyeinstellungen konfigurieren wird ein Test ausgeführt, um die Proxyverbindung überprüfen.
    - Sie verwenden einen benutzerdefinierten Proxy, ob der Standardproxy erfordert Authentifizierung Sie die Proxy-Details, einschließlich Proxy-Adresse und Port eingeben müssen.
    - Folgenden Urls sollte auf dem VMM-Server und Hyper-V-hosts
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Lassen Sie die IP-Adressen in [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/confirmation.aspx?id=41653) und HTTPS (443) Protokoll beschriebenen zu Weiße Liste IP-Adressbereiche der Azure-Region, die Sie verwenden möchten und Westen der USA müssten.
    - Verwenden Sie einen benutzerdefinierten Proxy wird automatisch mit den angegebenen Proxy-Anmeldeinformationen VMM RunAs-Konto (DRAProxyAccount) erstellt werden. Konfigurieren Sie den Proxyserver dieses Konto erfolgreich authentifizieren kann. Konteneinstellungen VMM RunAs können in der VMM-Konsole geändert werden. Hierzu öffnen Sie den Arbeitsbereich **Einstellungen** , **Sicherheit**klicken Sie auf **Run As Accounts**und ändern Sie das Kennwort für DRAProxyAccount. Sie müssen den VMM-Dienst neu starten, damit diese Einstellung wirksam wird.


8. Wählen Sie im **Registrierungsschlüssel**des Schlüssels, den von Azure Site Recovery heruntergeladen und auf dem VMM-Server kopiert.


10.  Verschlüsselungseinstellung wird nur verwendet, wenn Sie Hyper-V virtuelle Computer in VMM-Clouds in Azure Replikation. Wenn Sie an einem sekundären Standort replizieren sind wird es nicht verwendet.

11.  Geben Sie im Feld **Servername**Anzeigename zum Identifizieren des VMM-Servers im Tresor. Geben Sie in einer Clusterkonfiguration der Rollenname VMM Cluster.
12.  **Cloud-Metadaten synchronisieren** wählen möchten, ob mit den Metadaten für alle Wolken auf dem VMM-Server synchronisiert. Dieser Vorgang muss nur einmal auf jedem Server ausgeführt. Möchten Sie alle Wolken synchronisieren, können diese Einstellung deaktiviert lassen und Synchronisieren jedes Cloud in die Cloud-Eigenschaften in der VMM-Konsole.

13.  Klicken Sie auf **Weiter** um den Vorgang abzuschließen. Nach der Registrierung wird von Azure Site Recovery Metadaten aus dem VMM-Server abgerufen. Der Server wird auf der Registerkarte **VMM-Server** auf der Seite **Server** im Depot angezeigt.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Nachdem der Server in der Site Wiederherstellungskonsole **Quelle**verfügbar ist > **Vorbereiten Quelle** wählen Sie den VMM-Server und die Cloud in der Hyper-V-Hosts ist. Klicken Sie auf **OK**.

#### <a name="command-line-installation"></a>Befehlszeile installation

Azure Site Recovery-Anbieter können über die Befehlszeile installiert werden. Diese Methode kann verwendet werden, den Anbieter auf Server Core für Windows Server 2012 R2 installieren.

1. Den Key-Datei und Registrierung Anbieter in einen Ordner herunterladen Beispielsweise C:\ASR.
2. Beenden Sie System Center Virtual Machine Manager-Dienst.
3. Führen Sie diese Befehle Anbieter Installer Extrahieren aus ein Eingabeaufforderungsfenster mit erhöhten Rechten aus:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Führen Sie diesen Befehl, um den Provider zu installieren:

        C:\ASR> setupdr.exe /i

5. Führen Sie diese Befehle zum Registrieren des Servers in dem Depot:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Die Parameter lauten:

 - **/Credentials**: Erforderlicher Parameter, der die Position angibt, in dem die wichtigsten Registrierungsdatei befindet,  
 - **/FriendlyName**: Erforderlicher Parameter für den Namen des Hostservers Hyper-V, die in Azure Site Recovery-Portal angezeigt.
 - **/EncryptionEnabled**: Optionaler Parameter, der nur beim Replizieren von VMM in Azure verwenden.
 - **/ProxyAddress**: Optionaler Parameter, der die Adresse des Proxyservers.
 - **/ProxyPort**: Optionaler Parameter, der den Port des Proxyservers.
 - **/proxyUsername**: Optionaler Parameter, der den Proxy-Benutzernamen gibt (wenn Proxy eine Authentifizierung erfordert).
 - **/proxyPassword**: Optionaler Parameter, der gibt das Kennwort für die Authentifizierung am Proxyserver (wenn der Proxyserver Authentifizierung erfordert).  

## <a name="step-3-set-up-the-target-environment"></a>Schritt 3: Festlegen der Ziel-Umgebung

Wählen Sie Zielserver VMM und Cloud.

1. Klicken Sie auf **Prepare Infrastruktur** > **Ziel** und wählen Sie die VMM-Zielserver verwenden möchten.
2.  Wolken auf dem Server mit Website synchronisiert werden angezeigt. Wählen Sie die zielcloud.

    ![Ziel](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="step-4-set-up-replication-settings"></a>Schritt 4: Einrichten von Replikationseigenschaften

1. Erstellen Sie eine neue Replikation Richtlinie klicken Sie auf **Prepare Infrastruktur** > **Replikationseigenschaften** > **+ Erstellen und zuordnen**.

    ![Netzwerk](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. Geben Sie **Erstellen und verknüpfen Richtlinie** eine Richtlinie an. Die Quell- und Ziel-Typ sollte **Hyper-V**.
3. Aktivieren Sie in **Hyper-V-Host** welches Betriebssystem auf dem Host ausgeführt wird.

    > [AZURE.NOTE] VMM-Cloud kann mit verschiedenen (unterstützte) Versionen von Windows Server Hyper-V-Hosts enthalten, aber eine Replikationsrichtlinie angewendeten Hosts dasselbe Betriebssystem ausgeführt wird. Erstellen Sie separate Replikations-Policies, Server mit mehr als einem Betriebssystemversion vorhanden.

4. **Authentifizierungstyp** und **Authentifizierung Port** Geben Sie wie Datenverkehr zwischen der primären und Recovery Hyper-V-Host-Server authentifiziert wird. Wählen Sie **Zertifikat** nur einer Kerberos-Umgebung arbeiten. Azure Site Recovery konfiguriert automatisch Zertifikate für die Authentifizierung mit HTTPS. Sie müssen manuell tun. Standardmäßig wird Anschluss 8083 und 8084 (für Zertifikate) in der Windows-Firewall auf Hyper-V-Host-Server geöffnet. Wenn Sie **Kerberos**aktivieren, wird ein Kerberos-Ticket für die gegenseitige Authentifizierung der Hostserver verwendet. Beachten Sie, dass diese Einstellung nur für Hyper-V-Host-Server auf Windows Server 2012 R2 ausgeführt.
3. Sollen Sie im Feld **Häufigkeit kopieren** wie oft Delta Datenreplikation nach der ersten Replikation (alle 30 Sekunden 5 oder 15 Minuten).
4. Geben Sie im **Wiederherstellungspunkt Aufbewahrung**in Stunden Dauer der Beibehaltungsdauer für jede Wiederherstellungspunkt werden an. Geschützte Computer können zu einem beliebigen Punkt innerhalb eines Fensters wiederhergestellt werden.
6. **App konsistente Snapshot** -Frequenz angeben wie häufig (1 bis 12 Stunden) Recovery mit anwendungskonsistente Snapshots werden erstellt. Hyper-V verwendet zwei Arten von Snapshots – eines Standardsnapshots, die ein inkrementelles Snapshots der gesamte virtuelle Computer bereitstellt und ein anwendungskonsistente Snapshots, die einen Point-in-Time Snapshot der Anwendungsdaten innerhalb des virtuellen Computers annimmt. Anwendungskonsistente Snapshots verwenden Volume Shadow Copy Service (VSS), um sicherzustellen, dass Anwendung in einem konsistenten Zustand der Momentaufnahme. Beachten Sie, dass aktivieren anwendungskonsistente Snapshots beeinträchtigen sie die Leistung der Anwendung in Quelle virtuellen Maschinen ausgeführt. Sicherstellen Sie, dass der Wert kleiner als die Anzahl der zusätzlichen Wiederherstellungspunkte, die Sie konfigurieren.
7. **Datenübertragung Komprimierung** geben an, ob replizierte Daten werden komprimiert werden.
8. Wählen Sie an VM Replikat löschen deaktivieren Schutz für die Quelle VM **Replikat VM löschen** . Schutz für die Quelle VM deaktivieren entfernt aus Website Aktivieren dieser Einstellung Einstellungen Site Recovery für VMM entfernt die VMM-Konsole und das Replikat gelöscht.
3. Im **ersten Replikationsmethode** Wenn Replikation über das Netzwerk geben Sie an, ob die erste Replikation starten oder planen. Um Netzwerkbandbreiten einzusparen möchten Sie außerhalb der Stoßzeiten planen. Klicken Sie auf **OK**.

    ![Replikationsrichtlinie](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Beim Erstellen einer neuen Richtlinie hat es die VMM-Cloud automatisch zugeordnet. **Replikationsrichtlinie** klicken Sie auf **OK**. Stehen weitere VMM-Clouds (und die VMs werden) mit dieser Replikationsrichtlinie **Einstellungen** > **Replikation** > Richtlinienname > **VMM-Cloud zuordnen**.

    ![Replikationsrichtlinie](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### <a name="prepare-for-offline-initial-replication"></a>Offline Erstreplikation vorbereiten

Offline-Replikation der ersten Datenkopie möglich. Sie können dies wie folgt vorbereiten:

- Auf dem Quellserver werden Sie einen Pfad angeben, aus dem Datenexport stattfinden soll. Vollzugriff für NTFS- und Freigabeberechtigungen Berechtigungen mit dem VMM-Dienst auf der Exportpfad. Auf dem Zielserver werden Sie einen Pfad angeben, aus dem Datenimport stattfindet. Weisen Sie Berechtigungen auf diese Importpfad.
- Freigegeben Einfuhr- oder Pfad weisen Sie Administrator, Hauptbenutzer, Druck-Operator oder Serveroperatoren Gruppenmitgliedschaft für VMM-Dienstkonto auf dem Remotecomputer, auf dem die freigegebenen befindet zu.
- Verwenden Sie Ausführen als Konten hinzufügen Hosts, importieren und Exportieren von Pfaden, weisen lesen und Schreibberechtigungen für die Konten ausführen in VMM.
- Import und Export Aktien sollte nicht auf jedem Computer als ein Hyper-V-Host-Server befinden da Loopback Konfiguration von Hyper-V nicht unterstützt wird.
- In Active Directory eingeschränkte Hostserver mit virtuellen Computern zu schützen, aktivieren und konfigurieren Sie auf jedem Hyper-V Delegierung remote Computer vertrauen auf denen Import und Export Pfade wie folgt befinden:
    1. Öffnen Sie auf dem Domänencontroller **Active Directory-Benutzer und -Computer**.
    2. Klicken Sie in der Konsolenstruktur auf **Domänenname** > **Computer**.
    3. Maustaste auf Hyper-V-Host Server > **Eigenschaften**.
    4. Klicken Sie auf der Registerkarte **Delegierung** auf **Computer für Delegierungszwecke angegebener Dienste vertrauen**.
    5. Klicken Sie auf **Beliebiges Authentifizierungsprotokoll verwenden**.
    6. Klicken Sie auf **Hinzufügen** > **Benutzer und Computer**.
    7. Geben Sie den Namen des Computers, der den Exportpfad hostet > **OK**. Die Liste der verfügbaren Dienste, halten Sie die STRG-Taste und **Cifs**auf > **OK**. Wiederholen Sie für den Namen des Computers, den Importpfad befindet. Wiederholen diesen Sie Vorgang für weitere Hyper-V-Host-Server.


### <a name="configure-network-mapping"></a>Konfigurieren von Netzwerkzuordnung

Netzwerkzuordnung zwischen Quelle und Ziel einrichten.

- Überblick was Netzwerkübersicht [Lesen](#prepare-for-network-mapping) wird. Hinzufügen einer ausführlicheren Erläuterung [Lesen](site-recovery-network-mapping.md) .
- Stellen Sie sicher, dass virtuelle Computer in VMM-Server mit einem VM-Netzwerk verbunden sind.

Konfigurieren Sie Mappings wie folgt:

1. **Einstellung** > **Website Infrastruktur** > **Netzwerk zuordnen** > **Netzwerk Mappings** klicken Sie auf **+ Netzwerk zuordnen**.

    ![Netzwerkzuordnung](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Wählen Sie **Add Network Zuordnung** Registerkarte Quelle und Ziel VMM-Server. Die VMM-Server zugeordneten VM-Netzwerke werden abgerufen.
3. Wählen Sie **Quellnetzwerk**das Netzwerk aus, aus der VM-Netzwerke primäre VMM-Server zugeordnet werden soll.
6. Wählen Sie das Netzwerk auf dem sekundären VMM-Server verwenden möchten, im **Netz** . Klicken Sie auf **OK**.

    ![Netzwerkzuordnung](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Hier geschieht Netzwerkübersicht beginnt:

- Alle vorhandenen Replikat virtuellen Computern, die VM Quellnetzwerk entsprechen, wird mit dem Ziel VM Netzwerk verbunden.
- Neue virtuelle Computer, die mit der VM Quellnetzwerk verbunden wird nach der Replikation mit dem zugeordnet-Zielnetzwerk verbunden.
- Wenn Sie eine vorhandene Zuordnung mit einem neuen Netzwerk ändern, werden Replikat virtuelle Computer mit neuen Standardeinstellungen verbunden.
- Wenn das Zielnetzwerk mehrere Subnetze und von diesen Subnetzen hat denselben Namen wie Subnetz auf dem virtuellen Quellcomputers befindet, wird dann VM Replikat an Ziel Subnetz nach einem Failover erfolgen. Ist kein Ziel Subnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.

### <a name="configure-storage-mapping"></a>Konfigurieren von Speicher-Zuordnung
Beim Replizieren einer virtuellen Maschine auf einem Hyper-V-Host Quellserver auf einem Hyper-V-Host Zielserver ist replizierte Daten am Standardspeicherort gespeichert, die für den Zielhost Hyper-V in Hyper-V-Manager angezeigt wird. Mehr Kontrolle über die replizierten Daten können Sie Speicher zuordnen<br/><br/> Um Speicher-Zuordnung zu konfigurieren, müssen Sie Speicher Klassifikationen der Quelle und Ziel VMM-Server vor der Bereitstellung. Speicher-Zuordnung durch neue Azure-Portal wird derzeit nicht unterstützt. Es kann jedoch über Powershell aktiviert werden. [Erfahren Sie mehr](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-6-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Schritt 5: Planung

Jetzt haben Sie Ihre grundlegende Infrastruktur Sie denken über Planung und herausfinden, ob zusätzliche Ressourcen benötigen.

Site Recovery bietet eine Excel-basierten Kapazitätsplaner helfen die richtigen Ressourcen für Ihre quellumgebung Websitekomponenten, Netzwerke und Speicher reservieren. Sie können im Schnellmodus Einschätzung basiert auf einer durchschnittlichen Anzahl von VMs, Datenträger und Speicher oder im detaillierten Modus, in dem Zahlen Ebene Arbeitslast Eingabe werden, Planer ausführen. Bevor Sie beginnen müssen Sie:

- Informationen Sie zur replikationsumgebung, einschließlich VMs Laufwerke pro VMs und Speicher pro Datenträger.
- Schätzen Sie die tägliche (Änderung) Änderungsrate für replizierte deltadaten müssen. Der [Kapazitätsplaner für Hyper-V Replica](https://www.microsoft.com/download/details.aspx?id=39057) können Ihnen dabei helfen.

1.  Klicken Sie auf **herunterladen** , um das Tool herunterladen und ausführen. [Lesen Sie den Artikel](site-recovery-capacity-planner.md) , die das Tool beigefügt ist.
2.  Sie wählen Sie **Ja** , **haben Sie Capacity Planner**ausführen?

    ![Planen der Kapazität](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="control-bandwidth"></a>Steuerelement-Bandbreite

Nachdem Sie Echtzeit Delta Replikationsinformationen über Kapazität Planer Hyper-V Replica gesammelt haben, können mit Excel-basierten Kapazitätsplanertools berechnen die Bandbreite für die Replikation (Initial und Delta) müssen. Bandbreite für die Replikation steuern können Sie mithilfe einer Gruppenrichtlinie oder Windows PowerShell NetQos-Richtlinie konfigurieren. Es gibt einige Methoden möglich:

**PowerShell** | **Details**
--- | ---
**New - NetQosPolicy "QoS Ziel Subnetz"** | Datenverkehr von einem Hyper-V-Host unter Windows Server 2012 R2 sekundäre Subnetz zu drosseln. Verwenden Sie die primären und sekundären Subnetzen unterscheiden.
**New - NetQosPolicy "QoS Zielport"** | Datenverkehr von einem Hyper-V-Host mit Windows Server 2012 R2 an einen Zielport zu drosseln.
**Neue - NetQosPolicy "Schubkontrolle Datenverkehr von VMM"** | Aus vmms.exe Datenverkehr zu drosseln. Dies wird Hyper-V und Live Migration Datenverkehr drosseln. Sie können IP-Protokolle und Ports verfeinern entsprechen.

Mit Gewicht Bandbreite oder Beschränkung des Datenverkehrs von Bits pro sekundär. Wenn Sie einen Cluster verwenden, den Sie auf allen Clusterknoten müssen. Weitere Informationen finden Sie unter:


- Thomas Maurer Blog auf [Hyper-V Replica Datenverkehr drosseln](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/)
- Weitere Informationen über das [Cmdlet "New-NetQosPolicy"](https://technet.microsoft.com/library/hh967468.aspx).


## <a name="step-6-enable-replication"></a>Schritt 6: Ermöglichen Replikation

Aktivieren Sie Replikation jetzt wie folgt:

1. Klicken Sie auf **Schritt2: replizieren Anwendung** > **Quelle**. Nachdem Sie Replikation erstmalig aktiviert haben, klicken Sie auf **+ replizieren** im Tresor Replikation für zusätzliche Computer aktivieren.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. In der **Quelle** Blade-> Wählen Sie VMM-Server und der Cloud in der zu replizierenden Hyper-V-Hosts befinden. Klicken Sie auf **OK**.

    ![Aktivieren der Replikation](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. Überprüfen Sie in der **Ziel** -Blade sekundären VMM-Server und Cloud.
4. Wählen Sie in **virtuellen Maschinen** VMs aus schützen möchten.

    ![VM-Schutz aktivieren](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Fortschritt kann der **Schutz aktivieren** Aktion Settings > **Aufträge** > **Website Aufträge**. Nach **Abschließen der** Schutzauftrag führt kann der virtuelle Computer Failover.


>[AZURE.NOTE] Sie können auch Schutz für virtuelle Computer in VMM-Konsole. Klicken Sie auf der Symbolleiste in den Eigenschaften des virtuellen Computers auf **Schutz aktivieren** > Registerkarte **Azure Website wiederherstellen** .

Nachdem die Replikation aktiviert haben können Sie Eigenschaften für die VM **Einstellungen**anzeigen > **Repliziert Objekte** > Vm-Name. Auf dem Dashboard **Essentials** finden Sie Informationen über die Replikationsrichtlinie für die VM und den Status. Weitere Details klicken Sie auf **Eigenschaften** .

### <a name="onboard-existing-virtual-machines"></a>Integrierte vorhandenen virtuellen Computern

Haben Sie vorhandene virtuelle Computer in VMM die Hyper-V Replica replizieren können an Bord für Azure Site Recovery Schutz wie folgt:

1. Sicherstellen Sie, dass Hyper-V Server hosten die vorhandene VM in der primären Cloud befindet und Hyper-V Server hosten Replikat virtuellen Computer in der sekundären Cloud befindet.
2. Stellen Sie sicher, dass eine Replikationsrichtlinie für primäre VMM-Cloud konfiguriert ist.
2. Aktivieren Sie Replikation für den primären virtuellen Computer. Azure Site Recovery und VMM wird sichergestellt, dass dieselbe Replika-Server und virtuellen Computer erkannt und Azure Site Recovery wiederverwenden und Replikation mit den angegebenen Einstellungen erneut.


## <a name="step-7-test-your-deployment"></a>Schritt 7: Testen der Bereitstellung

Zum Testen der Bereitstellung können Sie ein testfailover für einen einzelnen virtuellen Computer ausführen oder erstellen Sie einen Wiederherstellungsplan, der einen oder mehrere virtuelle Computer enthält.

### <a name="prepare-for-failover"></a>Bereiten für Failover vor

- Um Ihre Bereitstellung vollständig testen benötigen Sie eine Infrastruktur für replizierte Computer wie erwartet. Wenn Sie Active Directory und DNS testen möchten können erstellen einen virtuellen Computer als Domänencontroller mit DNS und dies mit Azure Site Recovery Azure replizieren. Lesen Sie mehr in [Test-Failover Aspekte für Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- In diesem Artikel wird beschrieben, wie ein testfailover ohne Netzwerk ausgeführt werden. Diese Option wird getestet, dass die VM Failover Netzwerk-Einstellungen für die VM wird nicht testen. [Weitere Informationen](site-recovery-failover.md#run-a-test-failover) zu anderen Optionen.
- Wenn ein ungeplantes Failovers statt ein testfailover ausführen möchten Beachten Sie Folgendes:

    - Wenn möglich sollten Sie primären Computer herunterfahren, vor dem Ausführen eines ungeplanten Failovers. So wird sichergestellt, dass Quelle und Replikat Computern gleichzeitig haben.
    - Beim Ausführen eines ungeplanten Failovers beendet so alle Delta Daten nicht übertragen werden, nachdem ein ungeplantes Failovers beginnt Datenreplikation von primären Computer. Darüber hinaus beim Ausführen eines ungeplanten Failovers auf einen Wiederherstellungsplan wird bis zum Abschluss, ausgeführt, wenn ein Fehler auftritt.




### <a name="run-a-test-failover"></a>Führen Sie ein testfailover

1. Failover einer einzigen VM **Einstellungen** > **Elemente repliziert**, klicken Sie auf die VM > **+ Testfailover**.

    ![Testfailover](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Einen Wiederherstellungsplan **Einstellungen**Failover > **Recovery-Pläne**, mit der rechten Maustaste des Plans > **Test-Failover**. Erstellen Sie eine plan für die Wiederherstellung [Führen Sie diese Schritte](site-recovery-create-recovery-plans.md).
2. Wählen Sie in **Test-Failover** **keine**. Mit dieser Option testen Sie, dass die VM erwartungsgemäß Failover replizierte VM wird nicht mit einem Netzwerk verbunden.

    ![Testfailover](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Klicken Sie auf **OK** , um das Failover zu beginnen. Fortschritt kann durch Klicken auf die Eigenschaften des virtuellen Computers oder für den Einzelvorgang **Test-Failover** **Settings** > **Aufträge** > **Website Aufträge**.
4. Erreicht der Failover-Auftrag **abgeschlossen** -Testphase, führen Sie folgende Schritte aus:

    -  Das Replikat VM in der sekundären VMM-Cloud anzeigen
    -  Klicken Sie auf **den Test abgeschlossen** , testfailover zu beenden.
    -  Klicken Sie auf **Notizen** zu speichern Anmerkungen testfailover zugeordnet.

5. Test-VM wird auf demselben Server wie der Host erstellt auf dem VM Replikat vorhanden ist. Es wird auf derselben Wolke hinzugefügt in der VM Replikat befindet.
6. Nachdem erfolgreich überprüft die VMs beginnen klicken Sie auf **testfailover ist abgeschlossen**. In dieser Phase werden automatisch von Site Recovery während des Failovers Test erstellt Elemente gelöscht.  

    > [AZURE.NOTE] Wenn ein testfailover mehr weiterhin als zwei Wochen mit abgeschlossen ist.



### <a name="update-dns-with-the-replica-vm-ip-address"></a>DNS mit dem Replikat VM IP Adresse aktualisieren

Nach einem Failover müssen das Replikat VM nicht die IP-Adresse wie der primäre virtuelle Computer.

- Virtuelle Computer aktualisieren den DNS-Server, die sie mit nach dem start.
- Sie können auch DNS wie folgt aktualisieren:

#### <a name="retrieve-the-ip-address"></a>Die IP-Adresse abrufen

Führen Sie dieses Beispielskript zum Abrufen der IP-Adresse.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="update-dns"></a>DNS-Aktualisierung

Führen Sie dieses Beispiel zum Aktualisieren von DNS, IP-Adresse, die mit vorherigen Skript abgerufen.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Nächste Schritte

Nach der Bereitstellung eingerichtet ist und ausgeführt wird, zu verschiedenen Failover [mehr](site-recovery-failover.md) .
