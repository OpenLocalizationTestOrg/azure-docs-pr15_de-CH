<properties
    pageTitle="SQL Server Business Intelligence | Microsoft Azure"
    description="In diesem Thema Ressourcen mit dem klassischen Bereitstellungsmodell erstellt und beschreibt die Features Business Intelligence (BI) für SQL Server auf Azure virtuelle Maschinen (VMs) ausgeführt."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server Business Intelligence in Azure Virtual Machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Microsoft Azure Virtual Machine-Katalog enthält Bilder, die SQL Server-Installationen enthalten. In der Galeriebilder unterstützt SQL Server-Editionen sind die gleichen Installationsdateien auf lokalen Computern und virtuellen Computern installieren können. In diesem Thema werden die Funktionen SQL Server Business Intelligence (BI) auf die Bilder und Konfigurationsschritte nach ein virtuellen Computer bereitgestellt wird. Dieses Thema beschreibt außerdem unterstützte Topologien für BI-Funktionen und best Practices.

## <a name="license-considerations"></a>Aspekte der Lizenz

Gibt es zwei Methoden, um SQL Server in Microsoft Azure Virtual Machines Lizenz:

1. Lizenz Mobilität Leistungen, die Teil der Software Assurance Weitere Informationen finden Sie unter [Lizenzmobilität durch Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. Bezahlung pro Stunde von Azure virtuelle Computer mit SQL Server installiert. Siehe Abschnitt "SQL Server" bei der [Preisgestaltung von virtuellen Maschinen](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Finden Sie weitere Informationen über Lizenzierung und aktuellen [Virtuellen Maschinen Lizenzierung FAQ](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server Bilder in Azure Virtual Machine-Galerie

Microsoft Azure Virtual Machine-Katalog enthält mehrere Bilder, die Microsoft SQL Server enthalten. Software auf virtuellen Computerimages hängt von der Version des Betriebssystems und die Version von SQL Server. Die Liste der Bilder in der Galerie Azure Virtual Machine häufig ändert.

![SQL Azure VM-Galerie Bild](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) PowerShell-Skript gibt die Liste von Azure Bilder "SQL Server" in der ImageName:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Weitere Informationen zu Editionen und von SQL Server unterstützten Funktionen finden Sie unter:

- [SQL Server-Editionen](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [Von den Editionen von SQL Server 2016 unterstützte Funktionen](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>BI-Funktionen auf den SQL Server virtueller Computer Galeriebilder

Die folgende Tabelle zeigt die Business Intelligence-Funktionen auf Allgemeine Microsoft Azure Virtual Machine Galeriebilder für SQL Server "

- SQL Server 2016 RC3

- SQL Server 2014 SP1 Enterprise

- SQL Server 2014 SP1 Standard

- SQL Server 2012 SP2 Enterprise

- SQL Server 2012 SP2 Standard

|SQL Server BI-Funktion|Installiert das Galeriebild|Notizen|
|---|---|---|
|**Reporting Services im einheitlichen Modus**|Ja|Konfiguration der Berichts-Manager-URLs erfordert jedoch installiert. Siehe Abschnitt [Reporting Services konfigurieren](#configure-reporting-services).|
|**Reporting Services SharePoint-Modus**|Nein|Microsoft Azure Virtual Machine Galeriebild schließt nicht SharePoint- oder SharePoint-Installationsdateien. <sup>1</sup>|
|**Analysis Services-mehrdimensionale und Data Mining (OLAP)**|Ja|Installation und Konfiguration als Standard-Analysis Services-Instanz|
|**Tabellarische Analysis Services**|Nein|In SQL Server 2012 unterstützt wird 2014 und 2016 Bilder, aber es nicht standardmäßig installiert. Installieren Sie eine andere Instanz von Analysis Services. Siehe Abschnitt Installieren anderer SQL Server-Dienste und Features in diesem Thema.|
|**Analysis Services PowerPivot für SharePoint**|Nein|Microsoft Azure Virtual Machine Galeriebild schließt nicht SharePoint- oder SharePoint-Installationsdateien. <sup>1</sup>|

<sup>1</sup> Weitere Informationen zu SharePoint und Azure virtuellen Computern finden Sie unter [Microsoft Azure Architekturen für SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) und [Bereitstellung von SharePoint auf Microsoft Azure Virtual Machines](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Führen Sie den folgenden PowerShell-Befehl eine Liste der installierten Dienste abrufen, die den Namen "SQL" enthalten.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Allgemeine Empfehlung und Best Practices

- Die empfohlene Mindestgröße für eine virtuelle Maschine ist **A3** , wenn SQL Server Enterprise Edition verwenden. Die Größe des virtuellen Computers **A4** empfiehlt SQL Server BI-Bereitstellung von Analysis Services und Reporting Services.

    Finden Sie Informationen zum aktuellen VM-Größen [Größen für Azure Virtual Machine](virtual-machines-linux-sizes.md).

- Empfohlen für Datenträger zum Speichern von Daten, Protokolldateien und Sicherungsdateien auf anderen Laufwerken als **C**ist: und **D**:. Erstellen Sie z. B. Datenträger **E**: und **F**:.

    - Die Cachingrichtlinie für das Standardlaufwerk **C**Laufwerk: ist nicht optimal für die Arbeit mit Daten.

    - **D**: Laufwerk ist ein temporäres Verzeichnis, das hauptsächlich für die Auslagerungsdatei verwendet wird. **D**: Laufwerk nicht beibehalten und nicht im BLOB-Speicher gespeichert. Verwaltungsaufgaben, wie z. B. eine Änderung der Größe des virtuellen Computers **D**zurückgesetzt: Laufwerk. Es wird empfohlen, **nicht** verwenden, **D**: Laufwerk für Datenbankdateien, einschließlich Tempdb.

    Weitere Informationen zum Erstellen und Anhängen von Festplatten finden Sie unter [wie ein Datenträger mit einem virtuellen Computer](virtual-machines-windows-classic-attach-disk.md).

- Beenden oder Deinstallieren von Diensten, die Sie nicht verwenden möchten. Zum Beispiel wenn der virtuelle Computer nur für Reporting Services verwendet beenden oder deinstallieren Sie Analysis Services und SQL Server Integration Services. Das folgende Bild ist ein Beispiel für die Dienste, die standardmäßig gestartet.

    ![SQL Server-Dienste](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)

    >[AZURE.NOTE] Die SQL Server-Datenbank-Engine ist in unterstützten BI-Szenarien erforderlich. Auf einem Einzelserver VM Topologie muss das Datenbankmodul auf dem gleichen virtuellen Computer ausgeführt werden.

    Weitere Informationen finden Sie im folgenden: [Reporting Services deinstallieren](https://msdn.microsoft.com/library/hh479745.aspx) und [eine Instanz von Analysis Services deinstallieren](https://msdn.microsoft.com/library/ms143687.aspx).

- **Windows Update** und neue "wichtige Updates" prüfen. Microsoft Azure Virtual Machine-Images werden häufig aktualisiert. jedoch können wichtige Updates werden von **Windows Update** nach der letzten Aktualisierung der VM-image

## <a name="example-deployment-topologies"></a>Beispiel-Bereitstellungstopologien

Im folgenden sind Beispiel Bereitstellung, mit dem virtuelle Maschinen von Microsoft Azure. Topologien in diesen Diagrammen sind einige mögliche Topologien Microsoft Azure virtuelle Computer mit SQL Server BI-Funktionen verwenden.

### <a name="single-virtual-machine"></a>Einzelne virtuelle Maschine

Analysis Services, Reporting Services, SQL Server-Datenbankmodul und Daten Quellen auf einem virtuellen Computer.

![Szenario 1 virtuellen BI IAS](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Zwei virtuelle Computer

- Analysis Services, Reporting Services und SQL Server-Datenbank-Engine auf einem virtuellen Computer. Die Bereitstellung umfasst Report Server-Datenbanken.

- Datenquellen auf einer zweiten VM. Die zweite VM enthält SQL Server-Datenbankmodul als Datenquelle.

![Iaas BI-Szenario mit 2 virtuelle Computer](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Gemischte Azure-Daten in SQL Azure-Datenbank

- Analysis Services, Reporting Services und SQL Server-Datenbank-Engine auf einem virtuellen Computer. Die Bereitstellung umfasst Report Server-Datenbanken.

- SQL Azure-Datenbank ist.

![BI-Szenarien Neuerung und AzureSQL als Datenquelle](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybrid-Daten lokal

- In diesem beispielbereitstellung Analysis Services führen Reporting Services und SQL Server-Datenbank-Engine auf einem virtuellen Computer. Der virtuelle Computer hostet Report Server-Datenbanken. Der virtuelle Computer ist Mitglied einer lokalen Domäne durch Azure Virtual Networking oder einige andere VPN-Tunneling-Lösung.

- Lokal ist.

![BI-Szenarien Neuerung und auf lokale Daten](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Reporting Services einheitlichen Konfiguration

Galeriebild virtuellen Computer für SQL Server enthält Reporting Services einheitlichen Modus installiert, aber der Berichtsserver nicht konfiguriert ist. Die Schritte in diesem Abschnitt Konfigurieren Sie den Reporting Services-Berichtsserver. Weitere Informationen zum Konfigurieren von Reporting Services pur finden Sie unter [Installieren Reporting Services systemeigenen Modus Bericht Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

>[AZURE.NOTE] Ähnliche Inhalte, die Windows PowerShell-Skripts zum Konfigurieren des Berichtsservers verwendet, finden Sie [Mit PowerShell ein Azure VM mit einer einheitlichen Modus Berichtsserver erstellt](virtual-machines-windows-classic-ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Verbinden mit dem virtuellen Computer, und starten Sie die Reporting Services

Es gibt zwei allgemeine Arbeitsabläufe für die Verbindung zu einer Azure Virtual Machine:

- Zu verbinden, klicken Sie auf den Namen des virtuellen Computers **Klicken**. Öffnet eine Remotedesktopverbindung und die Computername automatisch ausgefüllt.

    ![Verbinden Sie mit virtuellen Azure-Computer](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)

- Verbinden Sie mit virtuellen Maschine mit Windows-Remotedesktopverbindung. In der Benutzeroberfläche des Remotedesktops:

    1. Geben Sie den **Dienstnamen Cloud** als Computername.

    1. Geben Sie Doppelpunkt und die öffentliche Portnummer, die für den remote desktop TCP-Endpunkt konfiguriert ist.

        MyService.cloudapp.NET:63133

        Weitere Informationen finden Sie unter [Was ist Cloud-Dienst?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).

**Starten Sie Reporting Services-Konfigurations-Manager.**

1. In **Windows Server 2012**:

1. Geben Sie **den Startbildschirm** **Reporting Services** Apps aufgelistet.

1. **Konfigurations-Manager für Reporting Services** und **als Administrator ausführen**.

1. In **Windows Server 2008 R2**:

1. Klicken Sie auf **Start**und dann auf **Alle Programme**.

1. Klicken Sie auf **Microsoft SQL Server 2016**.

1. Klicken Sie auf **Konfigurationstools**.

1. **Konfigurations-Manager für Reporting Services** und **als Administrator ausführen**.

Oder

1. Klicken Sie auf **Start**.

1. Geben Sie im Dialogfeld **Programme / Dateien durchsuchen** **reporting Services**. Wenn die VM Windows Server 2012 ausgeführt wird, geben Sie **reporting Services** auf dem Startbildschirm von Windows Server 2012.

1. **Konfigurations-Manager für Reporting Services** und **als Administrator ausführen**.

    ![Ssrs-Konfigurations-Manager suchen](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Konfigurieren von Reporting Services

**Dienstkonto und Webdienst-URL:**

1. Überprüfen Sie den **Servernamen** Name des lokalen Servers, und klicken Sie auf **Verbinden**.

1. Beachten Sie leere **Datenbank Berichtsservername**. Die Datenbank wird erstellt, wenn die Konfiguration abgeschlossen ist.

1. Überprüfen Sie den **Serverstatus Bericht** **gestartet**. Wenn Sie den Dienst in Windows Server-Manager überprüfen möchten, ist die **SQL Server Reporting Services** Windowsdienst.

1. Auf **Dienstkonto** und ändern Sie das Konto nach Bedarf. Wenn der virtuelle Computer in einer Umgebung ohne Domäne verknüpften verwendet wird, ist das integrierte Konto **ReportServer** ausreichend. Informationen für das Dienstkonto finden Sie unter [Dienstkonto](https://msdn.microsoft.com/library/ms189964.aspx).

1. Klicken Sie im linken Bereich auf **Webdienst-URL** .

1. Klicken Sie auf **Übernehmen** , um die Standardwerte konfigurieren.

1. Beachten Sie den **Report Server-Webdienst-URLs**. Beachten Sie der TCP-Standardport 80 und ist Teil der URL. In einem späteren Schritt erstellen Sie ein Microsoft Azure Virtual Machine für den Port.

1. Überprüfen Sie im **Ergebnisbereich** die Aktionen erfolgreich abgeschlossen.

**Datenbank:**

1. Klicken Sie im linken Bereich auf **Datenbank** .

1. Klicken Sie auf **Änderungsdatenbank**.

1. Überprüfen Sie **erstellen eine neue Berichtsserver-Datenbank** ausgewählt ist, und klicken Sie auf Weiter.

1. Überprüfen Sie **Servernamen** , und klicken Sie auf **Verbindung testen**.

1. Wenn das Ergebnis **erfolgreicher Verbindung testen**, klicken Sie auf **OK** und dann auf **Weiter**.

1. Hinweis der Datenbankname **ReportServer** und **Berichtsserver Modus** **ist** dann auf **Weiter**.

1. Klicken Sie auf der Seite **Anmeldeinformationen** auf **Weiter** .

1. Klicken Sie auf der Seite **Zusammenfassung** auf **Weiter** .

1. Klicken Sie auf **Weiter** , auf der Seite **fortsetzen und Fertigstellen** .

**Web-Portal, oder Berichts-Manager-URL für 2012 und 2014:**

1. Klicken Sie auf **Web Portal-URL**oder **URL des Berichts-Managers** 2014 bis 2012 im linken Bereich.

1. Klicken Sie auf **Übernehmen**.

1. Überprüfen Sie im **Ergebnisbereich** die Aktionen erfolgreich abgeschlossen.

1. Klicken Sie auf **Beenden**.

Informationen zum Berichtsberechtigungen finden Sie unter [Erteilen von Berechtigungen für einen Berichtsserver im systemeigenen Modus](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Wechseln Sie zum lokalen Bericht-Manager

Überprüfen Sie die Konfiguration wechseln Sie zum Berichts-Manager auf dem virtuellen Computer.

1. Starten Sie Internet Explorer auf dem virtuellen Computer mit Administratorrechten.

1. Wechseln Sie zu http://localhost/reports auf dem virtuellen Computer.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Verbindung zu Remote-Webportal Berichtmanager 2014 und 2012

Web-Portal oder Berichts-Manager 2014 bis 2012 auf dem virtuellen Computer von einem Remotecomputer herstellen möchten erstellen Sie einen neuen virtuellen Computer TCP-Endpunkt. Standardmäßig überwacht der Berichtsserver HTTP-Anfragen auf **Port 80**. Wenn Sie den Berichtsserver-URLs zur Verwendung eines anderen Ports konfigurieren, müssen Sie im folgenden Portnummer angeben.

1. Erstellen Sie einen Endpunkt für den virtuellen Computer der TCP-Port 80. Weitere Informationen finden Sie unter in diesem Dokument unter [Virtual Machine Endpunkte und Firewallports](#virtual-machine-endpoints-and-firewall-ports) .

1. Öffnen Sie Port 80 des virtuellen Computers Firewall.

1. Wechseln Sie zur Web-Portal oder Berichts-Manager mithilfe von Azure Virtual Machine **DNS-Namen** als Servernamen in der URL. Zum Beispiel:

    **Bericht**:  **Webportal**http://uebi.cloudapp.net/reportserver: http://uebi.cloudapp.net/reports

    [Konfigurieren einer Firewalls für Bericht-Serverzugriff](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Erstellen und Veröffentlichen von Berichten auf Azure Virtual Machine

In der folgenden Tabelle sind einige vorhandene Berichte auf dem Berichtsserver gehostet auf Microsoft Azure Virtual Machine von einem lokalen Computer veröffentlichen verfügbaren Optionen zusammengefasst:

- **Berichts-Generator**: der virtuelle Computer enthält auf-einmal Version von Microsoft SQL Server-Berichts-Generator SQL 2014 und 2012. Berichts-Generator die erste auf dem virtuellen Computer SQL 2016 starten:

    1. Starten Sie Ihren Browser mit Administratorrechten.

    1. Suchen Sie Webportal auf dem virtuellen Computer, und wählen Sie das **Download** -Symbol in der oberen rechten Ecke.
    
    1. Wählen Sie **Berichts-Generator**.

    Weitere Informationen finden Sie unter [Berichts-Generator starten](https://msdn.microsoft.com/library/ms159221.aspx).

- **SQL Server Data Tools**: VM: SQL Server Data Tools auf dem virtuellen Computer installiert und kann zum **Bericht Serverprojekten** und Berichte auf dem virtuellen Computer zu erstellen. SQL Server Data Tools können Berichte auf dem Berichtsserver auf dem virtuellen Computer veröffentlichen.

- **SQL Server Data Tools: Remote**: auf dem lokalen Computer Erstellen eines Reporting Services in SQL Server Data Tools, die Reporting Services-Berichte enthält. Konfigurieren des Projekts für die Verbindung mit dem Webdienst-URL.

    ![Projekteigenschaften für SSRS-Projekt SSDT](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)

- Erstellen einer. VHD Festplatte, Berichte und uploaden und schließen Sie das Laufwerk.

    1. Erstellen einer. Festplatte VHD-Datei auf dem lokalen Computer, der die Berichte enthält.

    1. Erstellen und Installieren eines Zertifikats Management.

    1. Hochladen Sie die VHD-Datei mithilfe des Add-AzureVHD-Cmdlet [Erstellen und Hochladen einer virtuellen Windows Server in Azure](virtual-machines-windows-classic-createupload-vhd.md)Azure.

    1. Virtuellen Computer angeschlossen.

## <a name="install-other-sql-server-services-and-features"></a>Installieren Sie anderer SQL Server-Dienste und features

Um zusätzliche SQL Server-Dienste wie Analysis Services im tabellarischen Modus installieren führen Sie SQL Server Setup-Assistenten. Die Setup-Dateien werden auf die Festplatte des virtuellen Computers.

1. Klicken Sie auf **Start** und dann auf **Alle Programme**.

1. Klicken Sie auf **Microsoft SQL Server 2016** **2014 für Microsoft SQL Server** oder **Microsoft SQL Server 2012** und klicken Sie auf **Konfigurationstools**.

1. Klicken Sie auf **SQL Server-Installationscenter**.

Oder C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe oder C:\SQLServer_11.0_full\setup.exe

>[AZURE.NOTE] Beim ersten Ausführen von SQL Server Setup Weitere Setup-Dateien können heruntergeladen werden und erfordern einen Neustart des virtuellen Computers und einen Neustart des SQL Server-Setup.
>
>Möchten Sie das Bild aus Microsoft Azure Virtual Machine wiederholt anpassen, sollten Sie SQL Server ein eigenes Bild erstellen. Analysis wurde Services SysPrep mit SQL Server 2012 SP1 CU2 aktiviert. Weitere Informationen finden Sie unter [Hinweise für die Installation von SQL Server mithilfe von SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) und [Sysprep-Unterstützung für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="to-install-analysis-services-tabular-mode"></a>Analysis Services tabellarischen Modus installieren

Die Schritte in diesem Abschnitt der Installation von Analysis Services tabellarischen Modus **zusammenfassen** . Weitere Informationen finden Sie unter:

- [Installieren von Analysis Services im tabellarischen Modus](https://msdn.microsoft.com/library/hh231722.aspx)

- [Tabellarische Modellierung (Adventure Works-Lernprogramm)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**So installieren Sie Analysis Services tabellarischen Modus**

1. SQL Server-Installation Wizard **Installation** im linken Fensterbereich und klicken **eigenständige neue SQL Server-Installation oder Hinzufügen von Features zu einer vorhandenen Installation**.

    - Wenn der **Ordner suchen**, wechseln Sie zu c:\SQLServer_13.0_full, c:\SQLServer_12.0_full oder c:\SQLServer_11.0_full und dann auf **Ok**.

1. Klicken Sie auf der Seite Product Updates auf **Weiter** .

1. Wählen Sie auf der Seite **Installationstyp** **Ausführen einer Neuinstallation von SQL Server aus** und auf **Weiter**.

1. Klicken Sie auf der Seite **Setup-Rolle** auf **SQL Server Funktionen Installation**.

1. Klicken Sie auf der Seite **Featureauswahl** auf **Analysis Services**.

1. Geben Sie einen beschreibenden Namen, wie **Tabellarisch** in Textfelder **Benannte Instanz** und die **Instanz-Id** auf der Seite **Konfiguration** .

1. Wählen Sie auf der Konfigurationsseite von **Analysis Services** **Tabellarischen Modus**. Den aktuellen Benutzer Administratorrechte Liste hinzufügen

1. Beenden Sie und schließen Sie den SQL Server Installationsassistenten.

## <a name="analysis-services-configuration"></a>Analysis Services-Konfiguration

### <a name="remote-access-to-analysis-services-server"></a>Remote-Zugriff auf Analysis Services-Server

Analysis Services-Server unterstützt nur Windows-Authentifizierung. Der virtuelle Computer muss zum Analysis Services-Clientanwendungen wie SQL Server Management Studio oder SQL Server Daten remote zugreifen, der lokalen Domäne Azure virtuellen Netzwerk verbunden werden. Weitere Informationen finden Sie unter [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

TCP-Port **2383**überwacht eine **Standardinstanz** von Analysis Services. Virtuelle Computer-Firewall geöffnet. Eine benannte Clusterinstanz von Analysis Services überwacht außerdem den Anschluss **2383**.

Für eine **benannte Instanz** von Analysis Services muss der SQL Server-Browserdienst Port Zugriff verwalten. Die SQL Server-Browser-Standardkonfiguration ist Port **2382**.

Virtuelle Computer-Firewall öffnen Sie Port **2382** und erstellen Sie eine benannte Instanz Port static Analysis Services.

1. Führen Sie den folgenden Befehl mit Administratorrechten, um Anschlüsse zu überprüfen, die bereits auf die VM und was Ports verwendet:

        netstat /ao

1. Mit SQL Server Management Studio static Analysis Services erstellen benannte Instanz Port aktualisieren 'Port' Wert in Tabellenform als allgemeine Instanzeigenschaften. Weitere Informationen finden Sie unter "verwenden Sie einen festen Anschluss für Standard oder benannte Instanz" in [der Windows-Firewall für den Analysis Services-Zugriff konfigurieren](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).

1. Starten Sie tabellarische Instanz von Analysis Services-Dienst.

Weitere Informationen finden Sie unter in diesem Dokument unter **Virtual Machine Endpunkte und Firewallports** .

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Virtual Machine Endpunkte und Firewall-Ports

Dieser Abschnitt fasst Microsoft Azure Virtual Machine Endpunkte erstellen und Ports in virtuellen Firewalls öffnen. In der folgenden Tabelle zusammengefasst, die **TCP-** Ports für Endpunkte erstellen und die Ports in virtuellen Maschinen Firewall öffnen.

- Wenn Sie eine VM und die beiden folgenden Elemente gelten, nicht VM Endpunkte erstellen müssen und nicht die Ports in der Firewall auf dem virtuellen Computer öffnen müssen.

    - Sie schließen Remote auf die SQL Server-Funktionen auf dem virtuellen Computer. Über eine Remotedesktopverbindung für die VM und Zugreifen auf die SQL Server-Funktionen, lokal auf dem virtuellen Computer gilt eine Remoteverbindung zu SQL Server-Features nicht.

    - Sie schließen nicht die VM zu einer lokalen Domäne Azure Virtual Networking oder anderen VPN-Tunneling-Lösung.

- Wenn der virtuelle Computer ist nicht Mitglied einer Domäne soll Remote verbinden Sie mit der SQL Server-Features für VM:

    - Öffnen Sie die Ports in der Firewall auf dem virtuellen Computer.

    - Erstellen Sie virtuelle Computer Endpunkte für die genannten Ports (*).

- Wenn der virtuelle Computer Mitglied einer Domäne über einen VPN-Tunnel wie Azure Virtual Networking ist, sind die Endpunkte nicht erforderlich. Öffnen Sie jedoch die Ports in der Firewall auf dem virtuellen Computer.

  	|Anschluss|Typ|Beschreibung|
|---|---|---|
|**80**|TCP|Berichtsserver RAS (*).|
|**1433**|TCP|SQL Server Management Studio (*).|
|**1434**|UDP|SQL Server-Browser. Dies ist erforderlich, wenn der virtuellen Computer in einer Domäne hinzugefügt.|
|**2382**|TCP|SQL Server-Browser.|
|**2383**|TCP|SQL Server Analysis Services-Standardinstanz und gruppierten benannten Instanzen.|
|**Benutzerdefinierte**|TCP|Erstellen Sie eine benannte Instanz Port eine Portnummer wählen Sie static Analysis Services und entsperren Sie die Portnummer in der Firewall.|

Weitere Informationen zum Erstellen von Endpunkten finden Sie unter:

- Erstellen Sie Endpunkte[Endpunkte auf einem virtuellen Computer einrichten](virtual-machines-windows-classic-setup-endpoints.md).

- SQL Server: Finden Sie im Abschnitt "Konfiguration Schritte für die Verbindung mit dem virtuellen Computer mit SQL Server Management Studio" [eine SQL Server-VM auf Azure bereitgestellt](virtual-machines-windows-portal-sql-server-provision.md).

Das folgende Diagramm veranschaulicht die Ports in der VM Firewall ermöglicht remote-Zugriff auf Features und Komponenten auf dem virtuellen Computer öffnen.

![Anschlüsse für Bi-Applikationen in Azure VMs öffnen](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Ressourcen

- Überprüfen Sie die Unterstützungsrichtlinie für Microsoft-Serversoftware in Azure Virtual Machine-Umgebung verwendet. Im folgende Thema sind die Unterstützung für Features wie BitLocker Failover Clustering und Netzwerklastenausgleich zusammengefasst. [Microsoft-Software unterstützen Azure Virtual Machines](http://support.microsoft.com/kb/2721672).

- [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md)

- [Virtuelle Computer](https://azure.microsoft.com/documentation/services/virtual-machines/)

- [Bereitstellen einer SQL Server-VM auf Azure](virtual-machines-windows-portal-sql-server-provision.md)

- [Wie einen Datenträger mit einem virtuellen Computer](virtual-machines-windows-classic-attach-disk.md)

- [Migration einer Datenbank auf SQL Server in Azure VM](virtual-machines-windows-migrate-sql.md)

- [Bestimmen Sie den Servermodus einer Analysis Services-Instanz](https://msdn.microsoft.com/library/gg471594.aspx)

- [Mehrdimensionale Modellierung (Adventure Works-Lernprogramm)](https://technet.microsoft.com/library/ms170208.aspx)

- [Azure Dokumentation Center](https://azure.microsoft.com/documentation/)

- [Verwenden von Power BI in einer Hybridumgebung](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [Senden von Feedback und Kontaktinformationen durch Microsoft SQL Server verbinden](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Communityinhalt

- [Azure SQL-Datenbank-Management mit PowerShell](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)
