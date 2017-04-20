<properties
    pageTitle="Konfigurieren Sie Gruppe immer auf Verfügbarkeit in Azure VM mit PowerShell"
    description="In diesem Lernprogramm verwendet Ressourcen mit dem klassischen Bereitstellungsmodell, und PowerShell immer auf Verfügbarkeit in Azure erstellen Sie eine Gruppe."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Konfigurieren Sie Gruppe immer auf Verfügbarkeit in Azure VM mit PowerShell

> [AZURE.SELECTOR]
- [Ressourcen-Manager: Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Ressourcen-Manager: manuell](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassisch: Benutzeroberfläche](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassisch: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Azure virtuelle Maschinen (VMs) können Datenbankadministratoren senken die Kosten für eine hohe Verfügbarkeit der SQL Server-System implementiert. In diesem Lernprogramm wird veranschaulicht, wie eine verfügbarkeitsgruppe mit SQL Server immer auf End-to-End in Azure-Umgebung implementieren. Am Ende des Lernprogramms besteht Lösung SQL Server immer auf Azure den folgenden Elementen:

- Ein virtuelles Netzwerk mit mehreren Subnetzen, einschließlich einer Front-End- und Back-End-Subnetz

- Ein Domänencontroller mit einer Domäne Active Directory (AD)

- Zwei SQL Server-VMs auf dem Back-End-Subnetz bereitgestellt und der AD-Domäne

- Ein WSFC-Cluster mit Quorummodell Knotenmehrheit 3 Knoten

- Eine verfügbarkeitsgruppe mit zwei synchrone Commit einer Datenbank Verfügbarkeit

Dieses Szenario wird nicht für die Wirtschaftlichkeit und andere Faktoren auf Azure seiner Einfachheit ausgewählt. Beispielsweise können Sie die Anzahl der VMs für eine zwei-Replikat Verfügbarkeit sparen Stunden in Azure Compute Verwendung des Domänencontrollers als der Dateifreigabenzeuge Quorum in einem Cluster mit 2 Knoten WSFC minimieren. Diese Methode verringert die Anzahl der VM aus der obigen Konfiguration.

Dieses Lernprogramm soll die Schritte oben beschriebenen Lösung einrichten, ohne die Details der einzelnen Schritte anzeigen. Daher verwendet er anstatt Sie Konfigurationsschritte GUI, PowerShell Skripts schnell durch die einzelnen Schritte ausführen. Es wird Folgendes vorausgesetzt:

- Sie haben bereits ein Azure-Konto mit dem virtuellen Computer Abonnement.

- [Azure PowerShell-Cmdlets](../powershell-install-configure.md)installiert.

- Sie haben bereits ein grundlegendes Verständnis der immer auf Verfügbarkeit für lokale Lösungen. Weitere Informationen finden Sie unter [Immer auf Verfügbarkeitsgruppen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Mit der Azure-Abonnement und Erstellen virtuelle Netzwerk

1. In einem PowerShell-Fenster auf Ihrem lokalen Computer Azure-Modul importieren, eine Veröffentlichung Datei auf den Computer herunterladen und Azure-Abonnement durch Importieren der heruntergeladenen publishing Einstellungen der PowerShell-Sitzung an.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Der Befehl **Get-AzurePublishgSettingsFile** generiert automatisch eine Azure Zertifikat auf den Computer gedownloadet. Browser automatisch geöffnet, und Sie werden aufgefordert, die Kontoanmeldeinformationen Microsoft Azure-Abonnement eingeben. Die heruntergeladenen **publishsettings** -Datei enthält alle Informationen benötigen Sie Ihre Azure-Abonnement verwalten. Nach dem Speichern dieser Datei in einem lokalen Verzeichnis importieren Sie **Importieren AzurePublishSettingsFile** Befehl.

    >[AZURE.NOTE] Publishsettings-Datei enthält die Anmeldeinformationen (unverschlüsselt) der Azure-Abonnements und Dienste verwalten. Die Erhöhung der Sicherheit für diese Datei ist vorübergehend außerhalb der Quellverzeichnisse (z. B. in den Ordner Libraries\Documents), und löschen sie nach Abschluss des Importvorgangs. Ein böswilliger Benutzerzugriff auf die Datei Publishsettings kann bearbeiten, erstellen und Löschen von Azure Services.

1. Definieren Sie eine Reihe von Variablen, mit denen Sie die Cloud IT-Infrastruktur zu erstellen.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Achten Sie auf Folgendes sicherstellen, dass die Befehle später erfolgreich ist:

    - Variablen **$storageAccountName** und **$dcServiceName** müssen eindeutig sein, da sie zu Ihrem Cloud-Konto und Cloud Speicherserver bzw. im Internet verwendet werden.

    - Namen für die Variablen **$affinityGroupName** und **$virtualNetworkName** sind im Konfigurationsdokument virtuelles Netzwerk konfiguriert, die Sie später verwenden.

    - **$sqlImageName** gibt den neuen Namen des VM-Image, das SQL Server 2012 Service Pack 1 Enterprise Edition enthält.

    - Der Einfachheit halber **Contoso 000** ein Kennwort während der gesamten praktischen Einführung verwendet.

1. Erstellen Sie eine Gruppe.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Erstellen eines virtuellen Netzwerks durch Importieren einer Konfigurationsdatei.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Die Konfigurationsdatei enthält das folgende XML-Dokument. Kurz gesagt, ein virtuelles Netzwerk namens **ContosoNET** in der Gruppe Affinität **ContosoAG**aufgerufen wird und diese Adresse Speicherplatz **10.10.0.0/16** und zwei Subnetzen, **10.10.1.0/24** und **10.10.2.0/24**im vorderen Subnetz und Subnetz bzw.. Vordere Subnetz wird Clientanwendungen wie Microsoft SharePoint platzieren und Back Subnetz ist, wo Sie die SQL Server-VMs platzieren. Wenn Sie zuvor die Variablen **$affinityGroupName** und **$virtualNetworkName** ändern, müssen Sie die entsprechenden Namen ändern.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Erstellen Sie ein Speicherkonto, die der Gruppe zugeordnet ist, erstellt und als aktuelle Speicherkonto in Ihrem Abonnement festgelegt.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Erstellen Sie den DC Server in neue Cloud Service und Verfügbarkeit.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Diese Reihe von weitergeleiteten Befehlen führen Sie Folgendes aus:

    - **New AzureVMConfig** erstellt eine VM-Konfiguration.

    - **Hinzufügen AzureProvisioningConfig** gibt die Konfigurationsparameter von einer eigenständigen WindowsServer.

    - **Hinzufügen AzureDataDisk** fügt den Datenträger, den Sie zum Speichern von Active Directory Daten verwenden Zwischenspeichern Option auf None festgelegt.

    - **New AzureVM** erstellt einen neuen Clouddienst sowie der Azure-VM im neuen Cloud-Dienst.

1. Warten Sie auf die neue VM vollständig bereitgestellt werden, und downloaden remote desktop-Datei in das Arbeitsverzeichnis. Da der Azure-VM bereitstellen sehr lange dauert, die While-Schleife weiterhin neue VM Umfrage bis zur Verfügung steht.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

DC-Server wird jetzt erfolgreich bereitgestellt. Anschließend konfigurieren Sie Active Directory-Domäne auf dem DC-Server. Lassen Sie PowerShell-Fenster auf Ihrem lokalen Computer geöffnet. Sie verwenden es später erneut die beiden virtuellen SQL Server-Computer erstellen.

## <a name="configure-the-domain-controller"></a>Konfigurieren des Domänencontrollers

1. Verbindung zum DC Server remote desktop-Datei starten. Des Administrators AzureAdmin Benutzername und Kennwort **Contoso 000**, die Sie beim Erstellen der neuen VM angegeben.

1. Öffnen Sie ein PowerShell im Administratormodus.

1. Die folgenden **DCPROMO. EXE** Befehl die Domäne " **corp.contoso.com** " Datenverzeichnisse auf Laufwerk M einrichten.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Nachdem der Befehl abgeschlossen ist, wird die VM automatisch neu gestartet.

1. Verbindung zum DC Server erneut mit remote desktop-Datei. Dieses Mal melden Sie sich als **CORP\Administrator**.

1. Öffnen Sie ein PowerShell im Administratormodus und importieren Sie Active Directory PowerShell Modul mit dem folgenden Befehl:

        Import-Module ActiveDirectory

1. Führen Sie die folgenden Befehle drei Benutzer der Domäne hinzufügen.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** wird verwendet, um die SQL Server-Instanzen, WSFC-Cluster und verfügbarkeitsgruppe konfigurieren. **CORP\SQLSvc1** und **CORP\SQLSvc2** werden für die beiden virtuellen SQL Server-Computer als SQL Server-Dienstkonten verwendet.

1. Anschließend führen Sie die folgenden Befehle zu **CORP\Install** die Berechtigungen zum Erstellen von Computerobjekten in der Domäne.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Angegebenen GUID ist der GUID für die Computer-Objekttyp. **CORP\Install** -Konto benötigt die Berechtigung **Alle Eigenschaften lesen** und **Erstellen** um direkte aktive Objekte für den WSFC-Cluster erstellen. **Alle Eigenschaften lesen** Berechtigungen ist bereits CORP\Install standardmäßig erteilt, müssen nicht explizit gewähren. Weitere Informationen zu Berechtigungen für das Erstellen des WSFC-Clusters finden Sie unter [Failover Cluster-Anleitung: Konfigurieren von Konten in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Damit Sie Active Directory und Benutzerobjekte konfiguriert haben, Sie zwei SQL Server virtuelle Computer erstellen und fügen sie diese Domäne.

## <a name="create-the-sql-server-vms"></a>SQL Server virtuelle Computer erstellen

1. Weiterhin PowerShell-Fenster, die auf dem lokalen Computer. Definieren Sie die folgenden zusätzlichen Variablen:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    IP-Adresse **10.10.0.4** ist in der Regel die erste VM zugewiesen im Subnetz **10.10.0.0/16** Azure virtuelles Netzwerk erstellen. Sie sollten überprüfen, ist die Adresse des Servers DC mit **IPCONFIG**.

1. Führen Sie die folgenden piped Befehle zum erste VM im WSFC-Cluster mit dem Namen **ContosoQuorum**erstellen:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Beachten Sie Folgendes zur obigen Befehl:

    - **New AzureVMConfig** erstellt eine VM-Konfiguration mit den Namen des gewünschten Verfügbarkeit. Nachfolgende VMs gleichnamige Verfügbarkeit Satz entstehen, damit sie auf dieselben Verfügbarkeit verbunden sind.

    - **Hinzufügen AzureProvisioningConfig** verknüpft VM mit Active Directory-Domäne erstellten.

    - **Set AzureSubnet** setzt die VM im Subnetz zurück.

    - **New AzureVM** erstellt einen neuen Clouddienst sowie der Azure-VM im neuen Cloud-Dienst. Der Parameter **DnsSettings** gibt an, dass der DNS-Server für die Server in der neuen Cloud-Dienst-IP-Adresse **10.10.0.4**hat die IP-des Servers DC Adresse. Dieser Parameter ist erforderlich, um neue VMs in der Cloud-Dienst erfolgreich auf Active Directory-Domäne beitreten können. Ohne diesen Parameter müssen Sie manuell die IPv4-Einstellungen in der VM DC-Server als primären DNS-Server verwenden, nachdem die VM bereitgestellt wird und dann die VM auf Active Directory-Domäne beitreten.

1. Führen Sie die folgenden piped Befehle der SQL Server-VMs namens **ContosoSQL1** und **ContosoSQL2**erstellen.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Beachten Sie Folgendes über die oben angeführten Befehle:

    - **Neu AzureVMConfig** verwendet die gleiche Verfügbarkeit festlegen als DC-Server und SQL Server 2012 Service Pack 1 Enterprise Edition Bild im virtuellen Katalog verwendet. Wird die Betriebssystem-CD lesen-Zwischenspeichern nur (keine Schreib-Cache). Es wird empfohlen, dass die Datenbankdateien auf einem separaten Datenträger migrieren, die VM zuordnen keine Lese-konfigurieren und Schreibcache. Allerdings ist das beste Schreibcache auf dem Datenträger des Betriebssystems zu entfernen, da entfernen kann Lese-Cache auf dem Betriebssystem-Datenträger.

    - **Hinzufügen AzureProvisioningConfig** verknüpft VM mit Active Directory-Domäne erstellten.

    - **Set AzureSubnet** setzt die VM im Subnetz zurück.

    - **Add AzureEndpoint** fügt Access Endpunkte Clientanwendungen diese Services-Instanzen von SQL Server im Internet zugreifen können. ContosoSQL1 und ContosoSQL2 sind verschiedene Ports zugewiesen.

    - **New AzureVM** erstellt den neuen virtuellen SQL Server-Computer im selben Cloud-Dienst als ContosoQuorum. Denselben Clouddienst müssen VMs versehen werden, wenn Sie dieselben Verfügbarkeit werden sollen.

1. Jede VM vollständig bereitgestellt werden und die remote desktop-Datei in das Arbeitsverzeichnis herunterladen warten. Die for-Schleife die drei neue VMs durchläuft und die Befehle in der obersten Ebene geschweiften Klammern für jeden führt.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    SQL Server VMs jetzt bereitgestellt und ausgeführt, aber sie mit SQL Server installiert, mit Standardoptionen.

## <a name="initialize-the-wsfc-cluster-vms"></a>Initialisieren Sie die WSFC-Cluster virtuellen Computer

In diesem Abschnitt müssen Sie drei Server ändern WSFC-Cluster und SQL Server-Installation verwenden. Insbesondere:

- (Alle Server) Sie müssen **das Failoverclusterfeature** installieren.

- (Alle Server) Sie müssen als **Administrator**des Computers **CORP\Install** hinzufügen.

- (ContosoSQL1 und ContosoSQL2 nur) Sie müssen **Sysadmin** -Rolle in der Standarddatenbank **CORP\Install** hinzufügen.

- (ContosoSQL1 und ContosoSQL2 nur) Sie müssen **Systemkonto** als Benutzernamen mit folgenden Berechtigungen hinzufügen:

    - Jede verfügbarkeitsgruppe ändern

    - SQL verbinden

    - Serverstatus anzeigen

- (ContosoSQL1 und ContosoSQL2 nur) Das **TCP-** Protokoll ist auf die SQL Server-VM bereits aktiviert. Allerdings müssen Sie noch die Firewall für remote-Zugriff auf SQL Server geöffnet.

Nun können Sie beginnen. Beginnend mit **ContosoQuorum**wie folgt:

1. Eine Verbindung zu **ContosoQuorum** remote desktop Dateien starten. Des Administrators **AzureAdmin** Benutzername und Kennwort **Contoso 000**, die Sie beim Erstellen der VMs angegeben.

1. Stellen Sie sicher, dass die Computer erfolgreich **corp.contoso.com**hinzugefügt haben.

1. Warten der SQL Server-Installations Ausführung automatisierter Initialisierungsaufgaben vor.

1. Öffnen Sie ein PowerShell im Administratormodus.

1. Installieren Sie das Feature Windows-Failovercluster.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Fügen Sie **CORP\Install** als lokaler Administrator an.

        net localgroup administrators "CORP\Install" /Add

1. ContosoQuorum melden. Sie sind jetzt mit diesem Server fertig.

        logoff.exe

Als Nächstes initialisieren Sie **ContosoSQL1** und **ContosoSQL2**. Führen Sie die folgenden Schritte aus, die für beide SQL Server VMs.

1. Eine Verbindung zu zwei SQL Server-VMs remote desktop Dateien starten. Des Administrators **AzureAdmin** Benutzername und Kennwort **Contoso 000**, die Sie beim Erstellen der VMs angegeben.

1. Stellen Sie sicher, dass die Computer erfolgreich **corp.contoso.com**hinzugefügt haben.

1. Warten der SQL Server-Installations Ausführung automatisierter Initialisierungsaufgaben vor.

1. Öffnen Sie ein PowerShell im Administratormodus.

1. Installieren Sie das Feature Windows-Failovercluster.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. **CORP\Install** als lokaler Administrator hinzufügen

        net localgroup administrators "CORP\Install" /Add

1. Importieren Sie die PowerShell-Anbieter von SQL Server.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. **CORP\Install** für die Standardinstanz des SQL Server-Rolle Sysadmin hinzufügen

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. Hinzufügen **Systemkonto** als Benutzernamen mit den drei oben beschriebenen Berechtigungen.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Öffnen des Firewalls für remote-Zugriff auf SQL Server.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Beide VMs melden.

        logoff.exe

Schließlich können Sie die verfügbarkeitsgruppe konfigurieren. PowerShell-Anbieter von SQL Server verwendet die Arbeit für **ContosoSQL1**ausführen.

## <a name="configure-the-availability-group"></a>Konfiguration der Availability Group

1. Eine Verbindung zu **ContosoSQL1** erneut aufrufen remote desktop Dateien. Anstatt das Computerkonto verwenden, melden Sie sich mit **CORP\Install**.

1. Öffnen Sie ein PowerShell im Administratormodus.

1. Definieren Sie die folgenden Variablen:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. Importieren Sie PowerShell-Anbieter für SQL Server.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Ändern Sie das SQL Server-Dienstkonto für ContosoSQL1 in CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Ändern Sie das SQL Server-Dienstkonto für ContosoSQL2 in CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. **CreateAzureFailoverCluster.ps1** [Erstellen WSFC](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) Cluster für immer auf Verfügbarkeit in Azure VM in das lokale Verzeichnis herunterladen Dieses Skript verwendet einen funktionalen WSFC-Cluster erstellen. Wichtige Informationen auf Interaktion zwischen WSFC und Azure-Netzwerk finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

1. In Ihrem Arbeitsverzeichnis ändern und WSFC-Cluster mit dem heruntergeladenen Skript erstellen.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Aktivieren Sie immer auf Verfügbarkeitsgruppen für die Standardinstanzen von SQL Server **ContosoSQL1** und **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Erstellen Sie ein Sicherungsverzeichnis und Berechtigungen für die SQL Server-Dienstkonten. Sie verwenden dieses Verzeichnis die Verfügbarkeit Datenbank auf dem sekundären Replikat.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Datenbank auf **ContosoSQL1** namens **MyDB1**erstellen, nehmen Sie eine vollständige Sicherung und eine Sicherung und auf **ContosoSQL2** mit der Option **WITH NORECOVERY** wiederhergestellt.

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. Erstellen Sie Verfügbarkeit Gruppe Endpunkte auf VMs SQL Server und die Berechtigungen auf die Endpunkte.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. Erstellen von Replikaten Verfügbarkeit.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Schließlich erstellen Sie die verfügbarkeitsgruppe und beitreten Sie sekundäre Replikat Verfügbarkeit Gruppe.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun erfolgreich SQL Server ständig implementiert eine verfügbarkeitsgruppe in Azure erstellen. Konfigurieren Sie einen Listener für diese verfügbarkeitsgruppe finden Sie unter [Konfigurieren einer ILB Listener für immer auf Verfügbarkeit in Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Weitere Informationen zur Verwendung von SQL Server in Azure finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
