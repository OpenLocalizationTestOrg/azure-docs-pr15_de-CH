<properties 
   pageTitle="StorSimple Virtual Array iSCSI-Server-Setup | Microsoft Azure"
   description="Beschreibt das Ausführen von Erstinstallation, den StorSimple iSCSI-Server registrieren und Gerät."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/18/2016"
   ms.author="alkohli" />


# <a name="deploy-storsimple-virtual-array--set-up-your-virtual-device-as-an-iscsi-server"></a>Bereitstellung StorSimple Virtual Array – virtuelles Gerät als iSCSI-Server einrichten

![iSCSI-Setup Prozessablauf](./media/storsimple-ova-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Übersicht

Dieses Lernprogramm Bereitstellung gilt für Microsoft Azure StorSimple Virtual Array (auch bekannt als StorSimple lokalen virtuellen Gerät oder StorSimple virtuelles Gerät) ausgeführten März 2016 allgemein verfügbar (GA) Version. In diesem Lernprogramm beschreibt die anfängliche Einrichtung, den StorSimple iSCSI-Server registrieren die Geräteinstallation abzuschließen und dann erstellen, bereitstellen, initialisieren und formatieren Volumes auf dem StorSimple virtuelles Gerät iSCSI-Server. StorSimple-Setup-Informationen in diesem Artikel gilt nur für virtuelle StorSimple-Arrays. 

Beschriebenen Verfahren dauern ca. 30 Minuten hier auf 1 Stunde. Die Informationen in diesem Artikel gilt nur für virtuelle StorSimple-Arrays.

## <a name="setup-prerequisites"></a>Installation erforderlicher Komponenten

Konfigurieren und Einrichten von virtuellen StorSimple-Gerät stellen Sie Folgendes sicher:

- Sie haben ein virtuelles Gerät bereitgestellt und gemäß [Bereitstellen StorSimple Virtual Array - Bereitstellung virtuelles Array in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) oder [Bereitstellung StorSimple Virtual Array - Bereitstellung virtuelles Array in VMware](storsimple-ova-deploy2-provision-vmware.md)mit.

- Sie haben Dienstregistrierungsschlüssel aus StorSimple Manager-Dienst, den Sie erstellt StorSimple virtuelle Geräte verwalten. Weitere Informationen finden Sie unter **Schritt2: Abrufen der Dienstregistrierungsschlüssel** in [Bereitstellen StorSimple Virtual Array - Portal vorbereiten](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

- Ist dies der zweiten oder nachfolgenden virtuelles Gerät, das mit einem vorhandenen StorSimple Manager-Dienst registriert sind, müssen Sie den Verschlüsselungsschlüssel Daten. Dieser Schlüssel wurde generiert, wenn das erste Gerät bei diesem Dienst registriert wurde. Wenn Sie diesen Schlüssel verloren haben, finden Sie unter **Abrufen den Verschlüsselungsschlüssel Daten** verwendet [Die Webbenutzeroberfläche StorSimple Virtual Array verwalten](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Schrittweise Einrichtung 

Verwenden Sie die folgenden Schritte zum Einrichten und konfigurieren Sie das virtuelle Gerät StorSimple:

-  [Schritt 1: Lokale Benutzeroberfläche Setup abgeschlossen und Ihr Gerät registrieren](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
-  [Schritt 2: Schließen Sie die erforderlichen Einrichtung](#step-2-complete-the-required-device-setup)
-  [Schritt 3: Hinzufügen eines Datenträgers](#step-3-add-a-volume)
-  [Schritt 4: Bereitstellen, initialisieren und Formatieren eines Datenträgers](#step-4-mount-initialize-and-format-a-volume)  

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Schritt 1: Lokale Benutzeroberfläche Setup abgeschlossen und Ihr Gerät registrieren 

#### <a name="to-complete-the-setup-and-register-the-device"></a>Die Einrichtung und das Gerät registrieren

1. Öffnen Sie ein Browserfenster, und mit der Web-Benutzeroberfläche eingeben:

    `https://<ip-address of network interface>`

    Verwenden Sie im vorherigen Schritt erwähnt Verbindungs-URL. Sie sehen eine Fehlermeldung darüber informiert, dass ein Problem mit dem Sicherheitszertifikat der Website vorliegt. Klicken Sie auf **die Seite**.

    ![Zertifikatfehler Sicherheit](./media/storsimple-ova-deploy3-iscsi-setup/image3.png)

2. Melden Sie sich als **StorSimpleAdmin**in der Web-Benutzeroberfläche des virtuellen Geräts an. Geben Sie das Administratorkennwort für Geräte, die Sie geändert in Schritt 3: virtuelle Gerät [Bereitstellen StorSimple Virtual Array - Bereitstellung ein virtuelles Gerät in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) oder [Bereitstellung StorSimple Virtual Array - Bereitstellung eines virtuellen Geräts in VMware](storsimple-ova-deploy2-provision-vmware.md)starten.

    ![Anmeldeseite](./media/storsimple-ova-deploy3-iscsi-setup/image4.png)

3. Sie gelangen auf **die Startseite** . Diese Seite beschreibt die verschiedenen Einstellungen konfigurieren und registrieren das virtuelle Gerät StorSimple Manager-Dienst erforderlich. Beachten Sie, dass die **Netzwerk-Einstellungen** **Webproxyeinstellungen**und **Intervalle** optional. Die einzige erforderliche Einstellung sind **Geräte** und **Cloud-Einstellungen**.

    ![Startseite](./media/storsimple-ova-deploy3-iscsi-setup/image5.png)

4. Auf der Seite **Einstellungen** unter **Netzwerkschnittstellen**werden Daten 0 automatisch konfiguriert werden. Jede Netzwerkschnittstelle wird standardmäßig automatisch zu einer IP-Adresse (DHCP). Daher werden eine IP-Adresse, Subnetzmaske und Gateway automatisch (für IPv4 und IPv6) zugewiesen werden.

    Wie Sie Ihr Gerät als iSCSI-Server (Bereitstellung Blockspeicher) bereitstellen möchten, empfehlen wir, deaktivieren Sie die Option **Get IP-Adresse automatisch** und statische IP-Adressen konfigurieren.

    ![Netzwerkeinstellungsseite](./media/storsimple-ova-deploy3-iscsi-setup/image6.png)

    Wenn Sie mehr als eine Netzwerkschnittstelle während der Bereitstellung des Geräts hinzugefügt haben, können Sie diese hier konfigurieren. Beachten Sie, dass die Netzwerkschnittstelle IPv4 nur oder als IPv4 und IPv6 konfigurieren können. Nur IPv6-Konfigurationen werden nicht unterstützt.

5. DNS-Server sind erforderlich, da sie verwendet werden, wenn das Gerät versucht, mit der Cloud Storage kommunizieren oder das Gerät nach Namen auflösen, wenn er als Dateiserver konfiguriert ist. Auf der Seite **Einstellungen** unter den **DNS-Server**:

    1. Primärer und sekundärer DNS-Server wird automatisch konfiguriert. Wenn Sie statische IP-Adressen konfigurieren möchten, können Sie DNS-Server angeben. Für hohe Verfügbarkeit sollten Sie einen primären und einen sekundären DNS-Server konfigurieren.

    2. Klicken Sie auf **Übernehmen**. Wendet und die Einstellungen überprüfen.

6. Auf dem **Gerät** :

    1. Das Gerät einen eindeutigen **Namen** zuweisen. Dieser Name kann 1-15 Zeichen lang sein und Buchstaben, Zahlen und Bindestriche enthalten.

    2. Klicken Sie auf **iSCSI-Server** ![Symbol für iSCSI-Server](./media/storsimple-ova-deploy3-iscsi-setup/image7.png) für den **Typ** des Geräts, das Sie erstellen. ISCSI-Server können Sie Blockspeicher bereitstellen.

    3. Geben Sie dieses Gerät Domäne hinzugefügt werden soll. Ist das Gerät ein iSCSI-Server ist der Domäne optional. Wenn Sie iSCSI-Server nicht zu einer Domäne hinzufügen möchten, klicken Sie auf **Übernehmen**, die Einstellungen angewendet werden und fahren Sie mit dem nächsten Schritt warten.

        Wenn Sie das Gerät zu einer Domäne hinzufügen möchten. Geben Sie einen **Domänennamen**, und klicken Sie dann auf **Übernehmen**.

        > [AZURE.NOTE] ISCSI-Server einer Domäne beitreten, sicher, dass Ihr virtuelle Array in eine eigene Organisationseinheit (OU) für Microsoft Azure Active Directory ist und keine Gruppenrichtlinienobjekte (GPO) angewendet.

    5. Ein Dialogfeld wird angezeigt. Geben Sie Ihre Anmeldeinformationen im angegebenen Format. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Die Anmeldeinformationen werden überprüft. Sie sehen eine Fehlermeldung, wenn die Anmeldeinformationen falsch sind.

        ![Anmeldeinformationen](./media/storsimple-ova-deploy3-iscsi-setup/image8.png)

    6. Klicken Sie auf **Übernehmen**. Wendet und die Einstellungen überprüfen.
 
7. Web-Proxyserver (optional) konfigurieren. Web-Proxy-Konfiguration ist, zwar optional werden Sie bei Verwendung ein Webproxy nur sie hier konfigurieren können.

    ![Web-Proxy konfigurieren](./media/storsimple-ova-deploy3-iscsi-setup/image9.png)

    Auf der Seite **WebProxy** :

    1. **Web-Proxy-URL** im folgenden Format angeben: *http://host-IP-Adresse* oder *FDQN:Port an*. Beachten Sie, dass HTTPS-URLs nicht unterstützt werden.

    2. Festlegen der **Authentifizierung** als **Basic** oder **None**.

    3. Wenn Sie Authentifizierung verwenden, müssen Sie einen **Benutzernamen** und **ein Kennwort**angeben.

    4. Klicken Sie auf **Übernehmen**. Überprüft wird und die konfigurierten Webproxyeinstellungen anwenden.
 
8. Intervalle für Ihr Gerät Zeitzone wie der primären und sekundären NTP-Servern (optional) konfigurieren. NTP-Server sind erforderlich, da das Gerät Zeit synchronisieren muss, damit es mit der Cloud authentifizieren kann.

    ![Uhrzeit](./media/storsimple-ova-deploy3-iscsi-setup/image10.png)

    Auf der Seite **Einstellungen** :

    1. Wählen Sie aus der Dropdown-Liste die **Zeitzone** anhand der geografischen Position in der Gerät bereitgestellt wird. PST ist die Standardzeitzone für Ihr Gerät. Das Gerät verwendet diese Zeitzone für alle geplanten Vorgänge.

    2. Geben Sie einen **primären NTP-Server** für das Gerät oder akzeptieren Sie den Standardwert time.windows.com. Sicherstellen Sie, dass Ihr Netzwerk NTP Datenverkehr vom Rechenzentrum mit dem Internet ermöglicht.

    3. Geben Sie optional einen **sekundäre NTP-Server** für Ihr Gerät.

    4. Klicken Sie auf **Übernehmen**. Dies überprüfen und konfigurierte Einstellungen.

9. Konfigurieren der Cloud für Ihr Gerät. In diesem Schritt die lokale Konfiguration beendet und das Gerät StorSimple Manager-Dienst registrieren.

    1. Den **Dienst-Registrierungsschlüssel** , die Sie in haben **Schritt2: Abrufen der Dienstregistrierungsschlüssel** in [Bereitstellen StorSimple Virtual Array - Portal vorbereiten](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

    2. Ist dies nicht das erste Gerät, das mit diesem Service registriert sind, müssen Sie den **Dienst Datenschlüssel**. Dieser Schlüssel ist mit Dienstregistrierungsschlüssel zusätzliche Geräte mit StorSimple-Manager-Dienst registriert. Weitere Informationen finden Sie auf der lokalen Benutzeroberfläche zu [den Verschlüsselungsschlüssel Daten](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) .

    3. Klicken Sie auf **Registrieren**. Dadurch wird das Gerät neu gestartet. Sie müssen warten Sie 2-3 Minuten, bevor das Gerät erfolgreich registriert. Nach dem Neustart des Geräts werden Sie auf die Anmeldeseite weitergeleitet.

       ![Gerät registrieren](./media/storsimple-ova-deploy3-iscsi-setup/image11.png)

10. Zurück zu klassischen Azure-Portal. Auf der Seite **Geräte** stellen Sie sicher, dass das Gerät mit dem Dienst hergestellt wurde anhand der Status. Der Gerätestatus wird **angezeigt**.

    ![Geräteseite](./media/storsimple-ova-deploy3-iscsi-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Schritt 2: Schließen Sie die erforderlichen Einrichtung

Um die Gerätekonfiguration des Geräts StorSimple abzuschließen, müssen Sie:

- Wählen Sie ein Speicherkonto, das Gerät zugeordnet.

- Wählen Sie für die Daten, die an der cloud-Verschlüsselung.

Die folgenden Schritte im klassischen Azure-Portal erforderlichen Setup abgeschlossen.

#### <a name="to-complete-the-minimum-device-setup"></a>Minimale Geräte-Einrichtung abgeschlossen

1. Wählen Sie auf der Seite **Geräte** das Gerät, das Sie gerade erstellt haben. Dieses Gerät wird als **aktiv**angezeigt. Klicken Sie auf dem Pfeil neben den Gerätenamen, und klicken Sie dann auf **Schnellstart**.

    ![Geräteseite](./media/storsimple-ova-deploy3-iscsi-setup/image13.png)

2. Klicken Sie auf **vollständige Gerät** um den Assistenten zum Konfigurieren von Geräten zu starten.

    ![Geräte-Assistent](./media/storsimple-ova-deploy3-iscsi-setup/image14.png)

3. Das Gerät konfigurieren auf **Standardeinstellungen** folgendermaßen Sie vor:

   1. Geben Sie ein Speicherkonto Geräts verwendet werden. Dieses Abonnement der Dropdownliste ein vorhandenes Speicherkonto auswählen oder **Weitere hinzufügen** , wählen Sie ein anderes Abonnement ein Konto angeben.

   2. Definieren Sie die Verschlüsselung für die Daten an die Cloud ruhende. (StorSimple verwendet AES-256-Verschlüsselung). Um die Daten zu verschlüsseln, aktivieren Sie das Kontrollkästchen **Cloud Storage-Verschlüsselung aktivieren** . Geben Sie eine Cloud-Speicher-Verschlüsselung, die 32 Zeichen. Geben Sie den Schlüssel, um es zu bestätigen.

   3. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-ova-deploy3-iscsi-setup/image15.png).

    ![Standardeinstellungen](./media/storsimple-ova-deploy3-iscsi-setup/image16.png)

    Die Einstellung werden jetzt aktualisiert. Nach Einstellung erfolgreich aktualisiert wurden, wird die Schaltfläche vollständige Einrichtung einrichten verfügbar. Sie kehren zum Gerät **Quick Start** -Seite zurück.                                                        

>[AZURE.NOTE]Sie können alle anderen geräteeinstellungen **Konfigurieren** Seite jederzeit ändern.

## <a name="step-3-add-a-volume"></a>Schritt 3: Hinzufügen eines Datenträgers

Die folgenden Schritte im klassischen Azure-Portal ein Volume erstellen.

#### <a name="to-create-a-volume"></a>Erstellen eines Volumes

1. Das Gerät **Schnellstart** klicken Sie auf **ein Volume hinzufügen**. Volume-Assistenten wird gestartet.

2. Volume-Assistenten unter **Standardeinstellungen**folgendermaßen Sie vor:

    1. Geben Sie einen eindeutigen Namen für den Datenträger. Der Name muss eine Zeichenfolge sein, die 3 bis 127 Zeichen enthält.

    2. Geben Sie eine Beschreibung für das Volume. Die Beschreibung wird die Volume-Besitzer identifizieren.

    3. Wählen Sie einen Verwendungstypen für das Volume. Verwendung möglich **Tiered Volume** oder **Volume lokal fixiert.** (**Tiered Volume** ist die Standardeinstellung.) Arbeitslasten, die lokale Garantien, niedrige Wartezeiten und höhere Leistung benötigen, wählen Sie **lokal fixiert** **Volume**. Wählen Sie für alle anderen Daten **Tiered** **Volume**.

        Ein lokales Volume dick bereitgestellt und stellt sicher, dass die primären Daten auf dem Datenträger auf dem Gerät bleibt und nicht in die Cloud verschütten. Wenn Sie lokale Volumes erstellen, überprüft das Gerät Speicherplatz auf lokalen Ebenen Bereitstellung ein Volumes der angeforderten Größe. Erstellen von lokalen Volumes erfordern verschütten vorhandene Daten in der Cloud und möglicherweise lange Zeit das Volume erstellt. Die gesamte Zeit hängt von der Größe das bereitgestellte Volume, Netzwerkbandbreite und Daten auf dem Gerät.

        Tiered Volume zum dünn bereitgestellt und kann sehr schnell erstellt werden. Beim Erstellen einer tiered Volumes etwa 10 % des Speicherplatzes auf der lokalen Ebene bereitgestellt und 90 % des in der Cloud bereitgestellt. Beispielsweise ein 1-TB-Volume bereitgestellt, 100 GB im lokalen Raum befinden würde und 900 GB wird in der Cloud bei der Datenebene. Dies bedeutet wiederum, die aus der lokalen Speicherplatz auf dem Gerät ausgeführt können nicht Sie mehrstufige Freigabe bereitstellen, (da die 10 % nicht verfügbar).

    4. Geben Sie die bereitgestellte Kapazität für den Datenträger. Beachten Sie, dass die angegebene Kapazität kleiner als die verfügbare Kapazität ist. Erstellen ein tiered Volume sollte die Größe 5 TB bis 500 GB. Geben Sie für ein lokales Volume eine Volume-Größe zwischen 50 und 500 GB. Verwenden Sie die verfügbare Kapazität als Leitfaden für die Bereitstellung eines Volumes. Wenn die verfügbaren lokalen 0 GB beträgt, dann nicht dürfen Sie ein lokales oder ein tiered Volume bereitstellen.

        ![Standardeinstellungen](./media/storsimple-ova-deploy3-iscsi-setup/image17.png)

    5. Klicken Sie auf das Pfeilsymbol ![Pfeil](./media/storsimple-ova-deploy3-iscsi-setup/image18.png) um zur nächsten Seite zu gelangen.

3. Fügen Sie auf der Seite **Einstellungen** einen neuen Access Control Datensatz (ACR hinzu):

    1. Geben Sie einen **Namen** für Ihre ACR.

    2. Geben Sie unter **iSCSI-Initiatornamen**iSCSI qualifizierten Namen (IQN) Windows-Host. Wenn der IQN haben, lesen Sie [Anhang A: Abrufen der IQN des Windows Server-Host](#appendix-a-get-the-iqn-of-a-windows-server-host).

    3. Wir empfehlen, eine Sicherung **einen Standard-Backup für dieses Volume aktivieren** das Kontrollkästchen zu aktivieren. Sicherung wird eine Richtlinie erstellen, die 22 Uhr täglich (Gerätezeit) ausgeführt und erstellt einen Snapshot Cloud dieses Volumes.

        ![zusätzliche Einstellung](./media/storsimple-ova-deploy3-iscsi-setup/image19.png)

    4. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Den Auftrag zum Erstellen von Volume wird gestartet. Sie sehen eine Statusnachricht ähnelt der folgenden.

        ![Statusnachricht](./media/storsimple-ova-deploy3-iscsi-setup/image20.png)

        Ein Volume wird mit den angegebenen Einstellungen erstellt. Standardmäßig werden Überwachung und Sicherung für das Volume aktiviert.

    5. Bestätigen, dass das Volume erfolgreich erstellt wurde, zum Wechseln der **Datenträger** -Seite Sie sollten das Volume aufgeführt sehen.

        ![](./media/storsimple-ova-deploy3-iscsi-setup/image21.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Schritt 4: Bereitstellen, initialisieren und Formatieren eines Datenträgers

Die folgenden Schritte bereitstellen, initialisieren und Formatieren der StorSimple-Volumes auf einem Windows-Server.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Laden, initialisiert und Formatieren eines Datenträgers

1. Starten Sie den Microsoft iSCSI-Initiator.

2. Klicken Sie im Fenster **iSCSI Initiator Properties** auf der Registerkarte **Erkennung** auf **Portal erkennen**.

    ![Entdecken Sie portal](./media/storsimple-ova-deploy3-iscsi-setup/image22.png)

3. Klicken Sie im Dialogfeld **Zielportal erkennen** die IP-Adresse des iSCSI-Netzwerk-Schnittstelle, und klicken Sie dann auf **OK**.

    ![IP-Adresse](./media/storsimple-ova-deploy3-iscsi-setup/image23.png)

4. Suchen Sie im Fenster **iSCSI Initiator Properties** auf der Registerkarte **Targets** **entdeckt Ziele**. (Jedes Volume werden erkannte Ziel.) Der Gerätestatus wird als **inaktiv**angezeigt.

    ![ermittelten Ziele](./media/storsimple-ova-deploy3-iscsi-setup/image24.png)

5. Wählen Sie ein Zielgerät aus und dann auf **Verbinden**. Nachdem das Gerät sollte den Status **verbunden**ändern. (Weitere Informationen zur Verwendung des Microsoft iSCSI-Initiators finden Sie unter [Installieren und Konfigurieren von Microsoft iSCSI-Initiator] [1].

    ![Zielgerät auswählen](./media/storsimple-ova-deploy3-iscsi-setup/image25.png)

6. Drücken Sie auf dem Windows-Host Windows-Logo-Taste + X und dann auf **Ausführen**.

7. Geben Sie **im Dialogfeld** **Diskmgmt.msc**. Klicken Sie auf **OK**, und das Dialogfeld **Datenträger** angezeigt. Rechten zeigt die Volumes auf dem Host.

8. Im Fenster **Datenträger** werden bereitgestellten Volumes angezeigt, wie in der folgenden Abbildung dargestellt. Maustaste ermittelten Volumes (klicken Sie auf Name des Laufwerks), und klicken Sie dann auf **Online**.

    ![Datenträger](./media/storsimple-ova-deploy3-iscsi-setup/image26.png)

9. Mit der rechten Maustaste, und wählen Sie **Datenträger initialisieren**.

    ![Datenträger 1 initialisieren](./media/storsimple-ova-deploy3-iscsi-setup/image27.png)

10. Wählen Sie im Dialogfeld die zu initialisierenden Datenträger, und klicken Sie auf **OK**.

    ![Festplatte 2 initialisieren](./media/storsimple-ova-deploy3-iscsi-setup/image28.png)

11. Neues einfaches Volume-Assistent wird gestartet. Wählen Sie Datenträger aus und dann auf **Weiter**.

    ![Assistenten 1](./media/storsimple-ova-deploy3-iscsi-setup/image29.png)

12. Das Volume zuweisen Sie einen Laufwerkbuchstaben zu, und klicken Sie auf **Weiter**.

    ![Assistent für neue Volume 2](./media/storsimple-ova-deploy3-iscsi-setup/image30.png)

13. Geben Sie die Parameter zum Formatieren des Datenträgers. **Unter Windows Server wird nur NTFS unterstützt.** Die AUS auf 64 KB festgelegt. Geben Sie eine Bezeichnung für den Datenträger. Es wird empfohlen für diesen Namen zu den Datenträgernamen mit virtuellen Geräts StorSimple bereitgestellt. Klicken Sie auf **Weiter**.

    ![Assistenten 3](./media/storsimple-ova-deploy3-iscsi-setup/image31.png)

14. Überprüfen Sie die Werte für den Datenträger, und klicken Sie dann auf **Fertig stellen**.

    ![Assistenten 4](./media/storsimple-ova-deploy3-iscsi-setup/image32.png)

    Die Volumes werden auf dem **Datenträger** als **Online** angezeigt.

    ![Volumes online](./media/storsimple-ova-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Verwenden der lokalen Web-Benutzeroberfläche zum [Verwalten der virtuellen StorSimple-Array](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Anhang A: Abrufen der IQN des Windows Server-Hosts

Führen Sie die folgenden Schritte iSCSI qualifizierten Namen (IQN) von einem Windows-Host, auf dem Windows Server 2012 ausgeführt wird.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Der IQN des Windows-Host zu

1. Starten Sie Microsoft iSCSI-Initiator auf dem Windows-Server.

2. Markieren Sie im Fenster **iSCSI Initiator Properties** auf der Registerkarte **Konfiguration** und kopieren Sie die Zeichenfolge aus dem Feld **Initiator** .

    ![iSCSI-Initiator-Eigenschaften](./media/storsimple-ova-deploy3-iscsi-setup/image34.png)

2. Speichern Sie diese Zeichenfolge.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



