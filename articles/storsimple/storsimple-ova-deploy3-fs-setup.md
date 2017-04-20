<properties
   pageTitle="3 - StorSimple Virtual Array Bereitstellen virtueller Geräte als Dateiserver einrichten"
   description="Dieses dritte Lernprogramm in StorSimple Virtual Array Bereitstellung weist das Einrichten eines virtuellen Geräts als Dateiserver."
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
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Bereitstellen von StorSimple Virtual Array - Satz als Dateiserver

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Einführung 

Dieser Artikel bezieht sich auf Microsoft Azure StorSimple Virtual Array (auch bekannt als StorSimple lokalen virtuellen Gerät oder virtuelles Gerät StorSimple) ausgeführten März 2016 allgemein verfügbar (GA). Dieser Artikel beschreibt, wie Erstinstallation, Dateiservers StorSimple registrieren, die Geräteinstallation abzuschließen und erstellen und Verbinden mit SMB-Freigaben. Dies ist der letzte Artikel der Serie Bereitstellung Lernprogramme virtual Array vollständig als Dateiserver oder ein iSCSI-Server verwendet.

Installation und Konfiguration-Prozess dauert ca. 10 Minuten.


## <a name="setup-prerequisites"></a>Installation erforderlicher Komponenten

Konfigurieren und Einrichten von virtuellen StorSimple-Gerät stellen Sie Folgendes sicher:

-   Sie haben ein virtuelles Gerät bereitgestellt und wie in die [Bereitstellung StorSimple virtuelles Array in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) oder [Bereitstellung StorSimple virtuelles Array in VMware](storsimple-ova-deploy2-provision-vmware.md)an.

-   Sie haben Dienstregistrierungsschlüssel aus StorSimple Manager-Dienst, den Sie erstellt StorSimple virtuelle Geräte verwalten. Weitere Informationen finden Sie unter [Schritt2: Abrufen der Dienstregistrierungsschlüssel](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) für StorSimple Virtual Array.

-   Ist dies der zweiten oder nachfolgenden virtuelles Gerät, das mit einem vorhandenen StorSimple Manager-Dienst registriert sind, müssen Sie den Verschlüsselungsschlüssel Daten. Dieser Schlüssel wurde generiert, wenn das erste Gerät bei diesem Dienst registriert wurde. Wenn Sie diesen Schlüssel verloren haben, finden Sie [den Verschlüsselungsschlüssel Daten abrufen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) StorSimple Virtual Array.

## <a name="step-by-step-setup"></a>Schrittweise Einrichtung

Verwenden Sie die folgenden Schritte zum Einrichten und konfigurieren Sie das virtuelle Gerät StorSimple.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Schritt 1: Lokale Benutzeroberfläche Setup abgeschlossen und Ihr Gerät registrieren 


#### <a name="to-complete-the-setup-and-register-the-device"></a>Die Einrichtung und das Gerät registrieren

1.  Öffnen Sie ein Browserfenster, und der lokale Web-Benutzeroberfläche an. Typ: 

    `https://<ip-address of network interface>`

    Verwenden Sie im vorherigen Schritt erwähnt Verbindungs-URL. Ein Fehler ist ein Problem mit dem Sicherheitszertifikat der Website angezeigt. Klicken Sie auf **dieser Webseite**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Melden Sie sich als **StorSimpleAdmin**in der Web-Benutzeroberfläche des virtuellen Geräts an. Geben Sie das Administratorkennwort für Geräte, die Sie geändert in Schritt 3: Bereitstellung [StorSimple virtuelles Array in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) oder bei [Einem Array StorSimple virtuellen VMware](storsimple-ova-deploy2-provision-vmware.md)virtuelle Gerät starten.

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  Sie gelangen auf **die Startseite** . Diese Seite beschreibt die verschiedenen Einstellungen konfigurieren und registrieren das virtuelle Gerät StorSimple Manager-Dienst erforderlich. Beachten Sie, dass die **Netzwerk-Einstellungen** **Webproxyeinstellungen**und **Intervalle** optional. Die einzige erforderliche Einstellung sind **Geräte** und **Cloud-Einstellungen**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  Auf der Seite **Einstellungen** unter **Netzwerkschnittstellen**werden Daten 0 automatisch konfiguriert werden. Jede Netzwerkschnittstelle wird standardmäßig automatisch zu IP-Adresse (DHCP). Daher werden eine IP-Adresse, Subnetz und Gateway automatisch (für IPv4 und IPv6) zugewiesen werden.

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Wenn Sie mehr als eine Netzwerkschnittstelle während der Bereitstellung des Geräts hinzugefügt haben, können Sie diese hier konfigurieren. Beachten Sie, dass die Netzwerkschnittstelle IPv4 nur oder als IPv4 und IPv6 konfigurieren können. Nur IPv6-Konfigurationen werden nicht unterstützt.

1.  DNS-Server sind erforderlich, da sie verwendet werden, wenn das Gerät mit der Cloud Storage kommunizieren oder zu Ihrem Gerät versucht namentlich als Dateiserver konfiguriert. Auf der Seite **Einstellungen** unter den **DNS-Server**:

    1.  Primärer und sekundärer DNS-Server wird automatisch konfiguriert. Wenn Sie statische IP-Adressen konfigurieren möchten, können Sie DNS-Server angeben. Für hohe Verfügbarkeit sollten Sie einen primären und einen sekundären DNS-Server konfigurieren.

    2.  Klicken Sie auf **Übernehmen**. Wendet und die Einstellungen überprüfen.

2.  Auf der Seite **Einstellungen** :

    1.  Das Gerät einen eindeutigen **Namen** zuweisen. Dieser Name kann 1-15 Zeichen lang sein und Buchstaben, Zahlen und Bindestriche enthalten.

    2.  Klicken Sie auf **Dateiserver** ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) für den **Typ** des Geräts, das Sie erstellen. Ein Dateiserver können Sie freigegebene Ordner erstellen.

    3.  Wie das Gerät einen Dateiserver ist, müssen Sie das Gerät zu einer Domäne hinzufügen. Geben Sie einen **Domänennamen**ein.

    1.  Klicken Sie auf **Übernehmen**.

2.  Ein Dialogfeld wird angezeigt. Geben Sie Ihre Anmeldeinformationen im angegebenen Format. Klicken Sie auf Kontrollkästchen. Die Anmeldeinformationen werden überprüft. Sie sehen eine Fehlermeldung, wenn die Anmeldeinformationen falsch sind.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Klicken Sie auf **Übernehmen**. Wendet und die Einstellungen überprüfen.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Sicherstellen Sie, dass Ihr virtuelle Array in eine eigene Organisationseinheit (OU) für Active Directory und keine Gruppenrichtlinienobjekte (GPO) angewendet oder geerbt werden. Gruppenrichtlinien kann wie Antivirensoftware auf StorSimple Virtual Array installieren. Zusätzliche Installation wird nicht unterstützt und kann zu einer datenbeschädigung führen. 

1.  Web-Proxyserver (optional) konfigurieren. Web-Proxy-Konfiguration ist, zwar optional werden Sie bei Verwendung ein Webproxy nur sie hier konfigurieren können.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    Der **WebProxy** -Seite:

    1.  **Web-Proxy-URL** im folgenden Format angeben: *http://&lt;Host-IP-Adresse oder FDQN&gt;: Port-Nummer*. Beachten Sie, dass HTTPS-URLs nicht unterstützt werden.

    2.  Festlegen der **Authentifizierung** als **Basic** oder **None**.

    3.  Wenn Authentifizierung verwenden, müssen Sie einen **Benutzernamen** und **ein Kennwort**angeben.

    4.  Klicken Sie auf **Übernehmen**. Überprüft wird und die konfigurierten Webproxyeinstellungen anwenden.

1.  Intervalle für Ihr Gerät Zeitzone wie der primären und sekundären NTP-Servern (optional) konfigurieren. NTP-Server sind erforderlich, da das Gerät Zeit synchronisieren muss, damit es mit der Cloud authentifizieren kann.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    Auf der Seite **Einstellungen** :

    1.  Wählen Sie aus der Dropdown-Liste die **Zeitzone** anhand der geografischen Position in der Gerät bereitgestellt wird. PST ist die Standardzeitzone für Ihr Gerät. Das Gerät verwendet diese Zeitzone für alle geplanten Vorgänge.

    2.  Geben Sie einen **primären NTP-Server** für das Gerät oder akzeptieren Sie den Standardwert time.windows.com. Sicherstellen Sie, dass Ihr Netzwerk NTP Datenverkehr vom Rechenzentrum mit dem Internet ermöglicht.

    3.  Geben Sie optional einen **sekundäre NTP-Server** für Ihr Gerät.

    4.  Klicken Sie auf **Übernehmen**. Dies überprüfen und konfigurierte Einstellungen.

1.  Konfigurieren der Cloud für Ihr Gerät. In diesem Schritt die lokale Konfiguration beendet und das Gerät StorSimple Manager-Dienst registrieren.

    1.  Den **Dienst-Registrierungsschlüssel** , die Sie in haben [Schritt2: Abrufen der Dienstregistrierungsschlüssel](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) für StorSimple Virtual Array.

    2.  Überspringen Sie diesen Schritt aus, wenn dies das erste Gerät mit diesem Service registriert ist und gehen Sie zum nächsten Schritt. Ist dies nicht das erste Gerät, das mit diesem Service registriert sind, müssen Sie den **Dienst Datenschlüssel**. Dieser Schlüssel ist mit Dienstregistrierungsschlüssel zusätzliche Geräte mit StorSimple-Manager-Dienst registriert. Finden Sie weitere Informationen zu [Service Datenschlüssel](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) auf dem lokalen Webserver Benutzeroberfläche.

    3.  Klicken Sie auf **Registrieren**. Dadurch wird das Gerät neu gestartet. Sie müssen warten Sie 2-3 Minuten, bevor das Gerät erfolgreich registriert. Nach dem Neustart des Geräts werden Sie auf die Anmeldeseite weitergeleitet.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Zurück zu klassischen Azure-Portal. Auf der Seite **Geräte** stellen Sie sicher, dass das Gerät mit dem Dienst hergestellt wurde anhand der Status. Der Gerätestatus wird **angezeigt**.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Schritt 2: Schließen Sie die erforderlichen Einrichtung

Um die Gerätekonfiguration des Geräts StorSimple abzuschließen, müssen Sie:

-   Wählen Sie ein Speicherkonto, das Gerät zugeordnet.

-   Wählen Sie für die Daten, die an der cloud-Verschlüsselung.

Führen Sie folgenden Schritte in der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) erforderlichen Setup abgeschlossen.

#### <a name="to-complete-the-minimum-device-setup"></a>Minimale Geräte-Einrichtung abgeschlossen

1.  Wählen Sie auf **Geräte** das Gerät gerade erstellten. Dieses Gerät wird als **aktiv**angezeigt. Klicken Sie auf den Pfeil für den Gerätenamen und klicken Sie dann auf **Schnellstart**.

2.  Klicken Sie auf **vollständige Gerät** um den Assistenten zum Konfigurieren von Geräten zu starten.

3.  Das Gerät konfigurieren auf **Standardeinstellungen** folgendermaßen Sie vor:

    1.  Geben Sie ein Speicherkonto Geräts verwendet werden. Sie können ein vorhandenes Speicherkonto in diesem Abonnement aus der Dropdownliste auswählen oder **Weitere hinzufügen** , um ein Konto ein anderes Abonnement wählen.

    2.  Definieren Sie die Verschlüsselung für alle die Data-at-Rest (AES-Verschlüsselung), die an der Cloud. Um die Daten zu verschlüsseln, das Kontrollkästchen Sie Kombinationsfeld **Cloud Storage Schlüssel**aktivieren. Geben Sie eine Cloud-Speicher-Verschlüsselung, die 32 Zeichen. Geben Sie den Schlüssel, um es zu bestätigen. Ein 256-Bit AES-Schlüssel wird mit dem benutzerdefinierten Schlüssel für die Verschlüsselung verwendet.

    3.  Klicken Sie auf Check ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

Die Einstellung werden jetzt aktualisiert. Nach Einstellung erfolgreich aktualisiert wurden, wird die vollständige Einrichtung Setup Schaltfläche abgeblendet. Sie kehren zum Gerät **Quick Start** -Seite zurück.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> Sie können alle anderen geräteeinstellungen **Konfigurieren** Seite jederzeit ändern.

## <a name="step-3-add-a-share"></a>Schritt 3: Hinzufügen einer Freigabe

Die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/) eine Freigabe erstellen.

#### <a name="to-create-a-share"></a>Erstellen eine Freigabe

1.  Das Gerät **Schnellstart** klicken Sie auf **Freigabe hinzufügen**. Freigabe-Assistenten wird gestartet.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Klicken Sie auf **Standardeinstellungen** folgendermaßen Sie vor:

    1.  Geben Sie einen eindeutigen Namen für die Freigabe. Der Name muss eine Zeichenfolge sein, die 3 bis 127 Zeichen enthält.

    2.  (Optional) Geben Sie eine Beschreibung für die Freigabe. Die Beschreibung wird die Freigabe Besitzer identifizieren.

    3.  Wählen Sie einen Verwendungstypen für die Freigabe. Der Verwendungstyp kann **Tiered** oder **lokal fixiert**, mit tiered Standard. Arbeitslasten, die lokale Garantien, niedrige Wartezeiten und höhere Leistung benötigen, wählen Sie eine Freigabe **lokal fixiert** . Wählen Sie für alle Daten einer **Tiered** -Freigabe.

    Lokales Freigabe dick bereitgestellt und stellt sicher, dass die primären Daten auf der Freigabe das Gerät lokal bleibt und nicht in die Cloud verschütten. Mehrstufige Freigabe wird zum dünn bereitgestellt. Beim Erstellen einer tiered Freigabe 10 % des Speicherplatzes auf der lokalen Ebene bereitgestellt und 90 % des in der Cloud bereitgestellt. Beispielsweise ein 1-TB-Volume bereitgestellt, 100 GB im lokalen Raum befinden würde und 900 GB wird in der Cloud bei der Datenebene. Dies bedeutet wiederum, dass wenn Sie aus der lokalen Speicherplatz auf dem Gerät ausführen, mehrstufige Freigabe bereitstellen kann nicht

1.  Die bereitgestellte Kapazität für die Freigabe angeben. Beachten Sie, dass die angegebene Kapazität kleiner als die verfügbare Kapazität ist. Mehrstufige Freigabe verwenden, sollte die Freigabe Größe 500 GB bis 20 TB. Bei einer lokalen Freigabe Schriftgrad eine Freigabe 50 GB bis 2 TB. Orientieren Sie die verfügbare Kapazität eine Freigabe bereitstellen. Wenn die verfügbaren lokalen 0 GB beträgt, dann nicht dürfen Sie lokale oder gestufte Freigaben bereitstellen.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  Klicken Sie auf das Pfeilsymbol ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) um zur nächsten Seite zu wechseln.

1.  Berechtigungen Sie auf der Seite **Berechtigungen** die auf den Benutzer oder die Gruppe, die auf diese Freigabe zugreifen. Geben Sie den Namen des Benutzers oder der Benutzergruppe in *<john@contoso.com>* Format. Wir empfehlen eine Benutzergruppe (anstelle eines einzigen Benutzers) verwenden, um Administratorrechte auf diese Freigaben zugreifen können. Nachdem Sie die Berechtigungen zugewiesen haben, können Sie dann Windows Explorer verwenden, diese Berechtigungen ändern.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Klicken Sie auf Check ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Eine Freigabe wird mit den angegebenen Einstellungen erstellt. Standardmäßig werden Überwachung und Sicherung für die Freigabe aktiviert.

## <a name="step-4-connect-to-the-share"></a>Schritt 4: Verbindung zur Freigabe

Sie müssen jetzt den Buchhaltungsdaten herstellen, die Sie im vorherigen Schritt erstellt haben. Führen Sie diese Schritte auf Ihrem Windows-Server.

#### <a name="to-connect-to-the-share"></a>Verbindung mit der Freigabe

1.  Drücken Sie ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. Geben Sie im Fenster Ausführen den * \\ * den Pfad ersetzen *Servernamen* der Gerätename, die Ihr Dateiserver zugewiesen. Klicken Sie auf **OK**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  Explorer wird geöffnet. Sie sollte nun die erstellten Freigaben als Ordner sehen. Wählen Sie aus, und doppelklicken Sie auf eine Freigabe (Ordner) zum Anzeigen des Inhalts.

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  Sie können nun Dateien Aktien und sichert.

![Symbol: Video](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **Video verfügbar**

Sehen Sie sich das Video zum Konfigurieren und StorSimple virtuelles Array als Dateiserver registrieren.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Verwenden der lokalen Web-Benutzeroberfläche zum [Verwalten der virtuellen StorSimple-Array](storsimple-ova-web-ui-admin.md).
