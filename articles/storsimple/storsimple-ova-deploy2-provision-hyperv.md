<properties
   pageTitle="Bereitstellen von StorSimple Virtual Array - Bereitstellung in Hyper-V"
   description="Diese zweite Lernprogramm StorSimple Virtual Array-Bereitstellung umfasst die Bereitstellung eines virtuellen Geräts in Hyper-V."
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
   ms.date="10/11/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-hyper-v"></a>Bereitstellen von StorSimple Virtual Array - Bereitstellung virtuelles Array in Hyper-V

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Übersicht

Lernprogramms Bereitstellung gilt für Microsoft Azure StorSimple virtuelle Arrays (auch bekannt als StorSimple für lokale virtuelle Geräte oder StorSimple virtuelle Geräte) ausgeführten März 2016 allgemein verfügbar (GA) Version. In diesem Lernprogramm beschreibt StorSimple virtuelles Array auf einem Hostsystem mit Hyper-V in Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 bereitstellen. Dieser Artikel bezieht sich auf die Bereitstellung von virtuellen StorSimple-Arrays in Azure-Verwaltungsportal als auch Microsoft Azure Government Cloud.

Sie benötigen Administratorrechte bereitstellen und Konfigurieren eines virtuelles Geräts. Die Bereitstellung und anfängliche Einrichtung dauert ca. 10 Minuten.


## <a name="provisioning-prerequisites"></a>Bereitstellung erforderlicher Komponenten

Hier finden Sie die erforderlichen Komponenten auf ein virtuelles Gerät auf einem Hostsystem mit Hyper-V auf Windows Server 2008 R2, Windows Server 2012 R2 oder Windows Server 2012 bereitstellen.

### <a name="for-the-storsimple-manager-service"></a>Für den StorSimple-Manager-Dienst

Bevor Sie beginnen, stellen Sie sicher, dass:

-   Sie haben alle Schritte [Vorbereiten Portal für StorSimple Virtual Array](storsimple-ova-deploy1-portal-prep.md)abgeschlossen.

-   Sie haben die virtuellen Geräts für Hyper-V von Azure-Portal. Weitere Informationen finden Sie unter [Schritt 3: virtuellen Geräts downloaden](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

    > [AZURE.IMPORTANT] Die Software StorSimple Virtual Array kann nur in Verbindung mit der Storsimple Manager-Dienst verwendet werden.

### <a name="for-the-storsimple-virtual-device"></a>Für das virtuelle Gerät StorSimple

Bevor Sie ein virtuelles Gerät bereitstellen, stellen Sie Folgendes sicher:

-   Sie haben Zugriff auf ein Hostsystem mit Hyper-V auf Windows Server 2008 R2 oder höher, die kann zur Rückstellung ein.

-   Das Hostsystem ist Folgendes virtuelle Gerät bereitstellen widmen:

    -   Mindestens 4 Kernen.

    -   Mindestens 8 GB RAM.

    -   Eine Netzwerkschnittstelle.

    -   Ein 500 GB virtuellen Datenträger für Daten.

### <a name="for-the-network-in-the-datacenter"></a>Für das Netzwerk im Rechenzentrum

Zunächst überprüfen Sie Netzwerken erforderlichen ein StorSimple virtuelles Gerät bereitzustellen und Datacenter-Netzwerk entsprechend konfigurieren. Weitere Informationen finden Sie unter [netzwerkanforderungen StorSimple Virtual Array](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Schrittweise Bereitstellung

Bereitstellen und zu einem virtuellen Gerät, müssen Sie die folgenden Schritte ausführen:

1.  Sicherstellen Sie, dass das Hostsystem ausreichende Ressourcen, um die minimale virtuelles Gerät erfüllen.

2.  Bereitstellen Sie ein virtuelles Gerät der Hypervisor.

3.  Virtuelle Gerät starten und die IP-Adresse.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements"></a>Schritt 1: Sicherstellen Sie, dass das Hostsystem minimale virtuelles Gerät erfüllt

Erstellen Sie ein virtuelles Gerät benötigen Sie:

-   Die Hyper-V-Rolle auf Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 SP1 installiert.

-   Microsoft Hyper-V-Manager auf einem Microsoft Windows-Client mit dem Host verbunden.

Sie müssen sicherstellen, dass Ihr virtuelles Gerät Folgendes widmen die zugrunde liegenden Hardware (Server System) auf der virtuelle Gerät erstellen kann:

- Mindestens 4 Kernen.
- Mindestens 8 GB RAM.
- Eine Netzwerkschnittstelle.
- Ein 500 GB virtuellen Datenträger für Daten.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Schritt 2: Bereitstellen eines virtuellen Geräts in hypervisor

Führen Sie die folgenden Schritte aus, um ein Gerät in der Hypervisor bereit.

#### <a name="to-provision-a-virtual-device"></a>Ein virtuelles Gerät bereitstellen

1.  Kopieren Sie auf Ihrem Windows Server virtuellen Geräts auf einem lokalen Laufwerk. Dies ist das Bild (VHD oder VHDX), das über das Azure-Portal herunterladen. Notieren Sie sich den Speicherort, in dem das Bild kopiert, wie Sie diese später in der Prozedur verwenden.

2.  Öffnen Sie **Server-Manager**. Klicken Sie in der oberen rechten Ecke auf **Extras** , und wählen Sie **Hyper-V-Manager**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

    Ausführen von Windows Server 2008 R2 Hyper-V-Manager zu öffnen. Klicken Sie im Server-Manager auf **Rollen > Hyper-V > Hyper-V-Manager**.

1.  In **Hyper-V-Manager**in den Bereich mit der rechten Maustaste die Systemknoten im Kontextmenü öffnen und dann auf **neu** > **virtuellen Computer**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  Klicken Sie auf der Seite **vor dem Starten** des Assistenten für neue virtuelle Computer auf **Weiter**.

1.  Geben Sie auf der Seite **Geben Sie Namen und Speicherort** einen **Namen** für das virtuelle Gerät. Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  Wählen Sie auf der Seite **Specify Generation** Bildtyp Gerät aus und dann auf **Weiter**. Diese Seite nicht angezeigt, wenn Sie Windows Server 2008 R2 verwenden.

    * Wählen Sie die **Generation 2** , wenn ein Bild vhdx für Windows Server 2012 oder höher heruntergeladen.
    * Wählen Sie **Generation 1** , wenn Sie ein VHD-Abbild für Windows Server 2008 R2 oder höher heruntergeladen.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  Geben Sie auf der Seite **Speicher zuweisen** **startspeicher** mindestens **8192 MB**nicht dynamischen Speicher aktivieren und dann auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  Geben Sie auf der Seite **Netzwerk konfigurieren** den virtuellen Switch mit dem Internet verbunden ist, und klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  Wählen Sie auf der Seite **virtuelle Festplatte verbinden** **eine vorhandene virtuelle Festplatte verwenden**, geben Sie den Speicherort des virtuellen Geräts (vhdx oder VHD) und klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  Überprüfen Sie die **Zusammenfassung** , und klicken Sie zum Erstellen des virtuellen Computers **abgeschlossen** . Aber nicht vorgreifen noch - müssen Sie noch einige CPUs und einem zweiten Laufwerk hinzufügen. 

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  Sie benötigen 4 Kerne, um die Mindestanforderungen. Hinzufügen virtuelle Prozessoren mit dem Hostsystem im Fenster **Hyper-V-Manager** im rechten Fensterbereich in der Liste der **virtuellen Computer**aktiviert suchen Sie den virtuellen Computer, den Sie gerade erstellt haben. Und wählen Sie mit der rechten Maustaste des Computernamens **Settings**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Klicken Sie auf der Seite **Einstellungen** im linken Fensterbereich auf **Prozessor**. Legen Sie im rechten Fensterbereich **Anzahl virtueller Prozessoren** auf 4 (oder mehr). Klicken Sie auf **Übernehmen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  Um die Mindestanforderungen müssen Sie auch einen 500 GB virtuellen Datenträger hinzufügen. Auf der Seite **Einstellungen** :

    1.  Wählen Sie im linken Bereich **SCSI-Controller**.
    2.  Wählen Sie im rechten Bereich **Festplatte,** und klicken Sie auf **Hinzufügen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  Auf der Seite **Festplatte** **virtuelle Festplatte** die Option, und klicken Sie auf **neu**. Dies startet den **Assistenten für neue virtuelle Festplatte**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  Klicken Sie auf der Seite **vor** der virtuellen Festplatte Assistenten auf **Weiter**.

1.  Übernehmen Sie auf der **Seite Wählen Sie Disk Format**die Standardoption **VHDX** -Format. Klicken Sie auf **Weiter**. Dieser Bildschirm wird nicht angezeigt, wenn Sie Windows Server 2012 R2 oder Windows Server 2008 R2 ausführen.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  **Wählen Sie Datenträger Seite**geben virtuelle Festplatte als **dynamisch erweiterbar** (empfohlen). Wenn Sie **feste Größe** Datenträger auswählen, funktioniert auch, aber Sie müssen lange warten. Wir empfehlen die **Differenzierung** Option nicht verwenden. Klicken Sie auf **Weiter**. Beachten Sie, dass **dynamisch erweiterbare** standardmäßig in Windows Server 2012 R2 und Windows Server 2012. In Windows Server 2008 R2 ist der Standardwert **fester Größe**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  Bieten Sie auf der Seite **Name und Pfad angeben** **Namen** als **Speicherort** (Sie können zu durchsuchen) für den Datenträger. Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  Auf der Seite **Datenträger konfigurieren** die Option **neue leere virtuelle Festplatte erstellen** , und geben Sie die Größe **500 GB** (oder mehr). 500 GB die Mindestanforderung ist, können Sie immer einen größeren Datenträger bereitstellen. Beachten Sie, dass nicht erweitern oder verkleinern die Festplatte einmal bereitgestellt. Weitere Informationen zur Größe des Datenträgers bereitstellen finden Sie im Abschnitt anpassen im Dokument [best Practices](storsimple-ova-best-practices.md#configuration-best-practices) . Klicken Sie auf **Weiter**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  Einzelheiten Sie auf der Seite **Zusammenfassung** die des virtuellen Datenträgers und zufrieden sind, klicken Sie auf **Fertig stellen** , um die Diskette zu erstellen. Der Assistent wird geschlossen und eine virtuelle Festplatte mit dem Computer hinzugefügt.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  Sie kehren zu **der Einstellungsseite** . Klicken Sie auf **OK** , um **die Einstellungsseite** schließen und zurück zu Hyper-V-Manager-Fenster.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Schritt 3: Virtuellen Geräte und die IP-Adresse

Die folgenden Schritte Ihr virtuelle Gerät starten und zu verbinden.

#### <a name="to-start-the-virtual-device"></a>Virtuelle Gerät starten

1.  Virtuelle Gerät zu starten.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Nach das Gerät ausgeführt wird, wählen Sie das Gerät, klicken Sie mit der rechten Maustaste auf und wählen Sie **Verbinden**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  Möglicherweise 5 bis 10 Minuten für das Gerät bereit. Eine Statusnachricht wird auf der Konsole den Fortschritt angezeigt. Nachdem das Gerät bereit ist, fahren Sie mit **Aktion**. Drücken Sie `Ctrl + Alt + Delete` virtuelles Gerät anmelden. Der Standardbenutzer ist *StorSimpleAdmin* und das Standardkennwort lautet *Kennwort1*.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Aus Sicherheitsgründen läuft das Administratorkennwort Gerät bei der ersten Anmeldung am. Sie werden aufgefordert, das Kennwort zu ändern.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

    Geben Sie ein Kennwort mit mindestens 8 Zeichen. Das Kennwort muss mindestens 3 4 Folgendes erfüllen: Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen. Geben Sie das Kennwort zur Bestätigung erneut ein. Sie werden benachrichtigt, dass das Kennwort geändert wurde.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Das Kennwort erfolgreich geändert wurde, kann das virtuelle Gerät starten. Warten Sie auf das Gerät.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

    Die Windows PowerShell-Konsole des Geräts wird mit einer Statusanzeige angezeigt.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  Schritte 6 bis 8 gelten nur beim Booten nicht DHCP-Umgebung. Sind Sie in einer Umgebung mit DHCP, überspringen Sie diese Schritte, und gehen Sie zu Schritt 9. Wenn Ihr Gerät nicht DHCP-Umgebung gestartet wird, wird den folgenden Bildschirm angezeigt.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

    Sie müssen jetzt das Netzwerk konfigurieren.

1.  Verwenden der `Get-HcsIpAddress` Befehl Listen Sie die Netzwerkschnittstellen auf dem virtuellen Gerät aktiviert. Wenn das Gerät eine Netzwerkschnittstelle aktiviert hat, wird dieser Schnittstelle zugewiesene Standardnamen `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Verwenden der `Set-HcsIpAddress` -Cmdlet zum Konfigurieren des Netzwerks. Ein Beispiel ist unten dargestellt:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Nach der Erstinstallation abgeschlossen ist und das Gerät hochgefahren wurde, sehen Sie den Geräte BannerText. Notieren Sie sich die IP-Adresse und zum Verwalten des Geräts die URL in den Bannertext angezeigt. Sie verwenden diese IP-Adresse der Web-Benutzeroberfläche des virtuellen Geräts und lokale Installation und Registrierung abzuschließen.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Optional) Führen Sie diesen Schritt, wenn Sie Ihr Gerät in der Regierung Cloud bereitstellen. Das Gerät wird jetzt USA FIPS Federal Information Processing Standard ()-Modus aktivieren. FIPS 140-Standard definiert Kryptografiealgorithmen genehmigtes US Federal Regierung Computersysteme zum Schutz sensibler Daten.
    1. Um die FIPS-Modus zu aktivieren, führen Sie das folgende Cmdlet:

        `Enter-HcsFIPSMode`

    2. Starten Sie das Gerät nach FIPS-Modus aktiviert haben, sodass kryptografischen Validierungen wirksam neu.

        > [AZURE.NOTE] Aktivieren oder Deaktivieren von FIPS-Modus auf dem Gerät. Abwechselnde Gerät zwischen FIPS und nicht FIPS-Modus wird nicht unterstützt.

Wenn Ihr Gerät nicht die Mindestkonfiguration Vorschriften erfüllt, sehen Sie einen Fehler in den Bannertext (siehe unten). Sie müssen die Konfiguration ändern, so, dass sie Ressourcen, die Mindestanforderungen erfüllen. Sie können dann starten und das Gerät an. Die Mindestkonfiguration Anforderungen in [Schritt 1: sicherstellen, dass das Hostsystem minimale virtuelles Gerät erfüllt](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Wenn einen Fehler während der Erstkonfiguration mit der lokalen Webbenutzeroberfläche auftreten, finden Sie folgender Workflows [Verwalten StorSimple Virtual Array](storsimple-ova-web-ui-admin.md)mit der lokalen Web-Benutzeroberfläche.

-   Diagnosetests [UI Websetup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)beheben.

-   [Generate Log Paket und Protokolldateien anzeigen](storsimple-ova-web-ui-admin.md#generate-a-log-package).

![Symbol: Video](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png)  **Video verfügbar**

Das Video zeigt wie StorSimple virtuelles Array in Hyper-V bereitstellen können.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## <a name="next-steps"></a>Nächste Schritte

-   [Virtuelle StorSimple Array als Dateiserver einrichten](storsimple-ova-deploy3-fs-setup.md)

-   [Virtuelle StorSimple Array als ein iSCSI-Server einrichten](storsimple-ova-deploy3-iscsi-setup.md)
