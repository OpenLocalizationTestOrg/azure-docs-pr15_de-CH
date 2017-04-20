<properties 
   pageTitle="Nicht wieder virtuelle VMware-Maschinen und physische Server auf der lokalen Website | Microsoft Azure"
   description="Informationen Sie zu nicht zu lokalen Standort nach einem Failover von virtuellen VMware Maschinen und physische Server in Azure." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required" 
   ms.date="08/22/2016"
   ms.author="ruturajd"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Fehler zurück virtuelle VMware-Maschinen und physische Server auf der lokalen Website

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-failback-azure-to-vmware.md)
- [Azure-Verwaltungsportal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure-Verwaltungsportal (Legacy)](site-recovery-failback-azure-to-vmware-classic-legacy.md)



Dieser Artikel beschreibt das wieder Azure virtuelle Computer von Azure lokalen Standort fehl. Führen Sie die Schritte in diesem Artikel möchten ein Failback der virtuellen VMware Maschinen oder Windows/Linux Servern Nachdem sie von der lokalen Failover haben site in Azure mit diesem [Lernprogramm](site-recovery-vmware-to-azure-classic.md).



## <a name="overview"></a>Übersicht

Dieses Diagramm zeigt die Failback-Architektur für dieses Szenario.

Verwenden Sie diese Architektur, wenn Prozess-Server lokal und ein ExpressRoute verwenden.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Verwenden Sie diese Architektur, wenn Prozessserver in Azure ist und ein VPN oder eine ExpressRoute-Verbindung.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Die vollständige Liste von Ports und das Failback Architektur Diagramm unten finden

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Funktioniert wie Failback:

- Nach dem Failover Azure durchgeführt haben, fehlschlagen Sie in wenigen Schritten zum lokalen Standort:
    - **Schritt 1**: Sie Azure VMs schützt, sodass sie auf Ihrer lokalen Website auf virtuelle VMware-Computer repliziert. Aktivieren Reprotection verschiebt die VM in einer Schutzgruppe Failback, die automatisch beim Failover Schutzgruppe ursprünglich erstellt. Wenn Sie hinzugefügt wurde die Schutzgruppe Failover einen Wiederherstellungsplan dann die Schutzgruppe Failback Plan automatisch hinzugefügt.  Beim Failback Geben Sie Failback planen.
    - **Schritt 2**: nach der Azure-VMs auf Ihrem lokalen Standort replizieren, Ausführen eine über von Azure fehlschlagen.
    - **Schritt 3**: Nachdem Daten wieder fehlgeschlagen ist, schützt Sie lokalen VMs, die, nicht, sodass sie in Azure replizieren.

> [AZURE.VIDEO enhanced-vmware-to-azure-failback]


### <a name="failback-to-the-original-or-alternate-location"></a>Failback auf den ursprünglichen oder alternativen

Wenn Failover VMware VM kann durchgeführt werden an dieselbe Quelle VM ggf. noch vor Ort. In diesem Szenario werden nur das Delta ändert sich wieder ein Fehler auf. Beachten Sie Folgendes:

- Physische Server Failover ist Failback immer neue VMware-VM.
    - Beachten Sie vor einen physischen Computer stets, dass
    - Physische Computer geschützt kommen als eine virtuelle Maschine von Azure VMware Failover
    - Stellen Sie sicher, dass Sie mindestens eine Master-Ziel-Server mit den erforderlichen ESX/ESXi Hosts, Failback müssen, ermitteln.
- Wenn Sie wieder zum ursprünglichen virtuellen ist Folgendes erforderlich:
    - Wenn die VM vCenter Server verwaltet müssen der Master-Ziel-ESX-Host Zugriff auf VMs Datenspeicher.
    - Wenn die VM auf einem ESX-Host wird nicht von vCenter muss die Festplatte des virtuellen Computers im Datenspeicher der MT-Host zugänglich sein.
    - Wenn die VM auf einem ESX-Host und vCenter nicht verwenden sollten Sie schließen Entdeckung des ESX-Hosts MT bevor Sie Einstellung. Dies gilt, wenn wieder Servern zu fehlgeschlagen sind.
    - Eine andere Option ist (lokale VM vorhanden) vor ein Failback löschen. Dann erstellen Failback anschließend eine neue VM auf dem gleichen Server als master-Ziel-ESX-Host.
    
- Beim Failback an einem anderen Speicherort Daten im gleichen Datastore und demselben ESX-Host wie das lokale master Zielserver wiederhergestellt werden. 


## <a name="prerequisites"></a>Erforderliche Komponenten

- Sie benötigen eine VMware-Umgebung um fehlschlagen virtuelle VMware-Computer und physischen Servern zurück. Fehlerhafte mit einem physischen Server wird nicht unterstützt.
- Um ein Failback sollten Sie ein Azure-Netzwerk erstellt haben Wenn Sie Schutz eingerichtet. Failback benötigt eine VPN- oder ExpressRoute Verbindung von Azure-Netzwerk in dem Azure-VMs auf der lokalen Website befinden.
- Soll die VMs Failback auf erfolgt über einen vCenter Server müssen Sie sicherstellen, haben Sie die erforderlichen Berechtigungen für die Ermittlung von VMs auf vCenter Servern. [Mehr](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
- Wenn Snapshots auf einen virtuellen Computer fehl Reprotection. Sie können Snapshots oder Datenträger. 
- Bevor Sie ein Failback müssen Sie eine Reihe von Komponenten:
    - **Prozessserver in Azure erstellen**. Dies ist eine Azure-VM, die Sie erstellen und während des Failbacks laufen müssen. Sie können den Computer nach Abschluss der Failback.
    - **Erstellen einer master Zielserver**: master-Ziel-Server Daten sendet und empfängt Failback. Erstellung von lokalen Verwaltungsserver hat standardmäßig master Zielserver. Jedoch je nach fehlerhaften Back Datenverkehr möglicherweise einen separates master Zielserver für das Failback zu erstellen.
    - Wenn Sie eine zusätzliche master Zielserver unter Linux erstellen möchten, müssen Sie Linux VM einrichten, bevor Sie master Zielserver installieren wie unten beschrieben.
- Konfigurationsserver ist erforderlich für lokale Wenn Sie ein Failback. Beim Failback muss der virtuelle Computer in der Server Konfigurationsdatenbank Versagen der Failback nicht erfolgreich vorhanden sein. Daher sicherstellen Sie, dass Sie regelmäßig geplante Sicherung Ihres Servers. Bei einem Notfall müssen Sie sie mit derselben IP-Adresse wiederherzustellen, so dass Failback wird.

## <a name="set-up-the-process-server-in-azure"></a>Prozessserver in Azure einrichten

Sie müssen einen Prozessserver in Azure installieren Azure VMs auf master Zielserver lokale Daten senden können. 

1.  Im Portal Site Recovery > **Configuration Server** auswählen, einen neuen Prozessserver hinzufügen.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)

2.  Geben Sie einen Prozess Servernamen an, und geben Sie einen Namen und Kennwort für die Verbindung der Azure-VM als Administrator verwenden. **Konfigurationsserver** wählen Sie lokalen Verwaltungsserver und der Azure Netzwerk mit der Prozess-Server bereitgestellt werden soll. Dies sollte im Netzwerk in dem Azure-VMs befinden. Geben Sie eine eindeutige IP-Adresse aus dem Subnetz auswählen und mit der beginnen Sie Bereitstellung.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

    Ein Auftrag zum Prozess-Server bereitstellen wird ausgelöst

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

    Nach der Bereitstellung des Prozessservers in Azure können melden Sie mithilfe der Anmeldeinformationen. Der ersten Anmeldung im Dialogfeld Server Prozess wird ausgeführt. Geben Sie die IP-Adresse des lokalen Verwaltungsserver und die Passphrase. Lassen Sie die Standardeinstellung für Port 443. Sie lassen Standard 9443 Port Datenreplikation, sofern diese Einstellung ausdrücklich geändert wird, beim Einrichten der lokalen Verwaltungsserver. 

    >[AZURE.NOTE] Server werden nicht unter **VM-Eigenschaften**angezeigt. Es ist nur unter der Registerkarte **Server** die Verwaltungsserver, registriert wurde. Es dauert über 10-15 Minuten für den Prozess-Server angezeigt werden.


## <a name="set-up-the-master-target-server-on-premises"></a>Richten Sie die master-Ziel-Server lokal

Master-Ziel-Server empfängt die Failback-Daten. Master Zielserver automatisch auf dem lokalen Server installiert ist, aber wenn Sie große Datenmengen Failback sind möglicherweise eine zusätzliche master Zielserver einrichten. Hierzu wie folgt:

>[AZURE.NOTE] Wenn Sie einen master Zielserver unter Linux installieren möchten, gehen Sie in der nächsten Prozedur.

1. Wenn Sie master Zielserver auf Windows installieren, öffnen Sie die Seite Schnellstart VM auf dem master Zielserver installieren, und downloaden die Installationsdatei Azure Site Recovery Unified Setup-Assistenten.
2. Führen Sie Setup aus und im **vor** **Hinzufügen zusätzlicher Server Deployment skalieren**.
3. Führen Sie den Assistenten auf die gleiche Weise habe bei [den Management-Server eingerichtet](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Geben Sie auf der Seite **Konfiguration Server** die IP-Adresse von diesem master Zielserver und eine Passphrase auf die VM an

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Linux VM als master Zielserver einrichten
Einrichten der Verwaltungsserver master Zielserver unter Linux VM Cent installieren müssen) Betriebssystem S 6.6 minimale SCSI-IDs für jede SCSI-Festplatte abrufen, einige zusätzliche Pakete installieren und einige benutzerdefinierte Änderungen.

#### <a name="install-centos-66"></a>Installieren von CentOS 6.6

1.  Installieren Sie das CentOS 6.6 minimale Betriebssystem auf dem Verwaltungsserver VM. Halten Sie ISO in ein DVD-Laufwerk, und starten Sie das System. Media Test überspringen, wählen Sie die Sprache Englisch wählen Sie **Grundlegende Speichergeräte**, sicher, dass die Festplatte nicht wichtigen Daten und klicken Sie auf **Ja**, alle Daten zu löschen. Geben Sie den Hostnamen des Management-Servers, und wählen Sie den Server-Netzwerkadapter.  Klicken Sie im Dialogfeld **System bearbeiten** wählen Sie** automatisch verbinden** und fügen Sie eine statische IP-Adresse, Netzwerk und DNS-Einstellungen. Geben Sie eine Zeitzone und Root-Passwort auf dem Verwaltungsserver. 
2.  Die Art der Installation gefragt möchten **Create Custom Layout** als Partition auswählen. Nach Anklicken **Weiter** **frei** wählen und klicken Sie auf erstellen. Erstellen Sie **/**, **var** und **/ home Partitionen** mit **FS-Typ:** **ext4**. Erstellen der Swap-Partition als **FS-Typ: Swap**.
3.  Wenn vorhandene Geräte gefunden werden, wird eine Warnung angezeigt. Klicken Sie auf **Format** , um das Laufwerk mit der Partition zu formatieren. Klicken Sie auf **ändern auf den Datenträger schreiben** , um die Partition ändern.
4.  Wählen Sie **Install Bootloader** > **nächsten** Bootloader auf der Root-Partition installieren.
5.  Nach Abschluss der Installation klicken Sie auf **neu starten**.


#### <a name="retrieve-the-scsi-ids"></a>Rufen Sie die SCSI-IDs

1. Rufen Sie nach der Installation der SCSI-IDs für jede SCSI-Festplatte auf dem virtuellen Computer ab Hierzu diese Herunterfahren Verwaltungsserver VM die VM-Eigenschaften in VMware mit der rechten Maustaste des VM-Eintrags > **Einstellungen bearbeiten** > **Optionen**.
2. Wählen Sie **Advanced** > **Allgemeiner Artikel** und **Konfigurationsparameter**auf. Diese Option ist deaktiviert, wenn der Computer ausgeführt wird. Um es zu aktivieren muss der Computer heruntergefahren werden.
3. Wenn die Zeile **Datenträger. EnableUUID** gibt sicherzustellen, dass der Wert auf **True** festgelegt (Groß-und Kleinschreibung). Wenn sie bereits Abbrechen und SCSI-Befehl innerhalb eines Gastbetriebssystems testen, nachdem es gestartet wurde. 
4.  Wenn die Zeile nicht vorhandene **Zeile hinzufügen** klicken und mit dem Wert **True** hinzufügen. Verwenden Sie keine Anführungszeichen.

#### <a name="install-additional-packages"></a>Zusätzliche Pakete installieren

Sie müssen einige zusätzliche Pakete heruntergeladen und installiert. 

1.  Stellen Sie sicher, dass der master Zielserver mit dem Internet verbunden ist.
2.  Führen Sie diesen Befehl zum Herunterladen und Installieren von 15 Pakete aus dem Repository CentOS: **# Yum – y Xfsprogs Perl Lsscsi Rsync Wget Kexec-Tools installieren**.
3.  Wenn die Quelle Computer geschützt sind Linux mit Reiser ausgeführt oder XFS Dateisystem für das Stamm- oder Boot-Gerät sollten Sie downloaden und installieren zusätzliche Pakete wie folgt:

    - # <a name="cd-usrlocal"></a>CD/usr/local
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>Wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>Wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
    - # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>Rpm-Ivh Kmod Reiserfs-0,0 1.el6.elrepo.x86_64.rpm Reiserfs-Utils-3.6.21-1.el6.elrepo.x86_64.rpm
    - # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>Wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
    - # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>Rpm-Ivh Xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Benutzerdefinierte übernehmen

Führen Sie Änderungen anwenden, nachdem Sie haben folgende Schritte nach der Installation und die Pakete installiert:

1.  Kopieren der RHEL 6 64 Unified Agent binäre der VM. Führen Sie diesen Befehl, um die Binärdatei entpacken: **tar – Zxvf <file name> **
2.  Führen Sie diesen Befehl, um Berechtigungen zu erteilen: **# Chmod 755./ApplyCustomChanges.sh**
3.  Führen Sie das Skript: **#./ApplyCustomChanges.sh**. Das Skript muss nur einmal ausgeführt werden. Starten Sie den Server aus, nachdem das Skript erfolgreich ausgeführt wird.


## <a name="run-the-failback"></a>Führen Sie das failback

### <a name="reprotect-the-azure-vms"></a>Einstellung der Azure-VMs

1.  Im Portal Site Recovery > Registerkarte **Maschinen** VM, die ist Failover und klicken Sie auf **erneut schützen**.
2.  **Master-Zielserver** und **Prozess-Server** wählen Sie master Zielserver lokale und Azure VM Prozess-Server.
3.  Wählen Sie das Konto für die Verbindung mit den virtuellen Computer konfiguriert.
4.  Wählen Sie die Failback-Version der Schutzgruppe. Für Beispiel, wenn die VM PG1 dann geschützt PG1_Failback auswählen muss.
5.  Wenn Sie an einem alternativen Speicherort wiederherstellen möchten, wählen Sie aufbewahrungslaufwerk und Datenspeicher für den master Zielserver konfiguriert. Wenn Sie auf der lokalen Website virtuelle VMware-Computer in Schutzplan Failback nicht verwendet denselben Datenspeicher als master Zielserver. Möchten Sie das Replikat wiederherstellen lokalen Azure VM auf denselben VM lokale VM bereits im gleichen Datastore wie master Zielserver sein. Ist keine lokale VM wird während der Reprotection eine neue erstellt.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)

6.  Nach Anklicken beginnt **OK** Reprotection eines Auftrags beginnen die VM von Azure zum lokalen Standort replizieren. Sie können den Fortschritt auf der Registerkarte **Aufträge** verfolgen.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Führen Sie einen Failover zum lokalen Standort

Nach Reprotection VM Failback Version der Schutzgruppe verschoben und verwendet für den Failover zu Azure ist eine Wiederherstellungsplan automatisch hinzugefügt.

1.  In der Seite **Wiederherstellungspläne** Wiederherstellungsplan Failback Gruppe und wählen Sie **Failover** > **Ungeplanten Failover**.
2.  Failover **Bestätigen** überprüfen Sie Failover Richtung (Azure) und wählen Sie den Wiederherstellungspunkt für das Failover (neueste) verwenden möchten. **Multi-VM** aktiviert, bei der Konfiguration von Replikationseigenschaften können Sie die aktuelle Anwendung oder konsistente Recovery wiederherstellen. Klicken Sie auf das Häkchen, um das Failover zu starten.
3.  Beim Failover wird der Azure-VMs Site Recovery heruntergefahren. Nachdem Sie überprüft haben, dass das Failback erwartungsgemäß abgeschlossen können Sie überprüfen können, dass Azure VMs heruntergefahren haben wie erwartet.

### <a name="reprotect-the--on-premises-site"></a>Einstellung der lokalen Website

Nach Abschluss der Failback Daten werden auf der lokalen Website jedoch nicht geschützt. Starten in Azure Replikation wieder folgendermaßen:

1.  Im Portal Site Recovery > **Maschinen** Registerkarte wählen ausgefallen VMs zurück und klicken Sie auf **erneut schützen**. 
2.  Vergewissern Sie sich, dass die Replikation funktioniert Azure erwartungsgemäß in Azure löschen (nicht aktiven) Azure-VMs, die wieder fehlgeschlagen sind.


### <a name="common-issues-in-failback"></a>Häufige Probleme bei der failback

1. Schreibgeschützte Benutzer vCenter Erkennung durchgeführt und Schutz von virtuellen Maschinen erfolgreich und Failover funktioniert. Zum Zeitpunkt des Failback schlägt fehl, da der Datenspeicher nicht gefunden. Als Symptom sehen Sie nicht Datenspeicher beim Schützen aufgeführt. Um dieses Problem zu beheben, können Sie vCenter Anmeldeinformationen mit entsprechenden Konto mit Berechtigungen aktualisieren und wiederholen Sie den Auftrag. [Lesen Sie mehr](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Beim Failback Linux VM und Prem, sehen Sie, dass das Netzwerk-Manager-Paket vom Computer deinstalliert. Ist die VM in Azure wiederhergestellt, Network Manager-Paket entfernt wird.
3. Ein virtueller Computer mit statischen IP-Adresse konfiguriert ist, in Azure Failover wird die IP-Adresse über DHCP erworben. Beim Failover wieder auf Prem weiterhin VM DHCP IP-Adresse erhalten. Sie müssen manuell anmelden in der Computer und die IP-Adresse wieder auf statische Adresse gegebenenfalls.
4. Wenn Sie kostenlose Version ESXi 5.5 oder 6 vSphere Hypervisor kostenlose Edition verwenden, Failback nicht erfolgreich Failover wäre erfolgreich Sie werden erfordern eine Testlizenz Failback aktivieren aktualisieren.

## <a name="failing-back-with-expressroute"></a>Wieder mit ExpressRoute

Sie können über eine VPN-Verbindung oder Azure ExpressRoute fehl. Möchten Sie ExpressRoute Beachten Sie Folgendes verwenden:

- ExpressRoute sollte im Azure virtuelle Netzwerk an dem Computer Fehler über und in der Azure-VMs befinden sich nach dem der Failover eingerichtet werden.
- Daten werden über einen öffentlichen Endpunkt auf ein Azure-Speicher repliziert. Sie richten öffentliche peering in ExpressRoute mit dem Ziel Data Center Site Recovery Replikation ExpressRoute verwenden.



