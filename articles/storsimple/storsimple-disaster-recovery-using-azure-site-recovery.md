<properties 
   pageTitle="Automatisieren von DR für Dateifreigaben in Azure Site Recovery mit StorSimple | Microsoft Azure"
   description="Beschreibt die Schritte und best Practices für eine Disaster Recovery-Lösung für Dateifreigaben auf StorSimple Speicher erstellen."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatisierte Disaster Recovery-Lösung mit Azure Site Recovery für Dateifreigaben auf StorSimple

## <a name="overview"></a>Übersicht

Microsoft Azure StorSimple ist eine hybride Cloud, die die Komplexität von unstrukturierten Daten häufig Dateifreigaben behebt. StorSimple verwendet Cloud-Speicher als Erweiterung der lokalen Lösung und automatisch Ebenen Daten lokal und cloudspeicher. Integrierte Datenschutz mit cloud-Snapshots und beseitigt die Notwendigkeit einer großen Speicherinfrastruktur.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) ist ein Azure-basierter Dienst, der Disaster Recovery (DR) Funktionen bereitstellt, durch Replikation, Failover und Recovery von virtuellen Maschinen orchestriert. Azure Site Recovery unterstützt Replikation Technologien konsequent replizieren, schützen und nahtlos Failover von virtuellen Maschinen und Applikationen Private/öffentliche oder gehosteten Wolken.

Azure Site Recovery, virtuellen Replikation und StorSimple Cloud Snapshot-Funktionen verwenden, um die vollständige Datei-Server-Umgebung zu schützen. Bei einer Unterbrechung können Sie einen Klick zu Ihrem-Dateifreigaben in Azure in wenigen Minuten.

Dieses Dokument erläutert ausführlich, wie Sie eine Disaster Recovery-Lösung für die Dateifreigaben auf StorSimple Speicher erstellen und geplante und ungeplante ausgeführt und Failover mit einem Mausklick Wiederherstellungsplan getestet. Im Wesentlichen wird wie Sie den Plan für die Wiederherstellung in Ihrem Tresor Azure Site Recovery ermöglichen StorSimple Failover bei Notfällen ändern können. Außerdem beschreibt es unterstützten Konfigurationen und Komponenten. Dieses Dokument geht davon aus, dass Sie die Grundlagen der Azure Site Recovery und StorSimple Architekturen sind.

## <a name="supported-azure-site-recovery-deployment-options"></a>Unterstützte Azure Site Recovery-Bereitstellungsoptionen

Kunden können Dateiserver als physische Server und virtuellen Computern (VMs) auf Hyper-V oder VMware bereitstellen und Dateifreigaben von Graviert aus StorSimple-Volumes erstellen. Azure Site Recovery können physische und virtuelle Bereitstellung an einem sekundären Standort und Azure schützen. Dieses Dokument erläutert Details einer DR-Lösung Azure als Recovery-Standort für einen Dateiserver, den Hyper-V-VM gehostet, und Dateifreigaben auf StorSimple Speicher. Andere Szenarien in der Dateiserver VM auf VMware VM oder einem physikalischen Computer können ebenso implementiert werden.

## <a name="prerequisites"></a>Erforderliche Komponenten

Implementierung einer per Mausklick Disaster Recovery Azure Site Recovery für Dateifreigaben auf StorSimple Speicher hat die folgenden Komponenten:

-   Lokale Windows Server 2012 R2 Dateiserver VM auf Hyper-V, VMware oder einem physischen Computer gehostet

-   StorSimple Speicher Gerät lokal registriert Azure StorSimple manager

-   StorSimple Cloud Appliance erstellt in Azure StorSimple Manager (Dies kann Herunterfahren Status beibehalten)

-   Dateifreigaben auf Volumes auf dem Datenträger StorSimple konfiguriert

-   [Azure Site Recovery Services Depot](../site-recovery/site-recovery-vmm-to-vmm.md) in ein Microsoft Azure-Abonnement erstellt wurde

Darüber hinaus ist Azure Recovery-Standort führen Sie [Azure Virtual Machine Readiness Assessment Tool](http://azure.microsoft.com/downloads/vm-readiness-assessment/) auf VMs mit Azure VMs und Azure Site Recovery Services kompatibel sind.

Zu Latenz Probleme (die höheren Kosten führen), erstellen Sie Ihr StorSimple Cloud Appliance Automation-Konto und Speicher-Konten im selben Bereich.

## <a name="enable-dr-for-storsimple-file-shares"></a>DR für Dateifreigaben StorSimple aktivieren  

Jede Komponente der lokalen Umgebung muss geschützt werden, um die vollständige Replikation und Recovery zu ermöglichen. In diesem Abschnitt wird beschrieben, wie Sie:

-   Einrichten von Active Directory und DNS-Replikation (optional)

-   Verwenden von Azure Site Recovery Schutz Dateiservers VM aktivieren

-   Schutz von StorSimple-volumes

-   Konfigurieren des Netzwerks

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Einrichten von Active Directory und DNS-Replikation (optional)

Wenn Sie Active Directory und DNS ausgeführt, sodass sie auf DR-Standort verfügbaren Computer schützen möchten, müssen Sie explizit geschützt (Dateiserver nach dem Failover mit Authentifizierung zugegriffen werden). Es gibt zwei empfohlene Optionen je nach Komplexität der Umgebung des Kunden vor Ort.

#### <a name="option-1"></a>Option 1

Hat der Kunde eine kleine Anzahl von Applikationen, ein einzelnen Domänencontroller für die gesamte lokale Website und dann den Domänencontrollercomputer an einem sekundären Standort replizieren (Dies gilt für Standort-zu-Standort- und Website Azure) mit Azure Site Recovery-Replikation über die gesamte Website fehlschlägt.

#### <a name="option-2"></a>Option 2

Wenn den Kunden eine große Anzahl von Applikationen, Active Directory-Gesamtstruktur ausgeführt wird und über einige Programme gleichzeitig fehlerhaft, dann empfehlen wir die Einrichtung eines zusätzlichen Domänencontrollers auf DR-Standort (ein sekundärer Standort oder in Azure).

Siehe [Automatische DR-Lösung für Active Directory und DNS mit Azure Site Recovery](../site-recovery/site-recovery-active-directory.md) Anleitung bei einem Domänencontroller auf DR-Standort. Der Rest dieses Dokuments wird davon ausgegangen, am DR-Standort ein Domänencontroller verfügbar ist.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Verwenden von Azure Site Recovery Schutz Dateiservers VM aktivieren

Dieser Schritt erfordert, dass die lokalen Dateiserver-Umgebung vorbereiten, erstellen ein Depot Azure Site Recovery vorbereiten und Datei Schutz der VM.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Vor-Ort-Dateiserver-Umgebung vorbereiten

1.  Legen Sie die **Benutzerkontensteuerung** auf **Nie benachrichtigen**. Dies ist erforderlich, damit Sie Azure Automatisierungsskripts verbinden iSCSI-Ziele nach Fehler von Azure Site Recovery verwenden können.

    1.  Drücken Sie die Windows-Taste + Q und nach **UAC**.

    2.  Wählen Sie **Die Benutzerkontensteuerung ändern**.

    3.  Ziehen Sie die Leiste nach unten in Richtung **Nie benachrichtigen**.

    4.  Klicken Sie auf **OK** , und wählen Sie dann **Ja** .

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  Installieren der VM-Agent auf jedem Dateiserver VMs. Dies ist erforderlich, damit auf die fehlgeschlagene Azure Automatisierungsskripts über VMs ausgeführt.

    1.  Zum [herunterladen den Agent](http://aka.ms/vmagentwin) `C:\\Users\\<username>\\Downloads`.

    2.  Öffnen Sie Windows PowerShell im Administratormodus (Ausführen als Administrator) und geben Sie den folgenden Befehl zum Downloadpfad navigieren:

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] Der Dateiname kann je nach Version ändern.

1.  Klicken Sie auf **Weiter**.

2.  **Der Vereinbarung** stimmen Sie zu, und klicken Sie auf **Weiter**.

3.  Klicken Sie auf **Fertig stellen**.


1.  Erstellen Sie Dateifreigaben mit Volumes aus StorSimple darzustellen. Weitere Informationen finden Sie unter [Verwenden der StorSimple Manager-Dienst zu managen](storsimple-manage-volumes.md).

    1.  Drücken Sie auf Ihrer lokalen virtuellen Computer Windows-Taste + Q und **iSCSI**suchen.

    2.  Wählen Sie **iSCSI-Initiator**.

    3.  Wählen Sie die Registerkarte **Konfiguration** , und kopieren Sie den Initiatornamen.

    4.  Melden Sie sich im [klassischen Azure-Portal](https://manage.windowsazure.com/).

    5.  Wählen Sie die Registerkarte **StorSimple** , und wählen Sie StorSimple Manager-Dienst, der das physische Gerät enthält.

    6.  Volume-Container erstellen und Volumes erstellen. (Diese Datenträger sind für die Datei Freigabe(n) auf VMs). Kopieren Sie den Initiatornamen und geben Sie einen passenden Namen für die Access Control Datensätze beim Erstellen von Volumes.

    7.  Wählen Sie die Registerkarte **Konfigurieren** und notieren Sie die IP-Adresse des Geräts.

    8.  Auf Ihrem lokalen VMs wieder an den **iSCSI-Initiator** , und geben Sie die IP-Adresse im Abschnitt Quick Connect. Klicken Sie auf **Quick Connect** (das Gerät sollte jetzt verbunden).

    9.  Öffnen Sie Azure-Verwaltungsportal, und wählen Sie die Registerkarte **Volumes und Geräte** . Klicken Sie auf **automatisch konfigurieren**. Das soeben erstellte Volume sollte angezeigt werden.

    10. Im Portal, wählen Sie die Registerkarte **Geräte** und dann **Erstellen Sie ein neues virtuelles Gerät.** (Virtuelle Gerät wird verwendet werden, wenn ein Failover auftritt). Dieses neue virtuelle Gerät kann im offline-Status zu Mehrkosten gehalten werden. Virtuelle Gerät offline, gehen zum Abschnitt **virtuellen Maschinen** auf dem Portal und Herunterfahren.

    11. Zurück zur lokalen VMs und öffnen Sie die Datenträgerverwaltung (Windows-Taste + X drücken, und wählen Sie **Disk Management**).

    12. Beachten Sie einige zusätzlichen Festplatten (abhängig von der Anzahl der Datenträger, die Sie erstellt haben). Zuerst klicken, wählen Sie **Datenträger initialisieren**und **OK**. Klicken Sie im Abschnitt **nicht zugeordnet** , wählen Sie **Neues einfaches Volume**weisen Sie einen Laufwerkbuchstaben zu und beenden Sie den Assistenten.

    13. Wiederholen Sie Schritt l für alle Laufwerke. Sie sehen jetzt alle Datenträger auf **Diesem PC** in Windows Explorer.

    14. Verwenden Sie Datei- und Speicherdienste Rolle erstellen von Dateifreigaben auf diesen Datenträger.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Erstellen und Vorbereiten von Azure Site Recovery-Tresor

Finden Sie in der [Dokumentation zu Azure Site Recovery](../site-recovery/site-recovery-hyper-v-site-to-azure.md) Einstieg mit Azure Site Recovery vor Schutz Dateiservers VM.

#### <a name="to-enable-protection"></a>Schutz aktivieren

1.  Trennen Sie iSCSI-Ziel von lokalen VMs, die über Azure Site Recovery schützen möchten:

    1.  Drücken Sie die Windows-Taste + Q und **iSCSI**suchen.

    2.  Wählen Sie **iSCSI-Initiator**.

    3.  Trennen Sie das Gerät StorSimple, das bereits verbunden. Alternativ können Sie den Dateiserver ausschalten, einige Minuten beim Schutz aktivieren.

    > [AZURE.NOTE] Dies bewirkt die Dateifreigaben, vorübergehend nicht verfügbar

1.  [Virtuelle Computer Schutz](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) Dateiservers VM von Azure Site Recovery-Portal.

2.  Zu Beginn die erste Synchronisierung kann das Ziel wieder wiederhergestellt werden. Fahren Sie mit den iSCSI-Initiator, wählen Sie das StorSimple und klicken Sie auf **Verbinden**.

3.  Wenn die Synchronisation abgeschlossen ist und des Status der VM **geschützt**, die VMs auswählen wählen Sie die Registerkarte **Konfigurieren** und das Netzwerk der VM entsprechend aktualisieren (Dies ist im Netzwerk, die über Unterbrechung fehlerhafte Teil). Wenn das Netzwerk nicht angezeigt wird, bedeutet das, dass die Synchronisierung noch läuft.

### <a name="enable-protection-of-storsimple-volumes"></a>Schutz von StorSimple-volumes

Wenn Sie nicht die Option **aktivieren standardmäßig Backup für dieses Volume** für die StorSimple-Volumes ausgewählt haben, zur **Backup-Policies** in der StorSimple Manager-Dienst und eine geeignete Sicherungsrichtlinie für alle Volumes erstellen. Wir empfehlen, die Häufigkeit von Backups auf Recovery Point Objective (RPO) festzulegen, die für die Anwendung angezeigt werden soll.

### <a name="configure-the-network"></a>Konfigurieren des Netzwerks

Konfigurieren Sie für Dateiserver VM Einstellungen in Azure Site Recovery VM-Netzwerke nach einem Failover mit richtigen DR-Netzwerk verbunden sind.

Sie können die VM in **VMM-Cloud** oder der **Gruppe** das Netzwerk konfigurieren auswählen, wie in der folgenden Abbildung dargestellt.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

Sie können einen Wiederherstellungsplan in ASR automatisiert Failover Dateifreigaben erstellen. Wenn eine Störung auftritt, können Sie Dateifreigaben in ein paar Minuten mit nur einem einzigen Klick bringen. Um die Automatisierung zu aktivieren, benötigen Sie ein Konto Azure Automation.

#### <a name="to-create-the-account"></a>Konto erstellen

1.  Zur klassischen Azure-Portal, und gehen Sie zum Abschnitt **Automatisierung** .

1.  Erstellen Sie ein neues automatisierungskonto. Bewahren Sie es an derselben Geo/Region in der StorSimple Cloud Appliance und Speicherkonten erstellt wurden.

2.  Klicken Sie auf **neu** &gt; **App Services** &gt; **Automatisierung** &gt; **Runbook** &gt; **Aus Galerie** erforderlichen Runbooks in Automation-Konto importieren.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  Fügen Sie die folgenden Runbooks im Bereich **Disaster Recovery** im Katalog:

    -   Ein Failover StorSimple volumecontainer

    -   Bereinigen der StorSimple-Volumes nach Test-Failover (TFO)

    -   Bereitstellen von Volumes auf StorSimple-Gerät nach einem failover

    -   Virtuelles Gerät StorSimple starten

    -   Benutzerdefiniertes Skript Erweiterung in Azure VM deinstallieren

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Veröffentlichen Sie alle Skripts Runbook in Automation-Konto auswählen und **Autor** Registerkarte. Nach diesem Schritt wird die Registerkarte **Runbooks** folgendermaßen angezeigt:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  Im Automation-Konto zur Registerkarte **Anlagen** , klicken Sie auf **Hinzufügen** &gt; **Anmeldeinformationen hinzufügen**, und fügen Sie Ihre Azure-Anmeldeinformationen – name die Anlage AzureCredential.

    Verwenden Sie die Windows PowerShell-Anmeldeinformationen. Dies sollte Anmeldeinformationen, die eine Organisations-ID-Benutzernamen und ein Kennwort mit Zugriff auf diese Azure-Abonnement und mehrstufige Authentifizierung deaktiviert enthält. Dies ist erforderlich, während der Failover für den Benutzer zu authentifizieren und Datei-Server-Volumes auf DR-Standort.

1.  Im automatisierungskonto Registerkarte **Anlagen** und dann auf **Einstellung hinzufügen** &gt; **Variable hinzufügen** und fügen Sie die folgenden Variablen hinzu. Sie können diese Vermögenswerte zu verschlüsseln. Recovery Plan vorgesehen werden. Planen der Wiederherstellung (das wird im nächsten Schritt erstellt) ist ein TestPlan, wenn Variablen sollte ein TestPlan StorSimRegKey AzureSubscriptionName ein TestPlan, und.

    -   *RecoveryPlanName* **-StorSimRegKey**: der Registrierungsschlüssel für den StorSimple-Manager-Dienst.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: der Name der Azure-Abonnement.

    -   *RecoveryPlanName* **ResourceName-**: der Name der StorSimple-Ressource, die das Gerät StorSimple hat.

    -   *RecoveryPlanName* **-Gerätename**: Gerät mit Failover.

    -   *RecoveryPlanName* **-TargetDeviceName**: StorSimple Cloud Appliance welche Container sind ein Failover.

    -   *RecoveryPlanName* **-VolumeContainers**: eine durch Trennzeichen getrennte Zeichenfolge Volume Container auf dem Gerät, die fehlgeschlagen. z. B. volcon1, volcon2, volcon3.

    -   RecoveryPlanName**-TargetDeviceDnsName**: der Dienstname des Zielgeräts (diese finden Sie im Abschnitt **virtuellen** : der Dienstname ist der DNS-Namen).

    -   *RecoveryPlanName* **-StorageAccountName**: der Name des Speicher, in dem Skript (die auf den fehlerhaften über VM ausführen) gespeichert. Dies ist Speicherkonto, die Speicherplatz Skript Zwischenspeichern.

    -   *RecoveryPlanName* **-StorageAccountKey**: die Zugriffstaste für das Speicherkonto oben.

    -   *RecoveryPlanName* **-ScriptContainer**: der Name des Containers, in dem das Skript in der Cloud gespeichert werden. Wenn der Container vorhanden ist, wird sie erstellt.

    -   *RecoveryPlanName* **-VMGUIDS**: auf einen virtuellen Computer schützen, weist Azure Site Recovery jede VM eine eindeutige ID, die die Details des über VM gibt. Der VMGUID wählen Sie die Registerkarte **Recovery Services** und klicken Sie auf **Element geschützt** &gt; **Gruppen** &gt; **Maschinen** &gt; **Eigenschaften**. Haben Sie mehrere virtuelle Computer, fügen Sie die GUIDs als durch Trennzeichen getrennte Zeichenfolge.

    -   *RecoveryPlanName* **-AutomationAccountName** – der Name des Kontos Automatisierung in der Sie die Runbooks und Vermögenswerte hinzugefügt haben.

    Beispielsweise ist der Name des Recovery-Plans FileServerpredayRP, erscheint dann die Registerkarte **Ressourcen** wie folgt nach dem Hinzufügen der Ressourcen.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Gehen Sie zum Abschnitt **Recovery Services** , und wählen Sie Azure Site Recovery-Depot, das Sie zuvor erstellt haben.

2.  Registerkarte **Wiederherstellungspläne** und einen neuen Wiederherstellungsplan wie folgt erstellen:

    ein.  Geben Sie einen Namen und wählen Sie die entsprechende **Gruppe**.

    b.  Wählen Sie die VMs aus der Schutzgruppe im Wiederherstellungsplan enthalten sein sollen.

    c.  Nach der Wiederherstellung erstellt wird, wählen sie Recovery Plan Anpassung Ansicht öffnen.

    d.  Wählen Sie **Herunterfahren, alle Gruppen**, klicken Sie auf **Script**und wählen Sie **eine primäre Skript vor dem Herunterfahren alle Gruppe hinzufügen**.

    e.  Wählen Sie das Automatisierung (in dem Sie die Runbooks hinzugefügt), und wählen Sie dann Runbook **über StorSimple Volume Container fehl** .

    f.  Klicken Sie auf **Gruppe 1: Starten Sie**, **virtuelle Computer**auswählen und Hinzufügen von VMs, die der Sanierungsplan geschützt werden.

    g.  Klicken Sie auf **Gruppe 1: Start** **Skript**auswählen und die folgenden Skripts in der Reihenfolge **nach Gruppe 1** Schritte hinzufügen.

    - Start-StorSimple-Virtual-Appliance runbook
    - Über StorSimple Volume Container Runbook fehl
    - Mount Volumes nach dem Failover runbook
    - Deinstallieren-Custom-Skript-Erweiterung runbook

1.  Hinzufügen einer manuelle Aktion nach oben 4 Skripts in derselben **Gruppe 1: nach der Schritte** Abschnitt. Diese Aktion ist der Punkt, an dem Sie überprüfen können, ob alles ordnungsgemäß funktioniert. Diese Aktion muss nur ein testfailover (nur auf **Failover testen** Kontrollkästchen) hinzugefügt werden.

2.  Fügen Sie nach der manuellen Aktion Bereinigungsskript mit dem gleichen Verfahren für die Runbooks verwendet hinzu. Speichern Sie den Wiederherstellungsplan.

    > [AZURE.NOTE] Ausführung ein testfailover sollten Sie alles Schritt manuell überprüfen, da StorSimple-Volumes, die auf dem Zielgerät geklont wurde nach Abschluss manuelle Aktion als Teil der Bereinigung gelöscht werden.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Führen Sie ein testfailover

Finden Sie im Handbuch [Active Directory DR-Lösung](../site-recovery/site-recovery-active-directory.md) Begleiter für bestimmte Aspekte zu Active Directory während des Failovers Test. Lokale Installation wird bei einem Failover der Test nicht überhaupt gestört. StorSimple-Volumes, die die lokale VM zugeordnet wurden, an die Cloud Appliance Azure StorSimple geklont. VM zu Testzwecken wird oben in Azure und geklonte Volumes VM zugeordnet sind.

#### <a name="to-perform-the-test-failover"></a>Das testfailover ausführen

1.  Wählen Sie in der Azure-Verwaltungsportal Site Recovery Vault.

1.  Klicken Sie auf den Wiederherstellungsplan für den Dateiserver VM erstellt.

2.  Klicken Sie auf **Testfailover**.

3.  Wählen Sie das virtuelle Netzwerk Test-Failover-Prozess zu starten.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  Bei die sekundäre Umgebung eingerichtet ist, können Sie die Validierung durchführen.

2.  Wenn die Prüfung abgeschlossen sind, klicken Sie auf **Prüfung abgeschlossen**. Die Umgebung für Failover gesäubert und der TFO-Vorgang abgeschlossen.

## <a name="perform-an-unplanned-failover"></a>Führen Sie ein ungeplantes Failovers

Bei einem ungeplanten Failover StorSimple-Volumes für das virtuelle Gerät Failover ein Replikat VM von Azure gebracht und Volumes VM zugeordnet sind.

#### <a name="to-perform-an-unplanned-failover"></a>Ein ungeplantes Failovers ausführen

1.  Wählen Sie in der Azure-Verwaltungsportal Site Recovery Vault.

1.  Klicken Sie auf den Wiederherstellungsplan für Dateiserver VM erstellt.

2.  Klicken Sie auf **Failover** und wählen Sie **Ungeplanten Failover**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Wählen Sie Zielnetzwerk aus und dann auf das Kontrollkästchen Symbol ✓ Failover zu starten.

## <a name="perform-a-planned-failover"></a>Durchführung eines geplanten failover

Bei einem geplanten Failover Datei lokalen Server virtueller Computer ordnungsgemäß heruntergefahren wird und ein backup der Volumes auf Gerät StorSimple Snapshots Cloud. StorSimple-Volumes für das virtuelle Gerät Failover ein Replikat VM ist auf Azure gebracht und Volumes VM zugeordnet sind.

#### <a name="to-perform-a-planned-failover"></a>Geplantes Failover ausführen

1.  Wählen Sie in der Azure-Verwaltungsportal Site Recovery Vault.

1.  Klicken Sie auf den Wiederherstellungsplan für den Dateiserver VM erstellt.

2.  Klicken Sie auf **Failover** und dann **Failover geplant**.

3.  Wählen Sie Zielnetzwerk aus und dann auf das Kontrollkästchen Symbol ✓ Failover zu starten.

## <a name="perform-a-failback"></a>Ein Failback durchführen

Bei einem Failback werden StorSimple volumecontainer auf das physische Gerät Failover nach einer Sicherung.

#### <a name="to-perform-a-failback"></a>Um ein Failback durchzuführen

1.  Wählen Sie in der Azure-Verwaltungsportal Site Recovery Vault.

1.  Klicken Sie auf den Wiederherstellungsplan für den Dateiserver VM erstellt.

2.  Auf **Failover** und wählen Sie **Failover geplanten** oder **ungeplanten Failover**.

3.  Klicken Sie auf **Richtung ändern**.

4.  Wählen Sie entsprechende Datensynchronisation und VM Optionen.

5.  Klicken Sie auf die Kontrollkästchen ✓ den Failback-Prozess gestartet.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Best Practices

### <a name="capacity-planning-and-readiness-assessment"></a>Kapazitäts-Planung und-Bereitschaft Bewertung


#### <a name="hyper-v-site"></a>Hyper-V-Website

Verwenden der [Benutzerkapazität Planner-Tool](http://www.microsoft.com/download/details.aspx?id=39057) Server, Speicher und Netzwerkinfrastruktur für die Hyper-V Replica Umgebung entwerfen.

#### <a name="azure"></a>Azure

Ausführen der [Azure Virtual Machine Readiness Assessment Tool](http://azure.microsoft.com/downloads/vm-readiness-assessment/) auf VMs mit Azure VMs und Azure Site Recovery Services kompatibel sind. Readiness Assessment Tool überprüft die VM-Konfigurationen und warnt bei Konfigurationen mit Azure nicht kompatibel sind. Beispielsweise gibt eine Warnung, wenn ein Laufwerk C: größer als 127 GB.


Planen der Kapazität von mindestens zwei wichtige Prozesse besteht:

-   Zuordnung lokalen Hyper-V VMs Azure VM-Größen (z. B. A6, A7 A8 und A9).

-   Bestimmen die erforderliche Internet-Bandbreite.

## <a name="limitations"></a>Grenzen

- Derzeit kann nur 1 StorSimple-Gerät (auf einer einzigen StorSimple Cloud Appliance) ein Failover. Das Szenario eines Dateiservers, die mehrere StorSimple Medien umfasst wird noch nicht unterstützt.

- Wenn Sie eine beim Aktivieren des Schutzes für einen virtuellen Computer Fehlermeldung stellen Sie sicher, dass Sie die iSCSI-Ziele getrennt.

- Alle durch backup-Policies auf volumecontainer gruppiert wurden volumecontainer werden gemeinsam ein Failover.

- Alle Volumes in der volumecontainern gewählte werden ein Failover.

- Volumes bis zu maximal 64 TB können nicht Failover ist maximal eine einzelne StorSimple Cloud Appliance 64 TB.

- Wenn das geplante/ungeplante Failover und VMs in Azure erstellt werden, dann nicht bereinigen VMs. Führen Sie stattdessen ein Failback. Löschen von VMs können nicht dann lokal VMs wieder aktiviert.

- Nach einem Failover sind Sie nicht die Volumes zu sehen, gehen Sie die VMs, öffnen Sie die Datenträgerverwaltung, lesen Sie die Datenträger und online schalten.

- In einigen Fällen können die Laufwerkbuchstaben in DR-Standort Buchstaben lokalen unterscheiden. In diesem Fall müssen Sie das Problem manuell zu beheben, nach Abschluss des Failovers.

- Mehrstufige Authentifizierung sollte für Azure Anmeldeinformationen deaktiviert werden, die die Automatisierung als Anlage Konto. Wenn diese Authentifizierung nicht deaktiviert ist, Skripts dürfen nicht automatisch ausgeführt, und der Wiederherstellungsplan fehl.

- Failover-Zeitlimit: das StorSimple-Skript Failover volumecontainer nimmt mehr Zeit als das Limit Azure Site Recovery per Skript (derzeit 120 Minuten) wird.

- Sicherungsauftrag Timeout: Zeitlimit der StorSimple Skript Backup der Volumes nimmt mehr Zeit als das Limit Azure Site Recovery per Skript (derzeit 120 Minuten).
 
    > [AZURE.IMPORTANT] Führen Sie die Sicherung von Azure-Portal manuell, und wiederholen Sie den Wiederherstellungsplan.

- Klonen Zeitlimit: das StorSimple-Skript ein Timeout erfolgt das Klonen von Volumes mehr Zeit überschreitet Azure Site Recovery per Skript (derzeit 120 Minuten).

- Synchronisation Fehler: der StorSimple Skripts Fehler besagt, dass die Sicherung nicht ausgeführt wurden, auch wenn die Sicherung erfolgreich im Portal. Eine mögliche Ursache dafür möglicherweise StorSimple Appliance Zeit möglicherweise nicht synchron mit der aktuellen Zeit in der Zeitzone.
 
    > [AZURE.IMPORTANT] Synchronisieren Sie die Zeit der Appliance mit der aktuellen Zeit in der Zeitzone.

- Failover-Anwendungsfehler: das StorSimple-Skript kann fehlschlagen, wenn wird ein Failover Appliance Recovery Plan ausgeführt wird.
    
    > [AZURE.IMPORTANT] Den Wiederherstellungsplan erneut, nach Abschluss des Failovers Appliance.

## <a name="summary"></a>Zusammenfassung

Azure Site Recovery verwenden, können einen vollständig automatisierte Disaster Recovery-Plan für einen Dateiserver VM mit Dateifreigaben auf StorSimple Speicher. Initiieren das Failover innerhalb von Sekunden überall bei Unterbrechung und erhalten die Anwendung innerhalb von wenigen Minuten.
