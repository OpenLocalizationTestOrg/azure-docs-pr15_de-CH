<properties
   pageTitle="Bereitstellen von StorSimple Virtual Array - Bereitstellung in VMware"
   description="Diese zweite Lernprogramm Bereitstellungsserie StorSimple Virtual Array umfasst die Bereitstellung eines virtuellen Geräts in VMware."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Bereitstellen von StorSimple Virtual Array - Bereitstellung virtuelles Array in VMware

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Übersicht 
Bereitstellung dieser praktischen Einführung gilt für StorSimple virtuelle Arrays (auch bekannt als StorSimple für lokale virtuelle Geräte oder StorSimple virtuelle Geräte) ausgeführten März 2016 allgemein verfügbar (GA) Freigabe. In diesem Lernprogramm beschreibt bereitstellen und StorSimple virtuelles Array auf einem Hostsystem mit VMware ESXi 5.5 und höher. Dieser Artikel bezieht sich auf die Bereitstellung von virtuellen StorSimple-Arrays in Azure-Verwaltungsportal als auch Microsoft Azure Government Cloud.

Sie benötigen Administratorrechte bereitstellen und zu einem virtuellen Gerät verbinden. Die Bereitstellung und anfängliche Einrichtung dauert ca. 10 Minuten.


## <a name="provisioning-prerequisites"></a>Bereitstellung erforderlicher Komponenten

Hier finden Sie die erforderlichen Komponenten auf einem Hostsystem VMware ESXi 5.5 ausführen und über ein virtuelles Gerät bereitstellen.

### <a name="for-the-storsimple-manager-service"></a>Für den StorSimple-Manager-Dienst

Bevor Sie beginnen, stellen Sie sicher, dass:

-   Sie haben alle Schritte [Vorbereiten Portal für StorSimple Virtual Array](storsimple-ova-deploy1-portal-prep.md)abgeschlossen.

-   Sie haben die virtuellen Geräts für VMware von Azure-Portal. Weitere Informationen finden Sie unter [Schritt 3: virtuellen Geräts downloaden](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>Für das virtuelle Gerät StorSimple 

Bevor Sie ein virtuelles Gerät bereitstellen, stellen Sie Folgendes sicher:

-   Sie haben Zugriff auf ein Hostsystem mit Hyper-V (2008 R2 oder höher) können zur Rückstellung ein.

-   Das Hostsystem ist Folgendes virtuelle Gerät bereitstellen widmen:

    -   Mindestens 4 Kernen.

    -   Mindestens 8 GB RAM.

    -   Eine Netzwerkschnittstelle.

    -   Ein 500 GB virtuellen Datenträger für Daten.

### <a name="for-the-network-in-datacenter"></a>Für das Netzwerk im Rechenzentrum 

Bevor Sie beginnen, stellen Sie sicher, dass:

-   Überprüft erforderlichen Netzwerken ein StorSimple virtuelles Gerät bereitgestellt und konfiguriert das Datacenter-Netzwerk gemäß den Vorschriften. Weitere Informationen finden Sie unter [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md).

## <a name="step-by-step-provisioning"></a>Schrittweise Bereitstellung 

Bereitstellen und zu einem virtuellen Gerät, müssen Sie die folgenden Schritte ausführen:

1.  Sicherstellen Sie, dass das Hostsystem ausreichende Ressourcen, um die minimale virtuelles Gerät erfüllen.

2.  Bereitstellen Sie ein virtuelles Gerät der Hypervisor.

3.  Virtuelle Gerät starten und die IP-Adresse.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Schritt 1: Sicherstellen Sie, dass Host-System mindestens virtuelles Gerät erfüllt

Erstellen Sie ein virtuelles Gerät benötigen Sie:

-   Zugriff auf ein Hostsystem mit VMware ESXi Server 5.5 und höher.

-   VMware vSphere-Client auf Ihrem System ESXi Host verwalten.

    -   Mindestens 4 Kernen.

    -   Mindestens 8 GB RAM.

    -   Eine Verbindung mit dem Netzwerk kann routing des Datenverkehrs mit Internet. Minimale Internetbandbreite sollte 5 Mbit/s optimale Funktionsweise des Geräts ermöglicht.

    -   Ein 500 GB virtuellen Datenträger für Daten.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Schritt 2: Bereitstellen eines virtuellen Geräts in hypervisor

Die folgenden Schritte ein virtuelles Gerät der Hypervisor bereit.

1.  Kopieren Sie virtuelles Geräteabbild auf Ihr System. Dies ist das Bild, das über die klassischen Azure-Portal herunterladen. 
    1.  Stellen Sie sicher, dass dies die neueste Datei, die Sie heruntergeladen haben. Wenn Sie das Bild bereits heruntergeladen, downloaden sie erneut, um sicherzustellen, dass Sie das aktuelle Bild. Das aktuelle Bild hat zwei Dateien (anstatt einer).
    2.  Notieren Sie sich den Speicherort, in dem das Bild kopiert, wie Sie diese später in der Prozedur verwenden.

2.  Mit dem Client vSphere ESXi-Server anmelden. Sie benötigen Administratorrechte, um einen virtuellen Computer erstellen.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  Wählen Sie im Client vSphere Abschnitt Bestand im linken ESXi Server.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  Die VMDK, zuerst ESXi Server hochgeladen werden. Navigieren Sie zu der Registerkarte **Konfiguration** im rechten Fensterbereich. Wählen Sie unter **Hardware** **Speicher**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  Wählen Sie im rechten Bereich unter **Datastores**Datenspeicher die VMDK-Datei hochgeladen werden soll. Der Datenspeicher muss genügend Speicherplatz für das Betriebssystem und Daten Datenträger.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Klicken Sie mit der rechten Maustaste und wählen Sie **Datastore durchsuchen**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  Ein **Datenspeicher** Browserfenster wird angezeigt.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Klicken Sie in der Tool-Leiste auf ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) Symbol, um einen neuen Ordner erstellen. Geben Sie den Namen des Ordners, und notieren Sie sich diese. Dieser Ordnername benötigen später beim Erstellen einer virtuellen Maschine (empfohlen wird empfohlen). Klicken Sie auf **OK**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  Der neue Ordner wird im linken Bereich des **Datastore-Browser**angezeigt.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Klicken Sie auf Upload ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) , und wählen Sie **Datei hochladen**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  Sie sollten nun durchsuchen und auf die VMDK-Dateien, die Sie heruntergeladen haben. Es werden zwei Dateien. Wählen Sie eine Datei hochladen.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Klicken Sie auf **Öffnen**. Den Upload VMDK-Datei an den angegebenen Datenspeicher wird jetzt gestartet. Die hochzuladende Datei einige Minuten dauern.


1.  Wenn der Upload abgeschlossen ist, sehen Sie die Datei im Datenspeicher im Ordner erstellten. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    Sie müssen jetzt die zweite VMDK-Datei im gleichen Datastore hochladen.

1.  Im Clientfenster vSphere zurück. ESXi Server ausgewählt mit der rechten Maustaste und wählen Sie **Neuer virtueller Computer**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  Einen **neuen virtuellen Computer erstellen** erscheint. Wählen Sie auf der Seite **Konfiguration** die Option **Benutzerdefiniert** aus. Klicken Sie auf **Weiter**.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  Geben Sie auf der Seite **Name und Pfad** den Namen des virtuellen Computers. Dieser Name sollte den Namen des Ordners (empfohlen) übereinstimmen in Schritt 8 angegebenen.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  Wählen Sie auf der Seite **Speicher** Datenspeicher VM bereitstellen möchten.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  Wählen Sie auf der Seite **Virtual Machine Version** **virtuellen Maschine: 8**. Beachten Sie, dass Versionen 8 bis 11 unterstützt werden.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  Wählen Sie auf der Seite **Gastbetriebssystem** **Gast-Betriebssystem** wie **Windows**. **Version**der Dropdown-Liste Wählen Sie **Microsoft Windows Server 2012 (64 Bit)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  Auf **CPUs** einstellen **der virtuellen Sockets** und **Anzahl der Kerne pro virtuellen Socket** so die **Gesamtzahl der Kerne** 4 (oder mehr). Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  Geben Sie auf der Seite **Speicher** 8 GB oder mehr RAM. Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  Geben Sie die Anzahl der Netzwerkschnittstellen auf der Seite **Netzwerk** . Die Mindestanforderung ist eine Netzwerkschnittstelle.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  Übernehmen Sie die Standardeinstellung **LSI Logic SAS-Controller**, auf **SCSI-Controller** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  Wählen Sie auf der Seite **Wählen Sie einen Datenträger** **verwenden einen vorhandenen virtuellen Datenträger**. Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  Klicken Sie auf der Seite **Vorhandenen Festplatten wählen** **Datenträgerpfad**auf **Durchsuchen**. Dies öffnet ein Dialogfeld **Durchsuchen Datenspeicher** . Navigieren Sie zum Speicherort, in dem die VMDK-Datei hochgeladen. Sie sehen jetzt nur eine Datei im Datenspeicher, wie die beiden Dateien zunächst hochgeladen zusammengeführt wurden. Wählen Sie die Datei, und klicken Sie auf **OK**. Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  Übernehmen Sie auf der Seite **Erweiterte Optionen** die Standardeinstellung, und klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  Überprüfen Sie auf der Seite **bereit zum Abschließen** alle Einstellungen den neuen virtuellen Computer. **Bearbeiten der virtuellen Computer vor Abschluss**überprüfen. Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  Suchen Sie auf der Seite **Eigenschaften von virtuellen Computern** in der Registerkarte **Hardware** die Hardware. Wählen Sie die **neue Festplatte**. Klicken Sie auf **Hinzufügen**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  Dadurch wird das Fenster **Hardware hinzufügen** . Wählen Sie auf der Seite **Gerät** unter **Wählen Sie den Gerätetyp, den Sie hinzufügen möchten** **Festplatte** und klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  Wählen Sie auf der Seite **Wählen Sie einen Datenträger** **ein neues virtuelles Laufwerk erstellen**. Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  Auf der Seite **Erstellen einer Festplatte** ändern Sie die **Größe** 500 GB (oder mehr). Wählen Sie unter **Datenträger bereitstellen** **Thin-Bereitstellung**. Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  Übernehmen Sie den Standardwert auf **Erweiterte Optionen** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  Überprüfen Sie auf der Seite **bereit zum Abschließen** Disk-Optionen. Klicken Sie auf **Fertig stellen**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  Sie kehren nun zur Seite Eigenschaften der virtuellen Computer. Dem virtuellen Computer eine neue Festplatte hinzugefügt. Klicken Sie auf **Fertig stellen**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  Navigieren Sie mit virtuellen Computers im rechten Fensterbereich ausgewählt auf der Registerkarte **Zusammenfassung** . Überprüfen Sie die Einstellung für den virtuellen Computer.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

Ihr virtueller Computer ist jetzt bereitgestellt. Der nächste Schritt ist auf diesem Computer einschalten und die IP-Adresse.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Schritt 3: Virtuellen Geräte und die IP-Adresse

Die folgenden Schritte Ihr virtuelle Gerät starten und zu verbinden.

#### <a name="to-start-the-virtual-device"></a>Virtuelle Gerät starten

1.  Virtuelle Gerät zu starten. VSphere Configuration Manager im linken Bereich Wählen Sie das Gerät und klicken Sie, um das Kontextmenü aufzurufen. **Wählen Sie** und dann **Einschalten**. Dies sollte Ihr virtueller Computer einschalten. Sie können den Status im **Aktuellen Aufgaben** unteren vSphere Client anzeigen.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  Die Installationsaufgaben dauert einige Minuten. Das Gerät läuft, navigieren Sie zur Registerkarte **Konsole** . Senden Sie Strg + Alt + Entf zur Anmeldung an des Geräts. Alternativ kann der Cursor im Konsolenfenster und drücken Sie Strg + Alt + EINFG. Der Standardbenutzer ist *StorSimpleAdmin* und das Standardkennwort lautet *Kennwort1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Aus Sicherheitsgründen läuft das Administratorkennwort Gerät bei der ersten Anmeldung am. Sie werden aufgefordert, das Kennwort zu ändern.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Geben Sie ein Kennwort mit mindestens 8 Zeichen. Das Kennwort muss 3 von 4 dieser Vorschriften enthalten: Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen. Geben Sie das Kennwort zur Bestätigung erneut ein. Sie werden benachrichtigt, dass das Kennwort geändert wurde.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Nachdem das Kennwort geändert wird, kann das virtuelle Gerät neu starten. Warten Sie, bis des Neustarts abgeschlossen. Die Windows PowerShell-Konsole des Geräts kann mit einer Statusanzeige angezeigt.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  Schritte 6 bis 8 gelten nur beim Booten nicht DHCP-Umgebung. Sind Sie in einer Umgebung mit DHCP, überspringen Sie diese Schritte, und gehen Sie zu Schritt 9. Wenn Ihr Gerät nicht DHCP-Umgebung gestartet wird, wird den folgenden Bildschirm angezeigt. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Sie müssen jetzt das Netzwerk konfigurieren.

1.  Verwenden der `Get-HcsIpAddress` Befehl Listen Sie die Netzwerkschnittstellen auf dem virtuellen Gerät aktiviert. Wenn das Gerät eine Netzwerkschnittstelle aktiviert hat, wird dieser Schnittstelle zugewiesene Standardnamen `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Verwenden der `Set-HcsIpAddress` -Cmdlet zum Konfigurieren des Netzwerks. Ein Beispiel ist unten dargestellt:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Nach der Erstinstallation abgeschlossen ist und das Gerät hochgefahren wurde, sehen Sie den Geräte BannerText. Notieren Sie sich die IP-Adresse und zum Verwalten des Geräts die URL in den Bannertext angezeigt. Sie verwenden diese IP-Adresse der Web-Benutzeroberfläche des virtuellen Geräts und lokale Installation und Registrierung abzuschließen.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Optional) Führen Sie diesen Schritt, wenn Sie Ihr Gerät in der Regierung Cloud bereitstellen. Das Gerät wird jetzt USA FIPS Federal Information Processing Standard ()-Modus aktivieren. FIPS 140-Standard definiert Kryptografiealgorithmen genehmigtes US Federal Regierung Computersysteme zum Schutz sensibler Daten.
    1. Um die FIPS-Modus zu aktivieren, führen Sie das folgende Cmdlet:
        
        `Enter-HcsFIPSMode`

    2. Starten Sie das Gerät nach FIPS-Modus aktiviert haben, sodass kryptografischen Validierungen wirksam neu.

        > [AZURE.NOTE] Aktivieren oder Deaktivieren von FIPS-Modus auf dem Gerät. Abwechselnde Gerät zwischen FIPS und nicht FIPS-Modus wird nicht unterstützt.


Wenn Ihr Gerät nicht die Mindestkonfiguration Vorschriften erfüllt, sehen Sie einen Fehler in den Bannertext (siehe unten). Sie müssen die Konfiguration ändern, so, dass sie Ressourcen, die Mindestanforderungen erfüllen. Sie können dann starten und das Gerät an. Die Mindestkonfiguration Anforderungen in [Schritt 1: sicherstellen, dass das Hostsystem minimale virtuelles Gerät erfüllt](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Wenn einen Fehler während der Erstkonfiguration mit der lokalen Webbenutzeroberfläche auftreten, finden Sie folgender Workflows [Verwalten StorSimple Virtual Array](storsimple-ova-web-ui-admin.md)mit der lokalen Web-Benutzeroberfläche.

-   Diagnosetests [UI Websetup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)beheben.

-   [Generate Log Paket und Protokolldateien anzeigen](storsimple-ova-web-ui-admin.md#generate-a-log-package)...

## <a name="next-steps"></a>Nächste Schritte

-   [Virtuelle StorSimple Array als Dateiserver einrichten](storsimple-ova-deploy3-fs-setup.md)

-   [Virtuelle StorSimple Array als ein iSCSI-Server einrichten](storsimple-ova-deploy3-iscsi-setup.md)

