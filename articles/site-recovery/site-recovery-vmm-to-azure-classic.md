<properties
    pageTitle="Replizieren von Hyper-V virtuelle Computer in VMM-Clouds in Azure | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie Hyper-V virtuelle Computer in Hyper-V-Hosts befindet sich im System Center VMM-Clouds in Azure replizieren."
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
    ms.topic="hero-article"
    ms.date="05/06/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Replizieren Sie Hyper-V virtuelle Computer in VMM-Clouds in Azure

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - Ressourcen-Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Verwaltungsportal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - klassisch](site-recovery-deploy-with-powershell.md)



Azure Site Recovery-Dienst trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern und Servern orchestriert. Computer können Azure oder einem lokalen sekundären Rechenzentrum repliziert werden. Finden Sie einen schnellen Überblick über [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md).

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt die Bereitstellung von Site Recovery um Hyper-V virtuelle Computer in Hyper-V-Host-Servern replizieren, die private VMM-Clouds in Azure befinden.

Der Artikel enthält Komponenten für das Szenario und zeigt, wie Site Recovery Depot einrichten Azure Site Recovery Provider installiert auf dem VMM-Quellserver abrufen, registrieren Sie den Server im Tresor, Azure Storage-Konto hinzufügen, Azure Recovery Services Agent auf Hyper-V-Hostservern installieren, konfigurieren Sie Schutz für alle geschützten virtuellen Maschinen angewendet werden VMM-clouds , und aktivieren Sie Schutz für diese virtuellen Computer. Testen Sie das Failover sicherzustellen, dass alles erwartungsgemäß beenden.

Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Architektur

![Architektur](./media/site-recovery-vmm-to-azure-classic/topology.png)

- Azure Site Recovery-Anbieter ist während der Site Recovery-Bereitstellung VMM installiert und der VMM-Server in der Site Recovery Tresor registriert. Der Anbieter kommuniziert mit Site Recovery, Replikation Orchestrierung behandeln.
- Der Azure Recovery Services ist auf Hyper-V-Host-Server während der Bereitstellung der Site Recovery Agent. Es behandelt die Datenreplikation Azure-Speicher.


## <a name="azure-prerequisites"></a>Azure-Komponenten

Hier ist in Azure benötigen.

**Voraussetzung** | **Details**
--- | ---
**Ein Azure-Konto**| Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.
**Azure-Speicher** | Sie benötigen ein Konto Azure-Speicher zum Speichern von replizierter Daten. Replizierte Daten in Azure-Speicher gespeichert und Azure VMs werden erstellt, wenn ein Failover auftritt. <br/><br/>Sie benötigen ein [standard Geo redundante Speicherkonto](../storage/storage-redundancy.md#geo-redundant-storage). Das Konto muss im Bereich der Site Recovery Service und dieselbe Abonnement zugeordnet werden. Beachten Sie, dass die Replikation auf Premium-Speicherkonten wird derzeit nicht unterstützt und sollte nicht verwendet werden.<br/><br/>[Informationen über](../storage/storage-introduction.md) Azure-Speicher.
**Azure-Netzwerk** | Sie benötigen Azure virtual Network, der Azure-VMs verbunden wird, wenn ein Failover auftritt. Azure virtuelle Netzwerk muss im Bereich der Site Recovery-Tresor.

## <a name="on-premises-prerequisites"></a>Lokalen Komponenten

Hier ist Sie benötigen vor Ort.

**Voraussetzung** | **Details**
--- | ---
**VMM** | Sie benötigen mindestens eine VMM-Server als physische oder virtuelle eigenständiger Server oder als virtueller Cluster bereitgestellt. <br/><br/>Der VMM-Server sollte mit den neuesten kumulativen Updates System Center 2012 R2 ausgeführt werden.<br/><br/>Sie benötigen mindestens eine Cloud auf dem VMM-Server konfiguriert.<br/><br/>Die Quelle Cloud, den zu schützenden enthalten eine oder mehrere VMM-Hostgruppen.<br/><br/>Weitere Informationen zum Einrichten von VMM-Clouds in [Exemplarische Vorgehensweise: Erstellen von privaten Clouds mit System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) Keith Mayers Blog.
**Hyper-V** | Hyper-V-Hostserver oder Cluster in der VMM-Cloud benötigen. Der Hostserver muss und eine oder mehrere VMs. <br/><br/>Hyper-V Server laufen auf mindestens **Windows Server 2012 R2** mit Hyper-V-Rolle oder **Microsoft Hyper-V Server 2012 R2** und die neuesten Updates installiert haben.<br/><br/>Alle VMs zu schützenden mit Hyper-V-Server muss in der VMM-Cloud befinden.<br/><br/>Wenn Sie Hyper-V in einer Notiz Cluster, Cluster-Broker arbeiten ist nicht automatisch erstellt, wenn einen statischen IP-Adresse-basierten Cluster haben. Sie müssen den Cluster-Broker manuell konfigurieren. [Erfahren Sie mehr](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) im Blogeintrag Andreas Finn.
**Geschützte Computer** | VMs zu schützen sollten [Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)erfüllen.


## <a name="network-mapping-prerequisites"></a>Netzwerk-Zuordnung erforderliche Komponenten
Wenn Sie virtuelle Computer in Azure Zuordnung Netzwerkkarten zwischen VM auf dem Quellserver VMM schützen und Azure Netzwerke, um Folgendes zu ermöglichen:

- Alle Computer der Failover in demselben Netzwerk, unabhängig von der Wiederherstellungsplan Verbindung in sind.
- Ist ein Netzwerk-Gateway auf dem Ziel Azure-Netzwerk, können virtuelle Computer mit anderen lokalen virtuellen Computer verbinden.
- Wenn Failover gleichen Wiederherstellungsplan Zuordnung nur virtuellen Maschinen im Netzwerk konfiguriert werden nicht werden nach Failover auf Azure miteinander verbinden können.

Wenn Sie Netzwerkzuordnung bereitstellen möchten benötigen Sie Folgendes:

- Die virtuellen Computer auf dem Quellserver VMM schützen möchten sollten mit einem VM-Netzwerk verbunden. Das Netzwerk sollte ein logisches Netzwerk verbunden, das der Cloud zugeordnet ist.
- Ein Azure-Netzwerk der replizierten virtuellen Computer nach einem Failover herstellen können. Sie wählen dieses Netzwerk bei Failover. Das Netzwerk sollte in der gleichen Region wie Ihr Abonnement Azure Site Recovery.

Vorbereitung der Netzwerk wie folgt zuordnen:

1. [Informationen zum](site-recovery-network-mapping.md) netzwerkanforderungen.
2. VM-Netzwerke in VMM vorbereiten:

    - [Logische Netzwerke einrichten](https://technet.microsoft.com/library/jj721568.aspx).
    - [VM-Netzwerke einrichten](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Schritt 1: Erstellen eines Depots Site Recovery

1. Melden Sie sich im [Verwaltungsportal](https://portal.azure.com) von VMM-Server zu registrieren.
2. Klicken Sie auf **Data Services** > **Recovery Services** > **Site Recovery Depot**.
3. Klicken Sie auf **neue** > **schnell erstellen**.
4. Geben Sie im Feld **Name**einen Anzeigenamen zu Tresor.
5. Wählen Sie im **Bereich**geografische Region für das Depot. Überprüfen Sie anzeigen unterstützte Regionen geografische Verfügbarkeit in [Azure Site Recovery Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Klicken Sie auf **Create Tresor**.

    ![Neues Depot](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Überprüfen Sie die Statusleiste, um sicherzustellen, dass das Depot erfolgreich erstellt wurde. Das Depot wird als **aktiv** auf Recovery Services aufgeführt.

## <a name="step-2-generate-a-vault-registration-key"></a>Schritt 2: Erstellen Sie einen Vault-Registrierungsschlüssel

Erstellen Sie einen Registrierungsschlüssel im Tresor. Azure Site Recovery Anbieter herunterladen und installieren auf dem VMM-Server verwenden Sie diesen Schlüssel, das Depot VMM-Server anmelden.

1. Der **Recovery-** Seite klicken Sie auf das Depot zum Öffnen der Seite Quick Start. Quick-Start kann auch jederzeit auf das Symbol geöffnet werden.

    ![Schnellstart-Symbol](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)

2. Wählen Sie in der Dropdownliste **zwischen einer lokalen VMM und Microsoft Azure**.
3. Klicken Sie in **VMM-Server Vorbereiten**auf **Schlüssel generieren** . Die Schlüsseldatei wird automatisch generiert und ist 5 Tage nach der Erstellung gültig. Wenn Sie nicht das Azure-Portal aus dem VMM-Server zugreifen, müssen Sie diese Datei auf den Server kopieren.

    ![Registrierungsschlüssel](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Schritt 3: Installieren Sie das Azure Site Recovery

1. Im **Quick Start** > **Vorbereiten VMM-Server**, klicken Sie auf **Download von Microsoft Azure Site Recovery Provider auf VMM-Server** die neueste Version der Installationsdatei Anbieter.
2. Führen Sie diese Datei auf dem Quellserver VMM.

    >[AZURE.NOTE] Wenn VMM in einem Cluster bereitgestellt wird, und installieren Sie den Anbieter für erstmals auf einem aktiven Knoten installieren und schließen Sie die Installation im Tresor VMM-Server registrieren. Installieren Sie den Provider auf den anderen Knoten. Beachten Sie, dass wenn Sie Anbieter aktualisieren müssen Sie auf allen Knoten zu aktualisieren, da sie alle die gleiche Provider Version ausgeführt werden soll.

3. Das Installationsprogramm führt eine Prüfung Vorraussetzungen und Zustimmung zu VMM-Dienst, um Anbieter Setup zu starten. VMM-Dienst wird automatisch gestartet, wenn Setup abgeschlossen ist. Bei Installation auf einem Cluster VMM werden Sie aufgefordert, die Clusterrolle beenden.

4. **Microsoft** Update können Sie Updates anmelden. Diese Einstellung aktiviert Anbieter Updates gemäß Ihrem Microsoft Update installiert werden.

    ![Microsoft-Updates](./media/site-recovery-vmm-to-azure-classic/updates.png)


5.  Installationsort für den Anbieter soll ** <SystemDrive>\Programme\Microsoft System Center 2012 R2\Virtual Computer Manager\bin**. Klicken Sie auf **Installieren**.

    ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)

6. Nach der Installation des Anbieters klicken Sie auf **Registrieren** , Registrieren des Servers im Tresor.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)

9. **Depotnamen**überprüfen Sie, ob das Depot in dem der Server registriert werden. Klicken Sie auf *Weiter*.

    ![Server-Registrierung](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)

7. Geben Sie **Internetzugang** wie der Anbieter auf dem VMM-Server mit dem Internet verbunden. Wählen Sie **Verbindung mit vorhandenen Proxyeinstellungen** , die Internet-Verbindung auf dem Server konfigurierten Standardeinstellungen.

    ![Internet-Einstellungen](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

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

    ![LastPage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Nach der Registrierung wird von Azure Site Recovery Metadaten aus dem VMM-Server abgerufen. Der Server wird auf der Registerkarte **VMM-Server** auf der Seite **Server** im Depot angezeigt.

### <a name="command-line-installation"></a>Befehlszeile installation

Azure Site Recovery-Anbieter kann auch mithilfe der folgenden Befehlszeile installiert werden. Diese Methode kann verwendet werden, den Anbieter auf Server Core für Windows Server 2012 R2 installieren.

1. Den Key-Datei und Registrierung Anbieter in einen Ordner herunterladen Beispiel: C:\ASR.
2. System Center Virtual Machine Manager beenden
3. Extrahieren Sie aus ein Eingabeaufforderungsfenster mit erhöhten Rechten Anbieter Installer mit diesen Befehlen:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Installieren Sie den Anbieter wie folgt:

        C:\ASR> setupdr.exe /i

5. Registrieren Sie den Anbieter wie folgt:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Wo sind Parameter wie folgt:

 - **/Credentials** : Erforderlicher Parameter, der die Position angibt, in dem die wichtigsten Registrierungsdatei befindet,  
 - **/FriendlyName** : Erforderlicher Parameter für den Namen des Hostservers Hyper-V, die in Azure Site Recovery-Portal angezeigt.
 - **/EncryptionEnabled** : optionale Parameter angeben, wenn Sie Ihre virtuellen Computer in Azure (bei Rest Encryption) Verschlüsselung möchten. Der Dateiname sollte die Erweiterung **PFX** haben.
 - **/ProxyAddress** : Optionaler Parameter, der die Adresse des Proxyservers.
 - **/ProxyPort** : Optionaler Parameter, der den Port des Proxyservers.
 - **/proxyUsername** : Optionaler Parameter, der den Proxy-Benutzernamen angibt.
 - **/proxyPassword** : Optionaler Parameter, Proxy-Kennwort angibt.  


## <a name="step-4-create-an-azure-storage-account"></a>Schritt 4: Erstellen Sie ein Konto Azure-Speicher

1. Haben Sie ein Azure Storage-Konto klicken Sie auf **Azure Storage Konto hinzufügen** , ein Konto zu erstellen.
2. Erstellen Sie ein Konto mit Geo-Replikation aktiviert. Es muss im Bereich der Azure Site Recovery-Dienst und dieselbe Abonnement zugeordnet werden.

    ![Konto](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [AZURE.NOTE] [Migration von Speicherkonten](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist für die Bereitstellung von Site Recovery Storage-Konten nicht unterstützt.

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Schritt 5: Installieren Sie den Agent Azure Recovery Services

Installieren Sie den Agent Azure Recovery Services auf jedem Server Hyper-V-Host in VMM-Cloud.

1. Klicken Sie auf **Quick Start** > **Azure Site Recovery Services Agent herunterladen und installieren auf Hosts** die neueste Version der Datei für die Agenteninstallation zu erhalten.

    ![Recovery Services Agent installieren](./media/site-recovery-vmm-to-azure-classic/install-agent.png)

2. Führen Sie die Installationsdatei auf jedem Hyper-V-Host-Server.
3. Klicken Sie auf der Seite **Erforderliche Komponenten prüfen** auf **Weiter**. Alle fehlenden erforderlichen Komponenten werden installiert.

    ![Erforderliche Dienste Wiederherstellungsagenten](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)

4. Geben Sie auf der Seite **Installation Settings** soll den Agent installieren und wählen Sie den Speicherort des in dem Sicherungsmetadaten installiert werden an. Klicken Sie auf **Installieren**.
5. Nach Abschluss der Installation klicken Sie auf **Schließen** um den Assistenten abzuschließen.

    ![MARS-Agent registrieren](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Befehlszeile installation

Sie können auch Microsoft Azure Recovery Services Agent über die Befehlszeile mit dem folgenden Befehl:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Schritt 6: Konfigurieren von Cloud Einstellungen

Nach der VMM-Server registriert ist, können Sie Cloud-Schutz konfigurieren. Die Option **Synchronisieren von Daten mit der Cloud** wird aktiviert, wenn der Anbieter installiert, so dass alle Wolken auf dem VMM-Server auf der Registerkarte <b>Geschützte Elemente</b> im Depot angezeigt werden.

![Veröffentlichte Cloud](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Klicken Sie auf Schnellstart auf **Schutz für VMM-Clouds einrichten**.
2. Klicken Sie auf der Registerkarte **Geschützte Elemente** auf die Cloud zu konfigurieren, und wechseln Sie zur Registerkarte **Konfiguration** .
3. Wählen Sie im **Ziel** **Azure**.
4. Wählen Sie im **Speicherkonto** Azure-Speicher-Konto für die Replikation verwenden.
5. Auf **Off**festgelegt **verschlüsseln Daten gespeichert** . Diese Einstellung gibt an, dass Daten zwischen dem lokalen Standort und Azure repliziert verschlüsselt werden soll.
6. Übernehmen Sie die Standardeinstellung im Feld **Häufigkeit kopieren** . Dieser Wert gibt an, wie häufig Daten zwischen Quelle und Ziel synchronisiert werden soll.
7. Übernehmen Sie die Standardeinstellung **beibehalten Wiederherstellungspunkte für**. Mit einem Standardwert von NULL wird nur der letzte Wiederherstellungspunkt für eine primäre virtuelle Maschine auf einem Host Replikatserver gespeichert.
8. **Häufigkeit von anwendungskonsistenten Snapshots**lassen Sie die Standardeinstellung. Dieser Wert gibt an, wie oft Snapshots erstellt. Snapshots verwenden Volume Shadow Copy Service (VSS), um sicherzustellen, dass Anwendung in einem konsistenten Zustand der Momentaufnahme.  Wenn Sie einen Wert festlegen, stellen Sie sicher, dass es kleiner als die Anzahl der zusätzlichen Wiederherstellungspunkte konfigurieren.
9. Geben Sie **Startzeit für Replikation**beim anfänglichen Replikation von Daten in Azure beginnen soll. Die Zeitzone auf dem Hostserver Hyper-V verwendet werden. Wir empfehlen die erste Replikation Spitzenzeiten planen.

    ![Replikationseigenschaften Cloud](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Nach dem Speichern wird ein Auftrag erstellt werden und kann auf der Registerkarte **Aufträge** überwacht werden. Alle Hyper-V-Hostservern in VMM Quelle Cloud werden für die Replikation konfiguriert.

Nach dem Speichern lässt Cloud Settings auf die Registerkarte **Konfigurieren** . Ziel oder Ziel Speicherkonto ändern müssen Sie Entfernen der Cloud-Konfiguration und konfigurieren dann die Cloud. Beachten Sie, dass das Speicherkonto ändern die Änderung nur für virtuelle Computer angewendet wird, die für den Schutz aktiviert werden, nachdem das Speicherkonto geändert wurde. Vorhandenen virtuellen Computer werden nicht in das neue Speicherkonto migriert.

## <a name="step-7-configure-network-mapping"></a>Schritt 7: Konfigurieren von Netzwerkzuordnung
Bevor Sie beginnen Netzwerkübersicht überprüfen Sie, ob virtuelle Maschinen auf dem Quellserver VMM mit einem VM-Netzwerk verbunden sind. Darüber hinaus erstellen Sie Azure virtuelle Netzwerke. Beachten Sie, dass ein Azure Netzwerk mehrere VM-Netzwerke zugeordnet werden können.

1. Klicken Sie auf Schnellstart auf **Netzwerke zuordnen**.
2. Wählen Sie auf der Registerkarte **Netzwerke** **Quellspeicherort**VMM-Quellserver. Wählen Sie im **Zielverzeichnis** Azure.
3. Eine Liste der VM-Netzwerke dem VMM-Server in **Netzwerken** werden angezeigt. Dem Abonnement zugeordneten Azure-Netzwerke werden in **Ziel** Netzwerke angezeigt.
4. Wählen Sie VM Quellnetzwerk und klicken Sie auf **zuordnen**.
5. Wählen Sie auf der Seite **Wählen Sie ein Zielnetzwerk** Ziel Azure-Netzwerk zu verwenden.
6. Klicken Sie auf das Häkchen, um die Zuordnung abzuschließen.

    ![Replikationseigenschaften Cloud](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Nach dem Speichern ein Einzelvorgang zum Verfolgen der Zuordnung und können auf der Registerkarte Aufträge überwacht werden. Alle vorhandenen Replikat virtuellen Computern, die VM Quellnetzwerk entsprechen, auf Azure verbundene Netzwerke. Neue virtuelle Computer, die mit der VM Quellnetzwerk verbunden wird nach der Replikation mit dem zugeordneten Azure Netzwerk verbunden. Wenn Sie eine vorhandene Zuordnung mit einem neuen Netzwerk ändern, werden Replikat virtuelle Computer mit neuen Standardeinstellungen verbunden.

Beachten Sie, dass wenn das Zielnetzwerk mehrere Subnetze und von Subnetzen hat denselben Namen wie Subnetz auf dem virtuellen Quellcomputers befindet und VM Replikat an Ziel Subnetz nach einem Failover verbunden. Ist kein Ziel Subnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.

> [AZURE.NOTE] [Migration von Netzwerken](../resource-group-move-resources.md) über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements ist nicht für Netzwerke für die Bereitstellung von Site Recovery unterstützt.

## <a name="step-8-enable-protection-for-virtual-machines"></a>Schritt 8: Aktivieren des Schutzes für virtuelle Maschinen

Nach Servern, Wolken und Netzwerke richtig konfiguriert sind, können Sie Schutz für virtuelle Maschinen in der Cloud. Beachten Sie Folgendes:

- Virtuelle Maschinen müssen [Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)erfüllen.
- Zum Schutz der Betriebssysteme und aktivieren müssen Eigenschaften für den virtuellen Computer festgelegt werden. Beim Erstellen eines virtuellen Computers in VMM anhand einer Vorlage für virtuelle Computer können Sie die Eigenschaft festlegen. Diese Eigenschaften für vorhandene virtuelle Computer legen Sie auf den Registerkarten **Allgemein** und **Hardwarekonfiguration** Eigenschaften virtueller Computer. Wenn Sie diese Eigenschaften in VMM festlegen werden Sie in Azure Site Recovery-Portal konfiguriert.

    ![Erstellen virtueller Computer](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Ändern von Eigenschaften für virtuelle Computer](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)


1. Damit Schutz auf der Registerkarte **virtuelle Maschinen** in der Cloud, in dem sich der virtuelle Computer befindet, klicken Sie auf **Schutz** > **virtuelle Computer hinzufügen**.
2. Wählen Sie aus der Liste der virtuellen Computer in die Cloud zu schützen.

    ![VM-Schutz aktivieren](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Fortschritt der Aktion **Schutz aktivieren** der Registerkarte **Aufträge** mit den anfängliche Replikation. Nach **Abschließen der** Schutzauftrag führt kann der virtuelle Computer Failover. Schutz aktiviert und virtuellen Computern repliziert werden, werden Sie in Azure angezeigt.


    ![Virtual Machine Schutzauftrag](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

3. Überprüfen Sie die Maschineneigenschaften der virtuellen und nach Bedarf ändern.

    ![Virtuelle Computer überprüfen](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)


4. Auf der Registerkarte **Konfigurieren** die Eigenschaften können folgende Netzwerkeigenschaften geändert werden.





- **Anzahl der Netzwerkadapter auf dem virtuellen Zielcomputer** - Anzahl Netzwerkadapter wird durch die Größe bestimmt der virtuelle Zielcomputer angeben. Überprüfen Sie [virtuellen Größe Spezifikationen](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) für die Anzahl der Adapter von der Größe des virtuellen Computers unterstützt. Ändern Sie die Größe für einen virtuellen Computer zu sichern, wird die Anzahl der Netzwerkadapter ändern Wenn **Konfigurieren** Seite das nächste Mal öffnen. Die Anzahl der Netzwerkadapter Ziel virtueller Computer ist die minimale Anzahl der Netzwerkadapter auf virtuellen Quellcomputers und die maximale Anzahl der Netzwerkadapter von der Größe des virtuellen Computers ausgewählt, wie folgt unterstützt:

    - Wenn die Anzahl der Netzwerkadapter auf dem Quellcomputer kleiner oder gleich der Anzahl der Netzwerkadapter für die Zielgröße Computer zulässig ist, haben das Ziel die gleiche Anzahl von Adaptern als Quelle.
    - Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputers die Höchstzahl überschreitet für die Größe und die maximale Ziel verwendet werden.
    - Beispielsweise wenn ein Quellcomputer verfügt über zwei Netzwerkadapter und die Zielgröße Computer vier unterstützt haben dem Zielcomputer zwei Adapter. Wenn der Quellcomputer hat zwei Adapter unterstützten Zielgröße unterstützt jedoch nur eine haben dem Zielcomputer nur eine Netzwerkkarte.    

- Netzwerkzuordnung des Netzwerks des virtuellen Quellcomputers **Netzwerk der virtuelle Zielcomputer** - Netzwerk, den virtuellen Computer Verbindung, bestimmt. Wenn virtuellen Quellcomputers mehr als einen Netzwerkadapter hat und Netzwerken zu anderen Netzwerken Ziel zugeordnet werden, müssen Sie zwischen den Netzwerken Ziel wählen.
- **Subnetz von jedem Netzwerkadapter** - für jeden Netzwerkadapter im Subnetz, wählen die fehlgeschlagene über virtuelle Maschine zu verbinden.
- **Ziel-IP-Adresse** : Wenn der Netzwerkadapter des virtuellen Quellcomputers konfiguriert ist eine statische IP-Adresse und die IP-Adresse für den virtuellen Zielcomputer bereitstellen kann. Verwenden Sie dieses Feature nach einem Failover die IP-Adresse des virtuellen Quellcomputers beibehalten. Sofern keine IP-Adresse erhält alle verfügbaren IP-Adressen bei Failover an den Netzwerkadapter. Ziel-IP-Adresse angegeben, wird bereits von einem anderen virtuellen Computer in Azure verwendet jedoch fehl Failover.  

    ![Netzwerkeigenschaften ändern](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

>[AZURE.NOTE] Virtuelle Linux-Computer mit statischen IP-Adresse werden nicht unterstützt.

## <a name="test-the-deployment"></a>Testen der Bereitstellung

Zum Testen der Bereitstellung führen Sie ein testfailover für einen einzelnen virtuellen Computer oder erstellen Sie einen Wiederherstellungsplan mehrere virtuelle Computer aus und führen Sie einen testfailover für den Plan.  

Testfailover simuliert Failover- und Mechanismus in einem isolierten Netzwerk. Beachten Sie Folgendes:

- Wenn Sie den virtuellen Computer in Azure mithilfe von Remotedesktop nach dem Failover verbinden möchten, aktivieren Sie Remotedesktopverbindung auf dem virtuellen Computer vor dem Ausführen von testfailover.
- Nach einem Failover verwenden Sie eine öffentliche IP-Adresse der virtuellen Computer in Azure mithilfe von Remotedesktop Verbindung. Wenn Sie dies tun möchten, sicherzustellen Sie, dass Sie Domänenrichtlinien nicht, die Verbindung mit einem virtuellen Computer eine öffentliche Adresse verhindern.

>[AZURE.NOTE] Um die beste Leistung bei Failover in Azure tun, daß der Azure-Agent auf dem geschützten Computer installiert haben. Dies hilft schneller starten und Diagnose bei Problemen hilft. Linux-Agent kann finden [hier](https://github.com/Azure/WALinuxAgent) - und Windows Agent finden Sie [hier](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

1. Fügen Sie auf der Registerkarte **Wiederherstellungspläne** einen neuen Plan. Geben Sie einen Namen **VMM** **Herkunftsart**und VMM Quellserver in **Quelle**und Ziel werden Azure.

    ![Wiederherstellungsplan erstellen](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)

2. Wählen Sie auf **virtuelle Computer auswählen** virtuellen Maschinen den Wiederherstellungsplan hinzu. Diese virtuellen Computer werden die Standardgruppe Recovery Plan hinzugefügt – Gruppe 1. Bis zu 100 VMs in einzelnen Wiederherstellungsplan getestet worden.

- Möchten Sie die Eigenschaften vor dem Hinzufügen des Plans überprüfen, klicken Sie auf den virtuellen Computer auf der Eigenschaftenseite der Cloud in der enthalten. Sie können auch die Eigenschaften in der VMM-Verwaltungskonsole konfigurieren.
- Alle virtuellen Computer, die angezeigt werden wurden für Schutz aktiviert. Die Liste enthält sowohl virtuelle Computer, die für den Schutz und die erste Replikation abgeschlossen und für Schutz aktiviert die anfängliche Replikation bis aktiviert sind. Nur virtuelle Computer mit der anfänglichen Replikation abgeschlossen können als Teil eines Recovery-Plans Failover.

    ![Wiederherstellungsplan erstellen](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Nachdem ein Wiederherstellungsplan, es erstellt wurde wird in der Registerkarte **Wiederherstellungspläne** angezeigt. Sie können auch Wiederherstellungsplan zum Automatisieren von Aktionen während eines Failovers [Azure Automatisierung Runbooks](site-recovery-runbook-automation.md) hinzufügen.

### <a name="run-a-test-failover"></a>Führen Sie ein testfailover

Es gibt zwei Möglichkeiten ein testfailover in Azure ausgeführt.

- **Failover ohne eine Azure testen**– diese Art von testfailover überprüft, dass der virtuelle Computer korrekt in Azure kommt. Der virtuelle Computer wird nicht nach einem Failover Azure Netzwerk verbunden.
- **Failover mit einer Azure testen**– dieses Typs Failover überprüft kommt die gesamten Umgebung wie erwartet und Failover der virtuellen Computer mit dem angegebenen Ziel Azure-Netzwerk verbunden. Für die Behandlung von Subnetz wird für Test Failover im Subnetz des virtuellen Testcomputers basierend auf dem Subnetz des virtuellen Computers Replikat herausgefunden werden. Dies unterscheidet reguläre Replikation, wenn Subnetz ein Replikat virtuellen Computer im Subnetz des virtuellen Quellcomputers basiert.

Möchten Sie ein testfailover für einen virtuellen Computer ohne Azure Zielnetzwerk Azure Schutz aktiviert ausführen müssen Sie nicht alles vorbereiten. Führen Sie ein testfailover mit einer Azure-Netzwerk müssen Sie ein neues Azure-Netzwerk, das isoliert wurde von Azure Produktionsnetzwerk (Standardverhalten beim Erstellen eines neuen Netzwerks in Azure) erstellen. Sehen Sie sich weitere Einzelheiten [ein testfailover ausgeführt](site-recovery-failover.md#run-a-test-failover) .


Sie müssen außerdem die Infrastruktur für den replizierten virtuellen Computer wie erwartet. Beispielsweise ein virtuellen Computer mit dem Domänencontroller und DNS in Azure mithilfe von Azure Site Recovery repliziert werden und im Testnetzwerk mit Failover Testen erstellt werden. Sehen Sie [Failover Aspekte für active Directory zu testen](site-recovery-active-directory.md#considerations-for-test-failover) Einzelheiten.

Ein Test-Failover führen Folgendes ausführen:

1. Wählen Sie auf der Registerkarte **Wiederherstellungspläne** den Plan, und klicken Sie auf **Test-Failover**.
2. Wählen Sie auf der Seite **Test-Failover bestätigen** **None** oder ein bestimmtes Azure-Netzwerk.  Beachten Sie, dass keine Auswahl testfailover Überprüfen der virtuellen Maschine auf ordnungsgemäß repliziert Azure jedoch Netzwerkkonfiguration Replikation aktivieren nicht.

    ![Kein Netzwerk](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)

3. Wenn für die Cloud-Verschlüsselung aktiviert ist, wählen Sie **Schlüssel** das Zertifikat aus, das während der Installation des Anbieters auf dem VMM-Server ausgestellt wurde, als Sie auf die Option zum Aktivieren der Verschlüsselung für eine.
4. Auf der Registerkarte **Aufträge** können Sie Failover verfolgen. Auch werden virtuelle Computer Test Replikat in Azure-Portal angezeigt. Wenn Sie aus dem lokalen Netzwerk Zugriff auf virtuelle Computer eingerichtet sind, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer initiieren.
5. Erreicht das Failover die Testphase **abgeschlossen** , klicken Sie auf testfailover Ende **Vollständig testen** . Sie können einen Drilldown **die Registerkarte Failover Fortschritt und Status nachzuverfolgen und Aktionen auszuführen, die erforderlich sind** .
6. Nach dem Failover können zu virtuellen Test Replikat in Azure-Portal Sie. Wenn Sie aus dem lokalen Netzwerk Zugriff auf virtuelle Computer eingerichtet sind, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer initiieren. Folgendermaßen Sie vor:

    1. Stellen Sie sicher, dass die virtuellen Computer erfolgreich starten.
    2. Wenn Sie den virtuellen Computer in Azure mithilfe von Remotedesktop nach dem Failover verbinden möchten, aktivieren Sie Remotedesktopverbindung auf dem virtuellen Computer vor dem Ausführen von testfailover. Sie müssen auch einen RDP-Endpunkt auf dem virtuellen Computer hinzufügen. Sie können eine [Azure Automatisierung Runbooks](site-recovery-runbook-automation.md) dazu nutzen.
    3. Nach dem Failover sicherzustellen, eine öffentliche IP-Adresse der virtuellen Computer in Azure mit dem Remotedesktop Verbindung mit müssen Domänenrichtlinien nicht, die Verbindung mit einem virtuellen Computer eine öffentliche Adresse verhindern.

7.  Nach Abschluss der Tests folgendermaßen Sie vor:
    - Klicken Sie auf **testfailover ist abgeschlossen**. Bereinigen der Umgebung automatisch ausschalten und Löschen von virtuellen Testcomputer.
    - Klicken Sie auf **Notizen** zu speichern Anmerkungen testfailover zugeordnet.

>

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum [Einrichten von Recovery-Plänen](site-recovery-create-recovery-plans.md) und [Failover](site-recovery-failover.md).
