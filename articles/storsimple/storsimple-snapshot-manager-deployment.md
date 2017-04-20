<properties 
   pageTitle="StorSimple Snapshot-Manager bereitstellen | Microsoft Azure"
   description="Informationen Sie zum Herunterladen und Installieren der StorSimple Snapshot-Manager ein MMC-Snap-in zum Verwalten von StorSimple Data Protection und Backup-Funktionen."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>Bereitstellen von StorSimple Snapshot-Manager-MMC-Snap-in

## <a name="overview"></a>Übersicht

StorSimple Snapshot-Manager ist ein Snap-in der Microsoft Management Console (MMC), die den Datenschutz und backup-Management in einer Umgebung mit Microsoft Azure StorSimple vereinfacht. StorSimple Snapshot Manager können Microsoft Azure StorSimple lokal verwalten und cloud-Speicherung als wäre es ein vollständig integriertes Speichersystem so Backup- und Restore-Prozesse vereinfachen und Kosten senken. 

Diese praktische Einführung beschreibt konfigurationsanforderungen sowie Verfahren zum Installieren, entfernen und Aktualisieren von StorSimple Snapshot-Manager.

>[AZURE.NOTE] 
>
>- StorSimple Snapshot-Manager können Sie Microsoft Azure StorSimple virtuelle Arrays verwalten (auch lokal StorSimple virtuelle Geräte).
>
>- Möchten Sie StorSimple Update 2 auf dem StorSimple-Gerät installieren, müssen Sie die neueste Version von StorSimple Snapshot-Manager und **vor der Installation von StorSimple 2**installieren. Die neueste Version von StorSimple Snapshot Manager ist abwärts kompatibel und funktioniert mit allen Versionen von Microsoft Azure StorSimple. Wenn Sie die vorherige Version von StorSimple Snapshot-Manager verwenden, müssen Sie aktualisieren (Sie müssen nicht auf die vorherige Version deinstallieren, bevor Sie die neue Version installieren).

## <a name="storsimple-snapshot-manager-installation"></a>StorSimple Snapshot-Manager-installation

StorSimple Snapshot-Manager kann auf Computern installiert werden, auf dem Windows Server 2008 R2 SP1, Windows Server 2012 oder Betriebssystem Windows Server 2012 R2 ausgeführt. Auf Servern unter Windows 2008 R2 müssen Sie auch Windows Server 2008 SP1 und Windows Management Framework 3.0 installieren. 

Installieren oder aktualisieren StorSimple Snapshot-Manager-Snap-in für Microsoft Management Console (MMC) stellen Sie sicher, dass der Microsoft Azure StorSimple Gerät und Host-Server ordnungsgemäß konfiguriert sind. 

## <a name="configure-prerequisites"></a>Komponenten konfigurieren

Die folgenden Abschnitte Übersicht Konfigurationsaufgaben, die Sie ausführen müssen, bevor Sie StorSimple Snapshot-Manager installieren. Microsoft Azure StorSimple Konfiguration und Setupinformationen an das System und eine schrittweise Anleitung finden Sie unter [Bereitstellen der lokalen StorSimple-Gerät](storsimple-deployment-walkthrough.md).

>[AZURE.IMPORTANT]Bevor Sie beginnen, lesen Sie die [Prüfliste zur Bereitstellung](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) und und [Deployment Prerequisites](storsimple-deployment-walkthrough.md#deployment-prerequisites) in [Ihrem lokalen StorSimple-Gerät bereitstellen](storsimple-deployment-walkthrough.md).
> <br>
 
### <a name="before-you-install-storsimple-snapshot-manager"></a>Vor der Installation von StorSimple Snapshot-Manager

1. Entpacken Sie und schließen Sie das Gerät Microsoft Azure StorSimple [Installieren Sie das Gerät StorSimple 8100](storsimple-8100-hardware-installation.md) oder [Geräts StorSimple 8600](storsimple-8600-hardware-installation.md)bereitstellen.

2. Stellen Sie sicher, dass der Host-Computer eines der folgenden Betriebssysteme ausgeführt wird:

    - Windows Server 2008 R2 (auf Servern unter Windows 2008 R2 müssen Sie auch Windows Server 2008 SP1 und Windows Management Framework 3.0 installieren)
    - Windows Server 2012
    - Windows Server 2012 R2
 
    Ein virtuelles StorSimple muss der Host einer Microsoft Azure Virtual Machine. 

3. Stellen Sie sicher, dass Sie Microsoft Azure StorSimple Konfiguration entsprechen. Details finden Sie im [Deployment Prerequisites](storsimple-deployment-walkthrough.md#deployment-prerequisites).

4. Verbinden Sie das Gerät an den Host und vornehmen Sie die anfängliche Konfiguration. Details finden Sie [Bereitstellungsschritte](storsimple-deployment-walkthrough.md#deployment-steps).

5. Erstellen von Volumes auf dem Gerät Host zuweisen und überprüfen, ob der Host bereitstellen und verwenden kann. StorSimple Snapshot-Manager unterstützt die folgenden Typen von Volumes: 

    - Basisvolumes
    - Einfache volumes
    - Dynamische volumes
    - Gespiegelte dynamische Volumes (RAID 1)
    - Cluster freigegebenen Clustervolumes
 
    Informationen zum Erstellen von Volumes auf der StorSimple oder virtuelles Gerät StorSimple, [Schritt 6: Erstellen Sie ein Volume](storsimple-deployment-walkthrough.md#step-6-create-a-volume), in [dem lokalen StorSimple-Gerät bereitstellen](storsimple-deployment-walkthrough.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Installieren Sie eine neue StorSimple Snapshot-Manager

Stellen Sie vor der Installation StorSimple Snapshot-Manager sicher, dass die Volumes StorSimple-Gerät oder StorSimple virtuelles Gerät bereitgestellt erstellt, initialisiert und formatiert beschriebenen [Komponenten konfigurieren](#configure-prerequisites).

>[AZURE.IMPORTANT]
>
>- Ein virtuelles StorSimple muss der Host einer Microsoft Azure Virtual Machine. 
>
>- Der Host muss Windows 2008 R2, Windows Server 2012 und Windows Server 2012 R2 ausgeführt werden. Wenn der Server Windows Server 2008 R2 ausgeführt wird, müssen Sie auch Windows Server 2008 SP1 und Windows Management Framework 3.0 installieren.
>
>- Bevor Sie das Gerät auf StorSimple Snapshot-Manager herstellen können, müssen Sie eine iSCSI-Verbindung vom Server zum StorSimple-Gerät konfigurieren.

Gehen Sie führen Sie eine Neuinstallation von StorSimple Snapshot-Manager. Wenn Sie ein Update installieren, gehen Sie zu [Aktualisieren oder installieren Sie StorSimple Snapshot-Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

- Schritt 1: Installieren von StorSimple Snapshot-Manager 
- Schritt 2: Verbinden eines Geräts StorSimple Snapshot-Manager 
- Schritt 3: Überprüfen Sie die Verbindung zum Gerät 

###<a name="step-1-install-storsimple-snapshot-manager"></a>Schritt 1: Installieren von StorSimple Snapshot-Manager

Gehen Sie StorSimple Snapshot-Manager installieren.

#### <a name="to-install-storsimple-snapshot-manager"></a>StorSimple Snapshot-Manager installieren

1. StorSimple Snapshot-Manager-Software (Go [StorSimple Snapshot-Manager](https://www.microsoft.com/download/details.aspx?id=44220) im Microsoft Download Center) herunterladen und lokal auf dem Server speichern.

2. Im Datei-Explorer mit der rechten Maustaste des komprimierten Ordners und dann auf **Alle extrahieren**.

3. Geben Sie im Fenster **(ZIP) Ordner extrahieren** im Feld **Wählen Sie ein Ziel und Extrahieren von Dateien** ein oder Durchsuchen Sie den Pfad, wo möchten Sie Datei extrahiert werden. 

       >[AZURE.IMPORTANT] StorSimple Snapshot-Manager muss auf Laufwerk C: installiert werden.
 
4. **Dateien nach Extrahierung anzeigen** das Kontrollkästchen und klicken Sie dann auf **extrahieren**.

    ![Das Dialogfeld Dateien extrahieren](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. Wenn die Extraktion abgeschlossen ist, wird der Zielordner geöffnet. Doppelklicken Sie auf Anwendung Einrichtung, die im Ordner angezeigt.

5. Wenn das **Setup erfolgreich** angezeigt wird, klicken Sie auf **Schließen**. Sie sollten StorSimple Snapshot-Manager-Symbol auf dem Desktop angezeigt.

    ![Desktop-Symbol](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Schritt 2: Verbinden eines Geräts StorSimple Snapshot-Manager

Gehen Sie StorSimple Snapshot-Manager mit einem StorSimple-Gerät herzustellen.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>Verbindung StorSimple Snapshot-Manager auf einem Gerät

1. Klicken Sie auf StorSimple Snapshot-Manager-Symbol auf Ihrem Desktop. StorSimple Snapshot-Manager angezeigt. Das Fenster enthält **einen Bereich** , **ein Ergebnisbereich** und **einen Aktionsbereich** . 

    ![StorSimple Snapshot-Manager-Benutzeroberfläche](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    - **Bereich (linker Bereich)** enthält eine Liste der Knoten in einer Baumstruktur organisiert. Sie können einige Knoten zum Auswählen einer Ansicht oder bestimmte Daten in Bezug auf diesen Knoten erweitern. Klicken Sie auf den Pfeil zum Erweitern oder Reduzieren eines Knotens. Klicken Sie auf ein Element im **Bereich eine Liste der verfügbaren Aktionen für das Element** . 

    - Im **Ergebnisbereich** (mittlerer Bereich) enthält ausführliche Statusinformationen zum Knoten, anzeigen oder Daten, die im **Bereich** ausgewählt.

    - Im **Aktionsbereich** werden die Vorgänge, die Sie ausführen können, Knoten, anzeigen oder Daten, die im **Bereich** ausgewählt.

    Eine vollständige Beschreibung der StorSimple Snapshot-Manager-Benutzeroberfläche finden Sie unter [StorSimple Snapshot-Manager-Benutzeroberfläche](storsimple-use-snapshot-manager.md).

2. Klicken Sie im **Bereich** klicken Sie **der Geräteknoten** und dann auf **Gerät konfigurieren**. Das Dialogfeld **Gerät konfigurieren** .

    ![Konfigurieren eines Geräts](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3. Wählen Sie im Listenfeld **Gerät** die IP-Adresse des Microsoft Azure StorSimple Gerät bzw. virtuellen Gerät. Geben Sie im Textfeld **Kennwort** das Kennwort StorSimple Snapshot-Manager, das für das Gerät im klassischen Azure-Portal erstellt. Klicken Sie auf **OK**.

4. StorSimple Snapshot-Manager sucht das Gerät, das Sie identifiziert. Wenn das Gerät verfügbar ist, fügt StorSimple Snapshot-Manager eine Verbindung. Sie können [die Verbindung mit dem Gerät zu überprüfen](#to-verify-the-connection) , um sicherzustellen, dass die Verbindung erfolgreich hinzugefügt wurde.

    Wenn das Gerät nicht verfügbar ist, gibt StorSimple Snapshot-Manager eine Fehlermeldung zurück. Klicken Sie auf **OK** , um die Fehlermeldung zu schließen, und klicken Sie auf **Abbrechen** , um das Dialogfeld **Gerät konfigurieren** zu schließen.

5. Verbindung zu einem Gerät importiert StorSimple Snapshot Manager jedes Volume-Gruppe für das Gerät konfiguriert, die Volume-Gruppe Backups verbunden ist. Volume-Gruppen, die nicht zugeordneten Backups werden nicht importiert. Außerdem werden backup-Policies, die für eine Volume-Gruppe erstellt wurden nicht importiert. Die importierten Gruppen finden Maustaste obersten **Volume Groups** -Knoten im **Bereichsfenster** und auf **umschalten Gruppen importiert**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Schritt 3: Überprüfen Sie die Verbindung zum Gerät

Gehen Sie folgendermaßen vor, um sicherzustellen, dass das Gerät StorSimple StorSimple Snapshot-Manager verbunden ist.

#### <a name="to-verify-the-connection"></a>Überprüfen Sie die Verbindung

1. Klicken Sie im **Bereich** **der Geräteknoten** .

    ![Status des Snapshot-Manager StorSimple](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. Überprüfen Sie im Bereich **Ergebnisse** : 

   - Wenn grüne auf das Gerätesymbol Anzeige **verfügbar** in der Spalte **Status** angezeigt wird, ist das Gerät angeschlossen. 

   - Wenn rote auf das Gerätesymbol Anzeige nicht verfügbar in der Spalte **Status** angezeigt wird, ist das Gerät nicht verbunden. 

   - Wenn in der Spalte **Status** **Aktualisieren** angezeigt wird, ist StorSimple Snapshot-Manager Volume-Gruppen und zugeordneten Backups für ein verbundenes Gerät abrufen.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Aktualisieren Sie oder installieren Sie StorSimple Snapshot-Manager

Sie sollten StorSimple Snapshot-Manager vor der Aktualisierung oder Neuinstallation die Software vollständig deinstallieren. 

Vor der Neuinstallation von StorSimple Snapshot-Manager, Sichern Sie die vorhandene StorSimple Snapshot-Manager-Datenbank auf dem Hostcomputer Dies speichert die Sicherungsinformationen Richtlinien und Konfiguration, damit einfach Daten aus einer Sicherung wiederherstellen können.

Gehen Sie folgendermaßen vor, wenn Sie aktualisieren oder erneut StorSimple Snapshot-Manager:

- Schritt 1: Deinstallieren Sie StorSimple Snapshot-Manager 
- Schritt 2: Die StorSimple Snapshot-Manager-Datenbank sichern 
- Schritt 3: Installieren Sie StorSimple Snapshot-Manager und Wiederherstellen der Datenbank 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Schritt 1: Deinstallieren Sie StorSimple Snapshot-Manager

Gehen Sie StorSimple Snapshot-Manager deinstallieren.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple Snapshot-Manager deinstallieren

1. Auf dem Hostcomputer **Control Panel**öffnen Sie, klicken Sie auf **Programme**und klicken Sie auf **Programme und Funktionen**.

2. Klicken Sie im linken Bereich auf **Deinstallieren oder Ändern von Programmen**.

3. Maustaste auf **StorSimple Snapshot-Manager**, und klicken Sie dann auf **Deinstallieren**.

4. StorSimple Snapshot-Manager-Installationsprogramm wird gestartet. Klicken Sie auf **Setup ändern**und dann auf **Deinstallieren**.

    >[AZURE.NOTE] Sind MMC-Prozesse im Hintergrund, StorSimple Snapshot-Manager oder Datenträger die Deinstallation fehl, und erhalten eine Nachricht an alle Instanzen von MMC schließen, bevor Sie versuchen, das Programm zu deinstallieren. **Wählen Sie **automatisch Programme schließen und versuchen, nach Abschluss der Installation starten****
 
5. Wenn die Deinstallation abgeschlossen ist, wird **Setup erfolgreich** angezeigt. Klicken Sie auf **Schließen**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Schritt 2: Die StorSimple Snapshot-Manager-Datenbank sichern

Gehen Sie zum Erstellen und speichern Sie eine Kopie der Datenbank StorSimple Snapshot-Manager.

#### <a name="to-back-up-the-database"></a>Zum Sichern der Datenbank

1. Beenden Sie Management von Microsoft StorSimple:

   1. Starten Sie Server-Manager.

   2. Wählen Sie auf dem Schaltpult Server-Manager im Menü **Extras** **Dienste**.

   3. Wählen Sie auf der Seite **Dienste** **Microsoft StorSimple-Verwaltungsdienst**.

   4. Klicken Sie im rechten Bereich **Microsoft StorSimple-Verwaltungsdienst**auf **Beenden Sie den Dienst**.

        ![StorSimple Manager beenden](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. Wechseln Sie zu C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData ist ein versteckter Ordner.

3. Suchen Sie die XML-Katalogdatei, kopieren Sie die Datei und speichern Sie die Kopie an einem sicheren Ort oder in der Cloud.

    ![StorSimple backup-Katalogdatei](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Management von Microsoft StorSimple Dienst: 

    1. Wählen Sie auf dem Schaltpult Server-Manager im Menü **Extras** **Dienste**.

    2. Wählen Sie auf der Seite **Dienste** **Microsoft StorSimple Management Service**e.

    3. Klicken Sie im rechten **Microsoft StorSimple-Verwaltungsdienst**auf **Dienst neu starten**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Schritt 3: Installieren Sie StorSimple Snapshot-Manager und Wiederherstellen der Datenbank

StorSimple Snapshot-Manager neu installieren, gehen Sie in [eine neue StorSimple Snapshot-Manager installieren](#install-a-new-storsimple-snapshot-manager). Dann gehen Sie zum Wiederherstellen der Datenbank StorSimple Snapshot-Manager.

#### <a name="to-restore-the-database"></a>Wiederherstellen die Datenbank

1. Beenden Sie Management von Microsoft StorSimple:

    1. Starten Sie Server-Manager.

    2. Wählen Sie auf dem Schaltpult Server-Manager im Menü **Extras** **Dienste**.

    3. Wählen Sie auf der Seite **Dienste** **Microsoft StorSimple-Verwaltungsdienst**.

    4. Klicken Sie im rechten Bereich **Microsoft StorSimple-Verwaltungsdienst**auf **Beenden Sie den Dienst**.

2. Wechseln Sie zu C:\ProgramData\Microsoft\StorSimple\BACatalog. 

     >[AZURE.NOTE] ProgramData ist ein versteckter Ordner.

3. Die XML-Katalogdatei löschen und mit der zuvor gespeicherten Version ersetzen.

4. Management von Microsoft StorSimple Dienst: 

    1. Wählen Sie auf dem Schaltpult Server-Manager im Menü **Extras** **Dienste**.

    2. Wählen Sie auf der Seite **Dienste** **Microsoft StorSimple-Verwaltungsdienst**.

    3. Klicken Sie im rechten **Microsoft StorSimple-Verwaltungsdienst**auf **Dienst neu starten**.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über StorSimple Snapshot-Manager gehen Sie zu [Was StorSimple Snapshot-Manager ist?](storsimple-what-is-snapshot-manager.md).

- Erfahren Sie mehr über die Benutzeroberfläche StorSimple Snapshot-Manager gehen Sie [StorSimple Snapshot-Manager-Benutzeroberfläche](storsimple-use-snapshot-manager.md).

- Mehr über StorSimple Snapshot-Manager finden Sie unter [StorSimple](storsimple-snapshot-manager-admin.md)-Snapshot-Manager verwalten Ihre StorSimple-Lösung.
