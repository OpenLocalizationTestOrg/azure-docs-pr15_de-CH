<properties 
    pageTitle="Erstellen eines virtuellen Computers mit einem einheitlichen Berichtsserver mithilfe von PowerShell | Microsoft Azure"
    description="Dieses Thema beschreibt und leitet Sie durch die Bereitstellung und Konfiguration von SQL Server Reporting Services-pur-Berichtsserver ein Azure Virtual Machine. "
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

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Mithilfe von PowerShell Azure VM mit einem einheitlichen Berichtsserver erstellen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Dieses Thema beschreibt und leitet Sie durch die Bereitstellung und Konfiguration von SQL Server Reporting Services-pur-Berichtsserver ein Azure Virtual Machine. Die Schritte in diesem Dokument kombinieren manuelle Schritte zum Erstellen der virtuellen Computer und ein Windows PowerShell-Skript zu Reporting Services auf dem virtuellen Computer konfigurieren. Das Skript enthält einen Firewallport für HTTP oder HTTPs öffnen.

>[AZURE.NOTE] Wenn Sie **HTTPS** auf dem Berichtsserver, **Überspringen Sie Schritt 2**nicht benötigen.
>
>Lesen Sie nachdem die VM in Schritt 1 erstellt haben den Abschnitt Skript verwenden, konfigurieren Sie den Berichtsserver und HTTP. Der Berichtsserver kann nach dem Ausführen des Skripts verwendet.

## <a name="prerequisites-and-assumptions"></a>Komponenten und Annahmen

- **Azure-Abonnement**: Überprüfen Sie die Anzahl der Kerne in Azure-Abonnement verfügbar. Die empfohlene Größe VM **a3**erstellen, sollten Sie **4** verfügbaren Kerne. Verwenden Sie eine VM-Größe von **A2**benötigen **2** verfügbaren Kerne.
    
    - Um den Kern Ihres Abonnements in der Azure-Verwaltungsportal überprüfen klicken Sie im linken Fensterbereich und dann Verwendung klicken Sie im oberen Menü.
    
    - Erhöhen Sie das Kontingent Core Support von [Azure](https://azure.microsoft.com/support/options/). VM Informationen finden Sie unter [Azure Virtual Machine Größe](virtual-machines-linux-sizes.md).

- **Windows PowerShell Skripts**: Thema wird vorausgesetzt, dass Sie grundlegende Kenntnisse von Windows PowerShell. Weitere Informationen zur Verwendung von Windows PowerShell finden Sie unter:

    - [Starten Sie Windows PowerShell auf WindowsServer](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Erste Schritte mit Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Schritt 1: Bereitstellung von Azure Virtual Machine

1. Wechseln Sie zum klassischen Azure-Portal.

1. Klicken Sie im linken Bereich auf **virtuellen Maschinen** .

    ![virtuelle Maschinen von Microsoft azure](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. **Klicken Sie auf.**

    ![neue Schaltfläche](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Klicken Sie auf **Katalog**.

    ![neue Vm aus Galerie](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Klicken Sie auf **SQL Server 2014 RTM Standard – Windows Server 2012 R2** , und klicken Sie dann auf den Pfeil, um fortzufahren.

    ![Weiter](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Wenn Sie Reporting Services datengesteuerten Abonnementsfeature, wählen Sie **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. Weitere Informationen zu SQL Server-Editionen und Unterstützung finden Sie unter [Funktionen durch die Editionen von SQL Server 2012 unterstützt](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).

1. Bearbeiten Sie auf der Seite **Konfiguration des virtuellen Computers** die folgenden Felder ein:
                                    
    - Wenn mehr als ein **VERÖFFENTLICHUNGSDATUM der VERSION**ist wählen Sie die aktuellste Version aus
    
    - **Name der virtuellen Maschine**: der Computername wird auch als Cloud-Dienst DNS-Standardnamen auf der nächsten Konfigurationsseite verwendet. Der DNS-Name muss in Azure Service eindeutig sein. Sollten Sie die Konfiguration der VM mit einem Computernamen, der beschreibt, wofür die VM verwendet wird. Beispielsweise Ssrsnativecloud.
    
    - **Tier**: Standard
    
    - **Größe: A3** ist die empfohlene Größe VM für SQL Server-Arbeitslasten. Wenn ein virtueller Computer nur als einen Berichtsserver eine VM A2 ist ausreichend verwendet, wenn der Berichtsserver eine große Arbeitslast auftritt. VM Preisinformationen finden Sie unter [Virtuelle Computer Preise](https://azure.microsoft.com/pricing/details/virtual-machines/).
    
    - **Neuer Benutzername**: der Namen, den Sie als Administrator auf dem virtuellen Computer erstellt.
    
    - **Neues Kennwort** und **bestätigen**. Dieses Kennwort wird für das neue Administratorkonto verwendet und es wird empfohlen, ein sicheres Kennwort verwenden.
    
    - Klicken Sie auf **Weiter**. ![Weiter](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Auf der nächsten Seite können bearbeiten Sie die folgenden Felder:

    - **Cloud-Dienst**: Erstellen **Einer neuen Cloud-Dienst**.
    
    - **Cloud Service DNS-Name**: Dies ist der öffentliche DNS-Namen des Cloud-Dienst, der die VM zugeordnet ist. Der Standardname ist der Name im eingegebenen Namen VM. In späteren Schritten des Themas vertrauenswürdiges SSL-Zertifikat erstellen und dann der DNS-Namen für den Wert "**ausgestellt für**" des Zertifikats verwendet.
    
    - **Region-Affinität Gruppe/Virtual Network**: Wählen Sie Ihre Region für Ihre Endbenutzer.
    
    - **Konto**: eine automatisch generierte Speicherkonto verwenden.
    
    - **Verfügbarkeit**: keine.
    
    - **ENDPUNKTE** Bleiben Sie **Remotedesktop** und **PowerShell** Endpunkte, und fügen Sie eine HTTP- oder HTTPS-Endpunkt je nach Umgebung.

        - **HTTP**: öffentliche und private Standardports sind **80**. Hinweis Wenn Sie einen privaten Port als 80 verwenden **$HTTPport = 80** im HTTP-Skript.

        - **HTTPS**: sind die Standardports für öffentliche und private **443**. Aus Sicherheitsgründen ist privaten Port ändern und konfigurieren Sie die Firewall und den Berichtsserver privaten Port. Weitere Informationen über Endpunkte finden Sie unter [Festlegen von Kommunikation mit einem virtuellen Computer](virtual-machines-windows-classic-setup-endpoints.md). Beachten Sie, dass wenn Sie einen anderen Port als 443 verwenden den Parameter **$HTTPsport = 443** HTTPS-Skript.
    
    - Klicken Sie auf Weiter. ![Weiter](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Übernehmen Sie den Standardnamen **den VM-Agent installieren** ausgewählt, auf der letzten Seite des Assistenten. Die Schritte in diesem Thema verwenden nicht den VM-Agent aber möchten Sie diesem virtuellen Computer beibehalten, die VM-Agent und Extensions können Sie zu er CM.  Weitere Informationen zu den VM-Agent finden Sie unter [VM und Extensions – Teil 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Eine standardmäßige Extensions installiert Active Directory ausgeführt wird "BGINFO"-Erweiterung, die VM-Desktop, Systeminformationen wie interne IP-Adresse und freien Speicherplatz anzeigt.

1. Klicken Sie auf abgeschlossen. ![Okay](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. Der **Status** der VM zeigt als **ab (Bereitstellung)** bei der Bereitstellung und anschließend **als** Wenn die VM bereitgestellt und betriebsbereit ist.

## <a name="step-2-create-a-server-certificate"></a>Schritt 2: Erstellen eines Zertifikats

>[AZURE.NOTE] Wenn Sie HTTPS auf dem Berichtsserver nicht benötigen, können Sie **Schritt 2** und Abschnitt **Skript verwenden, konfigurieren Sie den Berichtsserver und HTTP**. Das HTTP-Skript schnell den Berichtsserver konfigurieren und der Berichtsserver verwendet werden.

HTTPS auf dem virtuellen Computer verwenden, benötigen Sie ein vertrauenswürdiges SSL-Zertifikat. Je nach Szenario können Sie eine der beiden folgenden Methoden verwenden:

- Ein gültiges SSL-Zertifikat von einer Zertifizierungsstelle (CA) ausgestellt und von Microsoft. CA-Stammzertifikate müssen über Microsoft Root Certificate Program verteilt werden. Weitere Informationen zu diesem Programm finden Sie unter [Windows und Windows Phone 8 SSL Root Certificate Program Member CAs ()](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) und [Einführung in das Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).

- Ein selbstsigniertes Zertifikat. Selbstsignierte Zertifikate werden für die Produktion nicht empfohlen.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Ein Zertifikat von einer vertrauenswürdigen Zertifizierungsstelle (CA) erstellt

1. **Ein Serverzertifikat für die Website von einer Zertifizierungsstelle anfordern**. 

    Können Sie den Assistenten für Webserverzertifikate eine Zertifikatanforderungsdatei (Certreq.txt) generieren, die an eine Zertifizierungsstelle senden, oder um eine Anforderung für eine Onlinezertifizierungsstelle. Beispielsweise Microsoft Zertifikatsdienste in Windows Server 2012. Je nach der zusendet Ihr Serverzertifikat ist es mehrere Tage bis zu mehreren Monaten für die Zertifizierungsstelle Ihren Antrag genehmigt und Ihnen eine Zertifikatsdatei. 

    Weitere Informationen zum Anfordern von Serverzertifikaten finden Sie unter: 
    
    - [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx)verwenden.
    
    - Sicherheits-Tools zum Verwalten von Windows Server 2012.

    [Sicherheits-Tools zum Verwalten von Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] Das Feld **ausgestellt für** vertrauenswürdiges SSL-Zertifikat sollte **Cloud Service DNS-Namen** für den neuen virtuellen Computer verwendet.

1. **Das Serverzertifikat auf dem Webserver installieren**. In diesem Fall ist die VM, der Berichtsserver hostet, und die Website später erstellt, wenn Sie Reporting Services konfigurieren. Weitere Informationen zum Installieren des Serverzertifikats auf dem Webserver mithilfe der Zertifikats-MMC-Snap-in finden Sie unter [Installieren eines Serverzertifikats](https://technet.microsoft.com/library/cc740068).
    
    Wenn Sie in diesem Thema enthaltene Skript verwenden möchten, muss den Berichtsserver konfigurieren Wert der Zertifikate **Fingerabdruck** als Parameter des Skripts. Finden Sie im nächsten Abschnitt zum Fingerabdruck des Zertifikats zu erhalten.

1. Weisen Sie das Serverzertifikat auf dem Berichtsserver. Die Zuweisung erfolgt im nächsten Abschnitt beim Berichtsserver konfigurieren.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Virtuelle Computer selbstsigniertes Zertifikat verwenden

Ein selbstsigniertes Zertifikat wurde auf dem virtuellen Computer erstellt, wenn die VM bereitgestellt wurde. Das Zertifikat hat denselben Namen wie die VM DNS-Namen. Um Zertifikatfehler zu vermeiden, ist es erforderlich, dass das Zertifikat auf dem virtuellen Computer selbst und alle Benutzer der Website vertrauenswürdig ist.

1. **Vertrauenswürdige Stammzertifizierungsstellen**die Stamm-CA des Zertifikats auf lokale VM Vertrauen Zertifikat hinzufügen. Folgendes ist eine Zusammenfassung der erforderlichen Schritte. Wie der Zertifizierungsstelle vertrauen finden Sie [ein Serverzertifikat installieren](https://technet.microsoft.com/library/cc740068).

    1. Der Azure-Verwaltungsportal die VMs auswählen und klicken Sie auf Verbinden. Je nach Browserkonfiguration Ihres können Sie aufgefordert, eine RDP-Datei für die Verbindung mit dem virtuellen Computer speichern.
    
        ![Verbinden Sie mit virtuellen Azure-Computer](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Verwenden Sie VM Benutzernamen, Benutzernamen und Kennwort konfiguriert, wenn die VM erstellt. 
    
        In der folgenden Abbildung z. B. der Name ist **Ssrsnativecloud** und **Testuser**ist.
        
        ![Benutzername enthält vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Führen Sie mmc.exe. Weitere Informationen finden Sie unter [wie: Anzeigen von Zertifikaten mit dem MMC-Snap-in](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. Im Anwendungsmenü **Datei** Konsole das **Zertifikate** -Snap-in fügen Sie hinzu, wählen Sie **Computerkonto** Aufforderung und klicken Sie auf **Weiter**.
    
    1. Wählen Sie **Lokalen Computer** verwalten, und klicken Sie dann auf **Fertig stellen**.
    
    1. Klicken Sie auf **Ok** und dann die **Zertifikate - persönliche** Knoten, und klicken Sie dann auf **Zertifikate**. Das Zertifikat nach DNS-Namen der VM benannt und mit **cloudapp.net**endet. Maustaste auf Zertifikat, und klicken Sie auf **Kopieren**.
    
    1. Erweitern Sie den Knoten **Vertrauenswürdige Stammzertifizierungsstellen** **Zertifikate** Maustaste und klicken Sie auf **Einfügen**.
    
    1. Doppelklick auf den Zertifikatnamen unter **Vertrauenswürdige Stammzertifizierungsstellen** validieren, und stellen Sie sicher, dass keine Fehler vorliegen und Sie Ihr Zertifikat sehen. Möchten Sie in diesem Thema enthaltenen HTTPS-Skript verwenden, muss den Berichtsserver konfigurieren Wert der Zertifikate **Fingerabdruck** als Parameter des Skripts. **Zu den Fingerabdruckwert**folgendermaßen. Es ist auch ein Beispiel PowerShell Abrufen des Fingerabdrucks im Abschnitt [Skript so konfigurieren Sie den Berichtsserver und HTTPS verwenden](#use-script-to-configure-the-report-server-and-HTTPS).
        
        1. Doppelklicken Sie auf den Namen des Zertifikats an, z. B. ssrsnativecloud.cloudapp.net.
        
        1. Klicken Sie auf die Registerkarte **Details** .
        
        1. Klicken Sie auf **Fingerabdruck**. Der Wert des Fingerabdrucks im Feld Details, z. B. a6 angezeigt 08 3c df f9 0 b f7 e3 7c 25 Ed a4 Ed 7e Ac 91 9c 2c fb 2f.
        
        1. Kopieren Sie den Fingerabdruck speichern Sie Wert für später und bearbeiten Sie das Skript nun.
        
        1. (*) Entfernen Sie vor dem Ausführen des Skripts die Leerzeichen zwischen den Wertepaaren. Zum Beispiel wäre vor erwähnt Fingerabdruck jetzt a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
        
        1. Weisen Sie das Serverzertifikat auf dem Berichtsserver. Die Zuweisung erfolgt im nächsten Abschnitt beim Berichtsserver konfigurieren.

Bei Verwendung ein selbstsigniertes SSL-Zertifikats entspricht dem Namen des Zertifikats bereits der Hostname des virtuellen Computers. Daher das DNS des Computers bereits global registriert und kann von jedem Client zugegriffen werden.

## <a name="step-3-configure-the-report-server"></a>Schritt 3: Konfigurieren des Berichtsservers

Dieser Abschnitt führt Sie durch die Konfiguration der VM als einen Berichtsserver für Reporting Services im einheitlichen Modus. Eine der folgenden Methoden können Sie den Berichtsserver konfigurieren:

- Verwenden Sie das Skript auf den Berichtsserver konfigurieren

- Mit Configuration Manager den Berichtsserver konfigurieren.

Weitere Informationen finden Sie im Abschnitt [virtuellen Computer verbinden und starten Sie den Konfigurations-Manager für Reporting Services](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Authentifizierung Hinweis:** Windows-Authentifizierung ist die empfohlene Authentifizierungsmethode und die Reporting Services-Standardauthentifizierung kann. Nur Benutzer, die auf dem virtuellen Computer konfiguriert sind können Reporting Services und Reporting Services-Rollen zugewiesen.

### <a name="use-script-to-configure-the-report-server-and-http"></a>So konfigurieren Sie den Berichtsserver und HTTP-verwenden Sie Skript

Gehen Sie folgendermaßen vor, um das Windows PowerShell-Skript konfiguriert den Berichtsserver. Die Konfiguration umfasst HTTP, HTTPS nicht:

1. Der Azure-Verwaltungsportal die VMs auswählen und klicken Sie auf Verbinden. Je nach Browserkonfiguration Ihres können Sie aufgefordert, eine RDP-Datei für die Verbindung mit dem virtuellen Computer speichern.

    ![Verbinden Sie mit virtuellen Azure-Computer](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Verwenden Sie VM Benutzernamen, Benutzernamen und Kennwort konfiguriert, wenn die VM erstellt. 

    In der folgenden Abbildung z. B. der Name ist **Ssrsnativecloud** und **Testuser**ist.
    
    ![Benutzername enthält vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Öffnen Sie auf dem virtuellen Computer **Windows PowerShell ISE** mit Administratorrechten. PowerShell ISE ist standardmäßig in WindowsServer 2012 installiert. Es wird empfohlen, die ISE statt standard Windows PowerShell-Fenster verwenden, sodass die ISE Skript einfügen, Skript ändern und dann das Skript ausführen.

1. In Windows PowerShell ISE im Menü **Ansicht** und dann auf **Skriptfenster anzeigen**.

1. Kopieren Sie das folgende Skript, und fügen Sie das Skript in Windows PowerShell ISE Skriptfenster.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Wenn Sie einen HTTP-Port als 80 VM erstellt, ändern Sie den Parameter $HTTPport = 80.

1. Das Skript ist derzeit für Reporting Services konfiguriert. Ändern Sie ggf. das Skript für Reporting Services Version Teil des Pfades zu den Namespace "v11" in der Anweisung Get-WmiObject.

1. Führen Sie das Skript.

**Überprüfung**: Überprüfen Funktionalität des grundlegenden Berichts arbeiten, finden Sie im Abschnitt [Überprüfen Sie die Konfiguration](#verify-the-configuration) weiter unten in diesem Thema.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Skript zum Konfigurieren der Berichtsserver und HTTPS verwenden

Gehen Sie folgendermaßen vor, um den Berichtsserver konfigurieren mithilfe von Windows PowerShell. Die Konfiguration umfasst HTTPS und nicht HTTP.

1. Der Azure-Verwaltungsportal die VMs auswählen und klicken Sie auf Verbinden. Je nach Browserkonfiguration Ihres können Sie aufgefordert, eine RDP-Datei für die Verbindung mit dem virtuellen Computer speichern.

    ![Verbinden Sie mit virtuellen Azure-Computer](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Verwenden Sie VM Benutzernamen, Benutzernamen und Kennwort konfiguriert, wenn die VM erstellt. 

    In der folgenden Abbildung z. B. der Name ist **Ssrsnativecloud** und **Testuser**ist.

    ![Benutzername enthält vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Öffnen Sie auf dem virtuellen Computer **Windows PowerShell ISE** mit Administratorrechten. PowerShell ISE ist standardmäßig in WindowsServer 2012 installiert. Es wird empfohlen, die ISE statt standard Windows PowerShell-Fenster verwenden, sodass die ISE Skript einfügen, Skript ändern und dann das Skript ausführen.

1. Um ausgeführten Skripts zu aktivieren, führen Sie den folgenden Windows PowerShell-Befehl:

        Set-ExecutionPolicy RemoteSigned

    Anschließend können Sie Folgendes überprüfen die Richtlinie ausführen:

        Get-ExecutionPolicy

1. Klicken Sie in **Windows PowerShell ISE**auf das Menü **Ansicht** und dann auf **Skriptfenster anzeigen**.

1. Das folgende Skript kopieren und Einfügen im Skriptfenster Windows PowerShell ISE.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Ändern Sie den **$certificatehash** -Parameter im Skript:

    - Dies ist ein **Erforderlicher** Parameter. Zertifikat-Wert aus den vorherigen Schritten nicht gespeichert haben, verwenden eine der folgenden Methoden den Zertifikathashwert von Zertifikaten Fingerabdruck kopieren.:

        Öffnen Sie auf dem virtuellen Computer Windows PowerShell ISE, und führen Sie den folgenden Befehl:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        Die Ausgabe sieht etwa wie folgt. Wenn das Skript eine leere Zeile zurückgibt, die VM nicht Zertifikat konfiguriert beispielsweise, finden Sie im Abschnitt [virtueller Maschinen selbstsigniertes Zertifikat verwenden](#to-use-the-virtual-machines-self-signed-certificate).
    
    ODER
    
    - Führen Sie auf dem virtuellen Computer mmc.exe, und fügen Sie das **Zertifikate** -Snap-in.
    
    - Doppelklicken Sie unter dem Knoten **Vertrauenswürdige Stammzertifizierungsstellen** auf Ihrem Zertifikat. Bei Verwendung der VM Zertifikat das Zertifikat nach den DNS-Namen der VM benannt und endet mit **cloudapp.net**.
    
    - Klicken Sie auf die Registerkarte **Details** .
    
    - Klicken Sie auf **Fingerabdruck**. Der Wert des Fingerabdrucks wird im Feld Details zum Beispiel af 11 60 b6 4 b 28 8 d 89 0a 82 12 ff 6 a9 c3 66 4f 31 90 48
    
    - **Bevor Sie das Skript ausführen**, entfernen Sie die Leerzeichen zwischen den Wertepaaren. Beispielsweise af1160b64b288d890a8212ff6ba9c3664f319048

1. Ändern Sie den **$httpsport** -Parameter: 

    - Wenn Sie Port 443 für HTTPS-Endpunkt verwendet, müssen Sie nicht diesen Parameter im Skript aktualisiert. Verwenden Sie andernfalls Anschlusswert gewählte privaten HTTPS-Endpunkt auf dem virtuellen Computer konfiguriert.

1. Ändern Sie den **$DNSName** -Parameter: 

    - Das Skript wird ein Platzhalter-Zertifikat $DNSName konfiguriert = "+". Wenn Sie keine für eine Platzhalter-Zertifikat-Bindung konfigurieren wollen, kommentieren Sie $DNSName ="+"und aktivieren Sie die folgende Zeile, voll $DNSNAme Verweis, ## $DNSName="$server.cloudapp.net".

        Ändern Sie den Wert $DNSName, wenn nicht die virtuellen Computer DNS-Namen für Reporting Services verwenden möchten. Verwenden Sie den Parameter des Zertifikats muss auch dieser Name und Namen global auf einem DNS-Server registrieren.

1. Das Skript ist derzeit für Reporting Services konfiguriert. Ändern Sie ggf. das Skript für Reporting Services Version Teil des Pfades zu den Namespace "v11" in der Anweisung Get-WmiObject.

1. Führen Sie das Skript.

**Überprüfung**: Überprüfen Funktionalität des grundlegenden Berichts arbeiten, finden Sie im Abschnitt [Überprüfen Sie die Konfiguration](#verify-the-connection) weiter unten in diesem Thema. Zum Überprüfen des Zertifikats Bindung öffnen Sie ein Eingabeaufforderungsfenster mit Administratorrechten, und führen Sie den folgenden Befehl:

    netsh http show sslcert

Das Ergebnis umfasst Folgendes:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Verwenden Sie Konfigurations-Manager zum Konfigurieren des Berichtsservers

Wenn keine PowerShell Skript Berichtsserver konfigurieren möchten, führen Sie die Schritte in diesem Abschnitt mithilfe des Konfigurations-Managers von Reporting Services im einheitlichen Modus konfiguriert den Berichtsserver.

1. Der Azure-Verwaltungsportal die VMs auswählen und klicken Sie auf Verbinden. Verwenden Sie den Benutzernamen und das Kennwort konfiguriert, wenn die VM erstellt.

    ![Verbinden Sie mit virtuellen Azure-Computer](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. Führen Sie Windows Update und Installieren von Updates für die VM. Ist ein Neustart des virtuellen Computers erforderlich, starten Sie die VM und Wiederherstellen Sie der VM von klassischen Azure-Portal.

1. Menü Start des virtuellen Computers geben Sie **Reporting Services** und öffnen Sie **Konfigurations-Manager für Reporting Services**zu.

1. Lassen Sie die Standardwerte für **Servername** und **Berichtsserverinstanz**. Klicken Sie auf **Verbinden**.

1. Klicken Sie im linken Bereich auf **Webdienst-URL**.

1. Standardmäßig ist für HTTP-Port 80 mit "Alle zugewiesen" RS konfiguriert. Hinzufügen von HTTPS

    1. Im **SSL-Zertifikat**: Wählen Sie das Zertifikat zu verwenden, z. B. [VM-Name]. cloudapp.net. Wenn keine Zertifikate aufgelistet sind, finden Sie im Abschnitt **Schritt2: Erstellen eines Serverzertifikats** Informationen zu vertrauenswürdigen Zertifikat auf dem virtuellen Computer.
    
    1. Unter **SSL-Port**: 443 auswählen. Konfiguriert den privaten HTTPS-Endpunkt auf dem virtuellen Computer mit einem anderen privaten Anschluss verwenden Sie diesen Wert hier.
    
    1. Klicken Sie auf **Übernehmen** , und warten Sie, bis des Vorgangs abgeschlossen.

1. Klicken Sie im linken Bereich auf **Datenbank**.

    1. Klicken Sie auf **Preisgestaltungs ändern**.
    
    1. Klicken Sie auf **eine neue Berichtsserver-Datenbank erstellen** und dann auf **Weiter**.
    
    1. Übernehmen Sie die Standardeinstellung **Servername**: VM Namen und **Authentifizierungstyp** standardmäßig als **Aktueller Benutzer** - **Integrierte Sicherheit**. Klicken Sie auf **Weiter**.
    
    1. Übernehmen Sie die Standardeinstellung **Datenbankname** **ReportServer** und klicken Sie auf **Weiter**.
    
    1. Standardwert **Authentifizierungstyp** **Die Anmeldeinformationen** , und klicken Sie auf **Weiter**.
    
    1. Klicken Sie auf der Seite **Zusammenfassung** auf **Weiter** .
    
    1. Wenn die Konfiguration abgeschlossen ist, klicken Sie auf **Fertig stellen**.

1. Klicken Sie im linken Bereich auf **Bericht-Manager-URL**. Standardwert **Virtuelles Verzeichnis** wie **Berichte** , und klicken Sie auf **Übernehmen**.

1. Klicken Sie auf **Beenden** , um den Konfigurations-Manager für Reporting Services zu schließen.

## <a name="step-4-open-windows-firewall-port"></a>Schritt 4: Öffnen Windows Firewall-Ports

>[AZURE.NOTE] Wenn Sie eines dieser Skripts konfiguriert den Berichtsserver verwendet, können Sie diesen Abschnitt überspringen. Das Skript enthalten einen Schritt um den Firewall-Port öffnen. Die Standardeinstellung ist Port 80 für HTTP und 443 für HTTPS.

Um die Remoteverbindung zum Berichts-Manager oder den Berichtsserver auf dem virtuellen Computer muss einen TCP-Endpunkt auf dem virtuellen Computer. Es muss denselben Port in der VM-Firewall öffnen. Der Endpunkt wurde erstellt, wenn die VM bereitgestellt wurde.

Dieser Abschnitt enthält grundlegende Informationen zum Firewall-Ports zu öffnen. Weitere Informationen finden Sie unter [Konfigurieren einer Firewall für Serverzugriff Bericht](https://technet.microsoft.com/library/bb934283.aspx)

>[AZURE.NOTE] Wenn Sie das Skript zum Konfigurieren des Berichtsservers verwendet, können Sie diesen Abschnitt überspringen. Das Skript enthalten einen Schritt um den Firewall-Port öffnen.

Wenn Sie einen privaten Anschluss als 443 für HTTPS konfiguriert, ändern Sie das folgende Skript entsprechend. Öffnen Sie Port **443** in der Windows-Firewall führen Sie folgende Schritte aus:

1. Öffnen Sie ein Windows PowerShell mit Administratorrechten.

1. Wenn Sie Port 443 als HTTPS-Endpunkt auf dem virtuellen Computer konfiguriert verwendet, aktualisieren Sie den Port im folgenden Befehl, und führen Sie den Befehl:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. Wenn der Befehl abgeschlossen ist, **Ok** in der Befehlszeile angezeigt.

Um sicherzustellen, dass der Port geöffnet ist, öffnen Sie ein Windows PowerShell-Fenster und führen Sie folgenden Befehl:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Überprüfen Sie die Konfiguration

Überprüfen Funktionalität des grundlegenden Berichts funktioniert, öffnen Sie Ihren Browser mit Administratorrechten, und wählen Sie die folgenden Ad Bericht-Manager für Reporting URLS:

- Suchen Sie die VM auf die Berichtsserver-URL:

        http://localhost/reportserver

- Suchen Sie die VM auf den Berichts-Manager-URL:

        http://localhost/Reports

- Wechseln Sie vom lokalen Computer **remote** -Manager Bericht auf dem virtuellen Computer. Aktualisieren Sie DNS-Namen im folgenden Beispiel entsprechend. Aufforderung zur Eingabe eines Kennworts verwenden Sie Administratoranmeldeinformationen erstellten VM bereitgestellt wurde. Der Benutzername wird [Domäne]\[Benutzername] Format, wobei die Domäne der Computername, z. B. Ssrsnativecloud\testuser ist. Verwenden Sie keine HTTP-**S**, entfernen Sie **s** im URL. Finden Sie im nächsten Abschnitt Informationen zum Erstellen zusätzlicher Benutzer VM.

        https://ssrsnativecloud.cloudapp.net/Reports

- Wechseln Sie vom lokalen Computer remote Berichtsserver-URL. Aktualisieren Sie DNS-Namen im folgenden Beispiel entsprechend. Wenn Sie kein HTTPS verwenden, entfernen Sie die s im URL.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Erstellen von Benutzern und Rollen zuweisen

Konfigurieren und Überprüfen des Berichtsservers ist eine häufige Aufgabe erstellen einen oder mehrere Benutzer und Reporting Services-Rollen Benutzer zuweisen. Weitere Informationen finden Sie unter:

- [Erstellen eines lokalen Benutzerkontos](https://technet.microsoft.com/library/cc770642.aspx)

- [Gewähren Zugriff auf einem Berichtsserver (Berichts-Manager)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Erstellen und Verwalten von Rollen zugewiesen](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Erstellen und Veröffentlichen von Berichten auf Azure Virtual Machine

In der folgenden Tabelle sind einige vorhandene Berichte auf dem Berichtsserver gehostet auf Microsoft Azure Virtual Machine von einem lokalen Computer veröffentlichen verfügbaren Optionen zusammengefasst:

- **RS.exe Skript**: mit RS.exe Skript Berichtselemente aus und vorhandenen Berichtsserver an Microsoft Azure virtuellen Computers kopieren. Weitere Informationen finden Sie im Abschnitt "Nativen Modus in den einheitlichen Modus – Microsoft Azure Virtual Machine" im [Beispiel Reporting Services rs.exe Skript zum Migrieren von Content zwischen Berichtsserver](https://msdn.microsoft.com/library/dn531017.aspx).

- **Berichts-Generator**: der virtuelle Computer enthält auf-einmal Version von Microsoft SQL Server Berichts-Generator. Bericht-Generator das erste Mal auf dem virtuellen Computer zu starten:

    1. Starten Sie Ihren Browser mit Administratorrechten.
    
    1. Wechseln Sie zum Berichts-Manager auf dem virtuellen Computer, und klicken Sie im Menüband auf **Berichts-Generator** .

    Weitere Informationen finden Sie unter [Installieren und deinstallieren, unterstützen Berichts-Generator](https://technet.microsoft.com/library/dd207038.aspx).

- **SQL Server Data Tools: VM**: Erstellung virtueller Computer mit SQL Server 2012, wenn SQL Server Data Tools auf dem virtuellen Computer installiert und kann zum **Bericht Serverprojekten** und Berichte auf dem virtuellen Computer zu erstellen. SQL Server Data Tools können Berichte auf dem Berichtsserver auf dem virtuellen Computer veröffentlichen.
    
    Wenn Sie den virtuellen Computer mit SQL Server 2014 erstellt, können Sie SQL Server Data Tools - BI für visual Studio installieren. Weitere Informationen finden Sie unter:

    - [Microsoft SQL Server Data Tools - Business Intelligence für Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools - Business Intelligence für Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [SQL Server Data Tools und SQL Server Business Intelligence (SSDT-BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: Remote**: auf dem lokalen Computer Erstellen eines Reporting Services in SQL Server Data Tools, die Reporting Services-Berichte enthält. Konfigurieren des Projekts für die Verbindung mit dem Webdienst-URL.

    ![Projekteigenschaften für SSRS-Projekt SSDT](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Skript verwenden**: Skript Berichtinhalt kopieren verwenden. Weitere Informationen finden Sie unter [Beispiel Reporting Services rs.exe Skript zum Migrieren von Content zwischen Berichtsserver](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Kosten zu minimieren, verwenden Sie nicht die VM

>[AZURE.NOTE] Fahren Sie Zuschläge für Ihre Azure Virtual Machines bei minimieren, VM vom klassischen Azure-Portal. Verwenden Sie die Energieoptionen von Windows in einer VM auf den virtuellen Computer herunterfahren, werden für die VM weiterhin denselben Betrag erhoben werden. Um Kosten zu reduzieren, müssen Sie im klassischen Azure-Portal den virtuellen Computer herunterfahren. Wenn Sie die VM nicht mehr benötigen, müssen Sie Speicher kommt die VM und die zugehörigen VHD-Dateien löschen. Weitere Informationen finden Sie in den FAQ auf [Virtuellen Maschinen Preisdetails](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Weitere Informationen

### <a name="resources"></a>Ressourcen

- Ähnliche Inhalte in eine Einzelserver-Bereitstellung von SQL Server Business Intelligence und SharePoint 2013 finden Sie [Mit Windows PowerShell ein Azure VM mit SQL Server BI und SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).

- Ähnliche Inhalte einer Bereitstellung mit mehreren Servern von SQL Server Business Intelligence und SharePoint 2013 finden Sie unter [Bereitstellen von SQL Server Business Intelligence in Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).

- Allgemeine Informationen zu Bereitstellung von SQL Server Business Intelligence in Azure-Computer finden Sie unter [SQL Server Business Intelligence in Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).

- Weitere Informationen über die Kosten von Azure berechnen Sie Gebühren, finden Sie unter Registerkarte virtuelle Maschinen von [Azure Preise Rechner](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Communityinhalt

- Schrittweise Anleitung zum Erstellen eines Reporting Services systemeigenen Modus Berichtsserver Skript finden [Hosten SQL Reporting Services auf Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Links zu anderen Ressourcen für SQL Server in Azure VMs

[SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md)
