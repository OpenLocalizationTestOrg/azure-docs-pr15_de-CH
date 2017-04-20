<properties
    pageTitle="Entfernen von Servern und Schutz deaktivieren | Microsoft Azure" 
    description="Dieser Artikel beschreibt, wie ein Depot Site Recovery Server abmelden, und Schutz für virtuelle Computer und physischen Servern." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="remove-servers-and-disable-protection"></a>Entfernen von Servern und Schutz deaktivieren

Azure Site Recovery-Dienst trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern und Servern orchestriert. Computer können Azure oder einem lokalen sekundären Rechenzentrum repliziert werden. Finden Sie einen schnellen Überblick über [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie Server aus der Site Recovery Tresor abmelden und deaktivieren Sie den Schutz für virtuelle Maschinen von Site Recovery geschützt. 

Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="unregister-a-vmm-server"></a>VMM-Server Abmelden

VMM-Server von einem Depot wird durch Löschen des Servers auf der Registerkarte **Server** in Azure Site Recovery-Portal abmelden. Beachten Sie Folgendes:

-  **Verbundene VMM-Server**: Verbindung in Azure, Registrierung den VMM-Server empfohlen. Dies stellt sicher, dass auf lokale VMM-Server und VMM-Server zugeordnet (VMM-Server, die Wolken zu enthalten, die auf dem Server zugeordnet sind, die Sie löschen möchten) ordnungsgemäß bereinigt werden. Wir empfehlen, nur einen nicht verbundenen Server entfernen, wenn eine permanente Problem mit.
- **Nicht verbundene VMM-Server**: Wenn der VMM-Server verbunden ist, wenn Sie löschen müssen Skript manuell, um die Bereinigung auszuführen. Das Skript ist in der [Microsoft-Katalog](http://aka.ms/asr-cleanup-script-vmm). Beachten Sie die VMM-ID des Servers um manuelle Bereinigung abzuschließen.
- **VMM-Server im Cluster**: VMM-Server abmelden, die in einem Cluster bereitgestellt werden soll, gehen Sie folgendermaßen vor:

    - Wenn der Server verbunden ist, löschen Sie den angeschlossenen VMM-Server auf **der Registerkarte** Den Anbieter auf dem Server deinstallieren, melden Sie sich auf jedem Clusterknoten und vom Bedienfeld deinstallieren. Das Skript Cleanup im vorherigen Abschnitt auf allen passiven Knoten im Cluster Registrierungseinträge löschen verwiesen.
    - Wenn der Server verbunden ist, müssen Sie das Bereinigungsskript auf allen Clusterknoten ausgeführt.

### <a name="unregister-an-unconnected-vmm-server"></a>Registrierung VMM-server

Auf dem VMM-Server, den Sie entfernen möchten:

1. Registrierung des VMM-Servers von Azure-Portal.
2. Herunterladen Sie auf dem VMM-Server das Bereinigungsskript
3. Öffnen Sie PowerShell mit Ausführen als Administrator die Ausführungsrichtlinie Standardbereich (LocalMachine) ändern können.
4. Führen Sie die Schritte im Skript. 

In VMM-Server, die Wolken mit auf dem Server verbunden sind, die Sie entfernen möchten:

1. Führen Sie das Cleanup-Skript und führen Sie die Schritte 2 bis 4.
2. Geben Sie die VMM-ID für den VMM-Server nicht aufgehoben wurde. 
3. Dieses Skript entfernt die Registrierungsinformationen für den VMM-Server und Cloud Kopplungs-Information.


## <a name="unregister-a-hyper-v-server-in-a-hyper-v-site"></a>Hyper-V Server Hyper-V-Website abmelden

Bei der Bereitstellung Azure Site Recovery zu virtuellen Maschinen auf einem Hyper-V Server Hyper-V-Standort (mit keine VMM-Server) können Sie Hyper-V Server von einem Depot folgendermaßen Abmelden:

1. Deaktivieren Sie den Schutz für virtuelle Maschinen auf dem Hyper-V Server.
2. Wählen Sie auf der Registerkarte **Server** in Azure Site Recovery-Portal Server > löschen. Der Server muss in Azure verbunden werden, wenn Sie dies tun.
3. Führen Sie das folgende Skript auf dem Server bereinigen und aus dem Tresor abmelden. 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## <a name="stop-protecting-a-hyper-v-virtual-machine"></a>Beenden Sie den Schutz einer Hyper-V-Computer

Beenden einer Hyper-V-Computer schützen möchten, müssen Sie Schutz entfernen. Je nachdem wie Sie Schutz entfernen möglicherweise schutzeinstellungen auf dem Computer manuell bereinigen. 

### <a name="remove-protection"></a>Schutz aufheben

1. Wählen Sie den virtuellen Computer in die Cloud-Eigenschaften auf der Registerkarte **virtuelle Computer** > **Entfernen**.
2. Auf der Seite **bestätigen Entfernen der virtuellen Computer** haben Sie einige Optionen:

    - **Schutz zu deaktivieren**, aktivieren und diese Option zum Speichern der virtuelle Computer wird nicht geschützt werden von Site Recovery. Einstellungen für den virtuellen Computer werden automatisch bereinigt.
    - **Entfernen Sie aus dem Tresor**– bei Auswahl dieser Option die Virtual Machine von Site Recovery Tresor entfernt. Lokale Einstellungen für die virtuelle Maschine wird nicht beeinflusst. Sie müssen die Einstellung manuell zu Einstellungen und entfernen den virtuellen Computer aus dem Azure-Abonnement müssen sie bereinigen beschrieben manuell mit Einstellungen entfernen bereinigen.

Wenn der virtuelle Computer und die Festplatten löschen werden sie aus am Zielort entfernt.

### <a name="clean-up-protection-settings-manually-between-vmm-sites"></a>Einstellungen manuell (standortübergreifende VMM) bereinigen

Bei Auswahl von **Beenden Sie die Verwaltung der virtuellen Computer**manuell bereinigen Sie Einstellungen:

1. Führen Sie dieses Skript in der VMM-Konsole Einstellungen für den primären virtuellen Computer bereinigen, auf dem primären Server. Klicken Sie in der VMM-Konsole PowerShell VMM PowerShell-Konsole öffnen. Ersetzen Sie SQLVM1 durch den Namen des virtuellen Computers.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Auf dem sekundären VMM-Server dieses Skript die Einstellungen für den sekundären virtuellen Computer bereinigen:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. Auf dem sekundären VMM-Server nach dem Ausführen des Skripts aktualisiert die virtuellen Computer auf dem Hyper-V-Hostserver sekundäre VM wieder in der VMM-Konsole erkannt wird.
4. Die Schritte werden nur Replikation Settings-VMM-Server gelöscht. Wenn Sie virtuelle Computer Replikation für den virtuellen Computer entfernen möchten. Sie müssen dazu die folgenden Schritte auf dem primären und sekundären virtuellen Maschinen. Führen Sie die unten beschriebene Skript Replikation entfernen und Ersetzen SQLVM1 durch den Namen des virtuellen Computers.

        Remove-VMReplication –VMName “SQLVM1”


### <a name="clean-up-protection-settings-manually-between-on-premises-vmm-sites-and-azure"></a>Einstellungen manuell (zwischen lokalen VMM Standorte und Azure) bereinigen

1. Auf dem Quellserver VMM Skript dieses die Einstellungen für den primären virtuellen Computer bereinigen:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Die Schritte werden nur Replikation Settings-VMM-Server gelöscht. Nachdem Sie die Replikation von VMM-Server entfernt haben sicher, dass Sie die Replikation für die virtuellen Computer mit Hyper-V-Host-Server mit diesem Skript entfernen. Ersetzen Sie SQLVM1 durch den Namen der virtuellen Maschine und host01.contoso.com mit dem Namen des Hostservers Hyper-V.

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### <a name="clean-up-protection-settings-manually-between-hyper-v-sites-and-azure"></a>Einstellungen manuell (zwischen Hyper-V-Websites und Azure) bereinigen

1. Für die Hyper-V-Host Quellserver entfernen Replikation für den virtuellen Computer verwenden dieses Skript. Ersetzen Sie SQLVM1 durch den Namen des virtuellen Computers.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## <a name="stop-protecting-a-vmware-virtual-machine-or-a-physical-server"></a>Beenden Sie den Schutz einer virtuellen VMware-Maschine oder einen physischen server

Schutz von virtuellen VMware-Maschine oder einen physischen Server beenden möchten, müssen Sie Schutz entfernen. Je nachdem wie Sie Schutz entfernen möglicherweise schutzeinstellungen auf dem Computer manuell bereinigen. 

### <a name="remove-protection"></a>Schutz aufheben

1. Wählen Sie den virtuellen Computer in die Cloud-Eigenschaften auf der Registerkarte **virtuelle Computer** > **Entfernen**.
2. Wählen Sie eine der Optionen, auf dem **virtuellen Computer entfernen** :

    - **Deaktivieren Sie den Schutz (für Recovery Drilldown und Volume-Größe verwenden)**– nur sehen und aktivieren Sie diese Option, wenn Sie haben:
        - **Resized virtuellen Datenträger**, wenn Sie die Größe eines Volumes der virtuelle Computer in einen kritischen Zustand wechselt. In diesem Fall wählen Sie diese Option. Beibehaltung der Wiederherstellungspunkte in Azure Schutz deaktiviert. Wenn Sie Schutz für den Computer reaktivieren werden die Daten für Größe Datenträger in Azure übertragen.
        - **Einen Failover ausführen**, nachdem Sie Ihre Umgebung Azure ein Failover aus lokalen VMware virtuellen Computern oder Servern mit getestet haben, wählen Sie diese Option zum Schützen Ihrer lokalen virtuellen Computer erneut. Diese Option deaktiviert jede virtuelle Maschine und müssen zum Schutz für sie aktivieren. Beachten Sie Folgendes:
            - Deaktivieren den virtuellen Computer mit dieser Einstellung wirkt sich nicht auf das Replikat VM in Azure.
            - Mobility-Dienst darf nicht vom virtuellen Computer deinstalliert werden.
    
    - **Schutz zu deaktivieren**, aktivieren und diese Option zum Speichern der Computer wird nicht geschützt werden von Site Recovery. Einstellungen für den Computer werden automatisch bereinigt.
    - **Entfernen Sie aus dem Tresor**– bei Auswahl dieser Option der Computer nur von Site Recovery Tresor entfernt. Lokale Einstellungen für den Computer davon nicht betroffen. Auf dem Computer zu entfernen und den virtuellen Computer aus der Azure müssen Abonnement und die Einstellungen bereinigen den Mobility-Dienst deinstallieren.
    
        ![Optionen](./media/site-recovery-manage-registration-and-protection/remove-vm.png)

















