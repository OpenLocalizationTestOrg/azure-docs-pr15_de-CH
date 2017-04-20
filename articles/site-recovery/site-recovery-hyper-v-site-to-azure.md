<properties
    pageTitle="Replizieren von Hyper-V virtuelle Computer (ohne VMM) in Azure Azure-Portal mit Azure Site Recovery | Microsoft Azure"
    description="Beschreibt das Bereitstellen von Azure Site Recovery um orchestrieren Replikation, Failover und Recovery von lokalen Hyper-V VMs von VMM in Azure mithilfe von Azure Portal nicht verwaltet"
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
    ms.date="09/19/2016"
    ms.author="raynew"/>


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Replizieren von Hyper-V virtuelle Computer (ohne VMM) in Azure Azure-Portal mit Azure Site Recovery

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
- [Azure Classic](site-recovery-hyper-v-site-to-azure-classic.md)
- [PowerShell - Ressourcen-Manager](site-recovery-deploy-with-powershell-resource-manager.md)



Willkommen bei Azure Site Recovery! Verwenden Sie in diesem Artikel möchten Sie replizieren der lokalen Hyper-V virtuelle Maschinen, **sind nicht** in System Center Virtual Computer Manager (VMM) Wolken Azure verwaltet. Dieser Artikel beschreibt das Einrichten der Replikation mithilfe von Azure Site Recovery in Azure-Portal.

> [AZURE.NOTE] Azure hat zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager und Classic. Azure verfügt außerdem über zwei Portale – klassischen Azure-Portal, das klassische Bereitstellungsmodell unterstützt und Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle.

> Azure Site Recovery unterstützt Recovery und Migration von Hyper-V virtuelle Computer in Azure. Die Schritte in diesem Artikel gelten wie beim Konfigurieren der Replikation in Azure für Disaster Recovery oder zur Migration virtueller Computer in Azure

Azure Site Recovery im Azure-Portal bietet eine Reihe neuer Features:

- In der Azure sind Portal Azure Backup und Azure Site Recovery-Services in einem Tresor Recovery Services zusammengefasst, so dass einrichten und Verwalten von Business Continuity und Disaster Recovery (BCDR) von einem einzigen Ort. Ein einheitliches Dashboard können Sie Überwachung und Verwaltung der lokalen Sites und Azure public Cloud.
- Benutzer mit Azure Cloud Solution Provider (CSP) Programm bereitgestellt können jetzt Site Recovery-Vorgänge im Azure-Portal verwalten.
- Site Recovery im Azure-Portal kann auf Ressourcenmanager Speicherkonten Computer replizieren. Bei einem Failover erstellt Site Recovery Ressourcenmanager basierende VMs in Azure.
- Site Recovery weiterhin Replikation klassische Speicherkonten und Failover von virtuellen Maschinen mit dem klassischen Bereitstellungsmodell.


Nach dem Lesen dieser Beitrag Feedback unten in den Kommentaren Disqus. Fragen Sie technische [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Übersicht


Organisationen benötigen eine BCDR Strategie, die bestimmt, wie apps Arbeitslasten und Daten bleiben aktiv bei geplanten und ungeplanten Ausfällen normalen Arbeitsbedingungen so bald wie möglich wiederherstellen. Die BCDR Strategie schützen Sie Daten wiederhergestellt und sicherzustellen, dass Arbeitslasten bei Notfall ständig verfügbar sind.

Site Recovery ist eine Azure Service BCDR Strategie umzusetzen Replikation von lokalen Servern und virtuellen Computer in die Cloud (Azure) oder zu einem sekundären Datencenter beiträgt. Treten Ausfälle in Ihrem primären Standort, können Sie auf den zweiten Standort apps und Arbeitslasten verfügbar Failover. Sie können an Ihrem primären Standort nicht beenden zu. Weitere Informationen [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md)

Dieser Artikel bietet Informationen benötigen Sie replizieren der lokalen Hyper-V virtuelle Maschinen, **sind nicht** in System Center Virtual Computer Manager (VMM) Wolken Azure verwaltet. Es enthält eine Architekturübersicht Informationen und Bereitstellungsschritte zum Konfigurieren von lokalen Servern, Azure, Replikationsrichtlinie und Planung planen. Nach dem Einrichten der Infrastruktur können Sie Aktivieren der Replikation auf Computern zu schützen und führen ein testfailover Setup überprüfen. Sie können auch migrieren Sie Ihre virtuellen Computer in Azure Ausführen von geplanten Failover und führen Sie die Migration.

## <a name="business-advantages"></a>Geschäftliche Vorteile

- Bietet Offsite (Azure) Failover für Arbeitslasten in Ihrem Unternehmen und auf Hyper-V virtuelle Computer.
- Einfache Einrichtung und Verwaltung der Replikation, Failover und Recovery-Prozesse bereit eine Konsole Recovery Services.
- Können Sie problemlos Failover vom lokalen Infrastruktur in Azure und zurück (Wiederherstellen) von Azure zu lokalen Standort.
- Sie können Recovery-Pläne mit mehreren Computern konfigurieren, sodass tiered Arbeitslasten zusammen Failover.

## <a name="scenario-architecture"></a>Szenario-Architektur

Szenariokomponenten sind:

- **Hyper-V-Host oder Cluster**: lokale Hyper-V-Hostserver oder Cluster. Mit VMs zu schützenden Hyper-V-Hosts werden während der Bereitstellung der Site Recovery in logische Hyper-V-Websites gesammelt.
- **Azure Site Recovery und Recovery Services Agent**: Installation Installation Azure Site Recovery-Anbieter und Microsoft Azure Recovery Services Agent auf Hyper-V-Host-Server. Der Anbieter kommuniziert mit Azure Site Recovery über HTTPS 443 Orchestrierung replizieren. Der Agent auf dem Hyper-V-Hostserver repliziert Daten standardmäßig Azure-Speicher über HTTPS 443.
- **Azure**: Sie benötigen Azure-Abonnement, ein Konto Azure-Speicher repliziert Daten und Azure virtual Network, Azure VMs nach einem Failover mit einem Netzwerk verbunden sind.

![Hyper-V Sitearchitektur](./media/site-recovery-hyper-v-site-to-azure/architecture.png)



## <a name="azure-prerequisites"></a>Azure-Komponenten

Hier ist was Sie in Azure müssen Sie dieses Szenario bereitstellen.

**Voraussetzung** | **Details**
--- | ---
**Ein Azure-Konto**| Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.
**Azure-Speicher** | Sie benötigen ein Konto Standardspeicher. Sie können ein Speicherkonto LRS oder g. Wir empfehlen g, damit Daten sowie einen regionalen Ausfall oder die primäre Region wiederhergestellt werden kann. [Erfahren Sie mehr](../storage/storage-redundancy.md). Das Konto muss im Bereich Recovery Services Depot.<br/><br/> Premium-Speicher wird nicht unterstützt.<br/><br/> Replizierte Daten in Azure-Speicher gespeichert und Azure VMs werden erstellt, wenn ein Failover auftritt.<br/><br/> [Informationen über](../storage/storage-introduction.md) Azure-Speicher.
**Azure-Netzwerk** | Sie benötigen Azure virtual Network, der Azure-VMs verbunden wird, wenn ein Failover auftritt. Azure virtuelle Netzwerk muss im Bereich Recovery Services Depot.

## <a name="on-premises-prerequisites"></a>Lokalen Komponenten

Hier ist Sie benötigen vor Ort.

**Voraussetzung** | **Details**
--- | ---
**Hyper-V** | Eine oder mehrere lokale Server unter **Windows Server 2012 R2** mit neuesten Updates und Hyper-V-Rolle aktiviert oder **Microsoft Hyper-V Server 2012 R2**.<br/><br/>Hyper-V Server sollte eine oder mehrere virtuelle Computer enthalten.<br/><br/>Hyper-V Server sollte entweder direkt oder über einen Proxyserver mit dem Internet verbunden werden.<br/><br/>Hyper-V Server sollten gemäß [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") installiert ist.
**Anbieter und agent** | Bei Azure Site Recovery-Bereitstellung installieren Sie Azure Site Recovery-Anbieter. Die Provider-Installation installiert auch Azure Recovery Services Agent auf jedem Hyper-V Server mit virtuellen Computern zu schützen. Alle Hyper-V Server in einem Standortwiederherstellung müssen die gleichen Versionen der Anbieter und Agent.<br/><br/>Der Anbieter müssen Azure Site Recovery über das Internet herstellen. Datenverkehr kann direkt oder über einen Proxy gesendet werden. Beachten Sie, dass HTTPS Proxy wird nicht unterstützt. Der Proxyserver muss Zugriff auf: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blog.core.windows.net <br/><br/> *Store.Core.Windows.NET <br/><br/> https://www.msftncsi.com/ncsi.txt<br/><br/>Haben Sie Firewallregeln mit IP-Adresse auf dem Server sicher, dass Regeln in Azure Kommunikation ermöglichen. Sie müssen [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/confirmation.aspx?id=41653) und HTTPS (443)-Anschluss.<br/><br/>IP-Adressbereiche für Azure Region Ihres Abonnements und Westen der USA zulassen

## <a name="protected-machine-prerequisites"></a>Geschützte Computer erforderliche Komponenten


**Voraussetzung** | **Details**
--- | ---
**Geschützte VMs** | Vor dem Failover eines virtuellen Computers müssen Sie sicherstellen, dass der Name Azure VM zugewiesen [Azure Komponenten](site-recovery-best-practices.md#azure-virtual-machine-requirements)entspricht. Sie können den Namen ändern, nachdem Sie die Replikation für die VM aktiviert haben.<br/><br/> Einzelne Festplattenkapazität auf geschützten Computern sollte nicht mehr als 1023 GB sein. Eine VM können bis zu 16 Festplatten (also bis zu 16 TB).<br/><br/> Freigegebene Datenträger Gast Clustern nicht unterstützt.<br/><br/> Wenn die VM NIC-teaming hat wird nach einem Failover in Azure einer NIC konvertiert.<br/><br/>Schützen von VMs mit Linux und eine statische IP-Adresse wird nicht unterstützt.

## <a name="prepare-for-deployment"></a>Bereiten auf die Bereitstellung vor

Um für die Bereitstellung vorbereiten müssen:

1. [Einrichten einer Azure-Netzwerk](#set-up-an-azure-network) in der Azure-VMs befinden, wenn sie nach einem Failover erstellt werden.
2. [Einrichten eines Kontos Azure-Speicher](#set-up-an-azure-storage-account) für replizierte Daten.
3. [Vorbereiten der Hyper-V-Hosts](#prepare-the-hyper-v-hosts) , sicherzustellen, dass sie die erforderlichen URLs zugreifen können.

### <a name="set-up-an-azure-network"></a>Ein Azure-Netzwerk einrichten

Ein Azure-Netzwerk einrichten. Sie benötigen diese, sodass Azure VMs erstellt nach dem Failover mit einem Netzwerk verbunden sind.

- Das Netzwerk sollte im selben Bereich wie der in dem Depot Recovery Services bereitgestellt werden.
- Je nach Ressource gewünschte mit Failover Azure VMs richten Sie Azure-Netzwerk im [Ressourcenmanager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder [klassischen Modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Es wird empfohlen, dass Sie ein Netzwerk einrichten, bevor Sie beginnen. Wenn Sie nicht müssen während der Bereitstellung der Site Recovery durchführen.

> [AZURE.NOTE] [Migration von Netzwerken](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist nicht für Netzwerke für die Bereitstellung von Site Recovery unterstützt.

### <a name="set-up-an-azure-storage-account"></a>Einrichten eines Kontos Azure-Speicher

- Sie benötigen ein Konto standard Azure-Speicher für Daten in Azure repliziert.
- Je nach Ressource gewünschte mit Failover Azure VMs werden Sie ein Konto im [Ressourcenmanager](../storage/storage-create-storage-account.md) oder [klassischen Modus](../storage/storage-create-storage-account-classic-portal.md)festlegen.
- Wir empfehlen, dass Sie ein Speicherkonto einrichten, bevor Sie beginnen. Wenn Sie nicht müssen während der Bereitstellung der Site Recovery durchführen. Die Konten müssen im Bereich Recovery Services Depot.

> [AZURE.NOTE] [Migration von Speicherkonten](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist für die Bereitstellung von Site Recovery Storage-Konten nicht unterstützt.

### <a name="prepare-the-hyper-v-hosts"></a>Vorbereiten der Hyper-V-hosts

- Stellen Sie sicher, dass [Komponenten](#on-premises-prerequisites)der Hyper-V-Hosts einhalten.

### <a name="create-a-recovery-services-vault"></a>Erstellen eines Depots Recovery Services

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2. **Klicken Sie auf** > **Management** > **Backup und Site Recovery (OMS)**. Sie können auf **Durchsuchen klicken** > **Recovery Services** Depots > **Hinzufügen**.

    ![Neues Depot](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. **Name** Geben Sie einen Anzeigenamen zu Tresor an. Haben Sie mehrere Abonnements wählen Sie davon.
4. [Erstellen einer neuen Ressource](../resource-group-template-deploy-portal.md) oder wählen Sie eine vorhandene Azure Region angeben. Computer werden in diesem Bereich repliziert. Überprüfen Sie unterstützte Regionen finden Sie unter geografische Verfügbarkeit in [Azure Site Recovery Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Möchten Sie das Depot aus dem Dashboard zugreifen klicken Sie **auf Dashboard** und dann auf **Depot erstellen**.

    ![Neues Depot](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Neue Depot erscheinen im **Dashboard** > **alle Ressourcen**und auf die **Recovery Services Depots** Blade.

## <a name="getting-started"></a>Erste Schritte

Site Recovery bietet einen Einstieg Erfahrung, mit der Sie so schnell wie möglich bereitstellen. Erste Schritte überprüft Komponenten und Site Recovery-Bereitstellung in der richtigen Reihenfolge Schritte durchläuft.

Erste Schritte Sie wählen den Computer repliziert werden soll und auf repliziert werden soll. Einrichten von lokalen Servern, Azure-Speicherkonten und Netzwerke. Sie erstellen Replikations-Policies und Planung durchführen. Nach dem Einrichten der Infrastruktur können Sie Replikation für virtuelle Computer. Sie können ausführen Failover für bestimmte Computer oder Recovery-Pläne nicht über mehrere Computer erstellen.

Zunächst Einsteiger auswählen wie Sie Site Recovery bereitstellen möchten. Der Einstieg Fluss ändert sich je nach Bedarf Replikation etwas.



## <a name="step-1-choose-your-protection-goals"></a>Schritt 1: Auswählen der Ziele

Wählen Sie replizieren möchten und wo Sie zu replizieren.

1. Blade **-Rückgewinnungsservice Depots** Vault und klicken Sie auf **Settings**.
2. **Einstellungen** > **Erste Schritte** klicken Sie auf **Site Recovery** > **Schritt 1: Vorbereiten Infrastruktur** > **Schutzziel**.

    ![Ziele auswählen](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. **Schutzziel** wählen Sie **, Azure**, und wählen Sie **Ja, mit Hyper-V**. Wählen Sie **Nein** zu bestätigen, dass Sie VMM nicht verwenden. Klicken Sie auf **OK**.

    ![Ziele auswählen](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Schritt 2: Einrichten der Quell-Umgebung

Hyper-V-Site richten Sie ein, installieren Sie Azure Site Recovery Provider und Azure Recovery Services Agent auf Hyper-V-Hosts und registrieren Sie Hosts im Tresor zu.


1. Klicken Sie auf **Schritt 2: Vorbereiten der Infrastruktur** > **Quelle**. Hinzufügen eine neue Hyper-V-Website als Container für die Hyper-V-Hosts oder Clustern, klicken Sie auf **+ Hyper-V-Website**.

    ![Quelle festlegen](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. **Hyper-V erstellt Site** Blade Geben Sie einen Namen für die Site. Klicken Sie auf **OK**. Wählen Sie die soeben erstellte Site.

    ![Quelle festlegen](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Klicken Sie auf **+ Hyper-V Server** um einen Server zur Website hinzuzufügen.
4. Server **Hinzufügen** > **Server** überprüfen, **Hyper-V Server** angezeigt wird. Sicherstellen Sie, dass Hyper-V-Server hinzufügen möchten [Voraussetzung](#on-premises-prerequisites) erfüllt und kann auf die angegebenen URLs zugreifen.
4. Installation von Azure Site Recovery Anbieter herunterladen. Diese Datei zum Installieren der Anbieter und der Recovery Services-Agent auf jedem Hyper-V-Host laufen.
5. Downloaden Sie den Registrierungsschlüssel. Sie benötigen diese, wenn Sie Setup ausführen. Der Schlüssel gilt für 5 Tage, nachdem Sie es erstellt haben.

    ![Quelle festlegen](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. Führen Sie den Anbieter Setupdatei auf jedem Host, der Hyper-V-Website hinzugefügt. Wenn Sie auf einem Hyper-V-Cluster installieren, führen Sie Setup auf jedem Clusterknoten. Installieren und Registrieren von einzelnen Hyper-V-Cluster-Knoten stellt sicher, dass virtuelle Computer geschützt bleiben, auch wenn sie über Knoten migrieren.

### <a name="install-the-provider-and-agent"></a>Anbieter und Agent installieren

1. Führen Sie den Anbieter Setupdatei.
2. **Microsoft** Update können Sie Updates anmelden, damit Microsoft Update Richtlinie Anbieter Updates installiert werden.
3. **Installation** akzeptieren Sie oder ändern Sie den Standardspeicherort für Anbieter und klicken Sie auf **Installieren**.
4. Klicken Sie in **Tresor** Seite auf **Durchsuchen** , um Vault-Schlüsseldatei auswählen, die Sie heruntergeladen haben. Geben Sie Azure Site Recovery-Abonnement und Vault-Name sowie der Hyper-V Hyper-V Server gehört.

    ![Server-Registrierung](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. angegeben Sie **Proxy** wie der Anbieter, die auf dem Server installiert, Azure Site Recovery über das Internet verbunden werden.

- Soll den Anbieter direkt wählen Sie **Verbinden direkt ohne Proxy aus**
- Wählen Sie mit dem Proxy herstellen, die aktuell auf dem Server eingerichtet werden soll **Verbindung mit vorhandenen Proxyeinstellungen**.
- Wenn Ihre vorhandene Proxy Authentifizierung erfordert oder eines benutzerdefinierten Anbieter Verbindung wählen Sie **Verbinden mit benutzerdefinierten Proxyeinstellungen**angezeigt werden soll.
- Verwenden Sie einen benutzerdefinierten Proxy müssen Sie die Adresse, Anschluss und Anmeldeinformationen angeben
- Verwenden Sie einen Proxy stellen werden sicher beschriebenen [Komponenten](#on-premises-prerequisites) URLs es zugelassen.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. nach Abschluss der Installation klicken Sie auf **Registrieren** , Registrieren des Servers im Tresor.

![Installation](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. nach Abschluss Registrierung Metadaten von Hyper-V Server von Azure Site Recovery abgerufen und der Server die **Einstellungen**angezeigt > **Website Infrastruktur** > Blade**Hyper-V-Hosts** .


### <a name="command-line-installation"></a>Befehlszeile installation

Azure Site Recovery Provider und Agent können auch mithilfe der folgenden Befehlszeile installiert werden. Diese Methode kann verwendet werden, den Anbieter auf Server Core für Windows Server 2012 R2 installieren.

1. Den Key-Datei und Registrierung Anbieter in einen Ordner herunterladen Beispielsweise C:\ASR.
2. Führen Sie diese Befehle Anbieter Installer Extrahieren aus ein Eingabeaufforderungsfenster mit erhöhten Rechten aus:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Führen Sie diesen Befehl, um die Komponenten zu installieren:

            C:\ASR> setupdr.exe /i

4. Führen Sie diese Befehle zum Registrieren des Servers in das Depot: CD C:\Program Files\Microsoft Azure Site Recovery Provider\
            C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe/r /Friendlyname <friendly name of the server> /Anmeldeinformationen<path of the credentials file>

<br/>
Wo:

- **/Credentials** : Erforderlicher Parameter, der die Position angibt, in dem die wichtigsten Registrierungsdatei befindet,  
- **/FriendlyName** : Erforderlicher Parameter für den Namen des Hostservers Hyper-V, die in Azure Site Recovery-Portal angezeigt.
- **/ProxyAddress** : Optionaler Parameter, der die Adresse des Proxyservers.
- **/ProxyPort** : Optionaler Parameter, der den Port des Proxyservers.
- **/proxyUsername** : Optionaler Parameter, der den Proxy-Benutzernamen gibt (wenn Proxy eine Authentifizierung erfordert).
- **/proxyPassword** : Optionaler Parameter, der gibt das Kennwort für die Authentifizierung am Proxyserver (wenn der Proxyserver Authentifizierung erfordert).


## <a name="step-3-set-up-the-target-environment"></a>Schritt 3: Festlegen der Ziel-Umgebung

Geben Sie das Konto Azure-Speicher für Replikation und Azure-Netzwerk mit dem Azure VMs nach einem Failover verbunden wird.

1.  Klicken Sie auf **Prepare Infrastruktur** > **Ziel** und wählen Sie den Azure-Abonnement verwenden möchten.
2.  Geben Sie das Bereitstellungsmodell für VMs nach einem Failover verwenden möchten.
3.  Site Recovery überprüft haben kompatible Azure-Speicherkonten und Netzwerke.

    ![Speicher](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.  Wenn Sie noch kein Speicherkonto erstellt und soll einen Ressourcen-Manager klicken Sie auf **+ Storage** Konto soll, Inline. Blade **Speicherkonto erstellen** Geben Sie einen Kontonamen, Typ, Abonnements und Speicherort. Das Konto sollte in demselben Speicherort wie das Depot Recovery Services.

    ![Speicher](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

    Möchten Sie ein Speicherkonto der klassischen Modell erstellen tun Sie diese [im Azure-Portal](../storage/storage-create-storage-account-classic-portal.md).

5.  Wenn Sie noch nicht Azure-Netzwerk erstellt und soll eine Ressourcen-Manager klicken Sie auf **+ Netzwerk** dazu, Inline. **Virtuelles Netzwerk erstellen** Blade Geben Sie einen Netzwerknamen, Adressbereich Subnetdetails, Abonnements und Speicherort. Das Netzwerk sollte am gleichen Speicherort wie das Depot Recovery Services.

    ![Netzwerk](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

    Möchten Sie ein Netzwerk mit dem klassischen Modell tun Sie das [in Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).


## <a name="step-4-set-up-replication-settings"></a>Schritt 4: Einrichten von Replikationseigenschaften

1. Erstellen Sie eine neue Replikation Richtlinie klicken Sie auf **Prepare Infrastruktur** > **Replikationseigenschaften** > **+ Erstellen und zuordnen**.

    ![Netzwerk](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. Geben Sie **Erstellen und verknüpfen Richtlinie** eine Richtlinie an.
3. Sollen Sie im Feld **Häufigkeit kopieren** wie oft Delta Datenreplikation nach der ersten Replikation (alle 30 Sekunden 5 oder 15 Minuten).
4. Geben Sie im **Wiederherstellungspunkt Aufbewahrung**in Stunden Dauer der Beibehaltungsdauer für jede Wiederherstellungspunkt werden an. Geschützte Computer können zu einem beliebigen Punkt innerhalb eines Fensters wiederhergestellt werden.
6. **App konsistente Snapshot** -Frequenz angeben wie häufig (1 bis 12 Stunden) Recovery mit anwendungskonsistente Snapshots werden erstellt. Hyper-V verwendet zwei Arten von Snapshots – eines Standardsnapshots, die ein inkrementelles Snapshots der gesamte virtuelle Computer bereitstellt und ein anwendungskonsistente Snapshots, die einen Point-in-Time Snapshot der Anwendungsdaten innerhalb des virtuellen Computers annimmt. Anwendungskonsistente Snapshots verwenden Volume Shadow Copy Service (VSS), um sicherzustellen, dass Anwendung in einem konsistenten Zustand der Momentaufnahme. Beachten Sie, dass aktivieren anwendungskonsistente Snapshots beeinträchtigen sie die Leistung der Anwendung in Quelle virtuellen Maschinen ausgeführt. Sicherstellen Sie, dass der Wert kleiner als die Anzahl der zusätzlichen Wiederherstellungspunkte, die Sie konfigurieren.
3. Festlegen Sie im **anfänglichen Replikation Startzeit** , wann die erste Replikation starten. Die Replikation erfolgt über die Internetbandbreite so außerhalb der Stoßzeiten planen möchten. Klicken Sie auf **OK**.

    ![Replikationsrichtlinie](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Beim Erstellen einer neuen Richtlinie hat es automatisch der Hyper-V-Website zugeordnet. Klicken Sie auf **OK**. Zuordnen von Hyper-V-Website (und es VMs) mit mehreren Replikations-Policy **Settings** > **Replikation** > Richtlinienname > **Hyper-V-Website zuordnen**.

## <a name="step-5-capacity-planning"></a>Schritt 5: Planung

Jetzt haben Sie Ihre grundlegende Infrastruktur Sie denken über Planung und herausfinden, ob zusätzliche Ressourcen benötigen.

Site Recovery bietet Kapazitätsplaner helfen die richtigen Ressourcen für Ihre quellumgebung Websitekomponenten, Netzwerke und Speicher reservieren. Sie können im Schnellmodus Einschätzung basiert auf einer durchschnittlichen Anzahl von VMs, Datenträger und Speicher oder im detaillierten Modus, in dem Zahlen Ebene Arbeitslast Eingabe werden, Planer ausführen. Bevor Sie beginnen müssen Sie:

- Informationen Sie zur replikationsumgebung, einschließlich VMs Laufwerke pro VMs und Speicher pro Datenträger.
- Schätzen Sie die tägliche (Churn) Änderungsrate für replizierte Daten müssen. Der [Kapazitätsplaner für Hyper-V Replica](https://www.microsoft.com/download/details.aspx?id=39057) können Ihnen dabei helfen.

1.  Klicken Sie auf **herunterladen** , um das Tool herunterladen und ausführen. [Lesen Sie den Artikel](site-recovery-capacity-planner.md) , die das Tool beigefügt ist.
2.  Sie wählen Sie **Ja** , **haben Sie Capacity Planner**ausführen?

    ![Planen der Kapazität](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Vorüberlegungen zum Netzwerk-Bandbreite

Die Kapazitätsplanertools können Sie berechnen die Bandbreite für die Replikation (erste Replikation und Delta) müssen. Um die bandbreitenverwendung für die Replikation steuern haben Sie einige Optionen:

- **Bandbreite**: Hyper-V, die in Azure repliziert Netzwerkverkehr über einen bestimmten Hyper-V-Host. Sie können Bandbreite auf dem Hostserver einschränken.
- **Optimieren der Bandbreite**: die Bandbreite für die Replikation mit zwei Registrierungsschlüssel beeinflussen.

#### <a name="throttle-bandwidth"></a>Bandbreite

1. Microsoft Azure Backup-MMC-Snap-in auf Hyper-V-Host-Server öffnen. Standardmäßig wird eine Verknüpfung für Microsoft Azure Backup auf dem Desktop oder in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klicken Sie im Snap-in- **Eigenschaften ändern**.
3. Wählen Sie auf der Registerkarte **Beschränkung** **Internet-Bandbreite für Backups Einschränkungen aktivieren**, legen Sie die Grenzwerte für die Arbeit und arbeitsfreien Stunden. Gültige Bereiche sind 512 Kbit/s bis 102 MB/s pro Sekunde.

    ![Bandbreite](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

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

## <a name="step-6-enable-replication"></a>Schritt 6: Ermöglichen Replikation

Aktivieren Sie Replikation jetzt wie folgt:

1. Klicken Sie auf **Schritt2: replizieren Anwendung** > **Quelle**. Nachdem die Replikation erstmalig aktiviert haben klicke Sie **+ replizieren** im Tresor Replikation für zusätzliche Computer aktivieren.

    ![Aktivieren der Replikation](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. In der **Quelle** Blade-> Wählen Sie die Hyper-V-Website. Klicken Sie auf **OK**.
3. Wählen Sie im **Ziel** Vault-Abonnement und Failover-Modell in Azure (Classic oder Resource Management) nach einem Failover verwenden möchten.
4. Wählen Sie das Speicherkonto, das Sie verwenden möchten. Möchten Sie einen anderen Speicher Konto als haben Sie [Erstellen](#set-up-an-azure-storage-account)können. Erstellen Sie ein Konto mit dem Ressourcen-Manager-Modell klicken Sie auf **neu erstellen**. Möchten Sie ein Speicherkonto der klassischen Modell erstellen tun Sie diese [im Azure-Portal](../storage/storage-create-storage-account-classic-portal.md). Klicken Sie auf **OK**.
5.  Wählen Sie Azure-Netzwerk und Subnetz mit dem Azure VMs verbunden wird, wenn sie nach einem Failover erstellt haben. Wählen Sie **konfigurieren jetzt für ausgewählte Computer** die Einstellung für alle Computer ausgewählten Schutz. Wählen Sie **später konfigurieren** Azure Netzwerk pro Computer auswählen. Wenn Sie ein anderes Netzwerk die verwenden möchten haben Sie [Erstellen](#set-up-an-azure-network)können. Klicken Sie zum Erstellen ein Netzwerks mit dem Ressourcen-Manager-Modell auf **neu erstellen**. Möchten Sie ein Netzwerk mit dem klassischen Modell tun Sie das [in Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wählen Sie ggf. ein Subnetz. Klicken Sie auf **OK**.

    ![Aktivieren der Replikation](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. In **virtuellen Maschinen** > **virtuellen Computer auswählen** auf und wählen Sie jeden Computer, die Sie replizieren möchten. Sie können nur Computer auswählen, für die Replikation aktiviert werden kann. Klicken Sie auf **OK**.

    ![Aktivieren der Replikation](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. **Eigenschaften** > **Eigenschaften konfigurieren**, wählen Sie das Betriebssystem für die ausgewählten virtuellen Computer und Betriebssystem-Datenträger. Azure VM Namen (Ziel) [Azure Virtual Machine Vorschriften](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllt überprüfen und bei Bedarf ändern. Klicken Sie auf **OK**. Sie können später weitere Eigenschaften festlegen.

    ![Aktivieren der Replikation](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. In **Replikationseigenschaften** > **Replikationseigenschaften konfigurieren**, wählen Sie die Replikationsrichtlinie für die geschützten virtuellen Computer anwenden möchten. Klicken Sie auf **OK**. Die Replikationsrichtlinie **Einstellungen**ändern > **Replikationsrichtlinien** > Richtlinienname > **Einstellungen bearbeiten**. Änderungen, die Sie verwendet für neue Computer und Computer bereits repliziert werden.

    ![Aktivieren der Replikation](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Fortschritt des Auftrags **Schutz aktivieren** **Einstellungen**verfolgen > **Aufträge** > **Website Aufträge**. Nach **Abschließen der** Schutzauftrag die Maschine läuft kann Failover.

### <a name="view-and-manage-vm-properties"></a>Anzeigen und Verwalten von VM-Eigenschaften

Überprüfen Sie die Eigenschaften des Quellcomputers empfohlen.

1. **Klicken Sie** > **Geschützte Objekte** > **Repliziert Objekte** > und wählen Sie den Computer.

    ![Aktivieren der Replikation](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. **Eigenschaften** können Sie Informationen für die VM-Replikation und Failover anzeigen.

    ![Aktivieren der Replikation](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. **Compute**und > **Eigenschaften berechnen** die Azure-VM-Name und Ziel Größe angeben. Ändern Sie den Namen Azure erfüllen Sie benötigen. Sie können auch anzeigen und Ändern von Informationen zum Zielnetzwerk, Subnetzmaske und IP-Adresse der Azure-VM zugewiesen werden. Beachten Sie Folgendes:

    - Sie können die Ziel-IP-Adresse festlegen. Wenn Sie eine Adresse, die fehlerhafte Maschine angeben werden DHCP verwenden. Wenn Sie eine Adresse, die nicht beim Failover verfügbar ist festlegen, wird das Failover fehl. Die gleiche IP-Zieladresse kann für testfailover verwendet werden, ist die Adresse im Testnetzwerk Failover.
    - Die Anzahl der Netzwerkadapter diktiert die Größe für der virtuelle Zielcomputer wie folgt angeben:

        - Wenn die Anzahl der Netzwerkadapter auf dem Quellcomputer kleiner oder gleich der Anzahl der Netzwerkadapter für die Zielgröße Computer zulässig ist, haben das Ziel die gleiche Anzahl von Adaptern als Quelle.
        - Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputers die Höchstzahl überschreitet für die Größe und die maximale Ziel verwendet werden.
        - Wenn beispielsweise ein Quellcomputer verfügt über zwei Netzwerkadapter die Zielgröße Computer unterstützt vier und dem Zielcomputer müssen zwei Adapter. Wenn der Quellcomputer hat zwei Adapter unterstützten Zielgröße unterstützt jedoch nur eine haben dem Zielcomputer nur eine Netzwerkkarte.     
        - Die VM hat mehrere Netzwerkadapter werden alle im gleichen Netzwerk herstellen.

    ![Aktivieren der Replikation](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.  **Datenträger** des Betriebssystems und der Datenträger auf dem virtuellen Computer angezeigt, die repliziert werden.


## <a name="step-7-test-the-deployment"></a>Schritt 7: Testen der Bereitstellung

Zum Testen der Bereitstellung führen Sie ein testfailover einer einzigen virtuellen Maschine oder einen Wiederherstellungsplan, der einen oder mehrere virtuelle Computer enthält.


### <a name="prepare-for-test-failover"></a>Vorbereitung der testfailover

- Führen Sie ein testfailover wird empfohlen, ein neues Azure-Netzwerk, das isoliert wurde von Azure Produktionsnetzwerk erstellen (Dies ist standardmäßig beim Erstellen eines neuen Netzwerks in Azure). [Weitere Informationen](site-recovery-failover.md#run-a-test-failover) zu Test-Failover ausgeführt.
- Um die beste Leistung bei einem zu Azure Failover, installieren Sie den Azure-Agent auf dem geschützten Computer. Es schneller starten und bei der Problembehandlung. Installieren Sie den Agent [Linux](https://github.com/Azure/WALinuxAgent) oder [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) .
- Um die Bereitstellung vollständig testen benötigen Sie eine Infrastruktur für replizierte Computer wie erwartet. Wenn Sie Active Directory und DNS testen möchten können erstellen einen virtuellen Computer als Domänencontroller mit DNS und dies mit Azure Site Recovery Azure replizieren. Lesen Sie mehr in [Test-Failover Aspekte für Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
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

- Zugeordnete Azure VM zu RDP NIC fügen Sie eine öffentliche IP-Adresse hinzu.
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

### <a name="run-a-test-failover"></a>Führen Sie ein testfailover

Test-Failover führen Folgendes ausführen:

1. Failover einer einzigen VM **Einstellungen** > **Elemente repliziert**, klicken Sie auf die VM > **+ Testfailover**.

    ![Testfailover](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Einen Wiederherstellungsplan **Einstellungen**Failover > **Recovery-Pläne**, mit der rechten Maustaste des Plans > **Test-Failover**. Erstellen Sie eine plan für die Wiederherstellung [Führen Sie diese Schritte](site-recovery-create-recovery-plans.md).

3. Wählen Sie **Test** -Failover Azure-Netzwerk mit dem Azure VMs nach einem Failover verbunden werden.

    ![Testfailover](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Klicken Sie auf **OK** , um das Failover zu beginnen. Fortschritt kann durch Klicken auf die VM Eigenschaften öffnen und im **Test-Failover** **Einstellungen** > **Website Aufträge**.
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


## <a name="failover"></a>Failover

Nach der anfänglichen Replikation für die Computer abgeschlossen ist, können Sie Failover bei Bedarf aufrufen. Site Recovery unterstützt verschiedene Failover - Test-Failover, Geplantes Failover und ungeplante Failover.
[Erfahren Sie mehr](site-recovery-failover.md) über verschiedene Failover und ausführliche Beschreibung, wann und wie sie durchführen.

> [AZURE.NOTE] Ist Ihre Absicht Azure virtuelle Computer migrieren, empfehlen wir einen [geplanten Failover-Vorgang](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) verwenden, um die virtuellen Computer in Azure migrieren. Nach der Überprüfung der migrierten Anwendung in Azure mit testfailover gehen Sie gemäß [Vollständige Migration](#Complete-migration-of-your-virtual-machines-to-Azure) für die Migration virtueller Maschinen. Sie brauchen eine Commit-oder löschen. Vollständige Migration schließt die Migration, den Schutz für den virtuellen Computer entfernt und Azure Site Recovery Abrechnung des Computers beendet.


###<a name="run-an-unplanned-failover"></a>Führen Sie ein ungeplantes Failovers

Dies sollte ein primärer Standort unzugänglich durch ein unerwartetes Ereignis wie ein Strom Ausfall oder Virus-Angriff wird gewählt werden. Dieses Verfahren beschreibt das Ausführen eines 'ungeplanten Failovers' für einen Wiederherstellungsplan. Alternativ können Sie das Failover für einen einzelnen virtuellen Computer auf der Registerkarte virtuelle Maschinen ausführen. Bevor Sie beginnen, stellen Sie sicher, dass alle virtuellen Computer zu Failover die erste Replikation abgeschlossen haben.

1. Wählen Sie **Wiederherstellungspläne > Recoveryplan_name**.
2. Auf Recovery Plan Blade **Ungeplanten Failover**.
3. Wählen Sie auf der Seite **ungeplanten Failover** Quell- und Zielspeicherorte.
4. Wählen Sie **Herunterfahren der virtuellen Computer und die neuesten Daten synchronisieren** , Site Recovery sollten geschützte virtuelle Maschinen und Daten synchronisieren, sodass die neueste Version der Daten Failover.
5. Nach dem Failover werden die virtuellen Computer in ein Commit aussteht.  Klicken Sie auf **Commit** Failover commit.

[Weitere Informationen](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Abschließen der Migration Ihrer virtuellen Maschinen in Azure


>[AZURE.NOTE] Die folgenden Schritte gelten nur, wenn Sie virtuelle Computer in Azure migrieren

1. Durchführung eines geplanten Failover als erwähnt [hier](site-recovery-failover.md)
2. In **Settings > replizierten Elemente**mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Vollständige Migration**

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

2. Klicken Sie auf **OK** , um die Migration abzuschließen. Sie können die Fortschritte verfolgen, indem die Eigenschaften des virtuellen Computers oder mithilfe der vollständigen Migration in **Settings > Site Aufträge**.


## <a name="monitor-your-deployment"></a>Überwachen der Bereitstellung

Hier ist, wie Sie die Konfiguration, Status und Zustand für Ihre Site Recovery-Bereitstellung überwachen können:

1. Klicken Sie auf das Depot auf dem **Essentials** -Dashboard. In diesem Dashboard können Sie Site Recovery Aufträge, Replikationsstatus Recovery-Pläne, Serverzustand und Ereignisse.  Sie können Grundlagen der Kacheln und Layouts, die Sie z. B. den Status der anderen Site Recovery und Backup-Depots besonders hilfreich sind.

    ![Grundlagen](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. Der **Gesundheit** Kachel können Sie Siteserver überwachen, die Problem und die in den letzten 24 Stunden von Site Recovery ausgelösten Ereignisse auftreten.
3. Verwalten und Überwachen der Replikation der **Objekte repliziert**, **Recovery-Pläne**, und **Wiederherstellungsaufträge Website** angeordnet. Aufträge in **Einstellungen**Drillinto -> **Aufträge** -> **Website Aufträge**.
