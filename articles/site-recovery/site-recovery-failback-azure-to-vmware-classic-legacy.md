<properties 
   pageTitle="Nicht wieder virtuelle VMware-Maschinen und physische Server von Azure VMware (Legacy) | Microsoft Azure" 
   description="Dieser Artikel beschreibt das Failback von virtuellen VMware-Maschine, die in Azure mit Azure Site repliziert wurden." 
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
   ms.workload="storage-backup-recovery" 
   ms.date="10/05/2016"
   ms.author="ruturajd@microsoft.com"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Nicht wieder virtuelle VMware-Maschinen und physische Server von Azure VMware mit Azure Site Recovery (Legacy)

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-failback-azure-to-vmware.md)
- [Azure-Verwaltungsportal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure-Verwaltungsportal (Legacy)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


Azure Site Recovery-Dienst trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern und Servern orchestriert. Computer können Azure oder einem lokalen sekundären Rechenzentrum repliziert werden. Finden Sie einen schnellen Überblick über [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie Back virtuellen VMware Maschinen und Windows/Linux Servern von Azure auf Ihrer lokalen Website fehlschlagen, nachdem Sie von der lokalen Website in Azure repliziert haben.

>[AZURE.NOTE] Dieser Artikel beschreibt eine ältere Szenario. Sie sollten nur die Anleitung in diesem Artikel verwenden, Azure mit [diesen legacy](site-recovery-vmware-to-azure-classic-legacy.md)repliziert. Wenn Sie die [Erweiterte Bereitstellung](site-recovery-vmware-to-azure-classic-legacy.md) eingerichtet führen Sie die Schritte in [diesem Artikel](site-recovery-failback-azure-to-vmware-classic.md) Failback. 


## <a name="architecture"></a>Architektur

Dieses Diagramm zeigt das Szenario Failover und Failback. Blauen Linien sind die Verbindung während eines Failovers verwendet. Die roten Linien sind während des Failbacks verwendete Verbindung. Linien mit Pfeilen gehen über das Internet.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Bevor Sie beginnen 

- Sollten Sie über die virtuelle VMware-Computer oder physische Server konnten und sollte in Azure ausgeführt werden.
- Beachten Sie, dass nur Back virtuellen VMware Maschinen und Windows/Linux physischen Servern von Azure virtuelle VMware-Computer am primären Standort lokal ausgeführt werden kann.  Physischen Computer wieder fehlgeschlagen sind, Failover in Azure konvertiert sie in Azure-VM und Failback VMware wird VMware VM konvertieren.

Hier wird das Failback einrichten:

1. **Failback Komponenten eingerichtet**: Sie müssen einen vContinuum-Server einrichten lokal und auf dem Konfigurationsserver VM in Azure. Sie werden als eine Azure VM Prozess-Server festlegen, zum Senden von Daten an das lokale master Zielserver. Bei dem Konfigurationsserver, der das Failover behandelt registriert den Prozess-Server. Installation von einem lokalen-Zielserver. Benötigen Sie einen master Zielserver Windows wird automatisch festgelegt bei der Installation von vContinuum wird. Benötigen Sie Linux müssen Sie manuell auf einem separaten Server eingerichtet.
2. **Schutz und Failbacks aktivieren**: nach Komponenten eingerichtet haben, in vContinuum Sie müssen Azure VMs Failover Schutz aktivieren. Sie führen Sie eine Überprüfung der Bereitschaft für VMs und führen Sie einen Failover von Azure auf Ihrer lokalen Website. Nach Abschluss der Failback Einstellung lokalen Computer, sodass sie in Azure replizieren.



## <a name="step-1-install-vcontinuum-on-premises"></a>Schritt 1: VContinuum lokal installieren

Sie müssen einen vContinuum-Server lokal installieren und es auf dem Konfigurationsserver.

1.  [VContinuum herunterladen](http://go.microsoft.com/fwlink/?linkid=526305). 
2.  Dann die Version [vContinuum aktualisiert](http://go.microsoft.com/fwlink/?LinkID=533813) .
3. Installieren Sie die neueste Version von vContinuum. Klicken Sie auf der Seite **Willkommen** auf **Weiter**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  Geben Sie auf der ersten Seite des Assistenten die CX-Server IP-Adresse und CX Serverport an. Wählen Sie **verwenden HTTPS**.

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  Suchen des konfigurationsservers IP-Adresse auf der Registerkarte **Dashboard** Konfigurationsserver VM in Azure.
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  Suchen des konfigurationsservers öffentliche HTTPS-Port auf der Registerkarte **Endpunkte** Konfigurationsserver VM in Azure.

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. Geben Sie auf der Seite **Kennwort CS** Passphrase ein, der Sie notiert bei den Konfigurationsserver registriert. Wenn Sie sich nicht erinnern sie Einchecken **C:\\Programme (x86)\\InMage Systeme\\private\\connection.passphrase** auf dem Konfigurationsserver VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  **Zielspeicherort wählen Sie** Seite Geben Sie soll vContinuum Server installieren und klicken Sie auf **Installieren an**.

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. Nach Abschluss der Installation können Sie vContinuum starten.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## <a name="step-2-install-a-process-server-in-azure"></a>Schritt 2: Installieren von einem Prozessserver in Azure 

Sie müssen einen Prozessserver in Azure installieren, sodass die VMs in Azure die Daten auf einem lokalen-Zielserver senden können. 

1.  Wählen Sie auf der Seite **Konfigurationsserver** in Azure einen neuen Prozessserver hinzufügen.

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  Geben Sie einen Prozessserver Name und Name und Kennwort für den virtuellen Computer als Administrator. Wählen Sie Konfigurationsserver den Prozess-Server registriert werden soll. Dies sollte der gleiche Server verwendeten schützen und nicht über die virtuellen Computer.
3.  Angeben des Azure-Netzwerks in dem Prozess-Server bereitgestellt werden soll. Es sollte im selben Netzwerk wie der Konfigurationsserver. Geben Sie ein eindeutiges IP-Adresse Subnetz und mit der beginnen Sie Bereitstellung.

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  Ein Auftrag wird ausgelöst, um den Prozess-Server bereitstellen.

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  Nach Prozess-Server in Azure bereitgestellt können Sie den Server mit den angegebenen Anmeldeinformationen anmelden. Registrieren Sie den Prozess-Server dasselbe lokale Prozess-Server registriert. 

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] Während des Failbacks registrierte Server sichtbar nicht unter VM-Eigenschaften in Site Recovery. Sie sind nur unter der Registerkarte **Server** Configuration Server, dem sie registriert sind. Es dauert ungefähr 10 bis 15 Minuten, bis sie der Prozess-Server auf der Registerkarte angezeigt.


## <a name="step-3-install-a-master-target-server-on-premises"></a>Schritt 3: Installation lokal master Zielserver

Eine Linux installieren müssen, oder master Zielserver Windows lokal, abhängig von Ihrem Quellbetriebssystem virtuelle Computer.

### <a name="deploy-a-windows-master-target-server"></a>Einen Windows-master-Zielserver bereitstellen

Ein Windows-master-Ziel ist bereits vContinuum Setup enthalten. Bei der Installation der vContinuum ein Masterserver auch auf demselben Computer bereitgestellt und dem Konfigurationsserver registriert.

1.  Bereitstellung zunächst erstellen Sie eine leere Computer lokal auf dem ESX-Host auf dem wiederherzustellenden VMs von Azure.

2.  Sicherstellen, dass mindestens zwei Datenträgern der VM – eine für das Betriebssystem verwendet und anderen für die aufbewahrungslaufwerk dient.

3.  Installieren des Betriebssystems.

4.  Installieren Sie die vContinuum auf dem Server. Dies schließt auch Installation des Zielservers master.

### <a name="deploy-a-linux-master-target-server"></a>Einen Linux-master-Zielserver bereitstellen

1.  Bereitstellung zunächst erstellen Sie eine leere Computer lokal auf dem ESX-Host auf dem wiederherzustellenden VMs von Azure.

2.  Sicherstellen, dass mindestens zwei Datenträgern der VM – eine für das Betriebssystem verwendet und anderen für die aufbewahrungslaufwerk dient.

3.  Das Linux-Betriebssystem zu installieren. Master Zielsystem Linux sollten LVM für Stamm oder Aufbewahrung Speicherplätze nicht verwenden. Master Zielserver Linux ist standardmäßig um LVM Datenträgern Partitionen Erkennung zu vermeiden.
4.  Partitionen, die Sie erstellen können:

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  Führen Sie die folgenden Installationsschritte vor Beginn der Zielinstallation master buchen.


#### <a name="post-os-installation-steps"></a>OS Installationsschritte buchen

Für jedes SCSI-Festplatte in einer virtuellen Linux-Maschine die SCSI-IDs zu erhalten, Aktivieren des Parameters "Disk. EnableUUID = TRUE "wie folgt:

1. Fahren Sie den virtuellen Computer.
2. VM-Eintrag im linken Bedienfeld klicken > **Einstellungen bearbeiten**.
3. Klicken Sie auf die Registerkarte **Optionen** . Wählen Sie **Erweitert\>allgemeiner Artikel** > **Parameter**. **Parameter** -Option ist nur verfügbar, wenn der Computer heruntergefahren wird.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. Überprüft, ob eine Zeile mit **Datenträger. EnableUUID** vorhanden ist. Und auf **False** festgelegt, müssen dann **True** (keine Groß-und Kleinschreibung). Wenn vorhanden ist, und true, klicken Sie auf **Abbrechen** und SCSI-Befehl innerhalb des Gast-Betriebssystems testen, nachdem es gestartet wird festgelegt. Wenn existiert nicht klicken Sie auf **Add Row**.
5. Datenträger hinzufügen. EnableUUID in der Spalte **Name** . Den Wert true festgelegt. Keine der oben genannten Werte mit Anführungszeichen hinzufügen.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>Zusätzliche Pakete heruntergeladen und installiert

Hinweis: Stellen Sie sicher, dass das System Internetkonnektivität vor dem Downloaden und installieren zusätzliche Pakete.

\#Yum -y Xfsprogs Perl Lsscsi Rsync Wget Kexec-Tools installieren

Dieser Befehl diese 15 Pakete von CentOS 6.6 Repository gedownloadet und installiert werden:

BC-1.06.95-1.el6.x86\_64 u/Min

Busybox 1.15.1 20.el6.x86\_64 u/Min

Elfutils-Libs-0.158-3.2.el6.x86\_64 u/Min

Kexec-Tools-2.0.0-280.el6.x86\_64 u/Min

Lsscsi 0,23 2.el6.x86\_64 u/Min

Lzo 2.03 3.1.el6\_5.1.x86\_64 u/Min

Perl-5.10.1-136.el6\_6.1.x86\_64 u/Min

Perl-Modul-Plug-3,90-136.el6\_6.1.x86\_64 u/Min

Perl-Pod-Escapes-1,04-136.el6\_6.1.x86\_64 u/Min
    
Perl-Pod-einfach-3.13-136.el6\_6.1.x86\_64 u/Min

Perl-Libs-5.10.1-136.el6\_6.1.x86\_64 u/Min

Perl Version-0.77 136.el6\_6.1.x86\_64 u/Min

Rsync 3.0.6 12.el6.x86\_64 u/Min

leicht lesbare 1.1.0 1.el6.x86\_64 u/Min

Wget 1.12 5.el6\_6.1.x86\_64 u/Min

Hinweis: Wenn der Quellcomputer Reiser oder XFS Dateisystem für das Stamm- oder Boot-Gerät verwendet, dann folgenden Pakete sollten werden heruntergeladen und installiert auf Linux master Ziel Schutz.

\#CD/usr/local

\#Wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#Wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#Rpm - Ivh Kmod-Reiserfs-0,0-1.el6.elrepo.x86\_64 Rpm Reiserfs-Utils-3.6.21-1.el6.elrepo.x86\_64 u/Min

\#Wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#Rpm - Ivh Xfsprogs-3.1.1-16.el6.x86\_64 u/Min

#### <a name="apply-custom-configuration-changes"></a>Konfiguration ändern

Stellen Sie bevor diese Änderungen sicher, dass im vorherigen Abschnitt abgeschlossen haben, und gehen Sie folgendermaßen vor:


1. Kopieren der RHEL 6 64 Unified Agent binäre neu erstellte Betriebssystem.

2. Führen Sie diesen Befehl, um die Binärdatei entpacken: **tar - Zxvf \<Namen\> **

3. Führen Sie diesen Befehl, um Berechtigungen zu erteilen: \# **Chmod 755./ApplyCustomChanges.sh**

4. Führen Sie das Skript: ** \# ./ApplyCustomChanges.sh**. Führen Sie das Skript nur einmal auf dem Server. Starten Sie den Server neu, nachdem das Skript ausgeführt wird.



### <a name="install-the-linux-server"></a>Installieren Sie den Linux-server


1. [Download](http://go.microsoft.com/fwlink/?LinkID=529757) der Installationsdatei.
2. Kopieren Sie die Datei auf den Linux-Master virtuellen Zielcomputer mit einem Dienstprogramm Sftp-Client Ihrer Wahl. Auch können Sie der Linux master virtuelle Zielcomputer anmelden und Wget das Installationspaket aus den bereitgestellten Link herunterladen
3. Melden Sie sich an die Linux master Ziel Server virtueller Computer mit einem ssh Client Ihrer Wahl
4. Wenn Sie Azure Netzwerk verbunden sind, auf denen Zielserver die Linux-master über eine VPN-Verbindung bereitgestellt, verwenden Sie interne IP-Adresse des Servers Sie in den virtuellen **Dashboard** -Registerkarte und Port 22 finden Verbindung mit den Linux master-Zielserver mit Secure Shell.
5. Wenn Sie über einen Internetzugang zum master Linux-Zielserver herstellen Linux master Zielserver öffentliche virtuelle IP-Adresse (aus der virtuellen Maschinen **Dashboard** -Registerkarte) und der öffentliche Endpunkt für ssh Linux Servder anmelden.
6. Extrahieren Sie die Dateien aus dem gezippte Linux master Target Server Installer Tar-Archiv mit: *"tar-Xvzf Microsoft ASR\_UA\_8.2.0.0\_RHEL6 64\"* aus dem Verzeichnis, das die Installer-Datei enthält.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. Wenn Sie extrahiert den Installer Dateien in ein anderes Verzeichnis in das Verzeichnis zu ändern das Inhaltsverzeichnis Tar archiviert wurden extrahiert. Von diesem Pfad ausgeführt "Sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. Wenn aufgefordert, eine primäre Rolle wählen Sie **2 (Master-Ziel)**. Lassen Sie die interaktive Installationsoptionen auf ihre Standardwerte.
9. Warten Sie Installation fortgesetzt und Host-Konfigurationsschnittstelle angezeigt werden. Host-Konfigurationsdienstprogramm für Linux master Sarget Server ist ein Befehlszeilen-Dienstprogramm. Größe nicht die ssh Client Dienstprogrammfenster. Mithilfe der Pfeiltasten die Option **Global** auswählen und drücken die EINGABETASTE auf der Tastatur.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)


10. Geben Sie im Feld **IP** die interne IP-Adresse dem Konfigurationsserver (Sie finden es im **Dashboard** der Registerkarte Konfigurationsserver VM) und drücken Sie die EINGABETASTE. Geben Sie im **Anschluss** **22** und drücken Sie die EINGABETASTE. 
11.  Lassen Sie **Verwenden HTTPS** als **Ja**, und drücken Sie die EINGABETASTE.
12.  Geben Sie die Passphrase generiert wurde auf dem Konfigurationsserver. Verwenden Sie PUTTY-Client aus einem Windows-Computer zu ssh auf virtuellen Linux-Maschine können UMSCHALT + EINFG Sie den Inhalt der Zwischenablage einfügen. Das Kennwort in die lokale Zwischenablage mit STRG + C kopieren und Einfügen UMSCHALT + EINFG verwenden. Drücken Sie die EINGABETASTE.
13.  Mithilfe der Pfeiltaste navigieren beenden, und drücken Sie die EINGABETASTE. Warten auf Abschluss der Installation.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Wenn aus irgendeinem Grund nicht registrieren Ihre master Zielserver Linux Server Konfiguration möglich wieder mit Host-Konfigurationsprogramm aus /usr/local/ASR/Vx/bin/hostconfigcli (Sie müssen zunächst in dieses Verzeichnis Zugriffsberechtigungen mit Chmod als super-User).


#### <a name="validate-master-target-registration"></a>Master-Ziel-Registrierung überprüfen

Sie können überprüfen, ob der master Zielserver Konfigurationsserver in Azure Site Recovery Tresor erfolgreich registriert wurde > **Configuration Server** > **Server-Details**.

>[AZURE.NOTE] Nach der Registrierung des master Zielserver ist Fehlermeldungen Konfiguration, die der virtuelle Computer wurde gelöscht von Azure oder Endpunkte sind nicht ordnungsgemäß konfiguriert, denn auch master Zielkonfiguration von Azure Dndpoints erkannt und master Ziel in Azure bereitgestellt wird, dies ist für eine lokale master Target Server lokal. Dies hat keine Auswirkung auf Failback und diese Fehler ignorieren. 



## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Schritt 4: Schützen der virtuellen Computer im lokalen Standort

Sie müssen VMs auf der lokalen Website schützen, bevor Sie ein Failback.

### <a name="before-you-begin"></a>Bevor Sie beginnen

Bei eine VM in Azure ist, fügt zusätzliche temp-Laufwerk für die Auslagerungsdatei hinzu. Dies ist ein zusätzliches Laufwerk normalerweise nicht erforderlich ist, von der fehlgeschlagenen über VM, da bereits ein Laufwerk für die Auslagerungsdatei möglicherweise. Vor reverse Schutz der virtuellen Computer müssen Sie sicherstellen, dass das Laufwerk offline geschaltet wird, damit es nicht geschützt ist. Hierzu wie folgt:

1.  Öffnen Sie Computermanagement und wählen Sie Speichermanagement, listet der Datenträger online und an den Computer angeschlossen.
2.  Wählen Sie die temporäre Diskette an den Computer angeschlossen und offline schalten. 

### <a name="protect-the-vms"></a>Schützen Sie die VMs

1. Überprüfen Sie in Azure-Portal Status des virtuellen Computers, um sicherzustellen, dass sie über Fehler sind.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. VContinuum auf Ihrem Computer zu starten.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. **Schutz** auf und wählen Sie den Betriebssystemtyp der 

4.  In neuem Fenster öffnen den **Betriebssystemtyp**auswählen > VMs zu Failback**Details abrufen** . **Details zur primären Server**identifizieren und die virtuellen Computer, die Sie schützen möchten. VMs werden unter den Hostnamen vCenter aufgeführt, die vor dem Failover erhalten.
5.  Wenn Sie einen virtuellen Computer zu aktivieren (bereits fehlgeschlagen in Azure) bietet ein Popupfenster zwei Einträge für den virtuellen Computer. Ist der Konfigurationsserver erkennt zwei Instanzen virtueller Computer registriert. Sie müssen den Eintrag für die lokale VM entfernen, damit Sie die richtige VM schützen können. Ermittlung den richtigen Azure VM Eintrag können Sie die VM Azure anmelden und auf C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc. Identifizieren Sie drscout.conf Datei der Host-ID Halten Sie im Dialogfeld vContinuum für die Host-ID auf dem virtuellen Computer gefunden. Löschen Sie alle Einträge. Wählen Sie die richtige VM können Sie die IP-Adresse verweisen. Die IP-Adresse Bereich lokal werden lokale VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. Beenden Sie auf dem vCenter Server den virtuellen Computer. Sie können auch die VMs lokal löschen.
7. Geben Sie den lokalen MT Server Sie VMs schützen möchten. Schließen Sie hierzu vCenter Server Failback werden soll.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. Wählen Sie den master Zielserver basierend auf dem Host die VM wiederhergestellt werden soll.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. Geben Sie die Replikationsoption für jeden virtuellen Computer. Dazu müssen Sie Recovery Seite Datastore auswählen, die VMs wiederhergestellt werden. Die Tabelle zeigt die verschiedenen Optionen für jede VM zu müssen.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **Option** | **Wert empfohlen**
    ---|---
    Prozessserver IP-Adresse | Wählen Sie den Prozessserver in Azure bereitgestellt
    Aufbewahrung Größe in MB| 
    Wert für die Beibehaltungsdauer | 1
    Stunden / | Tage
    Konsistenz-Intervall | 1
    Wählen Sie Ziel-Datenspeicher | Datenspeicher verfügbar auf Recovery-Standort. Datenspeicher sollte genügend Speicherplatz und zur ESX-Host, die Sie auf dem virtuellen Computer wiederherstellen.


10. Konfigurieren Sie die Eigenschaften der virtuellen Maschine nach Failover auf lokalen Website erwerben. Die Eigenschaften werden in der folgenden Tabelle zusammengefasst.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    **Eigenschaft** | **Details**
    ---|---
    Netzwerkkonfiguration| Wählen sie für jeden Netzwerkadapter erkannt und klicken Sie auf **Ändern** , um Failback IP-Adresse für den virtuellen Computer konfigurieren. 
    Hardware-Konfiguration| Geben Sie die CPU und den Speicher für die VM. Einstellungen können die VMs zugewiesen werden, die Sie schützen möchten. Die richtigen Werte für die CPU und den Speicher um zu ermitteln, kann IAAS VMs Rolle Größe finden und die Anzahl der Kerne und Speicher zugewiesen.
    Angezeigter name | Nach dem Failback lokal können Sie die virtuellen Computer umbenennen, in vCenter Lagerbestand angezeigt werden können. Der Standardname ist der virtuellen Computer Hostname. Ermittlung der Name finden Sie die VM-Liste in der Schutzgruppe.
    NAT-Konfiguration | Im folgenden ausführlich erläutert

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>NAT konfigurieren

1. Zum Schutz der virtuellen Computer zu aktivieren, müssen zwei Kommunikationskanäle hergestellt werden. Der erste Kanal ist zwischen dem virtuellen Computer und den Prozess-Server. Dieser Kanal sammelt die Daten aus der VM und den Prozess-Server sendet dann die Daten auf dem master Zielserver an. Sind Prozess-Server und der virtuelle Computer geschützt werden im selben Azure virtuelle Netzwerk müssen Sie NAT verwendet. Andernfalls geben Sie NAT-Einstellungen. Die öffentliche IP-Adresse des Prozessservers in Azure anzeigen 

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. Der zweite Kanal ist zwischen den Prozess-Server und dem master Zielserver. NAT verwenden können, hängt davon ab, ob Sie eine VPN-basierte Verbindung oder über das Internet kommunizieren. Wählen Sie NAt verwenden Sie VPN, aber nur, wenn Sie eine Internet-Verbindung verwenden.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. Wenn Sie die lokalen virtuellen Computer wie gelöscht haben und Datenspeicher, den Sie nicht wieder weiterhin der alten VMDK enthält müssen Sie sicherstellen, dass das Failback VM in eine neue Position erstellt wird. Dazu wählen Sie das **Erweitert** und geben Sie einen anderen Ordner im **Ordner Namen**wiederherstellen. Lassen Sie die anderen Optionen mit den Standardeinstellungen. Ordner Namen Einstellungen auf alle Server anwenden.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. Führen Sie Readiness überprüfen um sicherzustellen, dass die virtuellen Computer auf lokalen bereit. 

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)


5. Warten auf den Abschluss. Wenn es erfolgreich auf VMs können einen Namen für den Schutzplan angeben. Klicken Sie auf **schützen** soll.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)


6. Sie können in vContinuum überwachen.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. Überwachen Sie nach Schritt **Schutzplan Aktivierung** abgeschlossen VM Schutz Site Recovery-Portal.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. Den genauen Status auf die VM und die Fortschritte sehen.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Vorbereiten des Failback-Plans

Sie bereiten einen Failback Plan vContinuum, damit die Anwendung auf der lokalen Website jederzeit ausgeführt werden kann. Die Wiederauffuellungspläne ähneln Wiederauffuellungspläne im Site Recovery.

1.  VContinuum starten und **Verwalten Pläne**auswählen > **Wiederherstellen.** Sie sehen Liste aller Pläne, die VMs Failover verwendet wurden. Dieselben Pläne können Sie wiederherstellen.

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. Wählen Sie die Schutzplan und alle VMs, die sie wiederherstellen möchten. Wenn Sie jede VM auswählen sehen Sie weitere Details wie Ziel-ESX-Server und dem Quelldatenträger VM. Klicken Sie auf **nächste** wiederherstellen-Assistenten und wählen die VMs, die Sie wiederherstellen möchten.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. Sie können auf mehrere Optionen wiederherstellen, aber wir empfehlen Sie **Aktuelle Tag** und **Übernehmen für alle virtuellen Computer** , um sicherzustellen, dass die neuesten Daten vom virtuellen Computer verwendet werden.
4. Führen Sie die **Kontrollkästchen Readiness.** Überprüft die richtigen Parameter für VM Recovery konfiguriert werden. Klicken Sie auf **Weiter** , wenn die Überprüfung erfolgreich ausgeführt werden. Wenn nicht im Ereignisprotokoll, und beheben Sie die Fehler.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  **VM-Konfiguration** sicher, dass die Wiederherstellung ordnungsgemäß eingestellt sind. Sie können die VM-Einstellung bei Bedarf ändern.

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. Überprüfen Sie die Liste der virtuellen Computer, die wiederhergestellt werden und eine Wiederherstellung geben. Beachten Sie, dass VMs mit dem Hostnamen des Computers aufgelistet sind. Es eventuell schwierig, den virtuellen Computer Computer-Hostnamen zuordnen.
Zu den Namen, den virtuellen Computern in Azure **Dashboard** überprüfen VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. Geben Sie einen Plan, und wählen Sie **später wiederherstellen**. Es wird empfohlen, später wiederherstellen, da der ursprüngliche Schutz nicht abgeschlossen. 
11. Klicken Sie auf den Plan speichern oder die Wiederherstellung ausgelöst werden, wenn ausgewählte und nicht wiederherstellen **Wiederherstellen** . Überprüfen Sie den Wiederherstellungsstatus um festzustellen, ob der Plan gespeichert ist.

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Wiederherstellung virtueller Maschinen

Nachdem der Plan erstellt wurde, können Sie die virtuellen Computer wiederherstellen. Überprüfen Sie vor der virtuellen Computer Synchronisierung abgeschlossen haben. Replikationsstatus OK zeigt der Schutz abgeschlossen ist und der RPO-Schwellenwert erreicht wurde. Sie können Health VM Eigenschaften überprüfen.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Deaktivieren Sie vor dem Starten der Wiederherstellung Azure virtuelle Computer. Dadurch ist kein Split Brain und Benutzer nur eine Kopie der Anwendung zugreifen. 


1.  Starten Sie gespeicherten Plan. Wählen Sie im vContinuum Pläne **Überwachen** . Zeigt alle Pläne, die ausgeführt wurden.

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  Wählen Sie den Plan in **Recovery** , und klicken Sie auf **Start**. Sie können die Wiederherstellung überwachen. Nach Aktivierung der VMs auf Sie können Sie in vCenter.

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Schützen Sie wieder Failback Azure

Nach Abschluss der Failback werden soll die virtuellen Computer erneut schützen. Hierzu wie folgt:

1.  Überprüfen Sie, dass die virtuellen Computer lokal arbeiten und erreichbar sind.
2.  Im Portal Azure Site Recovery virtueller Computer auswählen und löschen. Wählen Sie den Schutz der virtuellen Computer deaktivieren. Dadurch, dass die virtuellen Computer nicht mehr geschützt sind.
3.  Löschen Sie in Azure fehlgeschlagenen über Azure virtuelle Computer
4.  Löschen Sie alte virtuelle Maschine auf vSpehere. Dies sind die zuvor Azure Failover VMs.
5.  Site Recovery-Portal schützen vor kurzem Failover VMs. Nachdem sie geschützt können Sie einen Wiederherstellungsplan hinzufügen werden.
 
## <a name="next-steps"></a>Nächste Schritte



- [Informationen über](site-recovery-vmware-to-azure-classic.md) virtuelle VMware-Maschinen und physische Server in Azure die verbesserte Bereitstellung replizieren.

 
