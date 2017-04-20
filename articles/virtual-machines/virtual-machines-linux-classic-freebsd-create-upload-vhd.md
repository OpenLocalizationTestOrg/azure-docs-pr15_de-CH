<properties
   pageTitle="Erstellen und Hochladen einer FreeBSD VM-Image | Microsoft Azure"
   description="Informationen Sie zum Erstellen und Laden eine virtuelle Festplatte (VHD), die enthält das Betriebssystem FreeBSD Azure virtuellen Computer erstellen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Erstellen und Hochladen einer VHD FreeBSD in Azure

In diesem Artikel wird das Erstellen und eine virtuelle Festplatte (VHD), die das Betriebssystem FreeBSD enthält, veranschaulicht. Nachdem er hochgeladen können es als Ihr eigenes Bild Sie einen virtuellen Computer (VM) in Azure erstellen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Erforderliche Komponenten
Es wird vorausgesetzt, dass Sie über Folgendes verfügen:

- **Ein Azure-Abonnement**– Wenn Sie kein Konto haben, können Sie in wenigen Minuten erstellen. Wenn Sie ein MSDN-Abonnement verfügen, finden Sie unter [monatliche Azure-Gutschrift für Visual Studio Abonnenten.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Erfahren Sie, wie [ein kostenloses Testabo erstellen](https://azure.microsoft.com/pricing/free-trial/).  

- **Azure PowerShell-Tools**– der Azure PowerShell Modul installiert und Ihr Azure-Abonnement konfiguriert werden muss. Herunterladen des Moduls finden Sie in der [Azure-downloads](https://azure.microsoft.com/downloads/). Eine Anleitung, die beschreibt wie installieren und Konfigurieren des Moduls ist hier verfügbar. Verwenden Sie das Cmdlet [Azure downloadet](https://azure.microsoft.com/downloads/) die virtuelle Festplatte hochladen.

- **FreeBSD Betriebssystem in einer VHD-Datei**muss – eine unterstützt FreeBSD auf einer virtuellen Festplatte installiert werden. Mehrere Tools vorhanden zum Erstellen von VHD-Dateien. Beispielsweise können Sie eine Virtualisierungslösung wie Hyper-V VHD-Datei erstellen und Installieren des Betriebssystems. Weitere Informationen zur Installation und Verwendung von Hyper-V [Installieren Sie Hyper-V und erstellen einen virtuellen Computer](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Neuere VHDX-Format wird in Azure nicht unterstützt. Hyper-V-Manager oder das Cmdlet [Vhd konvertieren](https://technet.microsoft.com/library/hh848454.aspx), um die Festplatte in VHD-Format konvertieren. Darüber hinaus ist ein [Lernprogramm auf MSDN zur Verwendung von FreeBSD mit Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

Diese Aufgabe umfasst die folgenden fünf Schritte.

## <a name="step-1-prepare-the-image-for-upload"></a>Schritt 1: Vorbereiten des Abbilds für Uploads

Führen Sie die folgenden Schritte auf dem virtuellen Computer, auf dem das Betriebssystem FreeBSD installiert:

1. Aktivieren Sie DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. SSH aktivieren.

    Nach der Installation von CD wird SSH standardmäßig aktiviert. Geben Sie Wenn sie aus irgendeinem Grund nicht aktiviert ist oder FreeBSD VHD direkt verwenden, Folgendes an:

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Einrichten einer seriellen Konsole.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Installieren Sie Sudo.

    In Azure ist das Root-Konto deaktiviert. Daher müssen Sie Sudo aus ein Benutzer zum Ausführen der Befehle mit erhöhten nutzen.

        # pkg install sudo
;
5. Erforderliche Komponenten für Azure Agent.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Installieren Sie Azure Agent.

    Immer auf [Github](https://github.com/Azure/WALinuxAgent/releases)finden Sie die neueste Version von Azure-Agent. Version 2.0.10 unterstützt + offiziell unterstützt FreeBSD 10 und 10.1 Version 2.1.4 offiziell FreeBSD 10.2 und späteren Versionen.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    2.0 verwenden wir 2.0.16 als Beispiel:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    2.1 verwenden wir 2.1.4 als Beispiel:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Nach der Installation von Azure Agent ist sollten Sie überprüfen, ob er ausgeführt wird:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. Entziehen Sie das System.

    Entziehen Sie das System zu reinigen für eine erneute Bereitstellung. Der folgende Befehl löscht auch das letzte bereitgestellte Benutzerkonto und die zugehörigen Daten:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Jetzt können Sie Ihre virtuellen Computer herunterfahren.

## <a name="step-2-create-a-storage-account-in-azure"></a>Schritt 2: Erstellen Sie ein Speicherkonto in Azure ##

Sie benötigen ein Speicherkonto in Azure VHD-Datei hochladen, damit sie zu einem virtuellen Computer verwendet werden kann. Klassische Azure-Portal können Sie ein Speicherkonto erstellen.

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.

2. Wählen Sie auf der Befehlsleiste **neu**.

3. Wählen Sie **Data Services** > **Speicher** > **schnell erstellen**.

    ![Schnelles Erstellen Sie ein Speicherkonto](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Füllen Sie die Felder wie folgt aus:

    - Geben Sie im Feld **URL** Subdomainnamen Storage Konto URL verwenden. Der Eintrag kann 3 24 Ziffern und Kleinbuchstaben enthalten. Dieser Name wird der Hostname in der URL, die auf Azure BLOB-Speicher, Azure Queue Storage oder Speicherressourcen Azure-Tabelle für das Abonnement verwendet wird.

    - Wählen Sie im Dropdown- **Standort/Gruppe** der **Standort oder Gruppe** speichern. Eine Gruppe können Sie die Cloud-Services und Speicher im gleichen Rechenzentrum.

    - Entscheiden Sie im Feld **Replikation** **Geo** redundanter für das Speicherkonto verwenden. Geo-Replikation ist standardmäßig aktiviert. Diese Option repliziert Daten an einem sekundären Standort kostenlos, damit Ihr Speichersystem an diesen Speicherort Failover bei großen Fehler am primären Standort. Sekundäre Standort wird automatisch zugewiesen und kann nicht geändert werden. Benötigen Sie mehr Kontrolle über den Speicherort der Cloud-basierte gesetzlichen oder Unternehmensrichtlinien können Sie Geo-Replikation deaktivieren. Bedenken Sie jedoch, wenn später Geo-Replikation aktivieren Sie eine einmalige Übertragung vorhandenen Daten auf den sekundären Standort repliziert Gebühr. Speicher werden ohne Geo-Replikation zu einem ermäßigten Preis angeboten. Weitere Informationen zum Verwalten von Geo-Replikation Speicherkonten finden Sie hier: [erstellen, verwalten oder löschen Sie ein Speicherkonto](../storage-create-storage-account/#replication-options).

    ![Speicher-Kontoinformationen eingeben](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Wählen Sie **Konto erstellen**. Das Konto wird nun unter **Speicher**.

    ![Speicher-Konto erstellt](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Erstellen Sie anschließend einen Container für die hochgeladenen VHD-Dateien. Wählen Sie den speicherkontonamen und dann **Container**.

    ![Speicher-Kontodetails](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Wählen Sie **einen Container erstellen**.

    ![Speicher-Kontodetails](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. Geben Sie im Feld **Name** einen Namen für den Container. Wählen Sie dann im Dropdownmenü **Zugriff** Zugriffsrichtlinie gewünschte.

    ![Containername](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Standardmäßig der Container ist privat und kann nur vom Kontoinhaber zugegriffen werden. Um öffentliche Lesezugriff Blobs im Container jedoch nicht die Eigenschaften und Metadaten ermöglichen die Option **Blob für den öffentlichen** . Um öffentliche Lesezugriff für den Container und Blobs zu ermöglichen, die Option **Öffentlichen Container** .

## <a name="step-3-prepare-the-connection-to-azure"></a>Schritt 3: Die Verbindung zum Azure vorzubereiten

Vor dem Hochladen einer VHD-Datei müssen Sie eine sichere Verbindung zwischen Ihrem Computer und Ihrem Azure-Abonnement einrichten. Azure Active Directory (Azure AD) Methode oder die Zertifikat-Methode können Sie dazu.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Azure AD-Methode eine VHD-Datei hochladen

1. Azure PowerShell-Konsole zu öffnen.

2. Geben Sie folgenden Befehl ein:  
    `Add-AzureAccount`

    Dieser Befehl öffnet anmelden, Ihren Arbeits- oder Schulcomputer Konto anmelden.

    ![PowerShell-Fenster](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure authentifiziert und speichert die Anmeldeinformationen. Dann wird das Fenster geschlossen.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Verwenden Sie die Zertifikat-Methode eine VHD-Datei hochladen

1. Azure PowerShell-Konsole zu öffnen.

2. Typ:  `Get-AzurePublishSettingsFile`.

3. Ein Browserfenster wird geöffnet und fordert Sie auf, eine PUBLISHSETTINGS-Datei herunterladen. Diese Datei enthält Informationen und ein Zertifikat für Ihre Azure-Abonnement.

    ![Browser Download-Seite](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Speichern Sie die Datei "publishsettings".

4. Typ:  `Import-AzurePublishSettingsFile <PathToFile>`, wobei `<PathToFile>` ist der vollständige Pfad zu der Datei "publishsettings".

   Weitere Informationen finden Sie unter [Erste Schritte mit Azure-Cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Weitere Informationen zum Installieren und Konfigurieren von PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Schritt 4: Laden Sie die VHD-Datei

Wenn Sie die VHD-Datei hochladen, können Sie es überall im BLOB-Speicher ablegen. Es folgen einige Begriffe, die Sie verwenden, wenn Sie die Datei hochladen:
-  **BlobStorageURL** ist die URL für das Speicherkonto, das Sie in Schritt2 erstellt haben.
-  **YourImagesFolder** ist der Container im BLOB-Speicher, wo Sie die Bilder speichern möchten.
- **VHDName** ist die Beschriftung im klassischen Azure-Portal zu der virtuellen Festplatte.
- **PathToVHDFile** ist der vollständige Pfad und Name der VHD-Datei.


Geben Sie im vorherigen Schritt verwendeten Azure PowerShell-Fenster

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Schritt 5: Erstellen einer VM mit der hochgeladenen VHD-Datei
Nach dem Hochladen der VHD-Datei können Sie es als Bild in die Liste der benutzerdefinierten Bilder hinzufügen, die Ihrem Abonnement zugeordnet sind, und erstellen einen virtuellen Computer mit dieser benutzerdefinierten.

1. Geben Sie im vorherigen Schritt verwendeten Azure PowerShell-Fenster

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Verwenden Sie Linux als Betriebssystem. Die aktuelle Version von Azure PowerShell akzeptiert nur "Linux" oder "Windows" als Parameter.

2. Nach Abschluss der Schritte wird das neue Bild bei Auswahl die Registerkarte **Bilder** klassischen Azure-Portal aufgeführt.  

    ![Wählen Sie ein Bild](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Erstellen Sie einen virtuellen Computer aus der Galerie. Das neue Bild wird jetzt unter **Eigene Bilder**.
4. Wählen Sie das neue Bild. Anschließend durchlaufen Sie auf einen Host-Name, Kennwort, SSH-Schlüssel usw. einrichten.

    ![Benutzerdefiniertes Bild](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Nach Abschluss der Bereitstellung sehen Sie FreeBSD VM in Azure ausgeführt.

    ![FreeBSD Bild in azure](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)
