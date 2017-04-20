<properties
    pageTitle="Replizieren von Hyper-V virtuelle Computer in VMM-Clouds an einem sekundären Standort VMM | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie Hyper-V virtuelle Computer in VMM-Clouds in VMM-Sekundärstandort mit Azure Site Recovery replizieren."
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

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Replizieren Sie Hyper-V virtuelle Computer in VMM-Clouds an einem sekundären Standort VMM

> [AZURE.SELECTOR]
- [Azure-portal](site-recovery-vmm-to-vmm.md)
- [Verwaltungsportal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - Ressourcen-Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Azure Site Recovery-Dienst trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern und Servern orchestriert. Computer können Azure oder einem lokalen sekundären Rechenzentrum repliziert werden. Finden Sie einen schnellen Überblick über [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie Hyper-V virtuelle Computer in Hyper-V-Host-Server repliziert, die in VMM-Clouds sekundäre Azure Site Recovery mit VMM-Website verwaltet werden.

Der Artikel enthält Komponenten, veranschaulicht Site Recovery Depot einrichten, installieren Sie das Azure Site Recovery auf Zielserver VMM, registrieren Sie den Server im Tresor, Schutz für VMM-Clouds konfigurieren und Schutz für Hyper-V virtuelle Computer aktivieren. Testen Sie das Failover sicherzustellen, dass alles erwartungsgemäß beenden.

Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Architektur

Die folgende Abbildung zeigt verschiedene Kommunikationskanäle und von Azure Site Recovery für die Orchestrierung und Replikation verwendeten ports

![E2E-Topologie](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie diese erforderlichen Komponenten verfügen:

**Erforderliche Komponenten** | **Details**
--- | ---
**Azure**| Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Site Recovery Preise.
**VMM** | Sie benötigen mindestens eine VMM-Server.<br/><br/>VMM-Server sollte mindestens ausführen System Center 2012 SP1 mit den neuesten kumulativen Updates.<br/><br/>Wenn Sie mit einem einzigen VMM-Server einrichten möchten, benötigen Sie mindestens zwei Wolken auf dem Server konfiguriert.<br/><br/>Wenn Sie mit zwei VMM-Server bereitstellen möchten, müssen jeden Server mindestens Cloud konfiguriert auf dem primären VMM-Server zu schützen und eine Wolke konfiguriert auf dem sekundären VMM-Server für Schutz und Recovery verwenden möchten<br/><br/>VMM-Clouds müssen das Hyper-V-Funktion Profil.<br/><br/>Die Quelle Cloud, den zu schützenden enthalten eine oder mehrere VMM-Hostgruppen.<br/><br/>Weitere Informationen zum Einrichten von VMM-Clouds in [Exemplarische Vorgehensweise: Erstellen von privaten Clouds mit System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) Keith Mayers Blog.
**Hyper-V** | Sie benötigen mindestens Hyper-V-Hostservern in den primären und sekundären VMM-Hostgruppen und eine oder mehrere virtuelle Computer auf jedem Hyper-V-Host-Server.<br/><br/>Host- und Zielsystem Hyper-V Server laufen mindestens Windows Server 2012 mit Hyper-V-Rolle und die neuesten Updates installiert haben.<br/><br/>Alle VMs zu schützenden mit Hyper-V-Server muss in der VMM-Cloud befinden.<br/><br/>Wenn Sie Hyper-V in einem Cluster ausführen, beachten Sie, dass haben Sie einen statischen IP-Adresse-basierten Cluster, Cluster-Broker nicht automatisch erstellt wird. Sie müssen Cluster Broker manuell konfigurieren. [Erfahren Sie mehr](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) im Blogeintrag Andreas Finn.
**Netzwerkzuordnung** | Sie können Netzwerkzuordnung, um sicherzustellen, dass replizierte virtuelle Computer optimal auf sekundäre Hyper-V-Host-Server nach einem Failover und entsprechende VM-Netzwerken herstellen können. Wenn Sie Netzwerkzuordnung nicht konfigurieren, wird nicht Replikat VMs nach einem Failover mit keinem Netzwerk verbunden.<br/><br/>Zum Einrichten der Netzwerkzuordnung während der Bereitstellung sicherstellen Sie, dass die virtuellen Computer auf der Hyper-V-Host mit VMM VM-Netzwerk verbunden sind. Dieses Netzwerk verknüpft werden soll, um ein logisches Netzwerk, die mit Cloud. < Br /<br/>Zielcloud auf dem sekundären VMM-Server, mit denen Sie für die Wiederherstellung muss ein entsprechende VM-Netzwerk konfiguriert, und es wiederum sollte werden mit entsprechenden logischen Netzwerk zielcloud zugeordnet.<br/><br/>[Erfahren Sie mehr](site-recovery-network-mapping.md) über Netzwerkzuordnung.
**Speicher-Zuordnung** | Beim Replizieren einer virtuellen Maschine auf einem Hyper-V-Host Quellserver auf einem Hyper-V-Host Zielserver ist replizierte Daten am Standardspeicherort gespeichert, die für den Zielhost Hyper-V in Hyper-V-Manager angezeigt wird. Mehr Kontrolle über die replizierten Daten können Sie Speicher zuordnen<br/><br/> Um Speicher-Zuordnung zu konfigurieren, müssen Sie Speicher Klassifikationen der Quelle und Ziel VMM-Server vor der Bereitstellung. [Erfahren Sie mehr](site-recovery-storage-mapping.md).


## <a name="step-1-create-a-site-recovery-vault"></a>Schritt 1: Erstellen eines Depots Site Recovery

1. Melden Sie sich im [Verwaltungsportal](https://portal.azure.com) von VMM-Server zu registrieren.

2. Erweitern Sie **Data Services** > **Recovery Services** und **Site Recovery Depot**auf.

3. Klicken Sie auf **neue** > **schnell erstellen**.

4. Geben Sie im Feld **Name**einen Anzeigenamen zu Tresor.

5. Wählen Sie im **Bereich** die geografische Region für das Depot. Überprüfen Sie anzeigen unterstützte Regionen geografische Verfügbarkeit in [Azure Site Recovery Preisdetails](http://go.microsoft.com/fwlink/?LinkId=389880)

6. Klicken Sie auf **Create Tresor**.

    ![Depot erstellen](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Überprüfen Sie in der Statusleiste das Depot erstellt wurde. Das Depot wird als **aktiv** auf Recovery Services aufgeführt.

## <a name="step-2-generate-a-vault-registration-key"></a>Schritt 2: Erstellen Sie einen Vault-Registrierungsschlüssel

Erstellen Sie einen Registrierungsschlüssel im Tresor. Azure Site Recovery Anbieter herunterladen und installieren auf dem VMM-Server verwenden Sie diesen Schlüssel, das Depot VMM-Server anmelden.

1. Der **Recovery-** Seite klicken Sie auf das Depot zum Öffnen der Seite Quick Start. Quick-Start kann auch jederzeit auf das Symbol geöffnet werden.

    ![Schnellstart-Symbol](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)

2. Wählen Sie in der Dropdownliste **zwischen zwei lokalen VMM**.
3. Klicken Sie auf **Schlüssel generieren Registrierungsdatei**in **VMM-Server Vorbereiten**. Die Schlüsseldatei wird automatisch generiert und ist 5 Tage nach der Erstellung gültig. Wenn Sie nicht das Azure-Portal aus dem VMM-Server zugreifen, müssen Sie diese Datei auf den Server kopieren.

    ![Registrierungsschlüssel](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Schritt 3: Installieren Sie das Azure Site Recovery

4. Die **Quick Start** **Vorbereiten VMM-Server**klicken Sie auf **Download von Microsoft Azure Site Recovery Provider auf VMM-Server** die neueste Version der Installationsdatei Anbieter.

2. Führen Sie diese Datei auf dem Quellserver VMM.

    >[AZURE.NOTE] Wenn VMM in einem Cluster bereitgestellt wird, und installieren Sie den Anbieter für erstmals auf einem aktiven Knoten installieren und schließen Sie die Installation im Tresor VMM-Server registrieren. Installieren Sie den Provider auf den anderen Knoten. Beachten Sie, dass wenn Sie Anbieter aktualisieren müssen auf allen Knoten aktualisiert werden, da sie alle die gleiche Provider Version ausgeführt werden soll.

3. Das Installationsprogramm hat einige **Überprüfen vor** und fordert Berechtigung zu VMM-Dienst, um Anbieter Setup zu starten. VMM-Dienst wird automatisch gestartet, wenn Setup abgeschlossen ist. Bei Installation auf einem Cluster VMM werden Sie aufgefordert, die Clusterrolle beenden.

4. **Microsoft** Update können Sie Updates anmelden. Diese Einstellung aktiviert Anbieter Updates gemäß Ihrem Microsoft Update installiert werden.

    ![Microsoft-Updates](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)

5. Installationsort soll ** <SystemDrive>\Programme\Microsoft System Center 2012 R2\Virtual Computer Manager\bin**. Klicken Sie auf die Schaltfläche installieren, installieren Sie den Anbieter.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)

6. Nach der Installation des Anbieters klicken Sie auf **Registrieren** , Registrieren des Servers im Tresor.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
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

13.  Klicken Sie auf **Weiter** um den Vorgang abzuschließen. Nach der Registrierung wird von Azure Site Recovery Metadaten aus dem VMM-Server abgerufen. Der Server wird in **VMM-Server** > **Server** im Tresor.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Befehlszeile installation

Azure Site Recovery-Anbieter kann auch über die Befehlszeile installiert werden. Diese Methode kann verwendet werden, den Anbieter auf Server CORE für Windows Server 2012 R2 installieren.

1. Den Key-Datei und Registrierung Anbieter in einen Ordner herunterladen Beispielsweise C:\ASR.
2. System Center Virtual Machine Manager-Dienst beenden
3. Extrahieren des Installers Anbieter diese Befehle von einer Befehlszeile mit **Administratorrechten** ausgeführt:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Installieren des Providers ausgeführt:

        C:\ASR> setupdr.exe /i

5. Registrieren Sie den Anbieter mit:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Die Parameter lauten:

 - **/Credentials**: Erforderlicher Parameter, der die Position angibt, in dem die wichtigsten Registrierungsdatei befindet,  
 - **/FriendlyName**: Erforderlicher Parameter für den Namen des Hostservers Hyper-V, die in Azure Site Recovery-Portal angezeigt.
 - **/EncryptionEnabled**: optionale Parameter, die Sie in VMM Azure Szenario verwenden, wenn Verschlüsselung Ihrer virtuellen Maschinen auf ruhende in Azure. Stellen Sie sicher, dass der Name der Datei die Erweiterung **PFX** hat.
 - **/ProxyAddress**: Optionaler Parameter, der die Adresse des Proxyservers.
 - **/ProxyPort**: Optionaler Parameter, der den Port des Proxyservers.
 - **/proxyUsername**: Optionaler Parameter, der den Proxy-Benutzernamen gibt (wenn Proxy eine Authentifizierung erfordert).
 - **/proxyPassword**: Optionaler Parameter, der gibt das Kennwort für die Authentifizierung am Proxyserver (wenn der Proxyserver Authentifizierung erfordert).  

## <a name="step-4-configure-cloud-protection-settings"></a>Schritt 4: Konfigurieren von Cloud Einstellungen

Nach dem VMM-Server registriert sind, können Sie Cloud-Schutz konfigurieren. Wenn der Provider installiert, aktiviert die Option **Synchronisieren von Daten mit der Cloud** werden alle Wolken auf dem VMM-Server auf der Registerkarte **Geschützte Elemente** im Depot angezeigt. Wenn Sie keine bestimmte Cloud mit Azure Site Recovery auf der Registerkarte **Allgemein** der Eigenschaftenseite Cloud in der VMM-Konsole synchronisieren.

![Veröffentlichte Cloud](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Klicken Sie auf Schnellstart auf **Schutz für VMM-Clouds einrichten**.
2. Wählen Sie auf der Registerkarte **VMM-Clouds** zu konfigurieren und auf der Registerkarte **Konfiguration** zur Cloud.
3. Wählen Sie im **Ziel** **VMM**.
4. Wählen Sie **Zielort**vor-Ort-VMM-Server, der die Cloud verwaltet, die für die Wiederherstellung verwenden möchten.
4. Wählen Sie im **zielcloud**zielcloud für das Failover von virtuellen Computern in der Cloud Quelle verwenden möchten. Beachten Sie Folgendes:

    - Wir empfehlen eine zielwolke wählen, die Wiederherstellung für die virtuellen Computer erfüllt, die Sie schützen.
    - Eine Wolke kann nur zu einer einzigen Cloud gehören – eine primäre oder eine zielwolke.

5. Im **Kopieren Häufigkeit**angeben, wie häufig Daten zwischen Quelle und Ziel 5he synchronisiert werden soll. Hinweis: Diese Einstellung ist nur relevant, wenn Hyper-V-Hosts Windows Server 2012 R2 ausgeführt wird. Für andere Server dient als Standardeinstellung 5 Minuten.
6. **Zusätzliche Wiederherstellungspunkte**geben an, ob zusätzliche Wiederherstellungspunkte erstellen. Der Standardwert 0 (null) gibt an, dass nur der letzte Wiederherstellungspunkt für eine primäre virtuelle Maschine auf einem Host Replikatserver gespeichert wird. Beachten Sie, dass mehrere Wiederherstellungspunkte aktivieren zusätzlichen Speicher Snapshots verlangt, die bei jedem Wiederherstellungspunkt gespeichert sind. Standardmäßig werden Wiederherstellungspunkte pro Stunde erstellt, sodass jedem Wiederherstellungspunkt eine Stunde lang Daten enthält. Des virtuellen Computers in der VMM-Konsole zugewiesene Recovery Point Wert sollte kleiner als der Wert nicht, die in der Wiederherstellungskonsole von Azure Website zuweisen.
7. **Häufigkeit von anwendungskonsistenten Snapshots**sollen Sie anwendungskonsistente Snapshots erstellen. Hyper-V verwendet zwei Arten von Snapshots – eines Standardsnapshots, die ein inkrementelles Snapshots der gesamte virtuelle Computer bereitstellt und ein anwendungskonsistente Snapshots, die einen Point-in-Time Snapshot der Anwendungsdaten innerhalb des virtuellen Computers annimmt. Anwendungskonsistente Snapshots verwenden Volume Shadow Copy Service (VSS), um sicherzustellen, dass Anwendung in einem konsistenten Zustand der Momentaufnahme. Beachten Sie, dass aktivieren anwendungskonsistente Snapshots beeinträchtigen sie die Leistung der Anwendung in Quelle virtuellen Maschinen ausgeführt. Sicherstellen Sie, dass der Wert kleiner als die Anzahl der zusätzlichen Wiederherstellungspunkte, die Sie konfigurieren.

    ![Schutz konfigurieren](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)

8. **Datenübertragung Komprimierung**geben an, ob replizierte Daten werden komprimiert werden.
9. Geben Sie **Authentifizierung**wie Datenverkehr zwischen der primären und Recovery Hyper-V-Host-Server authentifiziert wird. Wählen Sie HTTPS, wenn Sie eine funktionierende Kerberos-Umgebung konfiguriert haben. Azure Site Recovery konfiguriert automatisch Zertifikate für die Authentifizierung mit HTTPS. Es ist keine manuelle Konfiguration erforderlich. Wenn Sie Kerberos aktivieren, wird ein Kerberos-Ticket für die gegenseitige Authentifizierung der Hostserver verwendet. Standardmäßig wird Anschluss 8083 und 8084 (für Zertifikate) in der Windows-Firewall auf Hyper-V-Host-Server geöffnet. Beachten Sie, dass diese Einstellung nur für Hyper-V-Host-Server auf Windows Server 2012 R2 ausgeführt.
10. Ändern Sie **Port**die Portnummer auf der Quell- und Ziel-Host-Computern für den Replikationsverkehr überwachen. Beispielsweise können Sie die Einstellung ändern, Quality of Service (QoS) Netzwerkbandbreite für den Replikationsverkehr Einschränkung angewendet werden soll. Überprüfen, ob der Port von einer anderen Anwendung verwendet wird und in der die Firewall geöffnet ist.
11. **Replikationsmethode**angegeben Sie wie die erste Replikation der Daten aus Quelle Ziel erfolgt vor normalen Replikation:

    - **Netzwerk**– Kopieren von Daten über das Netzwerk kann zeitaufwendig und ressourcenintensiv. Es wird empfohlen, diese Option verwenden, enthält die Cloud virtuelle Maschinen mit relativ kleinen virtuellen Festplatten und der primäre Standort über eine Verbindung mit hoher Bandbreite an den sekundären Standort verbunden ist. Sie können angeben, dass die Kopie sofort, oder wählen Sie eine Uhrzeit. Netzwerkreplikation verwenden, sollten Sie Spitzenzeiten planen.
    - **Offline**– diese Methode gibt an, dass die erste Replikation mit externen Medien durchgeführt wird. Es empfiehlt Abbau Netzwerk-Performance und entfernten Standorten vermeiden soll. Dazu geben Sie den Exportspeicherort Quelle Cloud und Importspeicherort Cloud Ziel. Wenn Sie Schutz für einen virtuellen Computer aktivieren, wird die virtuelle Festplatte an angegebenen Export kopiert. Sie an den Zielstandort gesendet und an Import kopieren. Die Anwendung kopiert die importierte Informationen Replikat virtuellen Maschinen.

12. Wählen Sie **Löschen Replikat Virtual Machine** , VM Replikat gelöscht werden soll, wenn Sie den virtuellen Computer durch Auswahl der Option **Löschen Schutz für den virtuellen Computer** auf der Registerkarte virtuelle Maschinen Cloud-Eigenschaften Schutz beenden sollen. Diese Einstellung aktiviert, wenn Schutz deaktivieren die Virtual Machine von Azure Site Recovery entfernt Site Recovery Einstellungen des virtuellen Computers in der VMM-Verwaltungskonsole entfernt und das Replikat gelöscht.

    ![Schutz konfigurieren](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Nach dem Speichern wird ein Auftrag erstellt werden und kann auf der Registerkarte **Aufträge** überwacht werden. Alle Hyper-V-Hostservern in VMM Quelle Cloud werden für die Replikation konfiguriert. Cloud-Optionen können auf der Registerkarte **Konfigurieren** geändert werden. Möchten Sie die Ziel-Speicherort oder Ziel Cloud ändern müssen die Cloud-Konfiguration entfernen und konfigurieren die Cloud.

### <a name="prepare-for-offline-initial-replication"></a>Offline Erstreplikation vorbereiten

Sie müssen die folgenden Aktionen für die erste Replikation offline vorbereitet:

- Auf dem Quellserver werden Sie einen Pfad angeben, aus dem Datenexport stattfinden soll. Vollzugriff für NTFS- und Freigabeberechtigungen Berechtigungen mit dem VMM-Dienst auf der Exportpfad. Auf dem Zielserver werden Sie einen Pfad angeben, aus dem Datenimport stattfindet. Weisen Sie Berechtigungen auf diese Importpfad.
- Freigegeben Einfuhr- oder Pfad weisen Sie Administrator, Hauptbenutzer, Druck-Operator oder Serveroperatoren Gruppenmitgliedschaft für VMM-Dienstkonto auf dem Remotecomputer, auf dem die freigegebenen befindet zu.
- Verwenden Sie Ausführen als Konten hinzufügen Hosts, importieren und Exportieren von Pfaden, weisen lesen und Schreibberechtigungen für die Konten ausführen in VMM.
- Import und Export Aktien sollte nicht auf jedem Computer als ein Hyper-V-Host-Server befinden da Loopback Konfiguration von Hyper-V nicht unterstützt wird.
- In Active Directory eingeschränkte Hostserver mit virtuellen Computern zu schützen, aktivieren und konfigurieren Sie auf jedem Hyper-V Delegierung remote Computer vertrauen auf denen Import und Export Pfade wie folgt befinden:
    1. Öffnen Sie auf dem Domänencontroller **Active Directory-Benutzer und -Computer**.
    2. Klicken Sie in der Konsolenstruktur auf **Domänenname** > **Computer**.
    3. Maustaste auf Hyper-V-Host Server > **Eigenschaften**.
    4. Klicken Sie auf der Registerkarte **Delegierung** auf T**Rost Computer bei Delegierungen angegebener Dienste**.
    5. Klicken Sie auf **Beliebiges Authentifizierungsprotokoll verwenden**.
    6. Klicken Sie auf **Hinzufügen** > **Benutzer und Computer**.
    7. Geben Sie den Namen des Computers, der den Exportpfad hostet > **OK**. Die Liste der verfügbaren Dienste, halten Sie die STRG-Taste und **Cifs**auf > **OK**. Wiederholen Sie für den Namen des Computers, den Importpfad befindet. Wiederholen diesen Sie Vorgang für weitere Hyper-V-Host-Server.

## <a name="step-5-configure-network-mapping"></a>Schritt 5: Konfigurieren von Netzwerkzuordnung
1. Klicken Sie auf Schnellstart auf **Netzwerke zuordnen**.
2. Wählen Sie die VMM-Quellserver Netzwerken zugeordnet werden soll und Ziel VMM-Server, der die Netzwerken zugeordnet werden. Die Quelle und ihre zugeordneten Zielbenutzer-Netzwerken werden angezeigt. Ein leerer Wert ist für Netzwerke angezeigt, die derzeit nicht zugeordnet.
3. Wählen Sie ein Netzwerk im **Netzwerk auf** > **zuordnen**. Der Dienst erkennt die VM-Netzwerke auf dem Zielserver und angezeigt. Klicken Sie auf das Informationssymbol neben den Quell- und Netzwerk an die Subnetze für jedes Netzwerk.

    ![Konfigurieren von Netzwerkzuordnung](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)

4. Wählen Sie im Dialogfeld die VM-Netzwerke von VMM-Zielserver.

    ![Wählen Sie ein Zielnetzwerk](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)

5. Wenn Sie ein Netz auswählen, werden geschützten Wolken, mit dem das Quellnetzwerk angezeigt. Verfügbare Zielnetzwerke Schutz Wolken zugeordnet sind, werden ebenfalls angezeigt. Wir empfehlen Sie Zielnetzwerk auswählen, die alle Wolken zur Verwendung für den Schutz. Oder Sie gehen der VMM-Server auch die Cloud Eigenschaften hinzufügen das logische Netzwerk für Vm-Netzwerk, das Sie auswählen möchten.
6. Klicken Sie auf das Häkchen, um die Zuordnung abzuschließen. Einzelvorgang Zuordnung verfolgen. Sie finden sie auf der Registerkarte **Aufträge** .


## <a name="step-6-configure-storage-mapping"></a>Schritt 6: Konfigurieren von Speicher-Zuordnung
Beim Replizieren einer virtuellen Maschine auf einem Hyper-V-Host Quellserver auf einem Hyper-V-Host Zielserver ist replizierte Daten am Standardspeicherort gespeichert, die für den Zielhost Hyper-V in Hyper-V-Manager angezeigt wird. Mehr Kontrolle über die replizierten Daten können Sie Speicher-Mappings wie folgt konfigurieren:



1. Definieren Sie Speicher-Klassifikationen für Quelle und Ziel VMM-Server. [Erfahren Sie mehr](https://technet.microsoft.com/library/gg610685.aspx). Klassifikationen muss Hyper-V-Hostservern in Quell- und Wolken zur Verfügung. Klassifikationen müssen dieselbe Art von Speicher haben. Beispielsweise können Sie eine Klassifikation Quelle zuordnen mit SMB-Freigaben für eine zielklassifizierung, die CSVs enthält.
2. Nach Klassifikationen sind können Sie die Zuordnung erstellen. Klicken Sie dazu auf der Seite **Quick Start** > **Speicher zuordnen**.
3. Klicken Sie auf die Registerkarte **Speicher** > **Speicher Klassifikationen zuordnen**.
4. Wählen Sie auf der Registerkarte **Speicher Klassifikationen zuordnen** Klassifikationen für die Quelle und VMM Server. Speichern.

    ![Wählen Sie ein Zielnetzwerk](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)


## <a name="step-7-enable-virtual-machine-protection"></a>Schritt 7: VM-Schutz aktivieren
Nach Servern, Wolken und Netzwerke richtig konfiguriert sind, können Sie Schutz für virtuelle Maschinen in der Cloud.

1. Klicken Sie auf der Registerkarte **VMs** in der Cloud, in dem der virtuelle Computer befindet sich, auf **Schutz aktivieren** > **virtuelle Computer hinzufügen**.
2. Wählen Sie aus der Liste der virtuellen Computer in die Cloud zu schützen.

    ![VM-Schutz aktivieren](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)

3. Fortschritt der Aktion Schutz aktivieren der Registerkarte **Aufträge** mit den anfängliche Replikation. Nach Abschließen der Schutzauftrag führt kann der virtuelle Computer Failover. Schutz aktiviert und virtuellen Computern repliziert werden, werden Sie in Azure angezeigt.

    ![Virtual Machine Schutzauftrag](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

>[AZURE.NOTE] Sie können auch Schutz für virtuelle Computer in VMM-Konsole. Klicken Sie auf der Symbolleiste der Registerkarte **Azure Site Recovery** in den Eigenschaften des virtuellen Computers auf **Schutz aktivieren** .

### <a name="on-board-existing-virtual-machines"></a>Integrierte vorhandenen virtuellen Computern

Haben Sie vorhandene virtuelle Computer in VMM die Hyper-V Replica replizieren benötigen, integrierte sie Sie für Azure Site Recovery Schutz wie folgt:

1. Überprüfen Sie, ob Sie primäre und sekundäre Clouds verfügen. Sicherstellen Sie, dass der Hyper-V Server hosten die vorhandenen virtuellen Computer in der primären Cloud befindet und Hyper-V Server hosten Replikat virtuellen Computer in der sekundären Cloud befindet. Stellen Sie sicher, dass die Wolken Einstellungen konfiguriert haben. Die Zeit für Hyper-V Replica sollten übereinstimmen. Andernfalls kann virtuelle Computer Replikation nicht erwartungsgemäß.
2. Aktivieren Sie Schutz für den primären virtuellen Computer. Azure Site Recovery und VMM wird sichergestellt, dass dieselbe Replika-Server und virtuellen Computer erkannt und Azure Site Recovery wiederverwenden und Replikation mit Einstellungen im Cloud-Konfiguration erneut.


## <a name="test-your-deployment"></a>Testen der Bereitstellung

Zum Testen der Bereitstellung führen Sie ein testfailover für einen einzelnen virtuellen Computer oder erstellen Sie einen Wiederherstellungsplan mehrere virtuelle Computer aus und führen Sie einen testfailover für den Plan.  Testfailover simuliert Failover- und Mechanismus in einem isolierten Netzwerk.

### <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

1. Klicken Sie auf der Registerkarte **Wiederherstellungspläne** **Wiederherstellungsplan erstellen**.
2. Geben Sie einen Namen für die Wiederherstellungsplan und Quell- und VMM-Server. Der Quellserver müssen virtuelle Computer, die für Failover und Recovery aktiviert. Wählen Sie **Hyper-V** nur Wolken anzeigen, die für die Hyper-V-Replikation konfiguriert sind.

    ![Wiederherstellungsplan erstellen](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)

3. Wählen Sie Replikationsgruppen im **virtuellen Computer auswählen**. Alle virtuellen Computer der Replikation Gruppe markiert und den Wiederherstellungsplan hinzugefügt. Diese virtuellen Computer werden die Standardgruppe Recovery Plan hinzugefügt – Gruppe 1. Falls erforderlich, können Sie weitere Gruppen hinzufügen. Beachten Sie, dass nach der Replikation virtueller Maschinen gemäß der Reihenfolge der wiederherstellungsplangruppen gestartet werden.

    ![Hinzufügen von virtuellen Computern](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Nach einer Wiederherstellung Plan wurde in der Liste auf der Registerkarte **Wiederherstellungspläne** angezeigt.

###<a name="run-a-test-failover"></a>Führen Sie ein testfailover

1. Wählen Sie auf der Registerkarte **Wiederherstellungspläne** den Plan, und klicken Sie auf **Test-Failover**.
2. Wählen Sie auf der Seite **Test-Failover bestätigen** **keine**. Notiz, die mit dieser Option über VMs Replikat fehlgeschlagenen aktiviert wird nicht mit einem Netzwerk verbunden. Wird getestet, dass der virtuelle Computer erwartungsgemäß ein Failover der Replikation-Netzwerk wird nicht überprüft. Weitere Informationen zur Verwendung von verschiedenen Netzwerkoptionen [Führen Sie ein testfailover](site-recovery-failover.md#run-a-test-failover) betrachten.
3. Test-VM wird auf demselben Server wie der Host erstellt auf dem VM Replikat vorhanden ist. Es wird auf derselben Wolke hinzugefügt in der VM Replikat befindet.

### <a name="run-a-recovery-plan"></a>Führen Sie einen Wiederherstellungsplan
Nach der Replikation möglicherweise Replikat virtuellen Computer keine IP-Adresse, die IP-Adresse des primären virtuellen Computers identisch ist. Virtuelle Computer aktualisieren den DNS-Server, die sie mit nach dem start. Sie können auch ein Skript zum Aktualisieren der DNS-Server eine rechtzeitige Aktualisierung sicher hinzufügen.

#### <a name="script-to-retrieve-the-ip-address"></a>Skript zum Abrufen der IP-Adresse
Führen Sie dieses Beispielskript zum Abrufen der IP-Adresse.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Skript zum Aktualisieren von DNS
Führen Sie dieses Beispiel zum Aktualisieren von DNS, IP-Adresse, die mit vorherigen Skript abgerufen.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Informationen zum Datenschutz für Site Recovery

Dieser Abschnitt enthält zusätzliche Datenschutzinformationen für Microsoft Azure Site Recovery Service ("Service"). Datenschutzrichtlinie für Microsoft Azure Services finden Sie unter [Microsoft Azure-Datenschutzrichtlinie](http://go.microsoft.com/fwlink/?LinkId=324899)

**Funktion: Registrierung**

- **Funktionsweise**: Server Service registriert, sodass virtuelle Computer geschützt werden können
- **Informationen**: nach dem Registrieren des Dienstes erfasst, verarbeitet und überträgt Zertifikat Verwaltungsinformationen VMM-Server, zu Disaster Recovery mit den Namen der VMM-Server und den Namen des virtuellen Computers Wolken auf dem VMM-Server festgelegt wurde.
- **Verwendung von Informationen**:
    - Zertifikat – Hiermit identifizieren und authentifizieren den registrierten VMM-Server für den Zugriff auf den Dienst. Der Dienst verwendet den öffentlichen Schlüssel Teil des Zertifikats einen Token zu sichern, dem nur der registrierte VMM-Server zugreifen kann. Der Server muss dieses Token verwenden, um auf die Funktionen zugreifen.
    - Name des VMM-Servers – der VMM-Servernamen identifizieren und die entsprechende Wolken sich auf dem befinden VMM-Server kommunizieren muss.
    - Cloud-Namen aus dem VMM-Server-Namen Cloud ist erforderlich, wenn die Cloud Service mit pairing/entkoppeln Funktion beschrieben. Wenn der Cloud aus einem primären Rechenzentrum mit anderen Cloud im Rechenzentrum Recovery koppeln möchten, werden die Namen von allen Wolken Recovery-Rechenzentrum angezeigt.

- **Auswahl**: Diese Informationen ist Bestandteil des Registrierungsprozesses Service erleichtert Ihnen und dem Dienst den VMM-Server identifizieren die Azure Site Recovery sowie bietet den richtigen registrierten VMM-Server identifizieren. Wenn Sie diese Informationen an den Dienst senden möchten, verwenden Sie diesen Dienst nicht. Registrieren Sie Ihre Server und möchten diese Registrierung dazu Sie die VMM-Server-Informationen über das ServicePortal (den Azure-Portal) löschen.

**Funktion: Azure Site Recovery-Schutz aktivieren**

- **Funktionsweise**: der Azure Site Recovery-Provider installiert auf dem VMM-Server ist mit dem Dienst kommunizieren. Der Anbieter ist eine Dynamic Link Library (DLL) in VMM-Prozess gehostet. Nach der Installation des Anbieters wird das Feature "Datacenter Recovery" in der VMM-Verwaltungskonsole aktiviert. Alle neuen oder vorhandenen virtuellen Computer in der Cloud können eine Eigenschaft namens "Datacenter Recovery", um den virtuellen Computer zu schützen. Wenn diese Eigenschaft festgelegt ist, sendet der Provider Name und ID des virtuellen Computers an den Dienst. Virtuelle Schutz ist in Windows Server 2012 oder Windows Server 2012 R2 Hyper-V-Technologie für die Replikation aktiviert. Die Daten der virtuellen Maschine repliziert von Hyper-V-Hosts (in der Regel befindet sich in einem anderen "Recovery").

- **Informationen**: der Dienst erfasst, verarbeitet und überträgt Metadaten für den virtuellen Computer, einschließlich Name, ID, virtuelles Netzwerk und den Namen der Cloud zu der er gehört.

- **Verwendung von Informationen**: der Dienst verwendet die Angaben virtuellen Computerinformationen über das ServicePortal aufgefüllt.

- **Auswahl**: Dies ist ein wesentlicher Bestandteil des Dienstes und kann nicht deaktiviert werden. Möchten Sie diese Informationen an den Dienst gesendet, aktivieren Sie nicht Azure Site Recovery-Schutz für alle virtuellen Computer. Beachten Sie, dass alle Daten vom Anbieter mit dem Dienst über HTTPS gesendet wird.

**Funktion: Wiederherstellungsplan**

- **Funktionsweise**: dadurch einen Orchestrierung Plan für Rechenzentren "Recovery" erstellen. Sie können die Reihenfolge bestimmen, in der die virtuellen Computer oder eine Gruppe von virtuellen Computern am Recovery-Standort gestartet werden soll. Sie können auch automatisierte Skripts ausführen oder manuell auszuführende Aktion, zum Zeitpunkt der Wiederherstellung für jeden virtuellen Computer zu. Koordinierte Recovery Ebene der Plan für die Wiederherstellung wird in der Regel Failover (im nächsten Abschnitt behandelt) ausgelöst.

- **Informationen**: der Dienst erfasst, verarbeitet und überträgt Metadaten für den Wiederherstellungsplan virtuellen Metadaten und Metadaten von Scripts zur Automatisierung und Notizen manuell.

- **Verwendung von Informationen**: beschriebenen Metadaten zum Wiederherstellungsplan im ServicePortal erstellen.

- **Auswahl**: Dies ist ein wesentlicher Bestandteil des Dienstes und kann nicht deaktiviert werden. Möchten Sie diese Informationen an den Dienst gesendet, erstellen Sie nicht Wiederherstellungspläne in diesem Dienst.

**Funktion: Netzwerk-Zuordnung**

- **Funktion**: Diese Funktion Netzwerk Daten im primären Rechenzentrum Recovery-Rechenzentrum. Wenn die virtuellen Computer am Recovery-Standort wiederhergestellt werden, können diese Zuordnung Netzwerkkonnektivität für sie einrichten.

- **Informationen**: als Teil des Netzwerks Landkartenmerkmale der Dienst sammelt verarbeitet und überträgt die Metadaten der logische Netzwerke für jede Site (Primär- und Datacenter).

- **Verwendung von Informationen**: der Dienst anhand der Metadaten füllen das ServicePortal, in dem Sie die Netzwerkinformationen zuordnen.

- **Auswahl**: Dies ist ein wesentlicher Bestandteil des Dienstes und kann nicht deaktiviert werden. Möchten Sie diese Informationen an den Dienst gesendet, verwenden Sie die Funktion Zuordnung nicht.

**Funktion: Failover - geplante und ungeplante, test**

- **Funktionsweise**: Dadurch Failover für einen virtuellen Computer aus einer VMM verwalteten Rechenzentrum zu einem anderen verwalteten VMM-Rechenzentrum. Der Benutzer ihre Service Portal Failover-Vorgang ausgelöst. Mögliche Gründe für einen Failovercluster sind ein ungeplantes Ereignis (beispielsweise bei einer natürlichen disaster0; ein geplantes Ereignis (z. B. Datacenter Lastenausgleich), testfailover (z. B. eine Recovery Plan Probe).

Der Anbieter auf dem VMM-Server des Ereignisses vom Dienst benachrichtigt und führt eine Failover-Aktion auf dem Hyper-V-Host über VMM-Schnittstellen. Tatsächliche Failover des virtuellen Computers von Hyper-V-Hosts (normalerweise in einem anderen "Recovery" ausgeführt) wird von Windows Server 2012 oder Windows Server 2012 R2 Hyper-V-Replikationstechnik behandelt. Nach Abschluss des Failovers sendet der Anbieter auf dem VMM-Server des Rechenzentrums "Recovery" installiert die Informationen erfolgreich an den Dienst.

- **Informationen**: der Dienst verwendet die Angaben darin, den Status der Failover Aktivitätsinformationen über Service-Portal.

- **Verwendung von Informationen**: der Dienst verwendet die Angaben wie folgt:

    - Zertifikat – Hiermit identifizieren und authentifizieren den registrierten VMM-Server für den Zugriff auf den Dienst. Der Dienst verwendet den öffentlichen Schlüssel Teil des Zertifikats einen Token zu sichern, dem nur der registrierte VMM-Server zugreifen kann. Der Server muss dieses Token verwenden, um auf die Funktionen zugreifen.
    - Name des VMM-Servers – der VMM-Servernamen identifizieren und die entsprechende Wolken sich auf dem befinden VMM-Server kommunizieren muss.
    - Cloud-Namen aus dem VMM-Server-Namen Cloud ist erforderlich, wenn die Cloud Service mit pairing/entkoppeln Funktion beschrieben. Wenn der Cloud aus einem primären Rechenzentrum mit anderen Cloud im Rechenzentrum Recovery koppeln möchten, werden die Namen von allen Wolken Recovery-Rechenzentrum angezeigt.

- **Auswahl**: Dies ist ein wesentlicher Bestandteil des Dienstes und kann nicht deaktiviert werden. Möchten Sie diese Informationen an den Dienst gesendet, verwenden Sie diesen Dienst nicht.

## <a name="next-steps"></a>Nächste Schritte

Sie haben für ein testfailover Überprüfen Ihrer Umgebung erwartungsgemäß, [lernen Sie](site-recovery-failover.md) verschiedene Failover ausführen.
